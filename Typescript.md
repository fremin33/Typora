# Typescript

## Installer typescript

npm install -g typescript

## Compiler un fichier

tsc nom_fichier.ts

### Langage typé (on indique le type)

- boolean | string | number
- Array / Type[] / Array<Type>
- enum Nom {.,.,.}
- any
- void

```typescript
let nom: string="toto";
let age: number=25;

let list: number[] = [1, 2, 3];
let list: Array = [1, 2, 3];

enum Color {Red, Green, Blue}
let c: Color = Color.Green; 

let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

### Les interfaces

```typescript
interface Voiture {
    id: number;
    marque: string;
    prix: number ;
    plaque?: string; // Facultatif 
}
```

### Classe

```typescript
class Voiture {
  id: number;
  marque: string;
  prix: number ;

  // Toujours un constructor
  constructor( id: number,  marque: string, prix: number )
  {
    this.id=id;
    this.marque=marque;
    this.prix=prix;
  }

  afficher():void{
    console.log(this.marque+" "+this.prix);
  }    
}
```

### OU

```typescript
class Voiture {
  //suppression
  // Permet de simplifier le code précédent, on indique la porté
  constructor( private id: number, private  marque: string, private prix: number )
  {
    //suppression
  }

  afficher():void{
    console.log(this.marque+" "+this.prix);
  }    
}
```



### Création d'un objet 

```typescript
//creation d'un véhicule
let maVoiture:Voiture=new Voiture(1,"Ford",7000);
maVoiture.afficher();
```

## Compiler

```bash
tsc test.ts --watch
```



### Boucles : 

```typescript
let list:number = [4, 5, 6];

// Return l'index
for (let i in list) {
   console.log(i); // "0", "1", "2",
}
// Return la value
for (let i of list) {
   console.log(i); // "4", "5", "6"
}
```

### Import/export

- Pour travailler dans plusieurs fichiers, nous serons obligé d'utiliser des **export/import**
- Il sera nécessaire de compiler le code en "amd" (Asynchronous Module Definition )
- Utilisation du fichier **tsconfig.json**
- Il sera nécessaire d'utiliser **requirejs** pour appeler le programme principal

```typescript
export class Personne
{
    constructor(public id:number, public nom:string, public prenom:string, public age:number) {
    }

    // Affiche les informations d'une Personne dans la console
    toString():void {
        console.log(`${this.prenom} ${this.nom} à ${this.age}`);
    }
}

```

```typescript
import { Personne } from "./personne";
// Création d'une instance de personne
let firstPersonne:Personne = new Personne(1, 'Fremin', 'Florian', 22);
let secondPersonne:Personne = new Personne(2, 'Rey', 'Alain', 47);
// Appelle de la méthode de classe Tostring()
firstPersonne.toString();
secondPersonne.toString();

```

### On télécharge ensuite require.js

http://requirejs.org/docs/download.html

et on compile

```bash
tsc personne.ts main.ts --module amd --watch
```

```
<script data-main="main" src="require.js"></script>
tsc --init
• Modifiez commonjs en amd. 
tsc --watch 
```

