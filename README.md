# tpdockerdevops

---

### Partie 2 

-----

Installer l'image hello-world avec docker et la démarrer

`docker run hello-world`

-----

Installation d'une image ubuntu avec comme parametre it pour 'interactif' et bash pour spécifier le langage, ce qui permet de demarrer le conteneur ubuntu et de rentrer directement dedans en bash.

`docker run -it ubuntu bash`

**CTRL+D ou exit pour sortir**

---

La commande docker images permet de voir toutes les images qu'on a installé avec docker

`docker images`

---

La commande docker ps - a permet de voir tous les conteneurs en execution où pas via -a

`docker ps -a`

---

Le parametre -p permet de spécifier le port d'entré et de sortie du conteneur.

`docker run -p 80:80 nginx`

Le paramètre -d permet de lancer un conteneur en mode détaché

`docker run -p -d 80:80 nginx`



### Partie 5

On lance cette commande pour récuperer l'image nginx en local

`docker pull nginx`

Ensuite on lance la commande run avec le paramètre -v pour spécifier le volume (volumelocal:volumecontainer)

`docker run --name servweb --restart unless-stopped -d -p 80:80 -v /home/maxime/docker/nginx/html:/etc/nginx/html  nginx`

La commande cp permet de copier les fichiers du container en local

`docker cp servweb:/etc/nginx/nginx.conf /home/maxime/docker/nginx/html/nginx.conf`

---

### Partie 6

On build le fichier Dockerfile avec le nom nginxfile a la racine de la commande

```
FROM nginx
COPY html_directory/ /usr/share/nginx/html
```

`docker build -t nginxfile .`

Ensuite execute un run

`docker run --name some-nginx -d nginxfile`

On peut observer un facilité d'éxecution avec le dockerfile car on peut remove le container en gardant les paramètres dans le fichier Dockerfile

---

### Partie 7

On download en local les images mysql et phpmydadmin

`docker pull mysql:5.7`

`docker pull phpmyadmin/phpmyadmin`

On lance les conteneurs

`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag`

`docker run --name my-own-phpmyadmin -d --link some-mysql:db -p 8081:80 phpmyadmin/phpmyadmin`

---

### Partie 8

Dans le fichier docker-compose on mets :

```
version: '3.7'

networks:
  mynet:

volumes:
  db_data: {}

services:

  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - mynet

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:

   - "8181:80"
     environment:
            PMA_HOST: db
            PMA_PORT: 3306
            PMA_ARBITRARY: 1
            PMA_USER: wordpress
            PMA_PASSWORD: wordpress
         networks:
        - mynet
          depends_on:
             - db
```

Le fichier docker-compose.yml permet de définir et d'exécuter plusieurs conteneurs en utilisant une configuration unique. Cela facilite la gestion des conteneurs et leur exécution en production, car on peut définir toutes les options de configuration dans un seul fichier, puis utiliser une seule commande pour exécuter tous les conteneurs.

Cela peut être particulièrement utile lorsque il faut exécuter une application complexe composée de plusieurs conteneurs. Au lieu d'avoir à exécuter plusieurs commandes docker run pour chaque conteneur, on peut définir tous les conteneurs dans un fichier docker-compose.yml et les exécuter en utilisant la commande docker-compose up.


Pour configurer les variables dans un conteneur soit on spécifie les variables avec `-e` en executant la commande run soit avec docker on peut spécifier les variables dans la partie environment.

---

### Partie 9

```
version: '3.7'
 
networks:
  frontend:
  backend:
 
volumes:
  db_data: {}
 
services:
 
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
    networks:
      - backend
 
 app:
    image: praqma/network-multitool
    networks:
      - frontend
      - backend
 
  wordpress:
    image: wordpress:latest
    volumes:
      - ./docker/php.ini:/usr/local/etc/php/conf.d/wp-php.ini
      - ./wordpress:/var/www/html
    ports:
      - "84:80"
    restart: unless-stopped
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks:
      - frontend
    depends_on:
      - db
```

Avec la commande `docker network inspect 'networkname'` on peut afficher les informations sur le réseau et savoir quels sont les sérvices connecté à eux. Le résultat affichera les adresses IP attribué aux services et au réseau, ainsi que sur les réseaux auxquels chaque service est connecté.

Cette configuration pourrait être utilisée dans un environnement d'application où l'application est divisée en différents services qui communiquent entre eux via des réseaux.
Cela permet de sécuriser les communications entre ces différents services en limitant les interactions entre eux uniquement aux échanges nécessaires pour l'application.



