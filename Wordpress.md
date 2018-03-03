# Wordpress

5 décembre : Examen php

## Modifier un thème Wordpress

### Structure

```wordpress
Dans le dossier WordpressTp1/wp-content/themes
1 - Créer un dossier « basic-child »

2 - Créer un fichier style.css
/*
 * Theme Name: Miam'Blog
 * Template: basic
*/
@import url("../basic/style.css");

3 - Ajouter une image screenshot.png qui servira à illustrer le thème

4 - Changer le thème par le thème enfant que vous venez de créer dans Wordpress.
```





## Créer un thème Wordpress

cf CMDER pour initialiser un projet

### Structure minimal

```
Dans le dossier project/wp-content/themes
1 - Créer un dossier « firstTheme »
2 - Créer un fichier style.css, functions.php et index.php dans le dossier fisrtTheme
/*
Theme Name: Nom Du thème
Author: Fremin Florian
Author URI: florian.fremin.herokuapp.com
Description: Mon premier thème Wordpress
Version: 1.0
*/

cf : index.php est le fichier de base du site


3 - Ajouter une image screenshot.png qui servira à illustrer le thème

4 - Activer le thème dans Wordpress
```





### Structure commune

```
4 - Créer un fichier header.php et footer.php dans le dossier firstTheme
5 - Copiez les dossiers css, fonts, images et js dans votre thème
```

#### header.php

Dans le header on trouvera tout le code jusqu’à  </header>

```php+html
<!DOCTYPE html>
// Délivre automatique le bon code de langage
<html <?php language_attribute(); ?>
<head>
    <meta charset="<?php bloginfo('charset') ?>">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
 // Affiche les liens des scripts dans le header (CSS)
<?php wp_head();?> 
 </head>
 <body>
   <header>
   ...
   </header>
```

#### footer.php

Dans le footer on trouvera tout le code qui commence à partir de <footer>

```php+html
    <footer>
    ...
    </footer>
// Affiche les liens des scripts dans le footer (JS)     
<?php wp_footer();?>
</body>
</html>
```

#### functions.php

Fichier qui contient les fonctions Wordpress et nos propres fonctions et les actions utiliser par WP

```php
<?php
// Charge le fichier main.css dans le dossier css/main.css pour le head (cf: false).
function chargeStyle() {
    wp_enqueue_style( 'main', get_template_directory_uri().'/css/main.css', false );
}

// Charge le fichier main.js dabs le dossier js/main.js pour le footer en précisant qu'il faut chargé jquery (dépendance) (cf: true va dans dans le body).
function chargeScript() {
    wp_enqueue_script( 'bootstrap', get_template_directory_uri().'/js/bootstrap.min.js', array('jquery'), 1, true );
}

// On lance nos deux fonctions
add_action('wp_enqueue_scripts','chargeStyle');
add_action('wp_enqueue_scripts','chargeScript');

// Inclure du css dans l'espace admin
add_action('chargeStyle', 'ChargeStyleAdmin')
  
// Chargé des fontionnalités sur le site
  function setup () {
  // Permet le suport de vignette
  add_theme_support('post-thumbnails');
  // Retire le générateur de version
  remove_action('wp_head', 'wp_generator');
  // Retire les guillemet à la francaise
  remove_filter('the_content', 'wptexturize')
  // Permet de permettre à WP de compléter la balise title
  add_theme_support('title_tag')  
}
// On lance notre fonction
add_action('setup', 'chargeExtension')
add_action('init', 'mes_menus')
  
  function mes_menu() {
  register_nav_menu('menu1', 'Menu_principal')
}

```

```
6 - Créer un fichier front-page.php dans le dossier project
```

#### front-page.php

```php+html
// Charge le template header.php
<?php get_header(); ?>
	<section>
 // Charge le fichier fichier.php
 <?php get_template_part('fichier.php')?>
	</section>
// Charge le template footer.php
<?php get_footer(); ?>
```

#### Les images

```php+html
get_stylesheet_directory_uri()				=> Donne le chemin du dossier projet
http://lpcm2016.univ-lr.fr/ffremin/evento/wp-content/themes/projet

<img src="<?= get_stylesheet_directory_uri().'/images/slider/bg1.jpg';?>">
```

### Les boucles

```php
<?php
while (have_posts()) : the_post();					=> Récupère le contenu demander the_post
  the_tile()	=> le titre
  the_permalink()	=> le lien								=> Affiche le contenu
  the_content();	=> le contenu			
endwhile
?>
```

#### font-pages.php

```
<?php $the_query = new WP_Query(array('posts_per_page' => 3)); ?>
<?php if ($the_query->have_posts()) :
	while ($the_query->have_posts()) :
		$the_query->the_post();
		the_permalink(); => lien de l'article
		the_post_thumbnail('large', ['class' => 'img-responsive']) => chargé image à la une 

?>
<nav>
	<?php wp_nav_menu(['theme_location' => 'menu1'])
```



### Créer un modèle de page 

Créer un fichier template-contact.php

```php
<?php
/*
Template Name: Nom du modèle				=> Nom qui va apparaitre dans le menu Wordpress
*/
?>
```

### Les Hooks

Permet de modifier le comportement du site de Wordpress

```php
<?php
apply_filter('the_title', $title, $id)


add_filter('the_title', 'nom_de_la_fonction')=> On utilise notre fonction pour modifier the tile
?> 
Déclencher ma_fonction lorsque un l évènement save_post se produit
add_action('save_post', 'ma_fonction', 20)
do_action('save_post')
  
```

### internationalisation

```php
/* POEDIT permet d'afficher les identifiants avec la traduction */
<? _e('category')
echo __('category');
```

### protection

```
Ne pas appeller le comtpe admin
changer le prefixe lors de l'installation wp
BBQ => Détecte et protège des injections 
Wordfence
login security solution
BACKWPUP => Permet de sauvegarder le contenu du site wordpress (images, contenu ...)
htaccess
```

