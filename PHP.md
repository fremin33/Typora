# PHP

## Variables

```php
$mavariable = 3;							=> Affecter une variable de type integer

$mavariable = 'chaine de texte';			=> Affecter une variable de type string

define( 'MACONSTANTE' , 'sa valeur' );		=> Affecter une constante

$monarray = array('pomme','poire','banane');=> Affecter un tableau a une variable
											// $variable[0] vaut pomme
$variable = array(
	'post_type' => 'page',					=> Affecter un tableau associatif à une variable
	'posts_per_page' => 3					// $variable['post_type'] vaut page
);
 
```

## Condition

### if / elseif / else

```php
if( $mavariable == 2 ):
	//ici code php
elseif( $mavariable == 1 ):
	//ici code php
else:
	//ici code php
endif;
```

### foreach

```php
$tableau = array( 'rouge' => '#f00', 'bleu' => '#00f', 'vert' => '#0f0' );

foreach( $tableau as $cle => $valeur ) :
echo 'La clé est : ' . $cle . ', sa valeur est : ' . $valeur . '<br>';
endforeach;
```

### pdo

```php
						<-- Créer une connexion à la base de données -->
$bdd = new PDO( 'mysql:host=server;dbname=bdd', 'user', 'mdp' );

$requete = $bdd->query( 'SELECT * FROM matable' ); => Exécuter la requête SQL sans paramètre

$resultat = $requete->fetchAll(); 			 	   => Récupérer les données

foreach($resultat as $ligne) :
echo $ligne['nom_de_la_colonne'];				   => Afficher les données
endforeach;

Soit avec paramètres nommés (:param), soit avec des marqueurs (?)
  
$requete = $bdd->prepare('
	SELECT * FROM clients WHERE pays = :pays');	=> Exécuter la requête SQL avec paramètre
$requete->execute( array( ':pays' => 'FR' ) );	=> Fournit le tableau nécessaires au WHERE
$clients = $requete->fetchAll();				=> Afficher les données
```

## SuperGlobal

### $_GET

```php
http://example.com/?name=Yannick
<?= $_GET['name']; ?> 								=> Affiche Yannick
  
http://example.com/?name=Yannick&id=10&ville=Paris
<?= $_GET['id']; ?>									=> Affiche 10
$ville = $_GET['ville'];							=> $ville = Paris
```

### $_POST

```php+html
<form action="mapage.php" method="POST">
	<input type="text" name="nom" value="Anne">
	<input type="submit" value="ok">
</form>
<?= $_POST['nom']; ?>								=> Affiche Anne
```

### $_SESSION

Un tableau associatif des valeurs stockées dans les sessions pendant toute la durée de la navigation jusqua la fermeture du navigateur

```php
session_start(); => Pour utiliser les sessions il faut exécuter la fonction avant tout code HTML
$_SESSION['prenom_utilisateur'] = 'Cédric';	=>Permet de créer et stocker une valeur 
echo $_SESSION['prenom_utilisateur'];		=> Affiche la valeur de session;
session_destroy(); 							=> Détruit la session (ex: bouton déconnexion)
```

### $_COOKIE

```php
						<-- A utiliser avant tout code html ! -->
setcookie( 'pseudo' , 'cedric17' , strtotime( '+1 year' ) );
Argument 1 : nom
Argument 2 : valeur
Argument 3 : date d expiration en timestamp(seconde)

echo $_COOKIE['nom'];					=>  Afficher le nom du Cookie


setcookie ('nom', '',time()-3600);	=> On efface sa valeur et on le faitexpirer (ici 1h avant)
```

##  Protection

```
$_POST['value'] = htmlentities($_POST['value']) => Affiche les entité html pour éviter les hacks
$_POST['value'] = strip_tags($_POST['value']) => Supprime les entité html
// Affiche le texte si il ni a pas de caractère spéciaux
<?php if (preg_match("#^[a-zA-Z0-9]+$#", $_POST['value'])) { ?>
	<h3><?= $_POST['commentaire'] ?></h3>
<?php } else {  ?>
	<h3>Le texte n'est pas valide</h3>
<?php } ?>
```

## URL_rewritting

```bash
1- Créer un fichier .htaccess à la racine du projet
RewriteEngine on => Active le mode réecriture
2- On définit une règle d'écriture sur l'url que l'on veut réecrire
On souhaite que tous les urls avec categories/1, categories/2 ... soit rediriger vers  categories.php?id=$1, categories.php?id=$2 ... 
$0 = categories/([0-9]+)
$1 =([0-9]+)
+ signifie qu'on peut répeter les élements plusieurs fois
RewriteRule categories/([0-9]+) categories.php?id=$1 

3- On change la règle pour ajouter du texte avant l'id
$0 = categories/([a-zA-Z0-9]+)([0-9]+)
$1 = ([a-zA-Z0-9]+)
$2 =([0-9]+)
+ signifie qu'on peut répeter les élements plusieurs fois
\- signifie qu'on accepte les - !!! Ne pas oublier \ pour les caractères spéciaux
RewriteRule categories/([a-zA-Z0-9\-]+)([0-9]+) categories.php?id=$2

4- On optimise en créant un champ url dans la BDD avec le texte que l'on souhaite ajouter dans l'url pour corriger l'url si il est mal ecrit

Si l'url de la BDD ne correspond pas à l'url dans le get on change l'url avec le texte de l'url de la BDD
if (data['url'] != $_GET['url']) {
  header('localation:/categories/$data['url]-$_data['id'])
}

5- On modifie le .htaccess
RewriteRule categories/([a-zA-Z0-9\-]+)([0-9]+) categories.php?url$1&id=$2

```

## BBCOCDE

```
Le principe et de changer les balises html <h1> par des [h1] pour se protéger
```

