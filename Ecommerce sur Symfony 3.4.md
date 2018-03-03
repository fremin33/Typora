# Ecommerce sur Symfony 3.4

```bash
composer create-project symfony/framework-standard-edition ecommerceSymfony "3.4.*"
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



## Mon premier Hello World !

1. php bin/console generate bundle 
2. Bundle name : EcommerceBundle 
3. Ajouter l'autoload dans le fichier composer.json

```json
"autoload": {
  "psr-4": {
  	"": "src/"
},
```

4. On affiche notre premier Hello world

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Component\HttpFoundation\Response;

class DefaultController extends Controller
{
    /**
     * @Route("/")
     */
    public function indexAction()
    {
        // L'url "/" déclenche une Request et attend une Response
        return new Response('hello world !');
    }
}
```

5. On modifie le code pour afficher le nom entrer dans l'url

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Component\HttpFoundation\Response;

class DefaultController extends Controller
{
    /**
     * @Route("/{slug}")
     */
    public function indexAction($slug)
    {
        // On ajoute un slug qui sera affiché en l'ajoutant à l'url
        return new Response("hello $slug !" );
    
}
```

6. Enfin on utilise la méthode render pour rendre les informations à la vue

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

class DefaultController extends Controller
{
    /**
     * @Route("/{slug}")
     */
    public function indexAction($slug)
    {
        // On utilise la méthode render de twig pour envoyer le slug à la vue
        // render('chemin_de_la_page', ['nom_de_la_variable_dans_la_page' => $maVariable]
        return $this->render('ecommerce/index.html.twig', ['name' => $slug]);
    }
}

```

7. On affiche la variable dans la vue 

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
    Hello {{ name }} !
{% endblock %}
```



## Le moteur de template Twig

1. Définir une variable dans twig et l'afficher et y ajouter un filtre
   - Extends indique que l'on hérite de base.html.twig
   - Block précise que l'on va envoyer le contenu vers le block de base.html.twig

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% set lastname = 'fremin' %}
{% block body %}
    Hello {{ lastname }} !
    Hello {{ lastname | upper }} !
{% endblock %}
```

2. Faire une boucle simple dans twig

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
  {% for i in 0..10  %}
  	{{ i }} <br>
  {% endfor %}
{% endblock %}
```

3. Envoyer un tableau depuis et le controlleur à la vue et la parcourir

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

class DefaultController extends Controller
{
    /**
     * @Route("/")
     */
    public function indexAction()
    {
        // On crée un tableau que l'on va envoyer à la vue
        $amies = ['titi', 'toto', 'tata', 'titi'];
        return $this->render('ecommerce/index.html.twig', ['amies' => $amies]);
    }
}
```

4. On parcours les élements dans la vue et on les affiches

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
    {% for ami in amies  %}
        {{ ami }}
    {% endfor %}
{% endblock %}
```

## Notre première page

1. On crée une second action dans notre controlleur 

   -     @Route("/chemin_dans_l'url", name="nom_pour_creer_les_lien")
        @Route("/firstTest", name="first")

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

class DefaultController extends Controller
{
    /**
     * @Route("/")
     */
    public function indexAction()
    {
        return $this->render('ecommerce/index.html.twig', []);
    }

    /**
     * @Route("/firstPage", name="first")
     */
    public function firstPageAction()
    {
        return $this->render('ecommerce/firstPage.html.twig', []);
    }
}
```

2. On crée le fichier firstPage.html.twig

```twig
{# app/Ressources/views/ecommerce/firstPage.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
	<h1>Ceci est notre Première page</h1>
{% endblock %}
```

3. On crée le lien à partir de la page index.html.twig pour y accéder

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
    <a href="{{ path('first') }}">Première page</a>
{% endblock %}
```

4. On y ajoute les paramètres à travers les liens. (requirements permet d'ajouter une regex pour définir un type de paramètre (\d+ signifie tous les nombres numériques))

```php
<?php
#src/EcommerceBundle/Controller/DefaultController.php
namespace EcommerceBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

class DefaultController extends Controller
{
    /**
     * @Route("/")
     */
    public function indexAction()
    {
        return $this->render('ecommerce/index.html.twig', []);
    }

    /**
     * @Route("/firstPage/{id}", name="first", requirements={"id"="\d+"})
     */
    public function firstTestAction($id)
    {
        return $this->render('ecommerce/firstPage.html.twig', ['id' => $id]);
    }
}
```

5. On ajoute ce paramètre dans notre vue pour établir le lien vers la page 

```twig
{# app/Ressources/views/ecommerce/index.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
    <a href="{{ path('first', {'id': 1}) }}">Première page</a>
{% endblock %}
```

6. Enfin on affiche dans notre nouvelle l'id indiqué dans le path

```twig
{# app/Ressources/views/ecommerce/firstPage.html.twig #}
{% extends "base.html.twig" %}
{% block body %}
    <h1>Ceci est notre Première page avec l'id : {{ id }}</h1>
{% endblock %}
```

## Le template Ecommerce

Rappel : Pour supprimer un Bundle

- On supprime le Bundle
- On supprime les vues
- On supprime l'import dans le config.yml
- On supprime la route générer automatiquement dans le routing.yml
- On supprime le chargement du bundle dans le Appkernel

On définit la structure suivant : 

produit et panier controller

base.html

fichier css/images/js dans le dossier web

modules = utiliser sur plusieurs pages

modulesUsed = utiliser sur une seul page



création index + show pour les produits + folder vue

création index pour le panier + folder vue



generer pagesBundle avec une action et sa vue 



## Comprendre les entités

Entité et table au singulier

1. php bin/console doctrine:database:create

2. php bin/console doctrine:generate:entity

   nom

   description

   prix

   disponible

   categorie

   image

   tva

   1. Création du Repository (requête sql)
   2. Création de l'entité  (possibilité de modifié les elemnts) + getter et setter 

3. php bin/console doctrine:schema:update --dump -sql || --force



Créer un nouveau produit

1. On instance un produit

```php
$produit = new Produit();
```

Le service Doctrine

Le service Doctrine est celui qui va nous permettre de gérer la persistance de nos objets. Ce service est accessible depuis le contrôleur comme n'importe quel service :

```php
$this->getDoctrine();
```

l'EntityManager 

Il permet de dire à Doctrine « Persiste cet objet », c'est lui qui va exécuter les requêtes SQL.

```php
$em = $this->getDoctrine()->getManager();
```

Récupérer les repository

L'argument de la méthode`getRepository`est l'entité pour laquelle récupérer le repository. 

```
$advertRepository = $em->getRepository('OCPlatformBundle:Advert');
ou 
$advertRepository = $em->getRepository('OC\PlatformBundle\Entity\Advert');
```





On crée des fake produits 





## FosUserBundle

https://symfony.com/doc/master/bundles/FOSUserBundle/index.html

1 Créer un bundle Utilisateurs

2 Installer FosUser

3 Configurer

config.yml

```
fos_user:
    db_driver: orm
    firewall_name: main
    user_class: AppBundle\Entity\User
    service:                               
        mailer: fos_user.mailer.twig_swift
    from_email:
        address: "myadress@gmail.com"
        sender_name: "Flo-dev33"
```

Commande : 

```
bin/console fos:user:promote => Changer le role d'un user
ROLE_ADMIN

```

## Sucharger les vues FosUserBundle

copier colle le dossier vendor/friendsofsymfony/user-bundle/Ressources/view dans app/Resources/FOSUserBundle/views/layout.html.twig



1. On fais étendre le layout de foser user vers notre base.html.twig
2. php bin/console debug:router => afficher les routes dans le terminal



## Les formulaires

```php
    /**
     * @Route("/test")
     */
    public function testFormulaireAction()
    {
        $produit = new Produit();
        $form = $this->createForm(TestType::class, $produit);
        return $this->render('default/produits/test.html.twig', ['form' => $form->createView()]);
    }
```

TestType : 

```php
<?php

namespace EcommerceBundle\Form;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\FormBuilderInterface;

class TestType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        // Ici nous allons faire notre formulaire en php
        $builder
            ->add('nom')
            ->add('description', TextareaType::class, ['required' => true])
          // Affiche la liste des utilisateurs
          	->add('description', EntityType::class,
                [
                  'class' => 'UtilisateursBundle\Entity\Utilisateur'
                ])
          // Affiche la liste des pays
            ->add('description', CountryType::class)
          
                      ->add('description', ChoiceType::class,
                ['choices' =>
                    [
                        'Homme' => '0',
                        'Femme' => '1'], 'preferred_choices' =>
                    [
                        '0'
                    ]
                ]
            )
            ->add('description', EntityType::class,
                [
                    'class' => 'UtilisateursBundle\Entity\Utilisateur'
                ])
          
            ->add('Envoyer', SubmitType::class);
    }
}
```

Dans la vue : 

```
<form action="{{ path('test') }}" method="post">
    {{ form_start(form) }}
    {{ form_widget(form) }}
    {{ form_end(form) }}
</form>
```

Les type des formulaires :

https://symfony.com/doc/3.4/reference/forms/types.html



Récupérer les data

```php
    /**
     * @Route("/test", name="test")
     */
    public function testFormulaireAction(Request $request)
    {
        // On instancie un produit
        $produit = new Produit();
        // On crée le formulaire à partir de TestType et notre objet
        $form = $this->createForm(TestType::class, $produit);
        // Permet de manipuler les données après le post
        $form->handleRequest($request);
        if ($request->getMethod() == 'POST') {
            var_dump($form->getData());
        }
        return $this->render('default/produits/test.html.twig', ['form' => $form->createView()]);
    }
```



## Entité et relation

### OnetoOne

Dans l'entité Category.php

```php
    /**
     *
     * @ORM\OneToOne(targetEntity="EcommerceBundle\Entity\Media", cascade={"persist", "remove"})
     * @ORM\JoinColumn(nullable=false)
     */
    private $image;
```

Tout d'abord, j'ai choisi de définir L'entité Categorie comme entité propriétaire de la relation, car  une Categorie « possède » une image. On aura donc plus tendance à récupérer l'image à partir de l'annonce que l'inverse. Cela permet également de rendre indépendante l'entité images: 

elle pourra être utilisée par d'autres entités que Categorie, de façon totalement invisible pour elle.

Ensuite, vous voyez que seule l'entité propriétaire a été modifiée, ici Categorie. C'est parce qu'on a une relation unidirectionnelle, rappelez-vous, on peut donc faire`$categorie->getImage()`, mais pas `$image->getAdvert()`. Dans une relation unidirectionnelle, l'entité inverse, ici`Image`, ne sait en fait même pas qu'elle est liée à une autre entité, ce n'est pas son rôle.



Pour l'annotation : 

Elle possède au moins l'option`targetEntity`, qui vaut simplement le namespace complet vers l'entité liée.

Par défaut, une relation est facultative, c'est-à-dire que vous pouvez avoir une Categorie qui n'a pas d'Image liée. C'est le comportement que nous voulons pour l'exemple : on se donne le droit d'ajouter une annonce sans forcément trouver une image d'illustration. Si vous souhaitez forcer la relation, il faut ajouter l'annotation`JoinColumn`et définir son option`nullable`à`false`, comme ceci :



Parlons maintenant de l'option`cascade`que l'on a vu un peu plus haut. Cette option permet de « cascader » les opérations que l'on ferait sur l'entité `Catégory` à l'entité `Image`liée par la relation.



Pour prendre l'exemple le plus simple, imaginez que vous supprimiez une entité Catégorie  via un `$em->remove($categorie)`. Si vous ne précisez rien, Doctrine va supprimer l'`Advert` mais garder l'entité`Image`liée. Or ce n'est pas forcément ce que vous voulez ! Si vos images ne sont liées qu'à des annonces, alors la suppression de l'annonce doit entraîner la suppression de l'image, sinon vous aurez des`Images`orphelines dans votre base de données. C'est le but de`cascade`. Attention, si vos images sont liées à des annonces mais aussi à d'autres entités, alors vous ne voulez pas forcément supprimer directement l'image d'une annonce, car elle pourrait être liée à une autre entité.

On peut cascader des opérations de suppression, mais également de persistance. En effet, on a vu qu'il fallait persister une entité avant d'exécuter le `flush()`, afin de dire à Doctrine qu'il doit enregistrer l'entité en base de données. Cependant, dans le cas d'entités liées, si on fait un`$em->persist($advert)`, qu'est-ce que Doctrine doit faire pour l'entité`Image`contenue dans l'entité`Advert` ? Il ne le sait pas et c'est pourquoi il faut le lui dire : soit en faisant manuellement un`persist()`sur l'annonce *et* l'image, soit en définissant dans l'annotation de la relation qu'un`persist()`sur`Advert` doit se « propager » sur l'`Image`liée.

C'est ce que nous avons fait dans l'annotation : on a défini le`cascade`sur l'opération`persist()`, mais pas sur l'opération`remove()`(car on se réserve la possibilité d'utiliser les images pour autre chose que des annonces).



Relation Bidirectionnelle

```
   /**
     *
     * @ORM\ManyToOne(targetEntity="UtilisateursBundle\Entity\Utilisateur", inversedBy="commandes")
     * @ORM\JoinColumn(nullable=true)
     */
    private $utilisateur;
```

```
    /**
     *
     * @ORM\OneToMany(targetEntity="EcommerceBundle\Entity\Commande", mappedBy="utilisateur", cascade={"persist", "remove"})
     * @ORM\JoinColumn(nullable=true)
     */
    private $commandes;
```



## fixtureBundle

composer require --dev doctrine/doctrine-fixtures-bundle ^2.0

cf file: 

La version 3.0 bug

```
// Permet d'appeler dans une autre fixture
$this->addReference('categorie1', $categorie);
$this->getReference('categorie1');
```


## Première page dynamique:

Rendre une méthode de controller : 

```
   # PagesBundle/Controller/pagesController
   
   public function menuAction()
    {
        $em = $this->getDoctrine()->getManager();
        $posts = $em->getRepository(Page::class)->findAll();
        return $this->render('default/pages/moduleUsed/menu.html.twig', [
            'posts' => $posts
        ]);
    }
```

```
# App/Ressources/view/base.html.twig 
{{ render(controller('PagesBundle:Pages:menu')) }}
```

## Form  sans entité

https://symfony.com/doc/3.4/forms.html



episode13 a finir