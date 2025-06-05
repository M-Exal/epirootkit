# Frontend

Ce projet est une interface React (Next.js) pour envoyer des commandes à un serveur via une API REST, afficher les réponses et gérer une authentification simple par commande. L'interface s'inspire visuellement d'un terminal "Matrix rain" avec une police rétro.

---

## ⚠️ WIP ⚠️

### Must add pictures of working frontend

---

## 🎯 Présentation

Cette interface React permet d’envoyer des commandes chiffrées AES-CBC à un backend via une API REST (`/api/send`), de recevoir et afficher les réponses du serveur, et de suivre un historique des commandes exécutées.

Une authentification par commande spéciale (`success\u0000`) permet de déverrouiller l’envoi de commandes normales.

---

## 🧬 Technologies utilisées

- **React** (avec hooks et gestion d’état)
- **Next.js** (App router avec `use client`)
- **TypeScript**
- **CryptoJS** pour le chiffrement AES des commandes
- **Lucide-React** pour les icônes (ChevronLeft, ChevronRight, Lock)
- **Google Fonts (VT323)** pour un style de terminal rétro
- CSS personnalisé avec animations (`blink`)

---

## 🔒 Fonctionnalités principales

- Affichage d’un message d’accueil chargé depuis `/welcome.txt`
- Envoi d’une commande chiffrée AES-CBC (clé et IV codés en dur)
- Authentification via une commande spéciale (`success\u0000`)
- Affichage de la réponse du serveur, avec code de retour et sortie standard ou erreur
- Rafraîchissement du statut de connexion toutes les 2 secondes
- Affichage d’informations système récupérées via une commande spéciale (`infos`)
- Option d’affichage d’un historique des commandes via `/api/history`
- Gestion des erreurs de communication avec le serveur

---

## 🧱 Architecture et composants

- **SendCommand** : Composant principal de la page
  - `useHydrationWarning()` : Hook pour avertir des problèmes d'hydratation côté client
  - États : `content`, `authed`, `command`, `response`, `isConnected`, `infosContent`, etc.
  - Effets pour :
    - Charger message d’accueil
    - Vérifier statut de connexion
    - Récupérer infos système
    - Récupérer historique (si activé)
  - Fonctions :
    - `handleSubmit()` : Chiffre et envoie la commande au serveur
    - `renderResponse()` : Affiche la réponse du serveur
    - `renderInfos()` : Charge et affiche les informations système
- **MatrixRain** : (importé, non montré ici) animation d’arrière-plan "Matrix"

---

## 🧠 Détails techniques

### Chiffrement

- Utilisation de `CryptoJS.AES.encrypt` en mode CBC avec padding PKCS7
- Clé et vecteur d'initialisation codés en dur (clé : "1234567890abcdef", IV : "abcdef1234567890")
- Le texte de commande est chiffré puis encodé en Base64 avant envoi

### Communication

- POST vers `/api/send` avec JSON `{ command: <commande-chiffrée> }`
- GET vers `/api/status` pour vérifier la connexion du rootkit au serveur
- GET vers `/api/history` pour récupérer l’historique des commandes (quand activé)
- GET vers `/welcome.txt` pour charger le message d’accueil

### Réponse serveur

- La réponse attendue contient un champ `tcp_data`
- Le champ est analysé pour détecter un code de retour (ex: `return code: 0`)
- Le contenu est affiché avec une mise en forme conditionnelle (succès/erreur)

Authors:

- Alexis
- Antoine
