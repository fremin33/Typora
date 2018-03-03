Fichier accessible mais non visible pour l'utilisateur local 

/usr/local/bin 



Configurer Linux pour PHP/Apache/MySQL/Phpmyadmin/xdebugg



PHP dans le php.ini () (/etc/php/7.0/apache2/php.ini) on affiche les erreurs 

```
display_errors = on

# xdebug extension 
[xdebug]
xdebug.remote_enable = On
```



## Xdebugg

```
sudo apt-get install php-xdebug
```

On vérifie dans le phpinfo() que xdebug.remote_enable = 1 sinon : 



## Composer

Dans le dossier tmp on installe composer pour l'utilisateur 

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```





```
# Readme.md
Script qui permet d'automatiser certaines tâches relative à git et github
- Création d'un repository sur github à partir du nom du dossier courant
- Ajoute un fichier Readme.md
- Exécute les tâches initials (init, add, commit, push)

```



```
#!bin/bash
FOLDER=${PWD##*/}
curl -u 'fremin33' https://api.github.com/user/repos -d "{\"name\":\"${FOLDER}\"}";
git init;
git remote add origin git@github.com:fremin33/${FOLDER}.git;
echo "# Readme.md" >> README.md;
git add .;
git commit -m "Initial commit";
git push -u origin master;
```

