# HTML - CSS

## Structure HTML 5 :

```html
<body>
  <div id="wrapper">
    <header><nav>...</nav></header>
    <main>
      <article>
      	<header> …</header>
        <footer>…</footer>
      </article>
      <section>
        ...
      </section>
    </main>
    <aside>...</aside>
    <footer>...</footer>
  </div>
</body>
```

![Capture](C:\Users\flori\Desktop\Capture.PNG)

### Pseudo-classes des liens

```
a:link => lien standard
a:visited => lien visité (cliqué)
```

### Pseudo-classes dynamiques

```
element:hover => element pointé (MouseOver)
element:active => element pendant le clic
element:focus => element actuellement actif
```

### Créer un wrapper et le centrer

```css
#wrapper{
	width : 960px;
	margin : 0 auto;
}
```

### La propriété box-sizing

​	Boite sans box-sizing : Width = 80% sans le padding et sans les bordures
​	Boite avec box-sizing : Width = 80% avec padding et bordure

```css
* {
  box-sizing: border-box;
}
```

### Le positionnement

#### display: inline-block

Permet de mettre à ces éléments des dimensions de largeur et de hauteur :

// Ne pas oublier le bug des li (mettre sur la même ligne ou utiliser les commentaires)

​	width en % ne provoque pas de retour à la ligne, les élements se superposerons

```css
li {
    display: inline-block;
    width: 25%;
}
```

​	width en px provoquera un retour à la ligne :

```
li {
  display: inline-block;
  width: 200px;
 }
```

#### float

​	Aligne les éléments sur la gauche avec un retour à la ligne :

```
li {
  float: left;
  width: 200px;
 }
```

​	Aligne les élements sur la droite sans retour à la ligne :

```
li {
  float: right;
  width: 25%;
 }
```

​	Annule la propriété float : 

```
clear: both;
```

### flexbox

flex-direction: row| column

flex:wrap: now-wrap |wrap | wrap-reverse

flex-flow: flex-direction wrap

flex-grow: taille de l'élement (1)

flex-basis: capaciter ç réduire (0)



flex: 0 auto = dimensionne en fonction de width et de height les empêche de sétirer mais les autorise à se contracter;

flex: auto: dimensionne en fonction de width et height et les rend complémentement flexible