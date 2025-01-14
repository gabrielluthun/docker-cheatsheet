# Cheatsheet sur Docker

Ce document est un cheatsheet sur Docker. Il résume l'apprentissage de Docker via un repo de référence.

Ce cheatsheet est accessible aussi bien aux non-initiés partant de zéro, qu'aux utilisateurs un peu plus avancés ayant besoin d'un rappel.

---
## Sommaire

- [Quelle est la différence entre Bare Metal, VM et Container ?](#quelle-est-la-différence-entre-bare-metal-vm-et-container-)
- [Qu'est-ce que Docker ?](#quest-ce-que-docker-)
  * Pourquoi Docker a changé la donne ?
  * Les utilisations principales de Docker
  * Son impact
- [L'installation de Docker](#linstallation-de-docker)
- [Instance vs Image vs Conteneur](#instance-vs-image-vs-conteneur)
  * Instance
  * Image
  * Conteneur
- [Docker Hub](#docker-hub)
  * Pourquoi utiliser Docker Hub ?
  * Comment utiliser Docker Hub ?
  * Les niveaux d'abonnement
  * Les tags Docker Hub
  * Lier son terminal à Docker Hub
- [Les commandes Docker](#les-commandes-docker)
  * Les commandes de base pour les conteneurs Docker
  * Les commandes de base pour les images Docker
  * Exécuter des commandes à l'intérieur d'un conteneur Docker
  * Différences entre `docker run` et `docker exec`
  * Surveiller et dépanner un conteneur Docker
  * Dépanner un conteneur Docker

---
Avant de commencer, il faut voir la différence entre **Bare Metal**, **VM** et **Container**.

## Quelle est la différence entre Bare Metal, VM et Container ?

### Bare Metal

Un système dit **Bare Metal** est un système qui tourne directement sur le matériel.
Le meilleur exemple est lors d'une ouverture d'une application ou d'un jeu sur votre ordinateur : il tourne **directement** dessus.

### VM (Virtual Machine)

Un système dit **Virtual Machine** est comme un ordinateur dans un ordinateur. 

Chaque VM a son propre système d'exploitation, ses propres ressources, etc, sur le **même hôte**.

Il est tout à fait possible de faire tourner **plusieurs** VM sur un même hôte, ou encore un Windows en VM sur un Mac (ou inversement)...Etc

### Container
Un container est un peu comme un appartement dans un immeuble.
C'est-à-dire que chaque appartement possède ses propres ressources, mais **pas son propre immeuble**.
Concrètement, ça veut dire qu'un container possède ses **propres ressources** tout en **dépendant** du système d'exploitation de l'**hôte**.

### Tableau comparatif

| Critère | Bare Metal | VM | Container |
|---------|------------|-------|-----------|
| **Performance** | Meilleure performance (pas de couche de virtualisation) | Bonne performance avec surcharge due à la virtualisation | Très efficace (partage du système d'exploitation hôte) |
| **Isolation et Sécurité** | Moins d'isolation entre les applications | Excellente isolation (séparation complète) | Bonne isolation mais partage du même OS |
| **Flexibilité et Portabilité** | Moins flexible, lié au matériel | Flexible, déplaçable entre hôtes | Très flexible, exécutable partout |
| **Utilisation des ressources** | Utilisation complète des ressources | Moins efficace | Très efficace et optimale | 

---
## Qu'est-ce que Docker ? 

Docker est une plateforme de virtualisation légère, principalement connue pour sa solution de **conteneurisation**.

Docker est né en **2013** par la société dotCloud.
Le problème était le suivant : les développeurs exerçaient sur leur machine, et les applications ne fonctionnaient **pas** sur les serveurs de production.

### Pourquoi Docker a changé la donne ? 
4 raisons principales :
- **Isolation** : chaque conteneur fonctionne de façon isolée -> moins de conflits entre les applications + meilleure sécurité
- **Portabilité** : les conteneurs sont **légers** et **portables** -> peuvent être exécutés sur n'importe quel OS
- **Efficacité** : les conteneurs partagent le **même** OS hôte -> **moins** de ressources utilisées
- **Constance et reproduction** : Docker assure que les applications fonctionnent de la **même façon, partout**

### Les utilisations principales de Docker

Docker est utilisé dans plusieurs cas d'usage :
- **Développement d'applications** : les développeurs peuvent développer des applications sur leur machine, et les déployer sur n'importe quel serveur
- **Microservices** : Docker est idéal pour les architectures microservices 
*(découpage d'une application en plusieurs services)*
- **CI/CD** : Docker est utilisé dans les pipelines CI/CD pour automatiser le déploiement d'applications
- **Déploiement sur le cloud** : Docker est utilisé pour déployer des applications sur le cloud et est parfaitement intégré

### Son impact 

Son succès ne s'arrête pas à la résolution de problèmes de compatibilité.
Docker a introduit une **ère nouvelle** dans le domaine de l'architecture microservices, en popularisant les bonnes pratiques de **CI/CD**.

---
## L'installation de Docker 

**Note** : le propriétaire de ce repo étant sous MacOS, l'installation de Docker se fera sur MacOS.
Mais gardez à l'esprit que le processus d'installation est similaire quelque soit l'OS.

(*déso pour les utilisateurs de Windows ou Linux. <small>ou pas. 👀*)</small>


**Donc comment installer Docker sous Mac ?**

1. **Télécharger Docker Desktop** : [ici](https://www.docker.com/products/docker-desktop). 
Vérifiez bien la version de votre MacOS, et téléchargez la version correspondante.
Une fois téléchargé, ouvrez le fichier `.dmg` et suivez les instructions.

2. **Glisser-déposer Docker dans Applications** : une fois installé, glissez-déposez Docker dans le dossier Applications.

3. **Lancer Docker Desktop** : lancez Docker Desktop.
Par sécurité, MacOS vous demandera si vous êtes sûr de vouloir ouvrir l'application. Cliquez sur `Ouvrir`.

4. **Inscription** : une fois Docker Desktop ouvert, vous devrez vous inscrire pour obtenir un compte Docker si ce n'est pas déjà fait (*connexion possible via Google et GitHub)*.

5. **Vérification de l'installation** : pour vérifier que Docker est bien installé, ouvrez un terminal et           tapez la commande `docker --version`. 
Si Docker est bien installé, vous devriez voir la version de Docker s'afficher dans le terminal.
(*Exemple : `Docker version 27.4.0, build bde2b89`*)

6. **Pas de 6, c'est prêt !** 🎉

---
## Instance vs Image vs Conteneur

Sous Docker, deux termes reviennent souvent : **conteneur** et **image**.
Pour comprendre la différence, il faut d'abord aborder la notion d'instance.

### Instance
Pour expliquer ce qu'est une **instance**, prenons l'exemple de Spotify : chaque fois que vous lancez une musique au sein de la plateforme, vous écoutez une **instance** de cette chanson.

Ce n'est pas la chanson originale, mais une sorte de "copie" de celle-ci.

### Image
Une **image** est un **modèle** de conteneur. C'est-à-dire que c'est un **fichier** qui contient toutes les informations nécessaires pour créer un conteneur.
Elle ne change pas, mais selon les besoins, peut être :
- **transférée** (téléchargée, partagée, etc)
- **modifiée** (par création d'une nouvelle image)
- ou **supprimée**

### Conteneur
Un **conteneur** est une **instance** en temps réel d'une image. 
Chaque conteneur est une **application exécutable** qui fonctionne à partir de son image, mais qui possède son propre espace.
Chaque conteneur est une **application exécutable** qui fonctionne à partir de son image, mais qui possède son **propre espace**.

Maintenant, parlons de registre Docker.
Plus particulièrement, de Docker Hub.

---

## Docker Hub

**Docker Hub** est un **registre** de conteneurs. 
C'est une plateforme qui permet de stocker, partager et gérer des images Docker.

### Pourquoi utiliser Docker Hub ?
Les registres Docker **centralisent** un espace pour **stocker** et **partager** des images Docker, ce qui facilite la **collaboration** et le **partage** de conteneurs.

La particularité de Docker Hub est qu'il est **public**. 
Cela signifie que tout le monde peut y **accéder** et **télécharger** des images.

### Comment utiliser Docker Hub ?

1. **Créer un compte** : pour utiliser Docker Hub, vous devez **créer un compte**.
2. **Se connecter** : une fois le compte créé, connectez-vous à Docker Hub.
3. **Rechercher une image** : vous pouvez rechercher des images sur Docker Hub en utilisant la barre de recherche.
4. **Télécharger une image** : pour télécharger une image, cliquez sur l'image souhaitée, puis sur le bouton `Pull`.
5. **Utiliser une image** : une fois l'image téléchargée, vous pouvez l'utiliser pour créer un conteneur.

### Les niveaux d'abonnement
- **Gratuit** : pour les utilisateurs individuels, les petits projets et les tests
- **Payant** : pour les entreprises et les organisations

### Les tags Docker Hub
Les images Docker Hub sont organisées par **tags**.
Elles permettent de spécifier des versions, ou des configurations particulières d'un image.

Pour taguer une image, il faut utiliser la commande `docker tag`.

### Lier son terminal à Docker Hub
Pour lier son terminal à son compte Docker Hub, il faut utiliser la commande `docker login`.

Maintenant que c'est fait, passons <small>(enfin)</small> aux **commandes Docker**.

---
## Les commandes Docker 

Cette partie est dédiée aux commandes de base pour Docker.

### Les commandes de base pour les conteneurs Docker

#### Lancer un conteneur
```bash
docker run <nom_ou_id_conteneur>
```
Elle lance un instance d'un conteneur à partir d'une image spécifiée, et le lancera.

#### Lancer un conteneur sans bloquer le terminal
```bash
docker run -d <nom_ou_id_conteneur>
```

#### Lister les conteneurs actifs
```bash
docker ps
```
Affiche les conteneurs en cours d'exécution, fournissant des informations telles que l'ID, le nom, l'image, etc.

#### Lister tous les conteneurs, même ceux arrêtés
```bash
docker ps -a
```


#### Arrêter un conteneur
```bash
docker stop <nom_ou_id_conteneur>
```
#### Supprimer un conteneur
```bash
docker rm <nom_ou_id_conteneur>
```

### Les commandes de base pour les images Docker

#### Lister les images
```bash
docker images
```

#### Supprimer une image
```bash
docker rmi <nom_image>
```

#### Télécharger une image
```bash
docker pull <nom_image>
```

#### Pousser une image
```bash
docker push <nom_image>
```

Certaines images poussées sur Docker Hub peuvent être **taguées**, ce qui permet de spécifier des versions ou des configurations particulières.

#### Pousser une image avec un tag
```bash
docker push <nom_image>:<tag>
```

*Note : la commande de création d'images Docker est un peu plus avancée, c'est pourquoi elle n'est pas présente ici.*

### Exécuter des commandes à l'intérieur d'un conteneur Docker

#### Accéder au shell d'un conteneur Docker

```bash
docker exec -it <nom_ou_id_conteneur> sh
```

Ici, `-it` permet d'ouvrir un terminal interactif, et `sh` est le shell utilisé pour l'interaction.

#### Exécuter une commande à l'intérieur d'un conteneur Docker

```bash
docker exec <nom_ou_id_conteneur> <commande>
```

### Différences entre `docker run` et `docker exec`

- `docker run` : permet de **lancer** un conteneur à partir d'une image
- `docker exec` : permet d'**exécuter** une commande à l'intérieur d'un conteneur **déjà en cours d'exécution**

### Surveiller et dépanner un conteneur Docker

La **surveillance** et **dépannage** des conteneurs Docker est **essentielle** pour garantir le bon fonctionnement des applications.

Étapes pour sureiller un conteneur :

1 : *Lister les conteneurs*
```bash
docker ps -a
```

2 : *Vérifier les logs d'un conteneur Docker*
```bash
docker logs <nom_ou_id_conteneur>
```

3 : *Vérifier les statistiques d'un conteneur Docker*
```bash
docker stats <nom_ou_id_conteneur>
```

### Dépanner un conteneur Docker

#### Inspecter un conteneur
```bash
docker inspect <nom_ou_id_conteneur>
```

#### Exécution d'une commande dans un conteneur
```bash
docker exec -it <nom_ou_id_conteneur> <commande>
```

#### Redémarrer un conteneur
```bash
docker restart <nom_ou_id_conteneur>
```

#### Supprimer et recréer un conteneur
```bash
docker rm <nom_ou_id_conteneur>