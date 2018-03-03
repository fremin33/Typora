# FireBase

## Création et deploiment

1 - Crée un projet sur le site Firebase

https://console.firebase.google.com/

2 - On ajoute le script à notre projet pour y intégrer firebase

```html
<script src="https://www.gstatic.com/firebasejs/4.6.1/firebase.js">
    </script>
    <script>
        // Initialize Firebase
        var config = {
            apiKey: "AIzaSyBkmS1I89U1kjaP8bt7IpGjz2BDETcPl_E",
            authDomain: "fir-chat-cf9b3.firebaseapp.com",
            databaseURL: "https://fir-chat-cf9b3.firebaseio.com",
            projectId: "fir-chat-cf9b3",
            storageBucket: "fir-chat-cf9b3.appspot.com",
            messagingSenderId: "967624046375"
        };
        firebase.initializeApp(config);
</script>
```

3 - Héberger un projet sur Firebase

```bash
firebase init					=> Initialise un projet avec firebase
```

4 - Modifier le fichier firebase.json pour précisier le chemin du dossier web

```json
{
    "hosting": {
        "public": "App",
        "ignore": [
            "firebase.json",
            "**/.*",
            "**/node_modules/**"
        ]
    }
}
```

```bash
firebase serve 			=> Permet de lancer un serveur sur le projet 
firebase deploy			=> Permet de deployer le site sur le serveur
```

## Start project

1 - Dans la console de Firebase :

Develop >

​	Authentification : On sélectionne le type d'authentification à intégrer au site

​	Database>Règles :  On définit les droits sur la database

auth !=null || true || false





```
{
  "rules": {
    ".read": "auth != null", 
    ".write": "auth != null"
  }
}
```

Exemple de base de donnée à crée dans l'interface de firebase :

![BDD firebase](C:\Users\flori\Desktop\fireshot\BDD firebase.PNG)

## Fonction de base

```javascript
var database = firebase.database();			=> Permet de créer le lien avec la base de donnée
var Dbmessages = database.ref('Messages');	=> Renvoi un objet contenant les éléments Messages

```



#### Enregistre un changement au niveau de la BDD

```javascript
// Ecrase le contenu dans la BDD>Messages>auteur&contenu
function overwriteMessageData(auteur, contenu) {
     database.ref('Messages').set({
         auteur: auteur,
         contenu: contenu
     });
 }

// Ajoute le contenu dans la BDD>Messages>auteur&contenu
function writeMessageData(auteur, contenu) {
     database.ref('Messages').set({
         auteur: auteur,
         contenu: contenu
     });
 }

// Supprimer un message à partir d'une key
function removeMessage(id) {
    var monMessage = database.ref(`Messages/${id}`);
    monMessage.remove();
}
```

## Ecouteur d'évènement

```javascript
// Execute une fonction lors de l'ajout d'un élement
Dbmessages.on('child_added', function (data) {
    var auteur = data.val().auteur;
    var message = data.val().contenu;
    $('.messages').append(`<p data-id="${data.key}">Je m'appelle ${auteur} et mon message est : ${message}</p>`);
})

// Execute une fonction lors de la supression d'un élément
Dbmessages.on('child_removed', function (data) {
    $(`p[data-id="${data.key}"]`).hide();
})

// Ecute une fonction de la modification d'un élément
Dbmessages.on('child_changed', data => {
   console.log('new update')
});
```

