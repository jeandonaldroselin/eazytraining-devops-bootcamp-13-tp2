Bootcamp Devops Eazytraining · [1 Docker](../) · TP3

----------

Ce TP consiste à créer une réseau Docker puis deux conteneurs et réussir à réaliser un ping de part et d'autre.

## Étape 1 : Créer un Dockerfile

On reprend le contenu du [Dockerfile du TP2](../tp2/Dockerfile)

## Étape 2 : Ajouter l'installation du paquet iputils-ping

Contenu présent dans Dockerfile

## Étape 3 : Créer une nouvelle image (build)

Avec la commande suivante :

```bash
docker build . -t jeandonaldroselin/bootcamp-devops-1-docker-tp3:latest
```

## Étape 4 : Créer un réseau bridge my-net

Avec la commande suivante :

```bash
docker network create my-net
docker network ls
```

Normalement, vous devriez voir le réseau apparaitre dans la liste avec le nom my-net dans la colonne NAME.

## Étape 5 : Créer deux conteneur (mycontainer1 et mycontainer2) rattachés à ce réseau my-net

Il existe plusieurs méthodes pour rattacher des conteneurs à un réseau, nous allons utiliser celle-là qui est plus éfficace dans le cadre d'un TP :

```bash
docker run --name mycontainer1 -d --network my-net --publish 8080:80 jeandonaldroselin/bootcamp-devops-1-docker-tp3:latest

docker run --name mycontainer2 -d --network my-net --publish 8081:80 jeandonaldroselin/bootcamp-devops-1-docker-tp3:latest
```

## Étape 6 : Obtenir l'adresse IP des conteneurs crées avec docker inspect

Étant donné que nous aurons besoin de l'adresse IP des conteneurs mycontainer1 et mycontainer2
dans les étapes 5 et 6, il convient d'obtenir ses IP en amont.

Obtenez l'IP du conteneur mycontainer1 en executant la commande suivante :
```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer1
```

Obtenez l'IP du conteneur mycontainer2 en executant la commande suivante :
```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer2
```

## Étape 5 : Effectuer le ping depuis le conteneur mycontainer1

Connectez-vous au terminal du conteneur mycontainer1 avec la commande suivante :

```bash
docker exec -it mycontainer1 bash
```

Effectuez un ping du conteneur mycontainer2 en utilisant son adresse IP avec la commande suivante: 

```bash
ping (IP de mycontainer2)
```

Vous devriez voir des lignes contenant les données sous cette forme : 

```bash
PING mycontainer2 (IP de mycontainer2) 56(84) bytes of data.
64 bytes from (ID de mycontainer2) (IP de mycontainer2): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer2) (IP de mycontainer2): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer2) (IP de mycontainer2): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer2) (IP de mycontainer2): icmp_seq=1 ttl=64 time=0.030 ms
```

Effectuez un ping du conteneur mycontainer2 en utilisant son nom avec la commande suivante :

```bash
ping mycontainer2
```

Vous obtiendrez le même résultat.

## Étape 6 : Effectuer le ping depuis le conteneur mycontainer2

Connectez-vous au terminal du conteneur mycontainer2 avec la commande suivante :

```bash
docker exec -it mycontainer2 bash
```

Effectuez un ping du conteneur mycontainer1 en utilisant son adresse IP avec la commande suivante: 

```bash
ping (IP de mycontainer1)
```

Vous devriez voir des lignes contenant les données sous cette forme : 

```bash
PING mycontainer1 (IP de mycontainer1) 56(84) bytes of data.
64 bytes from (ID de mycontainer1) (IP de mycontainer1): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer1) (IP de mycontainer1): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer1) (IP de mycontainer1): icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from (ID de mycontainer1) (IP de mycontainer1): icmp_seq=1 ttl=64 time=0.030 ms
```

Effectuez un ping du conteneur mycontainer1 en utilisant son nom avec la commande suivante :

```bash
ping mycontainer1
```

Vous obtiendrez le même résultat.

----

# Divers

1) L'environnement de travail permettant la réalisation de ce TP a été construit avec la configuration vagrant suivante => https://github.com/diranetafen/cursus-devops/tree/master/vagrant/docker.