# Docker

1 - Récupérer et crée une image Debian

docker pull debian

2- 

 sudo docker run debian bin/echo "hello"

3 - Lance le container sans qu'il se ferme directement

- -t => Lance un pseudo terminal (comme ssh)
- -i => Récupère les info du container et les renvoi dans notre terminal
- -s => Lance le container en mode daemon (arrière plan)
- -P ou -p 80:80 =>  Spécifie le port à mapper 
- docker run -t -i debian /bin/bash
- docker ps => affiche la liste des containers en cours
- docker images => affihe la liste des images téléchargés
- ifconfig => Affiche les informations de la machine
- docker stop name_container => Stop un container
- docker start name_container => Lance un container
- docker ps --all => Affiche les containers lancé précédement
- docker rm id_container => Supprime un container (un peut chainer)
- docker rmi id_image => Supprime une image(on peut chainer)
- docker build -t florian/tuto . => Lancer un dockerfile
- docker stop $(docker ps -a -q) => Stop tous les containers



uname -a => Obtenir des informations à propos du noyau.

exit => Quitter le bash du container 



### Dockerfile

Crée un fichier Dockerfile

vi Dockerfile



*****

