# tpdockerdevops

---

Installer l'image hello-world avec docker et la démarrer

`docker run hello-world`

---

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

---



