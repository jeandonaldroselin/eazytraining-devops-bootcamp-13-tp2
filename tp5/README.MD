# Bootcamp Devops Eazytraining

## [Module 1 · Docker · Conteneurisation](https://github.com/jeandonaldroselin/eazytraining-devops-bootcamp-docker) > TP5

Ce TP consiste à créer une première infrastructure as code avec docker compose.

#### Étape 1 : Créer un réseau bridge odoo_network

```bash
docker network odoo_network
docker network ls
```

#### Étape 2 : Créer le fichier docker-compose.yml

Contenu présent dans [docker-compose.yml](docker-compose.yml) et s'inspirant des différents docker-compose.yml proposés sur la fiche docker hub de l'image odoo.

#### Étape 3 : Démarrer l'infrastructure
Avec la commande suivante :

```bash
docker compose up -d
```

Attention cette commande doit être exécutée depuis le dossier tp5 ou se trouve le fichier docker-compose.yml

#### Étape 4 : Vérifier que les conteneurs sont sur le réseau odoo_network

Avec la commande suivante :

```bash
docker network inspect odoo_network
```

Normalement, vous devriez voir les conteneurs apparaitre tout en bas dans le noeud  "Containers" apparaitre dans la liste avec le nom odoo_network.

Ça y est! vous devriez accéder à l'erp à l'adresse [http://localhost:80](http://localhost:80)

----

### Divers

1) L'environnement de travail permettant la réalisation de ce TP a été monté avec la configuration vagrant suivante => https://github.com/diranetafen/cursus-devops/tree/master/vagrant/docker.