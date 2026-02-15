Voici la traduction complète du fichier `README.md` en français, en conservant le formatage Markdown original pour que vous puissiez le copier-coller directement.

---

# Package IA Auto-hébergé (Self-hosted AI Package)

**Le Package IA Auto-hébergé** est un modèle Docker Compose ouvert qui amorce rapidement un environnement de développement **IA locale et Low Code** complet. Il inclut Ollama pour vos LLM locaux, Open WebUI pour une interface de chat avec vos agents n8n, et Supabase pour votre base de données, votre stockage vectoriel et l'authentification.

Ceci est la version de Cole avec quelques améliorations et l'ajout de Supabase, Open WebUI, Flowise, Neo4j, Langfuse, SearXNG et Caddy !
Des workflows d'agents IA RAG pré-construits (issus de la vidéo) sont inclus dans `n8n/backup/workflows/` - voir [Importation des workflows de démarrage](https://www.google.com/search?q=%23importation-des-workflows-de-d%C3%A9marrage) pour les instructions de configuration.

**IMPORTANT** : Supabase a mis à jour certaines variables d'environnement, vous devrez donc peut-être ajouter de nouvelles valeurs par défaut dans votre `.env` (disponibles dans mon `.env.example`) si vous aviez déjà ce projet en cours d'exécution et que vous récupérez les nouveaux changements. Plus précisément, vous devez ajouter "POOLER_DB_POOL_SIZE=5" à votre fichier `.env`. Ceci est requis si vous utilisiez le package avant le 14 juin.

## Liens Importants

* [Forum de la communauté Local AI](https://thinktank.ottomator.ai/c/local-ai/18) sur le oTTomator Think Tank.
* [Tableau Kanban GitHub](https://github.com/users/coleam00/projects/2/views/1) pour l'implémentation des fonctionnalités et la correction de bugs.
* [Kit de démarrage Local AI original](https://github.com/n8n-io/self-hosted-ai-starter-kit) par l'équipe n8n.
* Téléchargez mon intégration N8N + OpenWebUI [directement sur le site Open WebUI.](https://openwebui.com/f/coleam/n8n_pipe/) (plus d'instructions ci-dessous).

Sélectionné par [https://github.com/n8n-io](https://github.com/n8n-io) et [https://github.com/coleam00](https://github.com/coleam00), ce projet combine la plateforme n8n auto-hébergée avec une liste organisée de produits et composants IA compatibles pour démarrer rapidement la création de workflows IA auto-hébergés.

### Ce qui est inclus

✅ **[n8n Auto-hébergé](https://n8n.io/)** - Plateforme Low-code avec plus de 400 intégrations et des composants IA avancés.

✅ **[Supabase](https://supabase.com/)** - Base de données open source "as a service" - la base de données la plus utilisée pour les agents IA.

✅ **[Ollama](https://ollama.com/)** - Plateforme LLM multi-plateforme pour installer et exécuter les derniers LLM locaux.

✅ **[Open WebUI](https://openwebui.com/)** - Interface de type ChatGPT pour interagir en privé avec vos modèles locaux et vos agents N8N.

✅ **[Flowise](https://flowiseai.com/)** - Constructeur d'agents IA No/low code qui s'associe très bien avec n8n.

✅ **[Qdrant](https://qdrant.tech/)** - Stockage vectoriel (Vector Store) open source haute performance avec une API complète. Bien que vous puissiez utiliser Supabase pour le RAG, celui-ci a été conservé contrairement à Postgres car il est plus rapide que Supabase et constitue parfois une meilleure option.

✅ **[Neo4j](https://neo4j.com/)** - Moteur de graphe de connaissances qui propulse des outils comme GraphRAG, LightRAG et Graphiti.

✅ **[SearXNG](https://searxng.org/)** - Moteur de méta-recherche internet gratuit et open source qui agrège les résultats de jusqu'à 229 services de recherche. Les utilisateurs ne sont ni suivis ni profilés, d'où la pertinence avec le package IA local.

✅ **[Caddy](https://caddyserver.com/)** - HTTPS/TLS géré pour les domaines personnalisés.

✅ **[Langfuse](https://langfuse.com/)** - Plateforme d'ingénierie LLM open source pour l'observabilité des agents.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les logiciels suivants :

* [Python](https://www.python.org/downloads/) - Requis pour exécuter le script d'installation.
* [Git/GitHub Desktop](https://desktop.github.com/) - Pour une gestion facile du dépôt.
* [Docker/Docker Desktop](https://www.docker.com/products/docker-desktop/) - Requis pour exécuter tous les services.

## Installation

Clonez le dépôt et naviguez vers le répertoire du projet :

```bash
git clone -b stable https://github.com/coleam00/local-ai-packaged.git
cd local-ai-packaged

```

Avant de lancer les services, vous devez configurer vos variables d'environnement pour Supabase en suivant leur [guide d'auto-hébergement](https://supabase.com/docs/guides/self-hosting/docker#securing-your-services).

1. Faites une copie de `.env.example` et renommez-la en `.env` dans le répertoire racine du projet.
2. Définissez les variables d'environnement requises suivantes :
```bash
############
# Configuration N8N
############
N8N_ENCRYPTION_KEY=
N8N_USER_MANAGEMENT_JWT_SECRET=

############
# Secrets Supabase
############
POSTGRES_PASSWORD=
JWT_SECRET=
ANON_KEY=
SERVICE_ROLE_KEY=
DASHBOARD_USERNAME=
DASHBOARD_PASSWORD=
POOLER_TENANT_ID=

############
# Secrets Neo4j
############   
NEO4J_AUTH=

############
# Identifiants Langfuse
############

CLICKHOUSE_PASSWORD=
MINIO_ROOT_PASSWORD=
LANGFUSE_SALT=
NEXTAUTH_SECRET=
ENCRYPTION_KEY=  

```



> [!IMPORTANT]
> Assurez-vous de générer des valeurs aléatoires sécurisées pour tous les secrets. N'utilisez jamais les valeurs d'exemple en production.

3. Définissez les variables d'environnement suivantes si vous déployez en production, sinon laissez-les commentées :
```bash
############
# Config Caddy
############

N8N_HOSTNAME=n8n.votredomaine.com
WEBUI_HOSTNAME=:openwebui.votredomaine.com
FLOWISE_HOSTNAME=:flowise.votredomaine.com
SUPABASE_HOSTNAME=:supabase.votredomaine.com
OLLAMA_HOSTNAME=:ollama.votredomaine.com
SEARXNG_HOSTNAME=searxng.votredomaine.com
NEO4J_HOSTNAME=neo4j.votredomaine.com
LETSENCRYPT_EMAIL=votre-adresse-email

```



---

Le projet inclut un script `start_services.py` qui gère le démarrage des services Supabase et de l'IA locale. Le script accepte un drapeau `--profile` pour spécifier quelle configuration GPU utiliser.

### Pour les utilisateurs de GPU Nvidia

```bash
python start_services.py --profile gpu-nvidia

```

> [!NOTE]
> Si vous n'avez jamais utilisé votre GPU Nvidia avec Docker auparavant, veuillez suivre les
> [instructions Docker pour Ollama](https://github.com/ollama/ollama/blob/main/docs/docker.mdx).

### Pour les utilisateurs de GPU AMD sous Linux

```bash
python start_services.py --profile gpu-amd

```

### Pour les utilisateurs Mac / Apple Silicon

Si vous utilisez un Mac avec un processeur M1 ou plus récent, vous ne pouvez malheureusement pas exposer votre GPU à l'instance Docker. Il existe deux options dans ce cas :

1. Exécuter le kit de démarrage entièrement sur le CPU :
```bash
python start_services.py --profile cpu

```


2. Exécuter Ollama sur votre Mac pour une inférence plus rapide, et s'y connecter depuis l'instance n8n :
```bash
python start_services.py --profile none

```


Si vous souhaitez exécuter Ollama sur votre Mac, consultez la [page d'accueil d'Ollama](https://ollama.com/) pour les instructions d'installation.

#### Pour les utilisateurs Mac exécutant OLLAMA localement

Si vous exécutez OLLAMA localement sur votre Mac (pas dans Docker), vous devez modifier la variable d'environnement `OLLAMA_HOST` dans la configuration du service n8n. Mettez à jour la section `x-n8n` dans votre fichier Docker Compose comme suit :

```yaml
x-n8n: &service-n8n
  # ... autres configurations ...
  environment:
    # ... autres variables d'environnement ...
    - OLLAMA_HOST=host.docker.internal:11434

```

De plus, après avoir vu "Editor is now accessible via: https://www.google.com/search?q=http://localhost:5678/" :

1. Allez sur https://www.google.com/search?q=http://localhost:5678/home/credentials
2. Cliquez sur "Local Ollama service"
3. Changez l'URL de base pour "[http://host.docker.internal:11434/](https://www.google.com/search?q=http://host.docker.internal:11434/)"

### Pour tous les autres

```bash
python start_services.py --profile cpu

```

### L'argument environment

Le script **start-services.py** offre la possibilité de passer l'une des deux options pour l'argument environment, **private** (environnement par défaut) et **public** :

* **private :** vous déployez la stack dans un environnement sûr, donc beaucoup de ports peuvent être rendus accessibles sans avoir à se soucier de la sécurité.
* **public :** la stack est déployée dans un environnement public, ce qui signifie que la surface d'attaque doit être réduite au minimum. Tous les ports sauf le 80 et le 443 sont fermés.

La stack initialisée avec

```bash
   python start_services.py --profile gpu-nvidia --environment private

```

équivaut à celle initialisée avec

```bash
   python start_services.py --profile gpu-nvidia

```

## Déploiement dans le Cloud

### Prérequis pour les étapes ci-dessous

* Machine Linux (de préférence Ubuntu) avec Nano, Git et Docker installés.

### Étapes supplémentaires

Avant d'exécuter les commandes ci-dessus pour récupérer le dépôt et tout installer :

1. Exécutez les commandes en tant que root pour ouvrir les ports nécessaires :
* ufw enable
* ufw allow 80 && ufw allow 443
* ufw reload


---


**ATTENTION**
ufw ne protège pas les ports publiés par docker, car les règles iptables configurées par docker sont analysées avant celles configurées par ufw. Il existe une solution pour modifier ce comportement, mais cela dépasse le cadre de ce projet. Assurez-vous simplement que tout le trafic passe par le service Caddy via le port 443. Le port 80 ne doit être utilisé que pour rediriger vers le port 443.
---


2. Exécutez le script **start-services.py** avec l'argument d'environnement **public** pour indiquer que vous allez exécuter le package dans un environnement public. Le script s'assurera que tous les ports, sauf le 80 et le 443, sont fermés, ex :

```bash
   python3 start_services.py --profile gpu-nvidia --environment public

```

3. Configurez les enregistrements A (DNS) chez votre fournisseur pour faire pointer vos sous-domaines (définis dans le fichier .env pour Caddy) vers l'adresse IP de votre instance cloud.
Par exemple, un enregistrement A pour pointer n8n vers [IP de l'instance cloud] pour https://www.google.com/url?sa=E&source=gmail&q=n8n.votredomaine.com

**NOTE** : Si vous utilisez une machine cloud sans la commande "docker compose" disponible par défaut, comme une instance GPU Ubuntu sur DigitalOcean, exécutez ces commandes avant de lancer start_services.py :

* DOCKER_COMPOSE_VERSION=$(curl -s [https://api.github.com/repos/docker/compose/releases/latest](https://api.github.com/repos/docker/compose/releases/latest) | grep 'tag_name' | cut -d" -f4)
* sudo curl -L "[https://www.google.com/search?q=https://github.com/docker/compose/releases/download/$](https://github.com/docker/compose/releases/download/$){DOCKER_COMPOSE_VERSION}/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
* sudo chmod +x /usr/local/bin/docker-compose
* sudo mkdir -p /usr/local/lib/docker/cli-plugins
* sudo ln -s /usr/local/bin/docker-compose /usr/local/lib/docker/cli-plugins/docker-compose

## Importation des workflows de démarrage

Ce package inclut des workflows n8n pré-construits dans le dossier `n8n/backup/workflows/`. Pour les importer :

1. Ouvrez n8n à l'adresse [http://localhost:5678/](https://www.google.com/search?q=http://localhost:5678/) (ou votre domaine personnalisé si déployé dans le cloud).
2. Allez dans votre liste de workflows et cliquez sur le menu à trois points ou utilisez **Import from File** (Importer depuis un fichier).
3. Sélectionnez les fichiers JSON du dossier `n8n/backup/workflows/` sur votre machine locale.

Pour des instructions détaillées, consultez la [documentation officielle d'import/export n8n](https://docs.n8n.io/workflows/export-import/).

> [!NOTE]
> Vous devrez créer des identifiants (credentials) pour chaque workflow après l'importation. Voir l'étape 3 du Démarrage rapide ci-dessous.

## ⚡️ Démarrage rapide et utilisation

Le composant principal du kit de démarrage IA auto-hébergé est un fichier docker compose pré-configuré avec le réseau et le disque, donc il n'y a pas grand-chose d'autre à installer. Après avoir terminé les étapes d'installation ci-dessus, suivez les étapes ci-dessous pour commencer.

1. Ouvrez [http://localhost:5678/](https://www.google.com/search?q=http://localhost:5678/) dans votre navigateur pour configurer n8n. Vous n'aurez à faire cela qu'une seule fois. Vous ne créez PAS de compte avec n8n ici, c'est seulement un compte local pour votre instance !
2. Importez un workflow depuis `n8n/backup/workflows/` (voir [Importation des workflows de démarrage](https://www.google.com/search?q=%23importation-des-workflows-de-d%C3%A9marrage)), puis ouvrez-le depuis votre liste de workflows.
3. Créez des identifiants pour chaque service :
URL Ollama : http://ollama:11434
Postgres (via Supabase) : utilisez DB, username et password du fichier .env. IMPORTANT : L'hôte (Host) est 'db' car c'est le nom du service exécutant Supabase.
URL Qdrant : http://qdrant:6333 (La clé API peut être n'importe quoi car cela tourne localement).
Google Drive : Suivez [ce guide de n8n](https://docs.n8n.io/integrations/builtin/credentials/google/).
N'utilisez pas localhost pour l'URI de redirection, utilisez simplement un autre domaine que vous possédez, cela fonctionnera quand même !
Alternativement, vous pouvez configurer des [déclencheurs de fichiers locaux (local file triggers)](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.localfiletrigger/).
4. Sélectionnez **Test workflow** pour lancer le workflow.
5. Si c'est la première fois que vous exécutez le workflow, vous devrez peut-être attendre qu'Ollama finisse de télécharger Llama3.1. Vous pouvez inspecter les logs de la console docker pour vérifier la progression.
6. Assurez-vous d'activer le workflow et de copier l'URL du webhook de "Production" !
7. Ouvrez [http://localhost:3000/](https://www.google.com/search?q=http://localhost:3000/) dans votre navigateur pour configurer Open WebUI.
Vous n'aurez à faire cela qu'une seule fois. Vous ne créez PAS de compte avec Open WebUI ici, c'est seulement un compte local pour votre instance !
8. Allez dans Workspace -> Functions -> Add Function -> Donnez un nom + description puis collez le code de `n8n_pipe.py`.
La fonction est également [publiée ici sur le site d'Open WebUI](https://openwebui.com/f/coleam/n8n_pipe/).
9. Cliquez sur l'icône d'engrenage et définissez `n8n_url` avec l'URL de production du webhook que vous avez copiée à l'étape précédente.
10. Activez la fonction et elle sera désormais disponible dans votre menu déroulant de modèles en haut à gauche !

Pour ouvrir n8n à tout moment, visitez [http://localhost:5678/](https://www.google.com/search?q=http://localhost:5678/) dans votre navigateur.
Pour ouvrir Open WebUI à tout moment, visitez [http://localhost:3000/](https://www.google.com/search?q=http://localhost:3000/).

Avec votre instance n8n, vous aurez accès à plus de 400 intégrations et une suite de nœuds IA basiques et avancés tels que les nœuds
[AI Agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/),
[Text classifier](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.text-classifier/),
et [Information Extractor](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.information-extractor/).
Pour tout garder en local, n'oubliez pas d'utiliser le nœud Ollama pour votre modèle de langage et Qdrant comme stockage vectoriel.

> [!NOTE]
> Ce kit de démarrage est conçu pour vous aider à démarrer avec des workflows IA auto-hébergés. Bien qu'il ne soit pas entièrement optimisé pour des environnements de production, il combine des composants robustes qui fonctionnent bien ensemble pour des projets de preuve de concept (POC). Vous pouvez le personnaliser pour répondre à vos besoins spécifiques.

## Mise à jour (Upgrading)

Pour mettre à jour tous les conteneurs vers leurs dernières versions (n8n, Open WebUI, etc.), exécutez ces commandes :

```bash
# Arrêter tous les services
docker compose -p localai -f docker-compose.yml --profile <votre-profil> down

# Récupérer les dernières versions de tous les conteneurs
docker compose -p localai -f docker-compose.yml --profile <votre-profil> pull

# Redémarrer les services avec votre profil souhaité
python start_services.py --profile <votre-profil>

```

Remplacez `<votre-profil>` par l'un des suivants : `cpu`, `gpu-nvidia`, `gpu-amd`, ou `none`.

Note : Le script `start_services.py` lui-même ne met pas à jour les conteneurs - il les redémarre simplement ou les télécharge si vous les téléchargez pour la première fois. Pour obtenir les dernières versions, vous devez exécuter explicitement les commandes ci-dessus.

## Dépannage (Troubleshooting)

Voici des solutions aux problèmes courants que vous pourriez rencontrer :

### Problèmes Supabase

* **Redémarrage du Pooler Supabase** : Si le conteneur supabase-pooler redémarre sans cesse, suivez les instructions dans [cette issue GitHub](https://github.com/supabase/supabase/issues/30210#issuecomment-2456955578).
* **Échec du démarrage de Supabase Analytics** : Si le conteneur supabase-analytics ne démarre pas après avoir changé votre mot de passe Postgres, supprimez le dossier `supabase/docker/volumes/db/data`.
* **Si vous utilisez Docker Desktop** : Allez dans les paramètres Docker et assurez-vous que "Expose daemon on tcp://localhost:2375 without TLS" est activé.
* **Service Supabase Indisponible** - Assurez-vous de ne pas avoir de caractère "@" dans votre mot de passe Postgres ! Si la connexion au conteneur kong fonctionne (les logs du conteneur indiquent qu'il reçoit des requêtes de n8n) mais que n8n dit qu'il ne peut pas se connecter, c'est généralement le problème d'après ce que la communauté a partagé. D'autres caractères pourraient aussi être interdits, le symbole @ est juste celui dont je suis sûr !
* **Redémarrage de SearXNG** : Si le conteneur SearXNG redémarre sans cesse, exécutez la commande "chmod 755 searxng" dans le dossier local-ai-packaged pour que SearXNG ait les permissions nécessaires pour créer le fichier uwsgi.ini.
* **Fichiers non trouvés dans le dossier Supabase** - Si vous obtenez des erreurs concernant des fichiers manquants dans le dossier supabase/ comme .env, docker/docker-compose.yml, etc., cela signifie très probablement que vous avez eu une "mauvaise" récupération (pull) du dépôt GitHub Supabase lorsque vous avez exécuté le script start_services.py. Supprimez entièrement le dossier supabase/ dans le dossier Local AI Package et réessayez.

### Problèmes de support GPU

* **Support GPU Windows** : Si vous avez du mal à faire fonctionner Ollama avec le support GPU sous Windows avec Docker Desktop :
1. Ouvrez les paramètres Docker Desktop
2. Activez le backend WSL 2
3. Consultez la [documentation Docker GPU](https://docs.docker.com/desktop/features/gpu/) pour plus de détails


* **Support GPU Linux** : Si vous avez du mal à faire fonctionner Ollama avec le support GPU sous Linux, suivez les [instructions Docker Ollama](https://github.com/ollama/ollama/blob/main/docs/docker.md).

### Problèmes de nœuds n8n

* **Nœuds Local File Trigger ou Execute Command non disponibles** : À partir de n8n v2+, ces nœuds sont désactivés par défaut pour la sécurité. Pour les activer, décommentez `NODES_EXCLUDE=[]` dans la section `x-n8n` du fichier `docker-compose.yml` et redémarrez n8n. Voir [Accéder aux fichiers locaux](https://www.google.com/search?q=%23acc%C3%A9der-aux-fichiers-locaux) pour des instructions détaillées.

## 👓 Lectures recommandées

n8n regorge de contenu utile pour démarrer rapidement avec ses concepts d'IA et ses nœuds. Si vous rencontrez un problème, allez sur le [support](https://www.google.com/search?q=%23support).

* [Agents IA pour les développeurs : de la théorie à la pratique avec n8n](https://blog.n8n.io/ai-agents/)
* [Tutoriel : Construire un workflow IA dans n8n](https://docs.n8n.io/advanced-ai/intro-tutorial/)
* [Concepts Langchain dans n8n](https://docs.n8n.io/advanced-ai/langchain/langchain-n8n/)
* [Démonstration des différences clés entre agents et chaînes](https://docs.n8n.io/advanced-ai/examples/agent-chain-comparison/)
* [Que sont les bases de données vectorielles ?](https://docs.n8n.io/advanced-ai/examples/understand-vector-databases/)

## 🎥 Guide vidéo

* [Guide de Cole pour le Local AI Starter Kit](https://youtu.be/pOsO40HSbOo)

## 🛍️ Plus de modèles IA

Pour plus d'idées de workflows IA, visitez la **[galerie officielle de modèles IA n8n](https://n8n.io/workflows/?categories=AI)**. Depuis chaque workflow, sélectionnez le bouton **Use workflow** pour importer automatiquement le workflow dans votre instance n8n locale.

### Apprendre les concepts clés de l'IA

* [Chat Agent IA](https://n8n.io/workflows/1954-ai-agent-chat/)
* [Chat IA avec n'importe quelle source de données (utilisant aussi le workflow n8n)](https://n8n.io/workflows/2026-ai-chat-with-any-data-source-using-the-n8n-workflow-tool/)
* [Chat avec OpenAI Assistant (en ajoutant une mémoire)](https://n8n.io/workflows/2098-chat-with-openai-assistant-by-adding-a-memory/)
* [Utiliser un LLM open-source (via HuggingFace)](https://n8n.io/workflows/1980-use-an-open-source-llm-via-huggingface/)
* [Chat avec des documents PDF utilisant l'IA (citation des sources)](https://n8n.io/workflows/2165-chat-with-pdf-docs-using-ai-quoting-sources/)
* [Agent IA capable de scraper des pages web](https://n8n.io/workflows/2006-ai-agent-that-can-scrape-webpages/)

### Modèles IA locaux

* [Assistant Code Fiscal](https://n8n.io/workflows/2341-build-a-tax-code-assistant-with-qdrant-mistralai-and-openai/)
* [Découper des documents en notes d'étude avec MistralAI et Qdrant](https://n8n.io/workflows/2339-breakdown-documents-into-study-notes-using-templating-mistralai-and-qdrant/)
* [Assistant Documents Financiers utilisant Qdrant et](https://n8n.io/workflows/2335-build-a-financial-documents-assistant-using-qdrant-and-mistralai/) [ Mistral.ai](http://mistral.ai/)
* [Recommandations de recettes avec Qdrant et Mistral](https://n8n.io/workflows/2333-recipe-recommendations-with-qdrant-and-mistral/)

## Trucs & Astuces

### Accéder aux fichiers locaux

Le kit de démarrage IA auto-hébergé créera un dossier partagé (par défaut, situé dans le même répertoire) qui est monté sur le conteneur n8n et permet à n8n d'accéder aux fichiers sur le disque. Ce dossier dans le conteneur n8n est situé à `/data/shared` -- c'est le chemin que vous devrez utiliser dans les nœuds qui interagissent avec le système de fichiers local.

**Nœuds qui interagissent avec le système de fichiers local**

* [Lire/Écrire des fichiers depuis le disque (Read/Write Files from Disk)](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.filesreadwrite/)
* [Déclencheur de fichier local (Local File Trigger)](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.localfiletrigger/)
* [Exécuter une commande (Execute Command)](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.executecommand/)

**Activer les nœuds Local File Trigger et Execute Command**

À partir de n8n v2+, les nœuds `Local File Trigger` et `Execute Command` sont désactivés par défaut pour des raisons de sécurité. Pour les activer dans cet environnement local/auto-hébergé :

1. Ouvrez `docker-compose.yml`
2. Trouvez la section `x-n8n` et décommentez la ligne `NODES_EXCLUDE` :
```yaml
x-n8n: &service-n8n
  image: n8nio/n8n:latest
  environment:
    # ... autres variables ...
    - NODES_EXCLUDE=[]

```


3. Redémarrez le conteneur n8n :
```bash
docker compose -p localai -f docker-compose.yml --profile <votre-profil> up -d n8n

```



Voir [Changements majeurs n8n 2.0 (Breaking Changes)](https://docs.n8n.io/2-0-breaking-changes/#disable-executecommand-and-localfiletrigger-nodes-by-default) pour plus de détails.

## 📜 Licence

Ce projet (créé à l'origine par l'équipe n8n, lien en haut du README) est sous licence Apache License 2.0 - voir le fichier [LICENSE](https://www.google.com/search?q=LICENSE) pour plus de détails.
