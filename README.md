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
- Éviter les secrets dans les images

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

