# Bootcamp Devops Eazytraining

## [Module 1 · Docker · Conteneurisation](https://github.com/jeandonaldroselin/eazytraining-devops-bootcamp-docker) > TP4

Ce TP consiste en deux mini-exercices :

1) Créer un volume partagé entre deux conteneurs ubuntu1 et ubuntu2
2) Créer un volume de type bind mount permettant à un conteneur apache de lire le contenu d'un site web static.

### Exercice 1 

#### Étape 1 : Créez deux conteneurs rattachés au même volume

Avec la commande suivante :
```bash
docker run -d --name ubuntu1 -it -v myvol:/tmp ubuntu:18.04 /bin/bash
docker run -d --name ubuntu2 -it -v myvol:/tmp ubuntu:18.04 /bin/bash
```

On aurait pu executer la commande suivante pour créer le volume myvol avant le lancement de nos conteneurs pour plus de clareté :

```bash
docker volume create --name myvol
```

Cependant avec l'option -v au lancement d'un conteneur, docker crée automatiquement le volume s'il n'existe pas encore. Si le volume existe, docker l'utilise simplement.

#### Étape 2 : Créer un fichier toto.txt depuis le conteneur ubuntu1

Connectez-vous au terminal du conteneur ubuntu1 avec la commande suivante :

```bash
docker exec -it ubuntu1 bash
touch /tmp/toto.txt
echo "mon texte cool" >> /tmp/toto.txt
cat /tmp/toto.txt
```

Vous devriez voir dans le terminal affiché "mon texte cool"

#### Étape 3 : Vérifiez que le contenu du volume myvol soit écrit dans le conteneur ubuntu2

Connectez-vous au terminal du conteneur ubuntu2 avec la commande suivante :

```bash
docker exec -it ubuntu2 bash
cat /tmp/toto.txt
```
Vous retrouverez le contenu que vous avez saisit depuis le conteneur ubuntu1 !

### Exercice 2 

#### Étape 1 : Cloner le site static

Pour cela il suffit d'utiliser la commande suivante :

```bash
git clone https://github.com/diranetafen/static-website-example.git
```

#### Étape 2 : Démarrer un conteneur apache avec un volume

Démarrez un conteneur à partir de l'image officielle apache tout en specifiant un volume avec commande suivante: 

```bash
docker run -d --name apache1 -v ${PWD}/static-website-example:/usr/local/apache2/htdocs/ -p 8080:80 httpd:2.4
```

Le ${PWD} dans la commande permet de récupérer le chemin absolut correspondant à votre localisation actuelle dans le terminal. Très pratique pour éviter de taper manuellement le chemin complet.

Ça y est! vous devriez accéder au site à l'adresse [http://localhost:8080](http://localhost:8080)

----

### Divers

1) L'environnement de travail permettant la réalisation de ce TP a été monté avec la configuration vagrant suivante => https://github.com/diranetafen/cursus-devops/tree/master/vagrant/docker.