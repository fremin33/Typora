# Javascript

polyfill: faire tourner une nouvelle version sur un ancien navigateur

== transforme un langage en un autre

- babel
- traceur
- closure
- typescript (langage js objet et sera compilé en js)



## Nouveauté es6 : 

prototype = schéma d'un objet (permet de déclarer une méthode

- Déclaration de classe

- On garde le scope de this

- destructuration de tableau = comment obtenir des info d'un tableau

- destructurer un objet

- opérateur rest

- concatener 

- fonction sur les tableaux

- Promise = quand ma response est disponible, tu execute la fonction 

  objet.then(=> {}) 

- utilisation des fonctions euros () => {} à la placce de function() {}

## Les types de variables 

let et var sont déffiérents aux niveau du scope (porté)

```javascript
let a = 5 ;

if (true) {
  var c = 1;
  let d = 2;		// Let 
}
console.log(d) 			=> Undefined car il n'est disponible que dans la condition
console.log(c)			=> 1

for (let i=0; i<10; i++) {
  console.log(i)		// Affichera la valeur de i
}
console.log(i)			// Undefined car il n'est disponible que dans la condition

const d = 10; // Ne peut pas être modifié
let person = { nom: "Bob", age: "20"}
console.log(person.age)	// 20
person.age = 18;
// On peut aussi modifier un objet définit en tant que constante

let tab =[1, 2, 3]	=> Définir un tableau
for (let = 0; i> tab.length; i++) {
  console.log(tab[i])			
}

fonction qui crée un objet : 
funtion create () {
  let nom = "bob";
  let age = 22;
  return {nom, age}	// Return un objet
}

let p1 = create();

// Destructuration
let n = personne.nom;
let a = personne.age
=====
let {nom, age} = personne

// Racourcie de fonction
let titi=(a,b) => a + b
===
    let titi= function (let a, let b) {
      return a + b;
    }
clg(titi(3,4))


// Tableau
// Multiplue 1*3 , 2*3, 3*3
let t = [1, 2, 3]
let t2 = t.map(function(x) return x*3)
=====
let t2 = t.map(x => x*3)

let t = [7, 12, 16]
let t2 = t.filter(x => x>10)



class js = {
  constructor (param1, param2) {
    
  }
  
}


B-tomorow à bx
```

