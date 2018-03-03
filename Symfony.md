# Symfony

## Comment un nouveau projet :

```bash
composer create-project symfony/framework-standard-edition initiation_symfony "3.4.*"

Sur lpcm pour les droits
setfacl -R -d -m u:www-data:rwX -m u:$(whoami):rwX practiceSymfony/
setfacl -R -m u:www-data:rwX -m u:$(whoami):rwX practiceSymfony/

```

## Configurer la BDD

```
MyApp/app/config/parameters.yml
```

## Commande Symfony 

```
php bin/console									=> Afficher la liste des commandes disponibles
php bin/console server:run 127.0.0.1:8000				=> Lancer un serveur sur le port 8000
php bin/console generate:bundle							=> Création d’un bundle
php bin/console doctrine:generate:entity 
php bin/console doctrine:generate:entities MyApp 		=> Génération des getter et setter 
php bin/console doctrine:database:create				=> Création de la base de donnée
php bin/console doctrine:schema:create					=> Création des tables
php bin/console doctrine:schema:update --force			=> Mettre à jour les tables 
php bin/console assets:install web						=> Mettre à jour les CSS, JS et images
php bin/console cache:clear					   			=> Vider le cache
php bin/console fos:user:create username email password => Création d’un utilisateur pour FOSUserBundle

php bin/console fos:user:promote username ROLE_ADMIN 	=> Rendre un utilisateur admin
php bin/console cache:warmup --env=prod --no-debug 		=> Vérifier les entités
php bin/console generate:doctrine:crud					=> Générer un crud
```

## Configurer les routes :

```
MyApp/app/config/routing.yml
```



## Twig

```twig
App>Resources>views>base.html.ywig 				=> doctype + header + footer
{{ object.variable }}			   				=> afficher un élement
{% %}							   				=> Commande, boucle, condition
{% extends 'fichier' %}			   				=> Héritage
 <li><a href="{{ path('homepage') }}"> Accueil</a></li>
 <link rel="stylesheet" href="{{ asset('css/style.css') }}">
 <img src="{{ asset('images/team3.png') }}" alt=""> => va chercher dans le dossier web
```

Passer en mode developpement : 

```
http://lpcm2016.univ-lr.fr/ffremin/tp1Symfony/web/app_dev.php
```

src = configuration + controller

app = view

web = css/js/image

asset = lien avoir le dossier web



### commande de base

find($id)

findAll()

findBy(...) => Récupère les éléments en fonction de critère

findOneBy() => récupère le premier élément

findByX(...) => On remplace X par l'élement de notre entité



### MVC

Entity = Modèle

Une méthode de controller est associé à une page elle même associé à une action du controller

Cette action va récupérer des objets que le controlleur va les envoyé à la vue 

getDoctrine()->getManager() pour récupérer les entités;



### formulaire

- Créer le fomilaire d'une entité

```bash
bin/console doctrine:generate:form AppBundle:VotreEntité
```

## relation entre modèle

```
bin/console doctrine:schema:update
```

cf commande ORM



1. Générer l'entité
2. schema:update --dump-sql (voir les modifs)
3. schema:update --force (appliquer les modifs) en databse
4. config.yml doctrine>dbal>mapping-types>bit:boolean

### Requete avancé

on crée des fonctions dans les repository

### querybuilder

#### Exemple



