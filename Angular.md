# Angular

## Installation du client

Installez le cli en utilisant la commande :

```
npm install -g @angular/cli
```

Anglur CLI va nous permettre de créer un projet Angular, puis d’ajouter des composants, directives, pipe,
service etc … dont nous avons besoin pour créer une apps

## Créer un projet

Pour créer un projet, nous utiliserons la commande :

```bash
ng new premierEx 
```

## Lancer un serveur

Placez vous dans le dossier premierEx qui a été créé et lancez la commande :

```
ng serve --open
```

Cette commande va vous permettre de lancer le serveur, de voir vos fichiers et l’option « open » lancera
automatiquement votre navigateur sur l’url http://localhost:4200

## Les fichiers

- .editorconfig : ce fichier est issu du projet EditorConfig. Il a pour but de maintenir une cohérence dans le code entre l'ensemble des éditeurs et IDE du marché. Le fichier fonctionne nativement sur certains éditeurs alors qu'un plugin sera nécessaire pour d'autres. Très peu d'éditeurs/IDE ne connaissent pas ce fichier ; c'est donc un standard de fait.


- tslint.json : fichier définissant les règles de codage TypeScript. Tout comme le fichier .editorconfig, il est reconnu par la majorité des éditeurs de code.


- src : à la racine du répertoire src, on retrouve les fichiers classiques index.html, favicon.ico, styles.css, mais également le main.ts (bootstrap d'Angular 4), le fichier de configuration de la compilation TypeScript tsconfig.json, un fichier de définition TypeScript typings.d.ts
- src/app : on retrouve les sources de notre premier projet, dont notre premier component :
  AppComponent.



## Les components

- Un component ne gère que sa vue et ses données. Il n’a pas à modifier les données ou le DOM qui est en dehors de son scope.


- Chaque component a son propre cycle de vie.
- Les components communiquent entre eux via les événements, il n'y a pas d'interactions directes



## Let's Go

- les 4 fichiers du composants app.component permettant de décrire un composant.
  - app.component.css qui charge tous les élements du component
  - app.component.html
  - app.component.ts
  - app.module.ts

```typescript
// src/app/app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';
}
```
## Créer un composant

```
ng generate component nom_du_composant
```

## Modifier le template

```
// src/app/joueurs/joueurs.component.html
<h1>Titre</h1>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Amet aperiam consequ</p>
```

## Faire appelle à un composant

```
// src/app/app.component.html
<div class="jumbotron">
  <img src="../assets/logo.png" alt="">
  <h1>{{title}}</h1>
  <p>Une petite présentation des joueurs du stade</p>
</div>
<app-joueurs></app-joueurs>
```

## Créer une classe

```
ng generate class nom_classe
```

## Créer un objet 

```
// src/app/joueurs/joueur.component.ts
import {Component, OnInit} from '@angular/core';
import {Joueur} from "../share/joueur";

@Component({
  selector: 'app-joueurs',
  templateUrl: './joueurs.component.html',
  styleUrls: ['./joueurs.component.css']
})
export class JoueursComponent implements OnInit {
  unJoueur: Joueur;


  constructor() {

  }

  ngOnInit() {
    this.unJoueur = new Joueur(1,"botia","Levani","1.jpg",'3ieme ligne')
  }

  title = 'Equipe Pro';

}

```

## Afficher l'objet

```
// // src/app/joueurs/joueur.component.html
<h2>{{ title }}</h2>
<p> Joueur : {{ unJoueur.nom | uppercase}} {{ unJoueur.prenom}}</p>
<img src="../../assets/joueurs/{{unJoueur.images}}" alt="">

```

