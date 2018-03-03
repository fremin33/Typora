# CMDER 

## Git  

```bash
ssh-keygen 						=> Crée une clé public SSH
ssh-copy-id login@url			=> Copier sa clé sur un serveur pour sauter le mdp
cat ~/.ssh/id_rsa.pub | clip    => Copier la clé en dur
rm -rf .git						=> Enlever l initialisation de git
git add . 						=> Préparer les fichiers à commiter
git commit -m "My commit"		=> Créer un commit
git push origin ma_branche		=> Push les élements sur la branche
git checkout ma_branche			=> Changer de branche
git checkout -b ma_branche		=> Créer une nouvelle branche
git pull origin master 			=> Récupérer la branche master
git clone 						=> Récupérer un projet Git
ssh login@url					=> Se connecter en SSH à un serveur
```

### .gitignore

```bash
# Les ligne commençant par '#' sont des commentaires.
/folder 						=> Ignorer le dossier folder et son contenu
fichier.txt						=> Ignorer tous les fichiers nommés fichier.txt
*.html   						=> Ignorer tous les fichiers html
```



## Node 

```bash
npm init 							=> Initialise npm dans le projet et crée un fichier package.json
npm install package --save -dev		=> Installe un plugin node en ajoutant la dépendance au package.json
npm uninstall package --save 		=> Désinstalle un plugin npm en suprimmant la dépendance au package.json
npm install							=> Installe tous les plugins du package.json
```



## Bower 

```bash
bower init 						=> Initialise Bower dans le fichier et crée un fichier bower.json
bower install package --save 	=> Installe un plugin bower en ajoutant la dépendance au bower.json
bower uninstall package --save 	=> Désinstalle un plugin bower en suprimmant la dépendance au bower.json
bower install 					=> Installe tous les plugins du bower.json
```

### .bowerrc

```json
{
  "directory" : "app/js",		=> Permet de définir le dossier où seront installés les plugins
    "json" : "bower.json"   
}
```



## Wordpress 

```bash
 ssh login@url 						=> Se connecter en SSH à un serveur
 cd www								=> Accéder au dossier www
 mkdir projet 						=> Créer un dossier ayant le nom du projet
 wp core download --locale=fr_FR	=> Copie la dernière version de Wordpress
 wp core config --prompt            => Configurer le fichier de configuration Wordpress
 	Dbname : votre_identifiant
 	Dbuser : votre_identifiant
 	Dbpass : le même que pour la connexion SSH
 	Dbhost : localhost
 	Dbprefix : projet_
 	Dbcharset puis les options suivantes : laisser vide
 	
  wp core install –-prompt			=> Installer Wordpress avec la configuration définit	
 	url : http://lpcm2016.univ-lr.fr/votrelogin/evento/
	title : le titre de votre site, par exemple : evento
	admin_user : L’identifiant de votre choix pour accéder à l’administration. Notez le !
	password : Le mot de passe de votre choix : Conservez le, vous en aurez besoin !
	Email : votre email
	Skip email : laissez vide
	
cd .. 														=> Remonter au dossier précédent 
setfacl -R -d -m u:www-data:rwX -m u:$(whoami):rwX evento	 => Modification des droits
setfacl -R -m u:www-data:rwX -m u:$(whoami):rwX evento 		=> Modification des droits

nano evento/wp-config.php				=> Modifier le fichier wp-config.php
  define('FS_METHOD','direct');
  define('WP_PROXY_HOST','wwwcache.univ-lr.fr');
  define('WP_PROXY_PORT','3128'); 
```

## ZSH

Définir des alias sous linux

```bash
alias zshconfig="mate ~/.zshrc"
alias ohmyzsh="mate ~/.oh-my-zsh"
alias n="nano"
alias m="mkdir"
alias w="cd ~/Documents/UoMWorkspace/Semester2"
alias j="cd ~/Documents/UoMWorkspace/Semester2/COMP17412"
```

### Scripts

```bash
Configurer les allias
zshconfig || vi ~/.zshrc
~/ => permet de partir du dossier courant
chmod 755 nomficher.sh => rendre un script executable
Variable :
URL='http://login:password@example.com/one/more/dir/file.exe?a=sth&b=sth'
URL_NOPRO=${URL:7}
URL_REL=${URL_NOPRO#*/}
echo "/${URL_REL%%\?*}"


Demander à l'utilisateur de rentrer un paramètre : 
#!bin/bash
curl -u 'YOUR_GITHUB_USER_NAME' https://api.github.com/user/repos -d "{\"name\":\"$1\"}";
git init;
git remote add origin git@github.com:YOUR_GITHUB_USER_NAME/$1.git;

Here $1 is the repository name that is passed as an argument when invoking the script

On récupère le dossier pour le passer en paramètre à l'url
#!bin/bash
FOLDER=${PWD##*/}
curl -u 'fremin33' https://api.github.com/user/repos -d "{\"name\":\"${FOLDER}\"}";
git init;
git remote add origin git@github.com:fremin33/${FOLDER}.git;
echo "# test" >> README.md;
git init;
git add .;
git commit -m "first commit";
git push -u origin master;
```

