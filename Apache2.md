## Apache2

### Afficher les fichiers de configurations du site par default 

/etc/apache2/site-availables/000-default.conf

- On voit que le fichier 000-default.conf pointe vers /var/www/html

  ```
  DocumentRoot /var/www/html
  On change en 
  DocumentRoot /home/florian/www
  ```



/etc/apache2/site-anaible

Lien symbolique vers le site availables (on actives les lien symbolique ici avec les nouveau fichier.conf)



### Définir les dossiers où Apache aura accées

/ect/apache2/apache2.conf

Ici on définit les droits 

```bash
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





### Affiche les erreurs apache

/etc/apache2/var/log/apache2/error.log

Affiche les 30 dernières lignes du fichier log pour afficher les errors

```bash
tail -n 30 error.log  
```



## Méthode 2 

On garde la config de base de apache 

DocumentRoot var/www/html

et on crée un lien symbolique vers un dossier 

ln -s /home/florian/www var/www/mes_sites



### Droit pour apache 

Dans le dossier www on donne tous les droits à apache lecture, écriture

```
sudo setfacl -R -m u:florian:rwX -m u:www-data:rwX www
sudo setfacl -R -d -m u:florian:rwX -m u:www-data:rwX www 
```





vim : 

c$ => Supprime la ligne que l'on veut éditer

o