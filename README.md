Voici la version du **cours Docker** en **Markdown pur**, sans emojis et sans ton conversationnel :

---

````markdown
# Introduction à Docker : Un Guide Complet

Bienvenue dans ce guide complet sur Docker. Ce document fournit une compréhension claire et approfondie de Docker, de ses concepts fondamentaux à ses usages pratiques.

---

## Qu'est-ce que Docker ?

Docker est une plateforme open-source qui permet de créer, livrer et exécuter des applications dans des conteneurs. Un conteneur est une unité standard de logiciel qui regroupe le code et toutes ses dépendances pour que l'application s'exécute rapidement et de manière fiable dans n'importe quel environnement.

---

## Pourquoi utiliser Docker ?

- Portabilité : le même conteneur peut être exécuté sur différentes machines sans modifications.
- Isolation : chaque application tourne dans son propre conteneur.
- Rapidité : les conteneurs démarrent en quelques secondes.
- Légèreté : plus légers que les machines virtuelles, car ils partagent le noyau de l’hôte.
- Cohérence : comportement identique en développement, test et production.
- Écosystème : Docker Hub, Docker Compose, intégration avec Kubernetes.

---

## Concepts fondamentaux

### Images Docker

Une image est un modèle figé contenant le code de l’application, les bibliothèques, les dépendances et les fichiers nécessaires à l’exécution.

> Image = classe (définition), conteneur = objet (instance)

### Conteneurs Docker

Un conteneur est une instance active d'une image. Il est isolé du système d’exploitation hôte mais partage le noyau.

### Dockerfile

Fichier texte contenant les instructions de construction d’une image.

```dockerfile
FROM python:3.10
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
````

### Docker Hub et autres registries

Docker Hub est un registre d’images public. Il est possible d’utiliser des registres privés (GitLab, AWS ECR, etc.).

### Volumes

Les volumes permettent de stocker des données persistantes au-delà du cycle de vie d’un conteneur.

### Réseaux Docker

* bridge : réseau interne par défaut entre conteneurs
* host : accès direct au réseau de l’hôte
* none : aucun accès réseau
* overlay : réseau multi-hôtes (cluster)

---

## Installation de Docker (exemple Ubuntu)

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

### Images

```bash
docker pull ubuntu:latest
docker build -t monimage:1.0 .
docker images
docker rmi monimage:1.0
```

### Conteneurs

```bash
docker run ubuntu:latest echo "Bonjour"
docker run -d nginx
docker run -p 8080:80 nginx
docker ps
docker ps -a
docker stop <id>
docker start <id>
docker restart <id>
docker rm <id>
docker exec -it <id> bash
docker logs <id>
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
docker system prune
```

---

## Docker Compose

### Avantages

* Définit plusieurs services dans un fichier unique
* Démarrage en une seule commande
* Support du réseau, des volumes, de la dépendance entre services

### Exemple de fichier docker-compose.yml

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
docker compose up
docker compose up -d
docker compose down
docker compose build
docker compose ps
docker compose logs
```

---

## Bonnes pratiques

* Utiliser des images minimales (ex : alpine)
* Ne stocker aucun mot de passe dans l’image
* Utiliser `.dockerignore` pour réduire la taille de l’image
* Un conteneur = un seul processus
* Nettoyer les couches inutiles dans le Dockerfile
* Préciser les versions des images (ex: `nginx:1.21` au lieu de `latest`)

---

## Docker dans un pipeline CI/CD

1. Build : construction de l’image depuis le Dockerfile
2. Test : exécution de tests dans des conteneurs
3. Push : publication de l’image dans un registre
4. Deploy : déploiement de l’image sur les serveurs

---

## Orchestration avec Kubernetes

Kubernetes est utilisé pour gérer un grand nombre de conteneurs :

* Répartition automatique de la charge
* Scalabilité
* Surveillance
* Redémarrage automatique
* Mise à jour sans interruption

---

## Conclusion

Ce document présente l’essentiel pour comprendre et utiliser Docker :

* Maîtrise des images, conteneurs, volumes et réseaux
* Construction et exécution avec Dockerfile et Compose
* Intégration dans un workflow DevOps moderne

Docker est un outil fondamental pour le développement et le déploiement d'applications modernes.

```

Souhaitez-vous que je génère un fichier `.md` prêt à télécharger ?
```
