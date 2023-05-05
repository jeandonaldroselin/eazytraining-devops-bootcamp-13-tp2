# Bootcamp Devops Eazytraining

## [Module 1 · Docker · Conteneurisation](https://github.com/jeandonaldroselin/eazytraining-devops-bootcamp-docker) > TP2

Ce TP consiste à créer notre première image Docker en suivant les étapes suivantes : 

#### Étape 1 : Créer un Dockerfile

Contenu présent dans Dockerfile

#### Étape 2 : Installer git et nginx

Contenu présent dans Dockerfile

#### Étape 3 : Cloner le projet static-website-example

Contenu présent dans Dockerfile (git clone https://github.com/diranetafen/static-website-example.git)

#### Étape 4 : Lancer un build en local et tester l'image

Contenu présent dans Dockerfile

#### Étape 5 : Publier l'image sur le registry public Docker Hub manuellement

Avec la commande suivante :

```bash

docker image push jeandonaldroselin/eazytraining-devops-bootcamp-13-tp2:latest

```
#### Étape 6 : Automatiser le déploiement sur le registry public Docker Hub

Le déploiement est désormais déclenché automatiquement suite à chaque modification de la branche main (git push, PR ou modif en ligne). Les fichiers permettant cet automatisation se situent .github/workflows


----

### Divers

1) Le résultat est disponible ici => https://hub.docker.com/r/jeandonaldroselin/bootcamp-devops-1-docker-tp2.

2) L'environnement de travail permettant la réalisation de ce TP a été construit avec la configuration vagrant suivante => https://github.com/diranetafen/cursus-devops/tree/master/vagrant/docker.