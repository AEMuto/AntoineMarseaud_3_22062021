# Ohmyfood!
Ce projet a été réalisé dans le cadre de ma formation "Développeur d'application Javascript" chez Openclassrooms.

## Contexte
Ohmyfood! est une jeune startup qui voudrait s'imposer sur le marché de la restauration. L'objectif est de développer un site 100% mobile qui répertorie les menus de restaurants gastronomiques et d'implémenter des animations pour rendre l'expérience utilisateur visuellement attrayante.

## Technologies
J'ai utilisé la méthodologie [BEM](https://en.bem.info/methodology/) avec la syntaxe [SCSS](https://sass-lang.com/documentation/syntax) de [SASS](https://sass-lang.com/) pour la stylisation de l'ensemble du projet. Les icônes que l'on peut voir sont celles de la librairie [Fontawesome](https://fontawesome.com/).

## Cas d'usage
### Exemple: Animation du *Loading Spinner*
Le *Loading Spinner* que l'on aperçoit lorsque l'utilisateur navigue sur la page d'accueil est constitué d'une part :
- D'un élément `<svg>` logo.svg, divisé en `<g>` pour chaque lettres composant le logo, auxquels j'ai attribué un identifiant unique : `<g id="logo-XX">` où `XX` est un intervalle allant de `01` à `08`.
- Et d'autre part, d'un élément `<svg>` logo-offset.svg qui est le contour du logo à partir duquel je fait l'animation des traits circulant autour de celui-ci.

Ainsi pour la page d'accueil `index.html` nous avons l'html suivant pour notre *Loading Spinner* :
```html
<!-- Loader -->
<div class="loader">
  <!-- Contour du Logo -->
  <svg viewBox="0 0 320 65" class="offset">
    <use xlink:href="public/img/logo/logo-offset.svg#logo-offset" />
  </svg>
  <!-- Logo & Lettres le composant -->
  <svg viewBox="0 0 320 65" class="logo">
    <use xlink:href="public/img/logo/logo.svg#logo-01" />
    <use xlink:href="public/img/logo/logo.svg#logo-02" />
    <use xlink:href="public/img/logo/logo.svg#logo-03" />
    <use xlink:href="public/img/logo/logo.svg#logo-04" />
    <use xlink:href="public/img/logo/logo.svg#logo-05" />
    <use xlink:href="public/img/logo/logo.svg#logo-06" />
    <use xlink:href="public/img/logo/logo.svg#logo-07" />
    <use xlink:href="public/img/logo/logo.svg#logo-08" />
  </svg>
</div>
```
Ensuite je définis les animations de la manière suivante dans `/sass/layouts/_loader.scss` :
```scss
.loader {
  /*...*/
  animation: loader 5s 0.5s ease-in-out both; /* Animation d'apparition initial du loading spinner */
  background: $gradient;
  > * {
    position: absolute;
  }
  .offset {
    width: 80vw;
    animation: dash 2s 1.2s ease-in-out both alternate infinite; /* Animation des traits */
    /* Apparence des traits */
    fill: none;
    stroke: $clr-tertiary;
    stroke-dasharray: 100;
    stroke-dashoffset: 1000;
    stroke-linecap: round;
    stroke-width: 3;
  }
  .logo {
    /* Apparence des lettres */
    fill: #fff;
    stroke-width: 0;
    @for $i from 1 to 9 { /* Grâce à SASS nous pouvons boucler sur chaque lettres et incrémenter un délai */
      use:nth-child(#{$i}) {
        animation: letters 0.5s 0.15s * $i ease-in-out both; /* Animations des lettres du logo */
      }
    }
  }
}
```
Puis j'implémente chacune d'entre elles :
- Animation pour l'apparition du *loading spinner*.
```scss
@keyframes loader {
  85% {
    opacity: 100%;
  }
  99% {
    z-index: 5;
  }
  100% {
    z-index: -9999;
    opacity: 0%;
  }
}
```
- Animation des traits.
```scss
@keyframes dash {
  0% {
    stroke-dashoffset: 1000;
    stroke-width: 0;
  }
  25% {
    opacity: 75%;
    stroke-width: 3;
  }
  75% {
    opacity: 25%;
    stroke-width: 3;
  }
  100% {
    stroke-dashoffset: 0;
    stroke-width: 1;
  }
}
```
- Animation des lettres du logo.
```scss
@keyframes letters {
  0% {
    opacity: 0%;
    transform: translateY(-0.6rem);
  }
  70% {
    opacity: 100%;
  }
  100% {
    transform: translateY(0);
  }
}
```
