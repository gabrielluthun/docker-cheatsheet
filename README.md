# 🐳 Docker Cheatsheet

## Table des matières

1. [Introduction à Docker](#1-introduction-à-docker)
   1.1. [Différences Bare Metal vs VM vs Container](#11-différences-bare-metal-vs-vm-vs-container)
   1.2. [Docker en bref](#12-docker-en-bref)
   1.3. [Installation rapide (MacOS)](#13-installation-rapide-macos)

2. [Concepts fondamentaux](#2-concepts-fondamentaux)
   2.1. [Images et Conteneurs](#21-images-et-conteneurs)
   2.2. [Docker Hub](#22-docker-hub)
   2.3. [Bonnes pratiques](#23-bonnes-pratiques)

3. [Commandes Docker](#3-commandes-docker)
   3.1. [Gestion des conteneurs](#31-gestion-des-conteneurs)
   3.2. [Gestion des images](#32-gestion-des-images)
   3.3. [Surveillance et dépannage](#33-surveillance-et-dépannage)

4. [Création d'images Docker](#4-création-dimages-docker)
   4.1. [Structure Dockerfile](#41-structure-dockerfile)
   4.2. [Instructions courantes](#42-instructions-courantes)
   4.3. [Exemple de Dockerfile](#43-exemple-de-dockerfile)
   4.4. [Étapes de création](#44-étapes-de-création)
   4.5. [Optimisation](#45-optimisation)
   4.6. [Tags et versioning](#46-tags-et-versioning)

5. [Partage et déploiement](#5-partage-et-déploiement)
   5.1. [Partage d'images](#51-partage-dimages)
   5.2. [Bonnes pratiques de partage](#52-bonnes-pratiques-de-partage)

6. [Stockage et persistance](#6-stockage-et-persistance)

---

## 1. Introduction à Docker

### 1.1. Différences Bare Metal vs VM vs Container

| Type | Description | ✅ Avantages | ❌ Inconvénients |
|------|-------------|------------|----------------|
| **Bare Metal** | Direct sur hardware | Performance max | Peu flexible |
| **VM** | OS virtualisé complet | Isolation totale | Lourd en ressources |
| **Container** | Virtualisation légère | Rapide & portable | Partage l'OS hôte |

### 1.2. Docker en bref
- Plateforme de conteneurisation légère (2013)
- Isole les applications et leurs dépendances
- Garantit le fonctionnement identique sur tous les environnements

### 1.3. Installation rapide (MacOS)
1. Télécharger [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Installer et lancer l'application
3. Vérifier l'installation : `docker --version`

---

## 2. Concepts fondamentaux

### 2.1. Images et Conteneurs
- **Image** : Template en lecture seule
- **Conteneur** : Instance en cours d'exécution d'une image
- **Docker Hub** : Registre officiel d'images Docker

### 2.2. Docker Hub
Registre public d'images Docker
  ```bash
  docker login              # Se connecter
  docker pull <image>       # Télécharger
  docker push <image>       # Publier
  docker tag <image> <tag>  # Taguer
  ```

### 2.3. Bonnes pratiques
- Toujours spécifier la version des images
- Utiliser des images officielles
- Nettoyer régulièrement les conteneurs et images inutilisés
- Souvent nettoyer les fichiers temporaires avec `docker system prune`

---

## 3. Commandes Docker

### 3.1. Gestion des conteneurs
```bash
docker run <image>           # Lancer un conteneur
docker run -d <image>        # Lancer en arrière-plan
docker ps                    # Lister les conteneurs actifs
docker ps -a                # Lister tous les conteneurs
docker stop <id/nom>        # Arrêter un conteneur
docker rm <id/nom>          # Supprimer un conteneur
docker exec -it <id/nom> sh # Accéder au shell
```

### 3.2. Gestion des images
```bash
docker build -t <image:tag> .   # Construire une image
docker images               # Lister les images
docker pull <image>         # Télécharger une image
docker push <image>         # Pousser une image
docker rmi <image>         # Supprimer une image
```

### 3.3. Surveillance et dépannage
```bash
docker logs <id/nom>        # Voir les logs
docker stats <id/nom>       # Voir les stats
docker inspect <id/nom>     # Inspecter un conteneur
docker restart <id/nom>     # Redémarrer un conteneur
```

---

## 4. Création d'images Docker

### 4.1. Structure Dockerfile
```
- FROM : image de base
- RUN : commande à exécuter
- COPY et ADD : copier des fichiers vers l'image
- CMD et ENTRYPOINT : commande à exécuter au lancement
- EXPOSE : indiquer les ports à écouter
```

### 4.2. Instructions courantes
```
- WORKDIR : répertoire de travail
- ENV : variables d'environnement
- VOLUME : montage de volume
- USER : utilisateur par défaut
```

Format de fichier : fichier texte sans extension   
Son nom : `Dockerfile`

### 4.3. Exemple de Dockerfile

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

### 4.4. Étapes de création

- **Préparation** : Créer un répertoire de projet contenant un Dockerfile et un répertoire pour le contenu web.
- **Personnalisation du contenu** : Modifier `index.html` pour afficher votre message ou contenu personnalisé.
- **Mise à jour du Dockerfile** : Adapter le Dockerfile pour copier le contenu web dans l'image et exposer le bon port.
- **Construction de l'image** : Utiliser `docker build` pour créer votre image Docker personnalisée.
- **Exécution du conteneur** : Démarrer un conteneur à partir de votre image avec `docker run`, mappant le port approprié.

### 4.5. Optimisation

- **Images de Base Légères** : Préférez des versions "alpine" ou "slim" pour vos images de base.
- **Regroupement des Instructions `RUN`** : Combine les commandes avec `&&` pour réduire les couches.
- **Nettoyage après Installation** : Supprimez les caches et fichiers temporaires après l'installation des paquets.
- **Multi-stage Builds** : Utilisez des constructions multi-étapes pour garder l'image finale aussi légère que possible.
- **Minimisation des Fichiers Copiés** : Employez `.dockerignore` pour exclure des fichiers inutiles de l'image.
- **Variables d'Environnement** : Configurez l'application en utilisant des variables d'environnement pour plus de flexibilité.

### 4.6. Tags et versioning

Utilité des tags -> assurer le **versionning** de l'image

- **`<image>:<tag>`** : `<tag>` est la version de l'image

### 4.6.1. Exemple de tags
`docker build -t mon-application:v1.0 .`

- `mon-application` : nom de l'image
- `v1.0` : version de l'image

### 4.6.2. Bonnes pratiques de tags
- Utiliser des tags **explicites** (exemple : `v1.0`) 
- Utiliser le **semantic versioning** pour les tags : *major.minor.patch* (exemple : `v1.0.1`)
- Éviter la répétition de tags comme `latest` pour les images en production afin d'**éviter** les **incohérences**

---

## 5. Partage et déploiement

### 5.1. Partage d'images
- Collaboration : facilite le travail en équipe 
- Déploiement : permet un déploiement rapide et cohérent

### 5.1.1. Pousser une image
1. **Se connecter** à Docker Hub : `docker login`
2. **Taguer** l'image : `docker tag mon-application:v1.0 nom-utilisateur/mon-application:v1.0`
3. **Pousser** l'image : `docker push nom-utilisateur/mon-application:v1.0`

### 5.2. Bonnes pratiques de partage
- **Nommer clairement** les images : noms descriptifs et tags
- Être attentif à la **gestion des permissions**, notamment pour les données sensibles
- **Optimiser** les images avant de les partager

---

## 6. Stockage et persistance
