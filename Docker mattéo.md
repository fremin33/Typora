# Docker mattéo

1 - Fichier docker/build/php-apache/vhost/*.conf

Mettre le nom du dossier du projet

```
    DocumentRoot /var/www/one2one
```

docker-compose.yml

```
    php-apache:
        build:
          context: build/php-apache
          dockerfile: Dockerfile
        ports:
            - 80:80
        volumes:
            - ${APP}:/var/www/one2one    => mettre de nom indiquer dans le vh
            
```

docker exec -it e5c097dafd0f bash



penser a vider le dossier mysql pour un nouveau projet



accée localhost 