#  Installer un serveur Lamp sous linux 

## Installation de Apache2

```
apt-get install apache2
```

## Installation de Php

```
apt-get install php
# apt-get install  libapache2-mod-php7.0 
# changer de version de php : sudo update-alternatives --set php /usr/bin/php7.1
```

## Installation de Mysql

```
apt-get install mysql-server
sudo mysql --user=root mysql
CREATE USER 'fremin33'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'fremin33'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
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
DocumentRoot /var/www/html

<Directory /var/www/html>
    Options Indexes FollowSymlinks
    AllowOverride All
    Require all granted
</Directory>
```

### Créer un lien symbolique vers www

```bash
sudo ln -s /var/www/html /home/florian/www 
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
sudo apt-get install -y curl apt-transport-https ca-certificates &&
  curl --fail -ssL -o setup-nodejs https://deb.nodesource.com/setup_8.x &&
  sudo bash setup-nodejs &&
  sudo apt-get install -y nodejs build-essential
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



## Installation SDK et Android 

#### Installer le paquet android SDK

```
https://doc.ubuntu-fr.org/android_sdk
https://developer.android.com/studio/index.html#command-tools
On remplace tools par le nouveau tools
```

### Désinstaller et installer la bonne version de Java 

```
dpkg-query -W -f='${binary:Package}\n' | grep -E -e '^(ia32-)?(sun|oracle)-java' -e '^openjdk-' -e '^icedtea' -e '^(default|gcj)-j(re|dk)' -e '^gcj-(.*)-j(re|dk)' -e '^java-common' | xargs sudo apt-get -y remove
sudo apt-get -y autoremove

dpkg -l | grep ^rc | awk '{print($2)}' | xargs sudo apt-get -y purge

sudo bash -c 'ls -d /home/*/.java' | xargs sudo rm -rf

sudo rm -rf /usr/lib/jvm/

for g in ControlPanel java java_vm javaws jcontrol jexec keytool mozilla-javaplugin.so orbd pack200 policytool rmid rmiregistry servertool tnameserv unpack200 appletviewer apt extcheck HtmlConverter idlj jar jarsigner javac javadoc javah javap jconsole jdb jhat jinfo jmap jps jrunscript jsadebugd jstack jstat jstatd native2ascii rmic schemagen serialver wsgen wsimport xjc xulrunner-1.9-javaplugin.so; do sudo update-alternatives --remove-all $g; done

sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update   
sudo apt-get install openjdk-8-jdk  
```

### Installer Graddle 

```
sudo apt install gradle
```

## Virtual Box

Monter un dossier entre linux et windows permanent

```
mkdir ~/new
sudo mount -t vboxsf New ~/new
sudo nano /etc/fstab
New /home/user/new vboxsf defaults 0 0
```

