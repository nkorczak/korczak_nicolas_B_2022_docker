
Sommaire :
- [Configuration de la partie "Backend"](#configuration-de-la-partie-backend)
- [Configuration de la partie "Frontend"](#configuration-de-la-partie-frontend)
- [Configuration du "Docker-compose"](#configuration-de-la-partie-docker-compose)

## Configuration de la partie Backend

**Création du Dockerfile dans la partie "backend"**

```
1: FROM node:10-alpine
2: WORKDIR /app
3: COPY . .
4: RUN npm install
5: EXPOSE 36150
6: CMD ["node", "server.js"]
```

1: Le "FROM" définit l'image de base qui sera utilisée par les instructions suivantes

2: Le "WORKDIR" définit le répertoire de travail qui sera utilisé

3: Le "COPY" permet de copier les fichiers de notre machine locale vers le conteneur docker

4: Le "RUN" exécute des commandes pendant la création de l'image

5: Le "EXPOSE" permet d'exposer un port 

6: Le "CMD" permet de lancer les commandes par défaut lors du démarrage du conteneur docker



## Configuration de la partie Frontend

**Création du Dockerfile dans la partie "frontend"**

```
1: FROM node:14-alpine
2: WORKDIR /app
3: ENV PATH /app/node_modules/.bin:$PATH
4: COPY . .
5: RUN npm install
6: EXPOSE 8081
7: CMD ["npm", "start"]
```

1: Le "FROM" définit l'image de base qui sera utilisée par les instructions suivantes

2: Le "WORKDIR" définit le répertoire de travail qui sera utilisé

3: Le "ENV" définit les variables d'environnement utilisable dans le conteneur docker

4: Le "COPY" permet de copier les fichiers de notre machine locale vers le conteneur docker

5: Le "RUN" exécute des commandes pendant la création de l'image

6: Le "EXPOSE" permet d'exposer un port 

7: Le "CMD" permet de lancer les commandes par défaut lors du démarrage du conteneur docker



## Configuration de la partie docker-compose

**Création du fichier docker-compose.yml"**

```
version: "3"
services:

  frontend:
    build: ./frontend
    depends_on:
      - backend
    ports:
      - "8081:8081"
    networks:
      - backend-net
          
  backend:
    build: ./backend
    depends_on:
      - mongo
    ports:
      - "36150:36150"
    networks: 
     - backend-net
  
  mongo:
    image: mongo
    restart: always
    volumes: 
      - mongo:/data/mongo
    environment: 
      MONGODB_INITDB_ROOT_USERNAME: nico
      MONGODB_INITDB_ROOT_PASSWORD: korczak
    networks: 
     - backend-net

networks:
  backend-net:

volumes: 
  mongo:
```
Pour la "version", il nous faut choisir la version de docker-compose en fonction de la version du moteur docker

Pour les "services", les différents services définis sont les différentes applications du conteneur

Pour cela j'ai donc défini 3 applications :

--> Un service frontend pour le front

--> Un service backend pour le back

--> Un service mongo pour la base de données

De plus, je mets en place mes "networks" cela définit le réseau par défaut de l'application

Enfin, la partie volume permet de déclarer le volume qui nous servira à stocker les données de la base de données


**_Pour aller plus loin, nous avons modifié le port par défaut de l'application "ReactJS" sur le port 8081 et nous avons modifié le port d'accès au backend sur le 36150._**
