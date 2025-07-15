
# Introduction à Docker 

## Qu'est-ce que Docker ?

Docker est une **plateforme open-source** qui permet de **créer, livrer et exécuter des applications dans des conteneurs**.
Un conteneur est une **unité standard de logiciel** qui regroupe le **code source** de l’application ainsi que **toutes ses dépendances** (bibliothèques, binaires, fichiers de configuration) pour assurer une **exécution fiable et cohérente** dans n’importe quel environnement (développement, test, production...).

---

## Pourquoi utiliser Docker ?

* **Portabilité** : un conteneur peut être exécuté sans modification sur n’importe quel système hôte disposant de Docker.
* **Isolation** : chaque application s’exécute dans un environnement isolé, ce qui évite les conflits de dépendances.
* **Rapidité** : les conteneurs démarrent quasi instantanément car ils ne nécessitent pas de système d’exploitation complet.
* **Légèreté** : les conteneurs partagent le noyau du système hôte, ce qui réduit leur empreinte mémoire par rapport aux machines virtuelles.
* **Cohérence** : garantit le même comportement dans tous les environnements, de la machine du développeur au serveur de production.
* **Écosystème riche** : comprend Docker Hub (registre d’images), Docker Compose, intégration avec Kubernetes, etc.

---

## Concepts fondamentaux

### Images Docker

Une **image Docker** est un **modèle figé** contenant l’environnement nécessaire à l’exécution d’une application : système minimal, dépendances, code, etc.

> **Analogie** : une *image* est comme une *classe* en programmation orientée objet, tandis qu’un *conteneur* est comme une *instance* de cette classe.

### Conteneurs Docker

Un **conteneur** est une **instance en cours d’exécution** d’une image. Il est **isolé du système hôte**, mais partage son noyau. Chaque conteneur possède son propre système de fichiers, ses processus, et ses ports réseau.

### Dockerfile

Un **Dockerfile** est un fichier texte contenant les instructions nécessaires à la construction d’une image.

```dockerfile
FROM python:3.10
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
```

### Docker Hub et autres registries

**Docker Hub** est un registre d’images public. Il permet de télécharger des images officielles ou personnalisées.
D’autres registres peuvent être utilisés, comme **GitLab Container Registry**, **Amazon ECR**, **Harbor**, etc.

### Volumes

Les **volumes Docker** permettent de **conserver des données de manière persistante**, même après la suppression d’un conteneur. Ils sont particulièrement utiles pour les bases de données.

### Réseaux Docker

Docker propose plusieurs types de réseaux :

* **bridge** : réseau privé par défaut entre les conteneurs
* **host** : le conteneur utilise directement le réseau de la machine hôte
* **none** : le conteneur n’a aucun accès réseau
* **overlay** : réseau distribué pour les environnements multi-hôtes (utilisé avec Docker Swarm ou Kubernetes)

---

## Installation de Docker (exemple pour Ubuntu)

```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

---

## Commandes Docker essentielles

### Gestion des images

```bash
docker pull ubuntu:latest          # Télécharge l'image Ubuntu la plus récente
docker build -t monimage:1.0 .     # Construit une image depuis un Dockerfile
docker images                      # Liste les images présentes
docker rmi monimage:1.0            # Supprime une image
```

### Gestion des conteneurs

```bash
docker run ubuntu:latest echo "Bonjour"     # Exécute une commande dans un conteneur temporaire
docker run -d nginx                         # Lance un conteneur en arrière-plan
docker run -p 8080:80 nginx                 # Redirige les ports (localhost:8080 → conteneur:80)
docker ps                                   # Liste les conteneurs actifs
docker ps -a                                # Liste tous les conteneurs (actifs ou non)
docker stop <id>                            # Arrête un conteneur
docker start <id>                           # Démarre un conteneur arrêté
docker restart <id>                         # Redémarre un conteneur
docker rm <id>                              # Supprime un conteneur
docker exec -it <id> bash                   # Accède à un terminal interactif dans le conteneur
docker logs <id>                            # Affiche les logs du conteneur
```

### Volumes et réseaux

```bash
docker volume create monvolume
docker volume ls
docker volume rm monvolume

docker network ls
docker network create monreseau
```

### Nettoyage

```bash
docker system prune     # Supprime les conteneurs, images, volumes inutilisés
```

---

## Docker Compose

### Avantages

Docker Compose permet de **définir et exécuter des applications multi-conteneurs** à l’aide d’un simple fichier YAML.
Il facilite la configuration, l’orchestration et le déploiement local.

### Exemple de fichier `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### Commandes utiles

```bash
docker compose up           # Lance les services définis
docker compose up -d        # Lance en mode détaché
docker compose down         # Arrête et supprime les services
docker compose build        # Construit les images
docker compose ps           # Affiche les services en cours
docker compose logs         # Affiche les logs
```

---

## Docker dans un pipeline CI/CD

Docker est très utilisé dans l’automatisation du cycle de vie des applications :

1. **Build** : création d’une image à partir du code source via un Dockerfile.
2. **Test** : exécution de tests automatisés dans un environnement conteneurisé.
3. **Push** : publication de l’image sur un registre (Docker Hub, GitLab, ECR...).
4. **Deploy** : déploiement automatique de l’image sur un environnement cible (serveur, cluster...).

---

## Orchestration avec Kubernetes

**Kubernetes** est une plateforme d’orchestration open-source conçue pour :

* **Gérer des dizaines à des milliers de conteneurs** dans un cluster
* **Répartir automatiquement la charge** entre les nœuds
* **Faire monter ou descendre les services** (scalabilité horizontale)
* **Surveiller l’état des conteneurs**
* **Redémarrer automatiquement** les conteneurs défaillants
* **Assurer des mises à jour progressives et sans interruption**

---

