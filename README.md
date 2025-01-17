# Docker Cheatsheet

## Table des matières
- [Différences Bare Metal vs VM vs Container](#différences-bare-metal-vs-vm-vs-container)
- [Docker en bref](#docker-en-bref)
- [Installation rapide (MacOS)](#installation-rapide-macos)
- [Concepts clés](#concepts-clés)
- [Commandes essentielles](#commandes-essentielles)
  - [Gestion des conteneurs](#gestion-des-conteneurs)
  - [Gestion des images](#gestion-des-images)
  - [Surveillance et dépannage](#surveillance-et-dépannage)
- [Docker Hub](#docker-hub)
- [Bonnes pratiques](#bonnes-pratiques)
- [Création d'image](#création-dimage)
  - [Structure Dockerfile](#structure-dockerfile)
  - [Instruction courantes](#instruction-courantes)
- [Exemple de Dockerfile](#exemple-de-dockerfile)
- [Étapes pour créer une image](#étapes-pour-créer-une-image)
- [Optimisation des images Docker](#optimisation-des-images-docker)
- [Les tags dans les images Docker](#les-tags-dans-les-images-docker)
  

## Différences Bare Metal vs VM vs Container

| Type | Description | Avantages | Inconvénients |
|------|-------------|-----------|---------------|
| **Bare Metal** | Système directement sur le matériel | Performance maximale | Moins flexible |
| **VM** | Système virtualisé complet | Isolation totale | Ressources importantes |
| **Container** | Virtualisation légère | Rapide et portable | Partage l'OS hôte |

## Docker en bref
- Plateforme de conteneurisation légère (2013)
- Isole les applications et leurs dépendances
- Garantit le fonctionnement identique sur tous les environnements

## Installation rapide (MacOS)
1. Télécharger [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Installer et lancer l'application
3. Vérifier l'installation : `docker --version`

## Concepts clés
- **Image** : Template en lecture seule
- **Conteneur** : Instance en cours d'exécution d'une image
- **Docker Hub** : Registre officiel d'images Docker

## Commandes essentielles

### Gestion des conteneurs
```bash
docker run <image>           # Lancer un conteneur
docker run -d <image>        # Lancer en arrière-plan
docker ps                    # Lister les conteneurs actifs
docker ps -a                # Lister tous les conteneurs
docker stop <id/nom>        # Arrêter un conteneur
docker rm <id/nom>          # Supprimer un conteneur
docker exec -it <id/nom> sh # Accéder au shell
```

### Gestion des images
```bash
docker images               # Lister les images
docker pull <image>         # Télécharger une image
docker push <image>         # Pousser une image
docker rmi <image>         # Supprimer une image
```

### Surveillance et dépannage
```bash
docker logs <id/nom>        # Voir les logs
docker stats <id/nom>       # Voir les stats
docker inspect <id/nom>     # Inspecter un conteneur
docker restart <id/nom>     # Redémarrer un conteneur
```

## Docker Hub
Registre public d'images Docker
  ```bash
  docker login              # Se connecter
  docker pull <image>       # Télécharger
  docker push <image>       # Publier
  docker tag <image> <tag>  # Taguer
  ```

## Bonnes pratiques
- Toujours spécifier la version des images
- Utiliser des images officielles
- Nettoyer régulièrement les conteneurs et images inutilisés
- Souvent nettoyer les fichiers temporaires avec `docker system prune`

## Création d'image
Pour créer une image -> Dockerfile
### Structure Dockerfile :
```
- FROM : image de base
- RUN : commande à exécuter
- COPY et ADD : copier des fichiers vers l'image
- CMD et ENTRYPOINT : commande à exécuter au lancement
- EXPOSE : indiquer les ports à écouter
```

### Instruction courantes
```
- WORKDIR : répertoire de travail
- ENV : variables d'environnement
- VOLUME : montage de volume
- USER : utilisateur par défaut
```

Format de fichier : fichier texte sans extension   
Son nom : `Dockerfile`

### Exemple de Dockerfile

Exemple de Dockerfile pour une application **Node.js** avec **NestJS** :
```bash
FROM node:hydrogen-slim

# Create app directory
WORKDIR /api/

# Copy files
COPY . .

# Install app dependencies
RUN npm -i

# Bundle app source
EXPOSE 3000

# Run the app
CMD ["/bin/bash","-c", "npm run start"]
```
- `FROM` : Définit l'image de base, ici `node:hydrogen-slim`.
- `WORKDIR` : Définit le répertoire de travail.
- `COPY` : Copie les fichiers de l'image de l'hôte depuis le chemin vers le chemin dans les conteneurs qui démarreront de l'image.
  - `.` : Le dossier dans lequel se trouve le `DockerFile`.
  - ` .` : Le chemin d'accès `/api/`.
- `RUN npm i` : Exécute la commande `npm install`
- `EXPOSE 3000` : Expose le port `3000` (celui de NestJS par défaut) pour permettre la communication avec l'application qui sera conteneurisé.
- `CMD ...` : Exécute une commande avec le shell `bash` de l'image de base
  - `"/bin/bash"` : Représente le shell à utiliser.
  - `"-c"` : Oblige l'utilisation de `bash` au lieu de `sh`. 
  - `npm run start` : Éxecute la commande de démarrage de l'application

### Étapes pour créer une image :

- **Préparation** : Créer un répertoire de projet contenant un Dockerfile et un répertoire pour le contenu web.
- **Personnalisation du contenu** : Modifier `index.html` pour afficher votre message ou contenu personnalisé.
- **Mise à jour du Dockerfile** : Adapter le Dockerfile pour copier le contenu web dans l'image et exposer le bon port.
- **Construction de l'image** : Utiliser `docker build` pour créer votre image Docker personnalisée.
- **Exécution du conteneur** : Démarrer un conteneur à partir de votre image avec `docker run`, mappant le port approprié.

---
## Optimisation des images Docker

- **Images de Base Légères** : Préférez des versions "alpine" ou "slim" pour vos images de base.
- **Regroupement des Instructions `RUN`** : Combine les commandes avec `&&` pour réduire les couches.
- **Nettoyage après Installation** : Supprimez les caches et fichiers temporaires après l'installation des paquets.
- **Multi-stage Builds** : Utilisez des constructions multi-étapes pour garder l'image finale aussi légère que possible.
- **Minimisation des Fichiers Copiés** : Employez `.dockerignore` pour exclure des fichiers inutiles de l'image.
- **Variables d'Environnement** : Configurez l'application en utilisant des variables d'environnement pour plus de flexibilité.

---
## Les tags dans les images Docker

Utilité des tags -> assurer le **versionning** de l'image

- **`<image>:<tag>`** : `<tag>` est la version de l'image

### Exemple :
`docker build -t mon-application:v1.0 .`

- `mon-application` : nom de l'image
- `v1.0` : version de l'image

### Bonnes pratiques 
- Utiliser des tags **explicites** (exemple : `v1.0`) 
- Utiliser le **semantic versioning** pour les tags : *major.minor.patch* (exemple : `v1.0.1`)
- Éviter la répétition de tags comme `latest` pour les images en production afin d'**éviter** les **incohérences**

---
## Partager une image Docker

### Pourquoi partager une image Docker ?
- Collaboration : facilite le travail en équipe 
- Déploiement : permet un déploiement rapide et cohérent

### Pousser une image
1. **Se connecter** à Docker Hub : `docker login`
2. **Taguer** l'image : `docker tag mon-application:v1.0 nom-utilisateur/mon-application:v1.0`
3. **Pousser** l'image : `docker push nom-utilisateur/mon-application:v1.0`

### Bonnes pratiques pour le partage d'images
- **Nommer clairement** les images : noms descriptifs et tags
- Être attentif à la **gestion des permissions**, notamment pour les données sensibles
- **Optimiser** les images avant de les partager


