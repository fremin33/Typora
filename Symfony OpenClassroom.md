# Symfony OpenClassroom

## Démarrer un projet

```bash
composer create-project symfony/framework-standard-edition practiceSymfony "3.3.*"
```

## Symfony c'est quoi ?

### Structure des dossiers

1. app/ => Configuration de l'application et des views
2. bin/ => Ensemble des commandes exécutable (php bin/console)
3. src/ => Orgasitation du code en Bundle  (Modele, form, controller ...)
4. test/ => Contient les tests de notre applications
5. var/ => Contient les logs, cache (ne jamais écrire dedans)
6. vendor/ => Contient les bibliothèques externes de notre application (doctrine, twig, ...)
7. web/ => Destiné au visiteur (image, css, js, video, ..)
8. app.php || app_dev.php =>  Controlleur frontal (point d'entrée de l'application comme index.php)
9. var/logs/dev. log => Affiche les logs (évènements, erreurs)

### Architecture MVC

1. Controlleur => Génère la réponse à la requête HTTP demandée par notre visiteur. Il est la couche qui se charge d'analyser et de traiter la requête de l'utilisateur. Le contrôleur contient la logique de notre site Internet et va se contenter « d'utiliser » les autres composants : les modèles et les vues.
2. Modèle => Gère les données et le contenu. C'est le modèle qui sait comment récupérer des éléments, généralement via une requête au serveur SQL. Au final, il permet au contrôleur de manipuler les informations.
3. Vue => Sert à afficher les pages de notre application à travers le controlleur.

### Parcours d'une requête Symfony

1. Le visiteur demande une page 
2. Le controlleur frontal reçoit la requête 
3. Le kernel demande au routeur qu'elle controlleur exécuter pour cette requête et exécute ce même controlleur
4. Le controlleur demande au modèle les informations dont il a besoin et reçoi ainsi la vue avec les informations nécessaire et renvoi ainsi page page complète au visiteur 

### Bundle

Un bundle est une brique de l'application et regroupe tous ce qui concerne une même fonctionnalité.

- Bundle utilisateur (page classique, page d'administration, formulaire de connection, inscription ...)

  #### Structure

```
/Controller => Vos contrôleurs
/Dependencylnjection => Informations sur votre bundle
/Entity => Vos modèles
/Form => Vos éventuels formulaires
/Resources
--/config => Fichiers de configuration de votre bundle : routes
--/public => Fichiers publics de votre bundle : fichiers CSS et JavaScript, images, etc.
--/views => Vues de votre bundle, templates Twig
```

### Résumé 

- Symfony est organisé en six répertoires : app, bin, src, var, vendor et web.
- Le répertoire dans lequel on passera le plus de temps est src, qui contient le code source de notre site.
- Il existe deux environnements de travail : 
  - L'environnement prod est destiné à vos visiteurs : il est rapide à exécuter et ne divulgue pas les messages d'erreur.
  - L'environnement dev est destiné au développeur, c'est-à-dire vous : il est plus lent, mais offre plein d'informations utiles au développement. 

- Symfony utilise l'architecture MVC pour bien organiser les différentes parties du code source.
- Un bundle permet de bien organiser les différentes parties de votre site.

## Création d'un Bundle

### Commande 

```
php bin/console generate:bundle 
1- Partager le bundle ?
2- Nom du namespace (ff/PlatformBundle)
3- Nom du bundle (ffPlatformBundle)
4- Destination (src/)
5- Forme de configuration (yml)

```

### composer.json

```json
"autoload": {
  "psr-4": {
  	"": "src/"
},
```

### Résumé 

- Le code source situé dans src/Application/Bundle et dont le seul fichier obligatoire est la classe à la racine OCPlatformBundle .php ;
- Enregistrer le bundle dans le noyau pour qu'il soit chargé, en modifiant le fichier app/AppKernel.php
- Enregistrer les routes (si le bundle en contient) dans le routeur pour qu'elles soient chargées, en modifiant le fichier app/config/routing. yml.



## Mon premier Hello Wordl

### Création de la route 

```yaml
src/ff/PlatformBundle/Resources/config/routing.yml
hello_the_world:
    path:     /hello_world
    defaults: { _controller: ffPlatformBundle:Advert:index } 
```

- path => L'url à laquelle on souhaite que notre page soit accessible
- defaults => Paramètre de la route avec l'action (index) et le controlleur (advert)

### Création de la vue

On crée le fichier src/ff/PlatformBundle/Resources/views/Advert/index.html.twig

```
<hl>Hello {{ nom }} !</hl>
```



### Création du controlleur

On crée le fichier src/OC/PlatformBundle/Controller/AdvertController.php

```php
<?php
namespace ff\PlatformBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;

class AdvertController extends Controller
{

    public function indexAction()
    {
        $content = $this
          ->get('templating')
          ->render('ffPlatformBundle:Advert:index.html.twig', ['nom' => 'winzou']);
        return new Response($content);
    }
}
```

- L'objet ' templating ' permet de récupérer le contenu du template grâce à la méthode render src/ff/PlatformBundle/Resources/views/Advert/index.html.twig

### Vider le cache

```
php bin/console cache : clear --env=prod => Vide le cache en production (var/cache/prod)
php bin/console cache:clear => Vide le cache en developpement (car/cache/dev)
```

### Résumé

- Le rôle du routeur est de déterminer quelle route utiliser pour la requête courante.
- Le rôle d'une route est d'associer une URL à une action du contrôleur.
- Le rôle du contrôleur est de retourner au noyau un objet Response qui contient la réponse HTTP à envoyer à l'internaute (page HTML ou redirection).
- Le rôle des vues est de mettre en forme les données que le contrôleur lui donne, afin de construire une page HTML, un flux RSS, un e-mail, etc.

## Le routeur de Symfony (YAML)

### Starter

Dans le fichier  src/ff/PlatformBundle/Resources/config/routing.yml

```yaml
ff_platform_home:
    path:     /platform								=> Url à capturer
    defaults:
        _controller: ffPlatformBundle:Advert:index  => Méthode du controlleur et paramètre

oc_platform_view:
    path:     /platform/advert/{id}
    defaults:
        _controller: OCPlatformBundle:Advert:view
        
ff_platform_view_slug:
    path:     /platform/{year}/{slug}.{format}
    defaults:
        _controller: ffPlatformBundle:Advert:viewSlug
        format:    html									=> Paramètre facultatif
    requirements:
        year:     \d{4}									=> 4 chiffres uniquements
        format:    html|xml								=> html ou xml

oc_platform_add:
    path:     /platform/add
    defaults:
        controller: OCPlatformBundle:Advert:add
```

### Fonctionnement 

1. On appelle l'URL /platform/advert/5.
2. Le routeur essaie de faire correspondre cette URL avec le path de la première route, ici /platform. Cela ne correspond pas.
3. Le routeur passe donc à la route suivante. Il essaie de faire correspondre /platform/advert/5 avec /platform/advert/{id}.

4. Le routeur s'arrête donc de chercher, car il a trouvé une route qui correspond. Il demande à la route « Quels sont tes paramètres de sortie ? » et reçoit comme réponse « le contrôleur est ffPlatformBundle:Advert:view et la valeur
  $id vaut 5. »
5. Le routeur renvoie donc ces informations au kernel (le noyau de Symfony).
6. Le noyau exécute le bon contrôleur avec les bons paramètres.

### Méthode avec  paramètre

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
<?php
namespace ff\PlatformBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;

class AdvertController extends Controller
{
    public function viewAction($id) 
    {
        return new Response($id);
    }
  
    public function viewSlugAction ($year, $slug, $format) {
        return new Response("On passe un slug $slug : , une année : $year et un format: $format");
    }
}
```

### Paramètre 

1. Le paramètre {_locale} définit la langue dans laquelle on souhaite afficher la page
2. Le paramètre {_format} définit le Content-type de la page dans l'entête

### Ajout d'un prefixe

Dans le fichier app/config/routing.yml

```yaml
ff_platform:
    resource: "@ffPlatformBundle/Resources/config/routing.yml"
    prefix:   /platform

app:
    resource: '@AppBundle/Controller/'
    type: annotation
```

Dans le fichier  src/ff/PlatformBundle/Resources/config/routing.yml

On supprime 

```yaml
ff_platform_home:
    path:     /
    defaults:
        _controller: ffPlatformBundle:Advert:index

ff_platform_view:
    path:     /advert/{id}
    defaults:
        _controller: ffPlatformBundle:Advert:view

ff_platform_view_slug:
    path:     /{year}/{slug}.{format}
    defaults:
        _controller: ffPlatformBundle:Advert:viewSlug
        format:    html
    requirements:
        year:     \d{4}
        format:    html|xml

ff_platform_add:
    path:     /add
    defaults:
        controller: ffPlatformBundle:Advert:add
```

### Résumé

- Une route est composée au minimum de deux éléments : l'URL à faire correspondre (path) et le contrôleur à exécuter (paramètre _controller).
- Le routeur essaie de faire correspondre chaque route à l'URL appelée par l'internaute, en respectant l'ordre d'écriture dans le fichier : la première route qui correspond est sélectionnée.
- Une route peut contenir des paramètres, facultatifs ou non, représentés par les accolades {paramètre} et dont la valeur peut être soumise à des contraintes via la section requirements.
- Le routeur est également capable de générer des URL à partir du nom d'une route et de ses paramètres éventuels.

## Le controlleur

Le rôle du contrôleur est de retourner une réponse à partir d'une request

### Request

On souhaite récupérer le paramètre tag dans l'url http://localhost:8000/app_dev.php/platform/advert/5?tag=hello

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
<?php
namespace ff\PlatformBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\Request;

class AdvertController extends Controller
{
    public function viewAction($id,Request $request) {
        /* On récupère le ?tag dans une variable */
        $tag = $request->query->get('tag');
        return new Response("id: $id et le tag: $tag");
    }
}
```

| Type de paramètre   | Méthode Symfony      | Méthode php |
| ------------------- | -------------------- | ----------- |
| Variable d'url      | $request->query      | $_GET       |
| Variable formulaire | $request->request    | $_POST      |
| Variable cookie     | $request->cookie     | $_COOKIE    |
| Variable de serveur | $request->server     | $_SERVER    |
| Variable d'entête   | $request->header     | $_SER       |
| Paramètre de route  | $request->attributes | n/a         |

#### Méthode de la request

##### Requête http

```
$request->isMethod('POST') || $request->isMethod('GET')
```

##### Requête ajax

```
$request->isXmlHttpRequest()
```

### Response

Méthode simplifier pour render une vue avec des paramètre

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
public function viewAction($id, Request $request)
{
  $tag = $request->query->get('tag');
  return $this->render('ffPlatformBundle:Advert:view.html.twig', [
  	'id' => $id,
  	'tag' => $tag
  ]);
}
***************************************************************************************************
public function viewAction($id, Request $request)
{
  $tag = $request->query->get('tag');
  return $this>get{'templating')->renderResponse('ffPlatformBundle:Advert:view.html.twig', [
    'id' => $id,
    'tag' => $tag
  ]);
}
```

### Redirect Response

Redirige vers la route ff_platform_home

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
public function viewAction($id, Request $request)
{
  $url = $this->get('router')->generate('ff_platform_home');
  return new redirect($url);
}
***************************************************************************************************
  public function viewAction($id, Request $request)
{
    return redirectToRoute('ff_platform_home');
}
```

### Retourner du json

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
public function viewAction($id, Request $request)
{
    $response = new Response(json_encode(['id'=>$id]));
    $response->headers->set('Content-type', 'application/json');
    return $response;
}
***************************************************************************************************
public function viewAction($id, Request $request)
{
  	return new JsonResponse(array('id' => $id));
}
```

### Variable de session

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

On récupère la variable user_id et on change sa valeur avec le getteur et setteur

```php
public function viewAction($id, Request $request)
{
    $session = $request->getSession();
    $user_id = $session->get('user_id');
    $session->set('user_id', 91);
    return new Response('Ceci est un test');
}
```

### Message flash (addAction)

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
public function addAction(Request $request) {
    // On récupère la variable
    $session = $request->getSession();
    // On stock un message
    $session->getFlashBag()->add('info', 'Annonce bien enregistré');
    // Puis un autre
    $session->getFlashBag()->add('info', 'Oui oui il est bien enregistré');
    // On envoi les données à la vue avec une redirection
    return $this->redirectToRoute('ff_platform_view', ['id' => 5]);    
}
```

Dans le fichier  src/ff/PlatformBundle/Resources/view/Advert/view.html.twig

On affiche les messages enregistré précédemment

```twig
<body>
    <h1>L'id est : {{ id }}</h1>
    {% for message in app.session.flashbag.get('info') %}
    	<p>Message flash : {{ message }} </p>
    {% endfor %}
</body>
```

### Erreur 404

Dans le fichier  src/ff/PlatformBundle/Resources/view/Advert/view.html.twig

```php
public function indexAction($page)
{
    if ($page < 1) {
        // Génère une page d'erreur si le paramètre est infèrieur à 1
        throw new NotFoundHttpException("Page $page inexistante");
    }
    return $this->render('ffPlatformBundle:Advert:index.html.twig', []);
}
```



### Résumé

- Le rôle du contrôleur est de retourner un objet Response : ceci est obligatoire !
- Le contrôleur construit la réponse en fonction des données qu'il reçoit en entrée : paramètres de route et objet Request.
- Le contrôleur se sert de tout ce dont il a besoin pour construire la réponse ; la base de données, les vues, les différents services, etc.

## Le moteur de template twig

```twig
{{...}} affiche quelque chose
{%...%} fait quelque chose
{#...#} affiche un commentaire
{{ tab[n]}} affiche la valeur à l'index n de tab
{{ obj.id }} affiche l'id de l'objet obj
{{ variable_html | raw }} désactive la protection de twig
{{ app.request }} Objet request
{{ app.session }} Service de session
{{ app.environment }} Environnement courant
{{ app.debug }} true si le mode debug est activé sinon false
{{ app.user }} L'utilisateur courant
{% set f00='bar' %} affectation de variable

```

### Boucle for

```twig
{{ loop.index }} Le numéro de l'itération courante (en commençant par 1).
{{ loop.indexO }} Le numéro de l'itération courante (en commençant par 0).
{{ loop.revindex }} Le nombre d'itérations restant avant la fin de la boucle (en finissant par 1).
{{ loop.revindexO }} Le nombre d'itérations restant avant la fin de la boucle (en finissant par 0).
{{ loop .first }} True si c'est la première itération, False sinon.
{{ loop.last }} True si c'est la dernière itération, False sinon.
{{ loop.length }} Nombre d'idération dans la boucle
```

### Condition

```twig
{% if var is defined %}{% endif %} => Vérifie si une variable existe
{% if loop.index is even %}{% endif %} => Vérifie si l'index est pair
```

### Variable global

Dans le fichier app/config/config.yml

```yaml
twig:
 #...
 globals:
 nomdelavariable: ma-variable
```



### Récupérer le contenu d'un template en texte 

```php
$contenu = $this->renderView('ffPlatformBundle:Advert:email.txt.twig');
// On récupère du contenu et on l'envoi par mail
mail('florian.fremin33@gmail.com', 'Inscription OK', $contenu);
```

### Template structure

Dans le fichier src/ff/PlatformBundle/Resources/views/layout.html.twig

```twig
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title> {% block title %}Practice Symfony{% endblock title %}</title>
</head>
<body>
    {% block content %} {% endblock content %}
</body>
</html>
```

Dans le fichier src/ff/PlatformBundle/Resources/views/Advert/index.html.twig

```twig
{% extends "ffPlatformBundle::layout.html.twig" %}
{% block title %}{{ Voici la page index }}{% endblock title %}
{% block content %}
    <h1>Hello</h1>
{% endblock content %}
```

### Inclusion

On souhaite inclure un form utiliser dans le fichier add.html.twig et edit.html.twig

```twig
{% extends "ffPlatformBundle::layout.html.twig" %}
{% block body %}
	<h2>Ajouter une annonce</h2>
{{ include("OCPlatformBundle:Advert:form.html.twig") }}
{% endblock %}

```

### Inclusion des conteneur

Permet d'afficher des élements de la BDD par exemple de le layout.html.twig

```twig
<div id="menu">
{{ render (controller("OCPlatformBundle:Advert:menu")) }}
</div>

```

Dans le fichier src/ff/PlatformBundle/Controller/AdvertController.php

```php
public function menuAction()
{
// On fixe en dur une liste ici. Bien entendu, par la suite on la récupérera depuis la BDD !
$listAdverts=array(
array('id'=>2, 'title'=>'Recherche développeur Symfony'),
array('id'=>5, 'title'=>'Mission de webmaster'),
array('id'=>9, 'title'=>'Offre de stage webdesigner'));
return $this->render('OCPlatformBundle:Advert:menu.html.twig', array(
// Tout l'intérêt est ici : le contrôleur passe
// les variables nécessaires au template !
'listAdverts'=>$listAdverts)
```

Dans le fichier src/ff/PlatformBundle/Resources/views/Advert/menu.html.twig

```twig
<ul class="nav nav-pills nav-stacked">
{% for advert in listAdverts %}
  <li>
      <a href="{{ path('ff_platform_view',{'id':advert.id}) }}"> {{ advert.title }} </a>
  </li>
{% endfor %}
</ul>
```

### Résumé

- Un moteur de templates tel que Twig permet de bien séparer le code PHP du code HTML, dans le cadre de l'architecture MVC.
- La syntaxe { { var } } affiche la variable var.
- La syntaxe {% if... %} exécute quelque chose, ici une condition.
- Twig offre un système d'héritage, via {% extends %}, et d'inclusion, via { { include ( ) } } et { { render ( ) } }, très intéressant pour bien organiser les templates.
- Le modèle triple héritage est très utilisé pour des projets avec Symfony.

## Installer un Bundle avec Composer 

https://packagist.org/ => Source des bundles 

| Valeur                   | Exemple      | Description                              |
| ------------------------ | ------------ | ---------------------------------------- |
| Version exact            | "2.0.17"     | Version 2.0.17 téléchargé                |
| Version entre plage      | ">=2.0,<2.6" | Version la plus à jours entre 2.0 et 2.6 |
| Version plage sémantique | "~2 .1"      | Version la plus à jours en s'arretant avant la 3.0 |
| Version numero joker     | "2.0.*"      | Version la plus à jours de 2.0.0 à 2.0.X |

## Services 

Le service est une fonction de l'application (mailer par exemple cgargé d'envoyé des mails)

```php
  $this->container->get('mailer');		=> Permet de récupérer un service
  ****************************************************************************************
  $this->get('mailer')					=> Equivalence 

```

Pour crée un service, on crée une classe

```php
<?php
namespace ff\PlatformBundle\Antispam;

/**
 * Class Antispam
 *
 * @package ff\PlatformBundle\Antispam
 */
class ffAntispam {
    /**
     * Vérifie si le texte est un spam ou non
     * @param string $text
     * @return bool
     */
    private $mailer;
    private $locale;
    private $minLength;
    public function __construct(\Swift_Mailer $mailer, $locale, $minLength)
    {
    $this->mailer = $mailer;
    $this->locale = $locale;
    $this->minLength = $minLength;
    }


    public function isSpam($text) {
        return strlen($text)<$this->minLength;
    }
}

```

On le configurer dans le fichier src/ff/PlatformBundle/Ressources/config/services.yml

```yaml
services:
    ff_platform.antispam:								=> Nom du service
        class: ff\PlatformBundle\Antispam\ffAntispam  => Namespace du service
        arguments :
            - "@mailer"
            - %locale%
            - 50
```

Dans le fichier 

```php
    public function addAction(Request $request)
    {
      	// On récupère le service
        $antispam = $this->container->get('ff_platform.antispam');
        $text = "...";

        if ($antispam->isSpam($text))
        {
            throw new \Exception('Votre message a été envoyé comme étant un spam');
        }
        return $this->render('ffPlatformBundle:Advert:add.html.twig', []);
    }

```



## Entity

### Crée une entity

Configurer le fichier parameter.yml pour la connection à la BDD

```bash
php bin/console doctrine:database:create => Créer la BDD
php bin/console doctrine:generate:entity => Crée une entity
php bin/console doctrine:schema:update --dump-sql => Vérifier les changements
php bin/console doctrine:schema:update --force => Appliquer les changements
php bin/console doctrine:generate:entities ffPlatformBundle:Advert => crée les get et set
```

### Information

```php
// Indique que la classe est une entity
 @ORM\Entity		
// Précise le namespace du repository qui gère l'entity
@0RM\Entity(repositoryClass="ff\PlatformBundle\Entity\AdvertRepository") 
// Nom de la table qui sera attribué à l'entity dans la BDD   
@0RM\Table(name="ff_advert")
// Caractéristique des élements de la table
     /**
     * @var int
     *
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;
```

### Ajouter une column

```php
    /**
     * @var bool
     *
     * @ORM\Column(name="published", type="boolean")
     */
private $published=true;
```

### Persister des objets en BDD

```php
$doctrine = $this->getDoctrine();
```

### Récupérer un ORM

```php
$doctrine->getManager($name)
// On récupère le service entity_manager
$em = $this->getDoctrine()->getManager();
// On recup le repository
$advertRepository = $em->getRepository('ffPlatformBundle:Advert');
// Persister en BDD
$em->persist($entity);  => donne la responsabilité à doctrine
$em->detach($entity); => enleve la responsabilité à doctrine
$em->clear(); 	=> enleve tous les responsabilités à doctrine 
$em->flush();		=> execute l action
$em->contains($entity); => return true si l entity est prise en charge par le gestionnaire
$em->refresh($entity); => annule un changement précédemment effectué
$em->remove($entity); => supprime l'entity de la BDD'
  
// Récupérer une entite par son id 
$repository = $this->getDoctrine()
->getManager()
->getRepository('ffPlatformBundle:Advert');
$advert = $repository->find($id)
******************************************************************************************
$advert = $this->getDoctrine( )
->getManager()
->find('ffPlatformBundle:Advert', $id)


```



### Résumé

- Le rôle d'un ORM est d'organiser la persistance de vos données : vous manipulez des objets et lui s'occupe de les enregistrer dans une base de données

- L'ORM par défaut livré avec Symfony est Doctrine2.

- L'utilisation d'un ORM implique un changement de raisonnement : on utilise des objets et on raisonne en POO. C'est au développeur de s'adapter à Doctrine2 et non l'inverse !

- Une entité est, du point de vue PHP, un simple objet. Du point de vue de Doctrine, c'est un objet complété avec des informations de mapping qui lui permettent d'enregistrer correctement l'objet dans une base.

- Une entité est dans votre code un objet PHP qui correspond à un besoin et indépendant du reste de votre application.

- Avec Symfony, on récupère le gestionnaire d'entités de Doctrine2 depuis un contrôleur, via

  $this->getDoctrine()->getManager().

- L'EntityManager sert à manipuler les entités, tandis que les repositories servent à les récupérer.

## Relation entre les entités

### Entité propriétaire

Entité qui va contenir la réfèrence à l'entité inverse

### OnetoOne 1..1

Relation unique entre deux objets (ex: une image appartient à une annonce et une annonce à une seule image)

```php
 Dans advert car on aura tendance à récupérer une image à partir d un advert  
    /**
     * @ORM\OneToOne(targetEntity="ff\PlatformBundle\Entity\Image", cascade={"persist"})
     * @ORM\JoinColumn(nullable=false)
     */
    private $image;

******************************************************************************************$advert->getlmage(); => On peut maintenant utiliser cette méthode
```

- Elle possède au moins l'option targetEntity, qui vaut simplement l'espace de noms complet vers l'entité liée.
- Elle possède d'autres options facultatives, dont l'option cascade qui supprime l'image quand l'advert est delete
- nullable=false empêche que l'image ne soit pas présente



### ManytoOne 1..n

Relation qui lie l'entité A à plusieurs entité B (ex Une annonce peut contenir plusieurs candidatures, alors qu'une candidature n'appartient qu'à une seule annonce.)

Application est le propriétaire et les advert est l'entité inverse

Le many correspond au candidature (application en Anglais) une annonce à plusieurs candidature et Le propriétaire est celui qui contient la column de reférence (le many donc application)

nullable = false permet d'éviter de laisser des candidatures si il ni a pas d'annonce

```php
    /**
     * @ORM\ManyToMany(targetEntity="ff\PlatformBundle\Entity\Advert")
     * @ORM\JoinColumn(nullable=false)
     */
    private $advert;
```



### Many to Many n..n

Plusieurs objets en relation avec plusieurs autres ex( une categories peut avoir plusieurs annonces et une annonces peut avoir plusieurs category) J'ai mis Advert comme propriétaire de la relation. 

```php
   /**
    * @ORM\ManyToMany(targetEntity="ff\PlatformBundle\Entity\Category", cascade={"persist"})
     */
    private $categories;

    public function __construct() {
        $this->date = new \DateTime();
        $this->categories = new ArrayCollection();
    }
```

On utilise ici une ArrayCollection donc il faut l'initialiser dans le constructeur

### relation bidirectionnelle

le propriétaire  à le Many to One (toujours du many une annonce à plusieurs candidature) donc application à le many to one et le advert à le one to many

```php
    /**
     * @ORM\OneToMany(targetEntity="ff\PlatformBundle\Entity\Application", mappedBy="advert")
     */
    private $applications;
```

- Le targetEntity est évident : il s'agit toujours de l'entité à l'autre bout de la relation, ici Application. 
- Le mappedBy correspond, lui, à l'attribut de l'entité proprié- taire (Application) qui pointe vers l'entité inverse (Advert) : c'est le private $advert.

On ajoute le paramètre inverBy dans la classe Applications pour dire vers ou pointe l'entité Application

```php

    /**
     * @ORM\ManyToOne(targetEntity="ff\PlatformBundle\Entity\Advert", inversedBy="applications")
     * @ORM\JoinColumn(nullable=false)
     */
    private $advert;
```



## Récupérer des entité

### DQL

Le dql représente du SQL adapté aux objets

```php
public function myFindAllDql() {
    $query = $this->_em->createQuery('SELECT a FROM ffPlatformBundle:Advert a');
    $results = $query->getResult();
    return $results;
}
// a => * 
// On indique le chemin de l'entité (on pense objet) et on ajoute son allias

SELECT a.title FROM Advert a WHERE a.id IN (1,3,5)
  
$query = $this->_em->createQuery('SELECT a FROM Advert a WHERE a.id=:id');
$query->setParameter('id', $id);
// Utilisation de getSingleResult car la requête ne doit retourner qu'un
// seul résultat
return $query->getSingleResult();

```

On peut aussi tester les requête sql dans le terminal 

```bash
php bin/console doctrine:query:dql "SELECT a FROM ffPlatformBundle:Advert a".
```

### Jointure

### QueryBuilder

Construction d'une requête par étape

Le gestionnaire d'entités est accessible depuis un repo en utilisant l'attribut _em  soit $this->_em->createQueryBuilder()

```php
    public function myFindAll() {

        // On récupère l'entité grâce à son namespace
        $queryBuilder = $this->_em->createQueryBuilder('a');
        // On récupère la query à partir du queryBuilder
        $query = $queryBuilder->getQuery();
        // On récupère les résultats
        $results = $query->getResult();
        // On return les résultats
        return $results;
    }
******************************************************************************************    public function myFindAll() {
       return $this->createQueryBuilder('a')
			->getQuery();
			->getResult();
    }
******************************************************************************************
public function myFindOne($id)
{
    $qb = $this->_em->createQueryBuilder('a');
    $qb
        ->where('a.id=:id')
        ->setParameter('id', $id);

    return $qb
        ->getQuery()
        ->getResult();
}
******************************************************************************************public function findByAuthorAndDate($author, $year)
{
    $qb = $this->createQueryBuilder('a');
    $qb
        ->where('a.author=:author')
        ->setParameter('author', $author)
        ->andWhere('a.date<:date')
        ->setParameter('year', $year)
        ->orderBy('a.date', 'DESC');

    return $qb
        ->getQuery()
        ->getResult();
}
```

### Query

```php
// Return un tableau des résultats
getResult()
// A utiliser si il ni a pas de traitements (même chose que getResult mais plus rapide)
getArrayResult()
// Fonctionnement de twig
 { { advert. content } } exécute : $advert->getContent () 
// Return un tableau de valeur (count)
getScalarResult()
// Return un seul est unique resultat ou null si il ni en a pas
getOneOrNulIResult()
// Return un seul est unique resultat ou une exception si il ni en a pas
getSingleResult()
// Return une seul valeur ou une exeption si il ni a pas de resultat ou plusieurs  
getSingleScalarResult()
// Exécute une requete (Insert/Update/Delete)  
execute()
```



### EntityRepository

Les méthodes classique

#### Méthode de base

```php
$repository->find(5); 	=> On récupère l élement ayant l id 5
$repository->findAll()  => On récupère toutes les entités
$repository->findBy(	=> On récupère les élements à partir de filtre
	['author' => 'Florian'],    => Critere (récupère les élements dont l auteur est flo)
	['date' => 'desc'],			 => Trié par ordre decroissant
	5,							  => Limite à 5 elements
	0							  => Offset (a partir d ou il cherche donc du début)
);
$repository->findOneBy (...) => Comme findBy mais ne return qu un élement
$repository->findByX(...) => On modifie le X par une propriété de notre entité
ex : $repository->findByAuthor("Florian"); => return un tableau des annonces de flo
$repository->FindOneByX() => Comme findByX mais ne return qu un élement
```


