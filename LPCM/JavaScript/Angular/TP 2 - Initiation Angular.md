# Initiation Angular

## Installation d’angular CLI

- Dans un terminal, vérifiez les versions de node et de npm en utilisant les commandes node -v et npm -v. Vous devez avoir des versions au minimum pour node 6.9.x et npm 3.x.x
- Installez le cli en utilisant la commande

```bash
npm install -g @angular/cli
```

Anglur CLI va nous permettre de créer un projet Angular, puis d’ajouter des composants, directives, pipe,
service etc … dont nous avons besoin pour créer une apps



## Création d’un premier projet

Ouvrez un terminal et placez vous dans un dossier nommé par exemple angular

- Pour créer un projet, nous utiliserons la commande :

```bash
ng new premierEx
```

- Placez vous dans le dossier premierEx qui a été créé et lancez la commande :

```bash
ng serve --open
```

Cette commande va vous permettre de lancer le serveur, de voir vos fichiers et l’option « open » lancera
automatiquement votre navigateur sur l’url http://localhost:4200



## Structure

- src : à la racine du répertoire src, on retrouve les fichiers classiques index.html, favicon.ico, styles.css, mais également le main.ts (bootstrap d'Angular 4), le fichier de configuration de la compilation TypeScript tsconfig.json, un fichier de définition TypeScript typings.d.ts.
- src/app : on retrouve les sources de notre premier projet, dont notre premier component :
  AppComponent.

## Le Component Angular

Le component est issu de la mouvance « Web components ». Les Components reposent sur un certain
nombre de règles :

- Un component ne gère que sa vue et ses données. Il n’a pas à modifier les données ou le DOM
  qui est en dehors de son scope.
- Chaque component a son propre cycle de vie.
- Les components communiquent entre eux via les événements, il n'y a pas d'interactions directes



## Modification du premier projet

Le dossier « app » contient :

- Les fichier app.module.ts
- Les 4 fichiers du composants app.component permettant de décrire un composant.
- Ouvrez le fichier app.component.ts, vous obtiendrez le code ci dessous 

```typescript
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

### Explication :

Ce fichier continent plusieurs zones :

- Un espace réservé aux importations de code d'autres modules.
- Dans notre exemple, cet espace nous sert à charger la classe « Component » du module @angular/core qui n’est autre que le module « cœur » d’Angular ;
- @Component est un décorateur. Les décorateurs sont issus du langage TypeScript. Ils permettent de rajouter du comportement à une classe et ses membres. Ce décorateur ajoute donc un ensemble de propriétés à la classe AppComponent pour la « transformer » en Web Component Angular. Cet ensemble s’appelle les MetaDatas. On y retrouve notre template HTML (l'Import HTML W3C) et le selector (le Custom Element W3C) est repris comme container dans index.html



Modifiez le titre avec le texte « Mon premier composant »

- Supprimer le contenu du fichier « app.component.html » et ajoutez le code

```html
<h1>{{title}}</h1>
<p>On demarre doucement</p>
```

- Dans le fichier app.component.css, ajoutez la règle css permettant de mettre le titre h1 en rouge
- Ouvrez le fichier app.module.ts. Nous pouvons remarquer qu’il y a également 3 parties, les
  imports, les décorateurs et l’export de la classe

## Imbrication de Components

Lors de vos développements futurs, vous aurez souvent besoin d'imbriquer vos Components dans d'autres
components

### Utilisation de bootstrap

- Liez le fichier css de boostrap au fichier index.html


- Modifiez le title du component avec le titre « Stade Rochelais »
- Modifiez le template du component pour qu’il affiche un jumbotron (bootstrap) permettant de faire afficher le title ainsi qu’un paragraphe
- Les images seront stockées dans le dossier asset.



## Création d’un nouveau component

Pour créer un composant, vous utiliserez la commande d’angular CLI

```bash
ng generate component nom_du_composant
```

Cette commande permet de créer un dossier portant le « nom_du_composant » contenant nos 4 fichiers.

- Créez un composant nommé joueurs
- Modifiez le template pour afficher un titre et un lorem ipsum dans un paragraphe.
- Appelez ce composant dans le fichier app.component.html
- Regardez le code qui a été modifié dans app.module.ts



## Créer une classe

Pour créer une classe, vous utiliserez la commande d’angular CLI

```bash
ng generate class nom_classe
```

Cette commande permet de créer un fichier « nom_classe.ts ». Ce fichier sera placer dans le dossier app, si
vous souhaitez qu’il soit soit dans un dossier « share » par exemple il suffira de préciser le chemin avant
le nom de la classe dans la commande cli

- Créez une classe joueur. 
- Ajouter les attributs un id, un nom, un prenom, et des attributs optionnels nommés image, poste, nbMatches, pointsMarques, desccription.



## Utilisation de la classe

Nous souhaitons utiliser cette classe dans le composant joueurs.component.ts.

- Importez la classe Joueur
- Créez une variable unJoueur de type Joueur
- Créez le joueur suivant : new Joueur(1,"botia","Levani","1.jpg", »3ieme ligne)
- Créez une variable title contenant le texte « Equipe pro »
- Modifiez le template de ce component permettant d’afficher dans une balise h2 le contenu de la variable title
- Afficher dans un paragraphe le nom et le prénom du joueurs et sa photo



## Utilisation d'un pipe

Nous souhaitons afficher le nom du joueur en majuscule. Angular possède des composants appelés Pipe.
Certains existent déjà mais nous en créerons plus tard. Pour utiliser un pipe, vous utiliserez le caractère |

- Modifiez l’affichage du nom comme ceci

```html
{{unJoueur.nom | uppercase }}
```



## Un peu de boostrap

Faites afficher sous forme de « card » Bootstrap le nom, le prénom du joueur et l’image



## Plusieurs Joueurs

Nous souhaitons avoir dans notre component « joueurs.component.ts », un tableau contenant plusieurs
joueurs (Nous améliorerons ceci lors de la prochaine séance)

- Créer un attribut lesJoueurs de type Array
- Allouer ce tableau
- Ajoutez les 3 joueurs suivants dans le tableau. Nous ferons ceci dans la méthode ngOnInit()
  - new Joueur(1,"botia","Levani","1.jpg","3ieme ligne",83,90)
  - new Joueur(2,"gourdon","kevin","2.jpg","3ieme ligne",135,90)
  - new Joueur(3,"priso","dany","3.jpg","pilier",34,10)



## Directive ngFor (boucle)

Nous utiliserons cette directive pour boucler sur le tableau

- Avant de faire un affiche propre avec des « card » nous souhaitons faire afficher le nom et prenom de chaque joueur sous la forme d’une liste

```html
<ul>
 <li *ngFor="let joueur of lesJoueurs">
	{{joueur.nom | uppercase}} {{joueur.prenom}}
</li>
</ul>
```

- Affichez ces 3 joueurs sont la forme de « card » Boostrap

## Passage de paramètre

Créez un nouveau component nommé detail-joueur
Ce composant permettra d’afficher tous les attributs d’un joueur sous forme d’une liste. Ce composant
récupérera le joueur sur lequel l’utilisateur aura cliqué.

- Dans le fichier detail-joueur.component.ts,
- Importez la classe Personne,
- Ajouter le l’attribut suivant : 

```typescript
@Input() monJoueur: Joueur;
```

Ici l’attribut, monJoueur utilise l’annotation @Input permettant de recevoir des données en
utilisant le principe de propriété binding. 

Pour utiliser cette annotation, vous devez l’ajouter dans les imports

```typescript
import { Component, OnInit, Input } from '@angular/core';
```



Dans le fichier de template, faites afficher afficher toutes les caractéristiques de ce joueur
« monJoueur »
Vous allez à présent coder le principe suivant. Au clic sur le lien « détail » de chaque joueur alors les
caractéristiques de celui ci seront afficher dessous le listing de tous les joueurs.

- Ajoutez un attribut joueurSelectionne de type Joueur dans le fichier joueurs.component.ts
- Dans ce même fichier créer la méthode suivante :

```typescript
select(j:Joueur):void{
 console.log("joueur");
 this.joueurSeclectionne=j;
}
```

- Nous avons utiliser cette méthode dans le template « joueurs.component.html »

```html
<a href="#" class="btn btn-primary" (click)="select(joueur);">Détail</a>
```

- Ajouter le composant  à la fin du template « joueurs.component.html »

```html
<app-detail-joueur [monJoueur]="joueurSeclectionne"></app-detail-joueur> 
```

Nous appelons le component  en lui passant en paramètre le joueur sélectionné.
Que se passe-t-il lorsque aucun joueur n’est sélectionné ?



## Directive ngIf

Pour améliorer le problème nous allons utiliser la directive ngIf qui permet d’afficher ou non un élément
si le test est valide.

```html
<app-detail-joueur *ngIf="joueurSeclectionne" 
[monJoueur]="joueurSeclectionne"></app-detail-joueur>
```

## Amélioration

Améliorer l’affichage du détail d’un joueur ….. vous pouvez vous inspirer du site officiel (exemple :
https://www.staderochelais.com/les-equipes/dany-priso )…. mais pour l’instant nous travaillons en One
Page …. 