# Programmation orientée objet

### Les objets

Un « objet » est une représentation d'une chose matérielle ou immatérielle du réel à laquelle on associe des propriétés et des actions .

Les « attributs »  ou  « les propriétés » sont les caractéristiques propres à un objet. 

Les « méthodes » sont les actions applicables à un objet. 

Une « classe » est un modèle de données définissant la structure d'un objet

Lorsque l'on crée un objet, on réalise ce que l'on appelle une « instance de la classe ». 



### constructor 

Le constructeur est une méthode qui est appelée à la création de l'objet (instanciation). 

```php
class Exemple {
	public $attribut;
    function __construct ($atribut) {
        $this->attribut = $attribut;
    }
}
$exempleObject = new Exemple('monAttribut');
```



### Visibilité 

- public => Disponible partout 
- private => Disponible dans la classe
- protected => Disponible dans la classe et les enfants (héritage)



### Méthode et propriété static

Le fait de déclarer des propriétés ou des méthodes comme statiques vous permet d'y accéder sans avoir besoin d'instancier la classe. 

```php
class Foo
{
    public static $my_static = 'foo';

    public function staticValue() {
        // Self fais référence à la classe pas à l'objet
        return self::$my_static;
    }
}
// Appel de la variable
Foo::$my_static;
// Appel de la fonction 
Foo::staticValue();

```

### héritage

```php
// Appel la function atk de la class mère
public function atk($target) {
    parent::atk($target);
}
```

### $this, self et static

```
$this fait référence à l'object
self fait référence à la Classe
static fait référence à la Classe utiliser pour appeler une méthode (avec l'héritage par exemple)
```

### Singleton

```
C'est d'avoir qu'une seule et unique instance d'une même classe dans un programme.
Concrètement, un singleton est très simple à mettre en place. Il est composé de 3 caractéristiques :

Un attribut privé et statique qui conservera l'instance unique de la classe.
Un constructeur privé afin d'empêcher la création d'objet depuis l'extérieur de la classe
Une méthode statique qui permet soit d'instancier la classe soit de retourner l'unique instance créée.
```

### Factory

```
Le Factory est un design pattern incontournable qui va vous permettre de beaucoup mieux structurer vos classes. Le principe est d'avoir une classe qui va se charger de créer les objets dont on a besoin.
```

#### Injection de dépendances

```
Si une classe a besoin d'une instance d'une autre classe, que ce soit dans son constructeur ou dans une autre méthode (un setter par exemple), alors elle prend cette instance directement en paramètre et ne s'occupe certainement pas de l'instancier elle-même
```

