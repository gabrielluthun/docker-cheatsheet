# Cheatsheet sur Docker

Ce document est un cheatsheet sur Docker. Il résume l'apprentissage de Docker via un repo de référence.

Avant de commencer, il faut voir la différence entre Bare Metal, VM et Container.

### Quelle est la différence entre Bare Metal, VM et Container ?

#### Bare Metal

Un système dit **Bare Metal** est un système qui tourne directement sur le matériel.
Le meilleur exemple est lors d'une ouverture d'une application ou d'un jeu sur votre ordinateur : il tourne **directement** dessus.

#### VM (Virtual Machine)

Un système dit **Virtual Machine** est comme un ordinateur dans un ordinateur. 

Chaque VM a son propre système d'exploitation, ses propres ressources, etc, sur le **même hôte**.

Il est tout à fait possible de faire tourner **plusieurs** VM sur un même hôte, ou encore un Windows en VM sur un Mac (ou inversement)...Etc

#### Container
Un container est un peu comme un appartement dans un immeuble.
C'est-à-dire que chaque container a ses propres ressources, mais **pas son propre système d'exploitation**.
Concrètement, ça veut dire qu'un container possède ses **propres ressources** tout en **dépendant** du système d'exploitation de l'**hôte**.

### Tableau comparatif

| Critère | Bare Metal | VM | Container |
|---------|------------|-------|-----------|
| **Performance** | Meilleure performance (pas de couche de virtualisation) | Bonne performance avec surcharge due à la virtualisation | Très efficace (partage du système d'exploitation hôte) |
| **Isolation et Sécurité** | Moins d'isolation entre les applications | Excellente isolation (séparation complète) | Bonne isolation mais partage du même OS |
| **Flexibilité et Portabilité** | Moins flexible, lié au matériel | Flexible, déplaçable entre hôtes | Très flexible, exécutable partout |
| **Utilisation des ressources** | Utilisation complète des ressources | Moins efficace | Très efficace et optimale | 

---
### Qu'est-ce que Docker ? 

Docker est une plateforme de virtualisation légère, principalement connue pour sa solution de **conteneurisation**.

Docker est né en **2013** par la société dotCloud.
Le problème était le suivant : les développeurs exerçaient sur leur machine, et les applications ne fonctionnaient **pas** sur les serveurs de production.

#### Pourquoi Docker a changé la donne ? 

5 raisons principales :
- **Isolation** : chaque conteneur fonctionne de façon isolée -> moins de conflits entre les applications + meilleure sécurité
- **Portabilité** : les conteneurs sont **légers** et **portables** -> peuvent être exécutés sur n'importe quel OS
- **Efficacité** : les conteneurs partagent le **même** OS hôte -> **moins** de ressources utilisées
- **Constance et reproduction** : Docker assure que les applications fonctionnent de la **même façon, partout**

#### Les utilisations de Docker

Docker est utilisé dans plusieurs cas d'usage :
- **Développement d'applications** : les développeurs peuvent développer des applications sur leur machine, et les déployer sur n'importe quel serveur
- **Microservices** : Docker est idéal pour les architectures **microservices** (découpage d'une application en plusieurs services)
- **CI/CD** : Docker est utilisé dans les pipelines CI/CD pour automatiser le déploiement d'applications
- **Déploiement sur le cloud** : Docker est utilisé pour déployer des applications sur le cloud et est parfaitement intégré

#### Son impact 

Son succès ne s'arrête pas à la résolution de problèmes de compatibilité.
Docker a introduit une **ère nouvelle** dans le domaine de l'architecture microservices, en popularisant les bonnes pratiques de **CI/CD**.
