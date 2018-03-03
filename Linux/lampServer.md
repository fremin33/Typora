# Installer un serveur Lamp sous linux 

## Installation de Apache2

```
apt-get install apache2
```

## Installation de Php

```
apt-get install php
apt-get install  libapache2-mod-php7.0
```

## Installation de Mysql

```
apt-get install mysql-server
```

## Installation de PhpMyAdmin

```
apt-get install phpmyadmin
```



## Apache2

### Changer le dossier par default

On voit que le fichier 000-default.conf pointe vers /var/www/html , on le modifie

```bash
# etc/apache2/site-availables/000-default.conf
DocumentRoot /home/florian/www
```

### Configuration des paramètres du dossier où pointe Apache2

```bash
# ect/apache2/apache2.conf
<Directory /home/florian/www>
	Options Indexes FollowSymlinks
	AllowOverride All
	Require all granted
</Directory>
```

### Actualise la nouvelle configuration d'apache

```bash
sudo service apache2 reload
```

### Affiche les erreurs Apache

```bash
# etc/apache2/var/log/apache2/error.log
tail -n 30 error.log  
```

### Donner les droits à Apache

Dans le dossier  on donne tous les droits à apache lecture, écriture

```bash
# home/florian/
sudo setfacl -R -m u:florian:rwX -m u:www-data:rwX www
sudo setfacl -R -d -m u:florian:rwX -m u:www-data:rwX www 
```

## PHP

### Afficher les erreurs 

```php
# etc/php/7.0/apache2/php.ini
display_errors = on
```

### Installer xdebug

```
sudo apt-get install php-xdebug
```

### Activer xdebug 

```php
# xdebug extension
[xdebug]
xdebug.remote_enable = On
```

## Installler Composer

```bash
# tmp/
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

## Installer NodeJs et Npm

```bash
sudo apt-get update
sudo apt-get install nodejs npm
```

## Installer Git

```
sudo apt install git
```

### Génerer une clé ssh

```
ssh-keygen
```

### Copier la clé ssh

```
cat ~/.ssh/id_rsa.pub
```



vim : 

c$ => Supprime la ligne que l'on veut éditer