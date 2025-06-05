# ⚙️ Backend – Documentation technique

Ce backend Python agit comme **passerelle entre un frontend web et un module noyau** via un canal TCP local. Il expose des routes HTTP (API REST) permettant d’envoyer des commandes à des clients connectés sur un port TCP.

---

## 📁 Fichiers

- `server.py` : le serveur principal Flask + TCP
- `setup.sh` : script d'installation, de configuration réseau, de création d'environnement virtuel et de lancement

---

## 🚀 Lancer le backend

Depuis le dossier `backend/` :

```bash
chmod +x setup.sh
./setup.sh
```

Ce script :

- configure la route réseau vers `192.168.100.1`
- crée et active un environnement virtuel Python
- installe les dépendances
- lance le backend (server.py) en tâche de fond, avec les logs dans `out.log`(poour du debug)

## 🔌 Fonctionnement

### 1. Serveur TCP

Le backend écoute sur `0.0.0.0:4242` pour accepter les connexions TCP entrantes (ex. : depuis la machine victime).

🔄 Flux TCP :

- À chaque message reçu par un client TCP, le contenu est stocké dans une file Queue.
- Si un client se déconnecte ou échoue, sa socket est fermée proprement.

```python
TCP_HOST = '0.0.0.0'
TCP_PORT = 4242
tcp_clients = []
tcp_data_queue = queue.Queue()
```

### 2. API REST (via Flask)

#### `/api/send`:

- Attend un JSON de la forme :

  - `{ "command": "ls -la" }`

- Transmet la commande à tous les clients TCP connectés
- Attend une réponse dans les 5 secondes (sinon timeout)

#### `/api/status`

- Répond avec
  - `{ "connected": true }`

## 🧵 Concurrence

Un thread secondaire démarre le serveur TCP `(start_tcp_server())`, pendant que Flask tourne dans le thread principal :

```
tcp_thread = threading.Thread(target=start_tcp_server, daemon=True)
tcp_thread.start()
app.run(port=3001, debug=False)
```

## 📦 Dépendances

Seulement Flask

```
pip install flask
```

## ✍️ Auteurs

- Alexis 💻
