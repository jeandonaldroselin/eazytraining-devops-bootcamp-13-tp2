# Bootcamp Devops Eazytraining

## [Module 1 · Docker · Conteneurisation](https://github.com/jeandonaldroselin/eazytraining-devops-bootcamp-docker) > TP6

Ce TP consiste à créer un premier cluster en utilisant docker swarm et docker compose.
Ce cluster devra contenir un manager et un worker.

#### Étape 1 : Créer un Docker manager (créer le cluster)

Ajouter une première instance qui servira de Docker manager.

Pour transformer un hote docker en Docker manager, il suffit de lancer la commande suivante

```bash
docker swarm init
```

Normalement cette commande retournera le contenu suivant :

```bash
To add a worker to this swarm, run the following command:

    docker swarm join --token UN-TOKEN-UNIQUE-POUR-REJOINDRE-LE-SWARM ADRESSE-IP-CHOISIE:PORT
```

Dans certains cas (le mien), le contenu retourné demandera de relancer la commande en choisissant l'adresse IP que l'on souhaite utiliser pour permettre aux workers de rejoindre. Il faudra alors relancer la commande de cette manière : 

```bash
docker swarm init --advertise-addr ADRESSE-IP-CHOISIE
```

J'ai personellement choisit l'adresse IP visible dans le terminal root@ADRESSE-IP-CHOISIE

#### Étape 2 : Ajouter un Docker worker

Les opérations que nous allons exécuter ci-dessous sont à répéter autant de fois que vous souhaitez ajouter de Docker worker. J'ai personellement testé avec 2 workers.

Ajouter une deuxième instance qui servira de Docker worker.

Puis executer la commande précédemment affichée lorsqu'on a transformé le Docker host en Docker manager de cette façon :

```bash
docker swarm join --token UN-TOKEN-UNIQUE-POUR-REJOINDRE-LE-SWARM ADRESSE-IP-CHOISIE:PORT
```

Le terminal devrait renvoyer le contenu suivant :

```bash
This node joined a swarm as a worker.
```

#### Étape 3 : Déployer example voting app dans le clusteur

Lorsque l'on souhaite déployer des services au sein d'un cluster, il faut se rendre sur une des machine Docker manager.

Donc rendons nous sur l'instance sur lequel tourne le Docker manager et cloner le projet de la manière suivante :

```bash
git clone https://github.com/dockersamples/example-voting-app.git
cd example-voting-app
```
(vous pouvez également cloner ce projet et vous placer dans le dossier tp6/example-voting-app)

Déployer le projet en lancant la commande suivante :

```bash
docker stack deploy -c docker-stack.yml example-voting-app
```

Vérifier que les services sont bien déployés dans le cluster avec la commande suivante :

```bash
docker service ls
```

Normalement, un contenu sous la forme suivante est visible vous permettant de voir combien de répricas sont déployés.

```bash
ID             NAME                        MODE         REPLICAS   IMAGE                                          PORTS
y12t90pud45a   example-voting-app_db       replicated   1/1        postgres:15-alpine                             
lxqs1tyz4xrj   example-voting-app_redis    replicated   1/1        redis:alpine                                   
5bbx9ddswj7k   example-voting-app_result   replicated   1/1        dockersamples/examplevotingapp_result:latest   *:5001->80/tcp
ttk5e0d1mdjv   example-voting-app_vote     replicated   2/2        dockersamples/examplevotingapp_vote:latest     *:5000->80/tcp
g9chvhdndrk1   example-voting-app_worker   replicated   2/2        dockersamples/examplevotingapp_worker:latest
```

#### Étape 4 : Vérifier la répartition des conteneurs

Si vous souhaitez savoir dans quels workers sont déployés vos conteneurs, il suffit de lancer la commande suivante :

```bash
docker service ps example-voting-app_vote
```

Vous obtiendrez un résultat similaire à celui-ci :

```bash
ID             NAME                        IMAGE                                        NODE      DESIRED STATE   CURRENT STATE           ERROR     PORTS
v318xr2fpfta   example-voting-app_vote.1   dockersamples/examplevotingapp_vote:latest   node2     Running         Running 8 minutes ago             
pcwa3jqixt0f   example-voting-app_vote.2   dockersamples/examplevotingapp_vote:latest   node1     Running         Running 8 minutes ago
```

Une petite surprise, vous verrez que votre service "example-voting-app_vote" est déployé à la fois sur node1 et node2.

Le node1 est... le Docker manager. En effet celui-ci est également considéré comme un worker.

Ça y est ! vous pouvez accéder aux applications du vote en cliquant sur les port 5000 et 5001 qui sont apparus à côté du bouton "OPEN PORT". L'application qui tourne sur le port 5000 permet de voter, tandis que l'application qui tourne sur le port 5001 permet de restituer les votes à l'initiateur du vote.

----

### Divers

1) L'environnement de travail de ce TP est situé à l'adresse => https://docker.labs.eazytraining.fr. Une version publique est disponible sur l'adresse suivante => https://labs.play-with-docker.com/.