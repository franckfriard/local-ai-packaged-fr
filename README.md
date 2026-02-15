# 📦 Package IA Auto-hébergé (Self-hosted AI Starter Kit)

> **Note :** Ceci est une version française du projet original [local-ai-packaged](https://github.com/coleam00/local-ai-packaged) de Cole. Il s'agit d'un environnement complet pour déployer des agents IA en local ou sur serveur.

Ce modèle Docker Compose permet de lancer rapidement un environnement de développement **IA & Low-Code** entièrement fonctionnel et sécurisé. Il combine :
* **Ollama** pour exécuter vos modèles de langage (LLM) en local.
* **n8n** pour créer des workflows et des agents autonomes.
* **Open WebUI** pour une interface de chat (type ChatGPT) connectée à vos agents.
* **Supabase** pour la base de données (Postgres), le stockage vectoriel et l'authentification.

Cette version améliorée inclut également **Flowise**, **Neo4j**, **Langfuse**, **SearXNG** et **Caddy** ! Des workflows d'agents RAG pré-construits sont inclus dans le dossier `n8n/backup/workflows/`.

⚠️ **IMPORTANT :** Si vous mettez à jour une installation existante (avant le 14 juin), vous devez ajouter `POOLER_DB_POOL_SIZE=5` dans votre fichier `.env`.

---

## 🔗 Liens Utiles
* [Forum de la communauté Local AI](https://forum.ottomator.ai/) (oTTomator Think Tank)
* [Tableau de bord du projet](https://github.com/users/coleam00/projects/3) (Roadmap & Bugs)
* [Kit de démarrage original n8n](https://github.com/n8n-io/self-hosted-ai-starter-kit)

---

## 🛠️ Ce qui est inclus dans la stack

* ✅ **n8n (Self-hosted)** : Plateforme d'automatisation Low-code avec +400 intégrations.
* ✅ **Supabase** : Alternative Open Source à Firebase. La base de données de référence pour les agents IA.
* ✅ **Ollama** : Pour faire tourner les derniers modèles LLM (Llama 3, Mistral, etc.) en local.
* ✅ **Open WebUI** : Interface utilisateur riche pour interagir avec vos modèles et agents n8n.
* ✅ **Flowise** : Constructeur d'agents IA en mode "Drag & Drop".
* ✅ **Qdrant** : Base de données vectorielle haute performance (conservée en plus de Supabase pour sa rapidité spécifique sur le RAG).
* ✅ **Neo4j** : Moteur de graphe de connaissances (Knowledge Graph) pour le GraphRAG.
* ✅ **SearXNG** : Moteur de méta-recherche respectueux de la vie privée (agrège Google, Bing, etc. sans traçage).
* ✅ **Caddy** : Serveur Web automatique pour gérer le HTTPS et vos noms de domaine.
* ✅ **Langfuse** : Outil d'observabilité pour monitorer et débugger vos agents LLM.

---

## 📋 Prérequis

Avant de commencer, assurez-vous d'avoir installé :
* **Python** (Requis pour le script de démarrage)
* **Git** (Pour cloner le projet)
* **Docker & Docker Desktop** (Indispensable pour faire tourner les conteneurs)

---

## 🚀 Installation

### 1. Cloner le dépôt
```bash
git clone [https://github.com/franckfriard/local-ai-packaged-fr.git](https://github.com/franckfriard/local-ai-packaged-fr.git)
cd local-ai-packaged-fr

```

### 2. Configuration des variables d'environnement

Faites une copie du fichier d'exemple et renommez-le :

```bash
cp .env.example .env

```

Ouvrez le fichier `.env` et remplissez les variables de sécurité (générez des mots de passe forts pour la production !) :

```ini
############# Configuration N8N ############
N8N_ENCRYPTION_KEY=votre_clé_magique
N8N_USER_MANAGEMENT_JWT_SECRET=votre_secret_jwt

############# Secrets Supabase ############
POSTGRES_PASSWORD=votre_mot_de_passe_db
JWT_SECRET=votre_secret_jwt_supabase
ANON_KEY=votre_clé_anon
SERVICE_ROLE_KEY=votre_clé_service
DASHBOARD_USERNAME=admin
DASHBOARD_PASSWORD=votre_mot_de_passe_admin
POOLER_TENANT_ID=votre_tenant_id

############# Secrets Neo4j ############
NEO4J_AUTH=neo4j/votre_mot_de_passe

############# Identifiants Langfuse ############
CLICKHOUSE_PASSWORD=votre_mdp
MINIO_ROOT_PASSWORD=votre_mdp
LANGFUSE_SALT=votre_salt
NEXTAUTH_SECRET=votre_secret
ENCRYPTION_KEY=votre_clé

```

### 3. Démarrage des services

Le script `start_services.py` facilite le lancement selon votre matériel.

**Pour les utilisateurs Nvidia GPU :**

```bash
python start_services.py --profile gpu-nvidia

```

**Pour les utilisateurs Mac / Apple Silicon (M1/M2/M3) :**
Docker sur Mac ne peut pas accéder au GPU directement.

* **Option Recommandée :** Installez [Ollama pour Mac](https://ollama.com/download) directement sur votre machine (hors Docker) pour la vitesse, puis lancez le reste de la stack :
```bash
python start_services.py --profile none

```


*(Note : Dans ce cas, configurez `OLLAMA_HOST=host.docker.internal:11434` dans le `docker-compose.yml` section n8n)*.
* **Option Tout CPU (Lent) :**
```bash
python start_services.py --profile cpu

```



---

## 🌐 Environnement (Privé vs Public)

Le script accepte un argument `--environment` :

* `private` (par défaut) : Idéal pour le **local**. De nombreux ports sont ouverts pour faciliter le développement.
* `public` : Idéal pour un **VPS/Cloud**. Seuls les ports 80 et 443 sont ouverts via Caddy pour une sécurité maximale.

Exemple pour un déploiement serveur sécurisé :

```bash
python3 start_services.py --profile gpu-nvidia --environment public

```

---

## ⚡️ Démarrage Rapide & Utilisation

Une fois les services lancés :

1. **Accédez à n8n :** Ouvrez `http://localhost:5678`. Créez votre compte admin local.
2. **Importez les Workflows :**
* Dans n8n, allez dans "Workflows" > "Import from File".
* Sélectionnez les fichiers JSON situés dans le dossier `n8n/backup/workflows/` de ce projet.


3. **Configurez les Credentials dans n8n :**
* **Ollama :** URL = `http://ollama:11434`
* **Postgres (Supabase) :** Host = `db`, User/Pass = ceux de votre `.env`.
* **Qdrant :** URL = `http://qdrant:6333`


4. **Accédez à Open WebUI :** Ouvrez `http://localhost:3000`.
* Créez un compte (local uniquement).
* Pour connecter n8n : Allez dans `Workspace` > `Functions` > `Add Function`.
* Collez le code du fichier `n8n_pipe.py`.
* Activez la fonction et collez l'URL du Webhook de Production de votre workflow n8n.



---

## 🔧 Dépannage (Troubleshooting)

* **Supabase Pooler redémarre en boucle :** Voir cette [issue GitHub](https://github.com/supabase/supabase/issues).
* **Erreur de connexion Database :** Assurez-vous que votre mot de passe Postgres **ne contient pas** le caractère `@`.
* **Problème GPU Windows :** Vérifiez que vous utilisez bien le backend **WSL 2** dans Docker Desktop.
* **Nœud "Local File Trigger" manquant :** Dans n8n v2+, ce nœud est désactivé par sécurité. Pour l'activer, décommentez `NODES_EXCLUDE=[]` dans le fichier `docker-compose.yml`.

---

## 📜 Licence

Ce projet est sous licence **Apache-2.0**.
Basé sur le travail de l'équipe n8n et les contributions de la communauté Open Source.

```

```
