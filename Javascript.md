## Javascript

```javascript
var DBRef = firebase.database();			=> Récupération de la référence de la racine
var messagesRef = DBRef.ref('Messages') ;	=> Atteindre un nœud grâce à son nom
`je m'appelle ${variable}`					=> Interpollation avec back-tick 
```

### Jquery

```javascript
	// On ajoute un évènement sur un élement dynamique
$('body').on('click', '#listeConcerts li', function () {
    $(this).hide();
});
	// Exemple de plusieur évènement sur un élément
$(this).on({
  click: function() {
    console.log("clique détecté");
  },
  dblclick: function() {
    console("double clique détecté");
  },
  mouseenter: function() {
    console.log("souris entrer");
  },
  mouseleave: function() {
    console.log("souris sorti");
  },
  mousemove: function() {
    console.log(event.pageX);
    console.log(event.pageY);
  }
});

```



### Ajax

// Intégrer Ajax sur un site

```
function integrerAjax(id)
$.ajax({
    type: "GET || POST",
    url: "url API || url BDD",
    data: {
       id : id, 
       name : 'Florian'
    },
    dataType: "xml, json,, jsonp script, or html",
    success: function (response) {
        console.log('Succes')
    }
});
```



### API REST

1. Affichage de résultats simples dans la console
  En Javascript, si t est un tableau de données, t[0] permet d'accéder au premier élément du tableau.
  Si ob est une variable contenant un objet, ob.prenom permet d'accéder au champ prenom d'ob.

