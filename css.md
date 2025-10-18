# CSS CheatSheet

## Table des matières
- [Sélecteurs](#sélecteurs)
- [Propriétés de base](#propriétés-de-base)
- [Box Model](#box-model)
- [Logical Properties (nouveauté)](#logical-properties)
- [Typographie](#typographie)
- [Couleurs et arrière-plans](#couleurs-et-arrière-plans)
- [Flexbox](#flexbox)
- [Grid](#grid)
- [Positionnement](#positionnement)
- [Transformations et animations](#transformations-et-animations)
- [Variables CSS](#variables-css)
- [Media Queries](#media-queries)
- [Pseudo-classes](#pseudo-classes)
- [Pseudo-éléments](#pseudo-éléments)
- [Propriétés modernes](#propriétés-modernes)

---

## Sélecteurs

### Sélecteurs de base
```css
* { }                    /* Sélecteur universel */
element { }              /* Sélecteur de type */
.classe { }              /* Sélecteur de classe */
#id { }                  /* Sélecteur d'ID */
[attribut] { }           /* Sélecteur d'attribut */
[attribut="valeur"] { }  /* Attribut avec valeur exacte */
[attribut^="valeur"] { } /* Commence par */
[attribut$="valeur"] { } /* Se termine par */
[attribut*="valeur"] { } /* Contient */
```

### Combinateurs
```css
div p { }        /* Descendant (tous les p dans div) */
div > p { }      /* Enfant direct */
div + p { }      /* Frère adjacent (immédiatement après) */
div ~ p { }      /* Frères généraux (tous après) */
```

### Groupement
```css
h1, h2, h3 { }   /* Groupement de sélecteurs */
```

---

## Propriétés de base

### Display
```css
display: block;              /* Élément de bloc */
display: inline;             /* Élément en ligne */
display: inline-block;       /* En ligne avec dimensions */
display: flex;               /* Conteneur flexible */
display: inline-flex;        /* Flex en ligne */
display: grid;               /* Grille */
display: inline-grid;        /* Grille en ligne */
display: none;               /* Masqué */
display: contents;           /* Transparent dans le DOM */
```

### Visibilité
```css
visibility: visible;         /* Visible */
visibility: hidden;          /* Caché mais occupe l'espace */
opacity: 0.5;                /* Transparence (0 à 1) */
```

---

## Box Model

### Dimensions classiques
```css
width: 300px;                /* Largeur */
height: 200px;               /* Hauteur */
min-width: 200px;            /* Largeur minimale */
max-width: 800px;            /* Largeur maximale */
min-height: 100px;           /* Hauteur minimale */
max-height: 500px;           /* Hauteur maximale */
```

### Marges classiques
```css
margin: 20px;                /* Toutes les marges */
margin: 10px 20px;           /* Vertical | Horizontal */
margin: 10px 20px 15px;      /* Haut | Horizontal | Bas */
margin: 10px 20px 15px 25px; /* Haut | Droite | Bas | Gauche */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 15px;
margin-left: 25px;
margin: auto;                /* Centrage horizontal */
```

### Padding classique
```css
padding: 20px;               /* Tous les paddings */
padding: 10px 20px;          /* Vertical | Horizontal */
padding: 10px 20px 15px;     /* Haut | Horizontal | Bas */
padding: 10px 20px 15px 25px;/* Haut | Droite | Bas | Gauche */
padding-top: 10px;
padding-right: 20px;
padding-bottom: 15px;
padding-left: 25px;
```

### Box-sizing
```css
box-sizing: content-box;     /* Défaut (width = contenu) */
box-sizing: border-box;      /* width = contenu + padding + border */
```

---

## Logical Properties

### Marges logiques (nouveauté CSS)
```css
/* Remplacent les propriétés physiques pour le support multilingue */
margin-block: 20px;          /* margin-top + margin-bottom */
margin-block-start: 10px;    /* margin-top (en LTR) */
margin-block-end: 10px;      /* margin-bottom (en LTR) */
margin-inline: 20px;         /* margin-left + margin-right */
margin-inline-start: 10px;   /* margin-left (en LTR) */
margin-inline-end: 10px;     /* margin-right (en LTR) */
```

### Padding logique
```css
padding-block: 20px;         /* padding-top + padding-bottom */
padding-block-start: 10px;   /* padding-top (en LTR) */
padding-block-end: 10px;     /* padding-bottom (en LTR) */
padding-inline: 20px;        /* padding-left + padding-right */
padding-inline-start: 10px;  /* padding-left (en LTR) */
padding-inline-end: 10px;    /* padding-right (en LTR) */
```

### Dimensions logiques
```css
inline-size: 300px;          /* width (en mode horizontal) */
block-size: 200px;           /* height (en mode horizontal) */
min-inline-size: 200px;      /* min-width */
max-inline-size: 800px;      /* max-width */
min-block-size: 100px;       /* min-height */
max-block-size: 500px;       /* max-height */
```

### Bordures logiques
```css
border-block: 2px solid black;           /* border-top + border-bottom */
border-block-start: 1px solid red;       /* border-top (en LTR) */
border-block-end: 1px solid blue;        /* border-bottom (en LTR) */
border-inline: 2px solid black;          /* border-left + border-right */
border-inline-start: 1px solid red;      /* border-left (en LTR) */
border-inline-end: 1px solid blue;       /* border-right (en LTR) */

border-start-start-radius: 10px;         /* border-top-left-radius */
border-start-end-radius: 10px;           /* border-top-right-radius */
border-end-start-radius: 10px;           /* border-bottom-left-radius */
border-end-end-radius: 10px;             /* border-bottom-right-radius */
```

### Position logique
```css
inset: 10px;                 /* top, right, bottom, left */
inset-block: 10px;           /* top + bottom */
inset-block-start: 10px;     /* top (en LTR) */
inset-block-end: 10px;       /* bottom (en LTR) */
inset-inline: 10px;          /* left + right */
inset-inline-start: 10px;    /* left (en LTR) */
inset-inline-end: 10px;      /* right (en LTR) */
```

---

## Typographie

### Police
```css
font-family: Arial, sans-serif;
font-size: 16px;
font-weight: normal;         /* 100-900 ou normal, bold */
font-style: italic;          /* normal, italic, oblique */
font-variant: small-caps;
line-height: 1.5;            /* Hauteur de ligne */
letter-spacing: 2px;         /* Espacement des lettres */
word-spacing: 4px;           /* Espacement des mots */
```

### Texte
```css
text-align: left;            /* left, right, center, justify */
text-align: start;           /* Logique : début du texte */
text-align: end;             /* Logique : fin du texte */
text-decoration: underline;  /* none, underline, overline, line-through */
text-transform: uppercase;   /* uppercase, lowercase, capitalize */
text-indent: 20px;           /* Indentation première ligne */
white-space: nowrap;         /* normal, nowrap, pre, pre-wrap */
word-wrap: break-word;       /* Césure des mots longs */
word-break: break-all;       /* Césure agressive */
text-overflow: ellipsis;     /* Gestion du débordement */
overflow-wrap: break-word;   /* Césure moderne */
```

### Propriétés modernes de texte
```css
text-wrap: balance;          /* Équilibre les lignes (nouveauté) */
text-wrap: pretty;           /* Améliore l'apparence (nouveauté) */
hyphens: auto;               /* Césure automatique */
```

---

## Couleurs et arrière-plans

### Couleurs
```css
color: red;                  /* Nom de couleur */
color: #ff0000;              /* Hexadécimal */
color: rgb(255, 0, 0);       /* RGB */
color: rgba(255, 0, 0, 0.5); /* RGB avec transparence */
color: hsl(0, 100%, 50%);    /* HSL */
color: hsla(0, 100%, 50%, 0.5); /* HSL avec transparence */
color: oklch(0.6 0.3 30);    /* OKLCH (nouveauté) */
color: oklab(0.6 0.3 -0.2);  /* OKLAB (nouveauté) */
```

### Arrière-plans
```css
background-color: blue;
background-image: url('image.jpg');
background-repeat: no-repeat; /* repeat, repeat-x, repeat-y */
background-position: center;  /* top, bottom, left, right, center, 10px 20px */
background-size: cover;       /* auto, cover, contain, 100px 200px */
background-attachment: fixed; /* scroll, fixed, local */

/* Raccourci */
background: #fff url('image.jpg') no-repeat center/cover;
```

### Dégradés
```css
background: linear-gradient(to right, red, blue);
background: linear-gradient(45deg, red, blue);
background: radial-gradient(circle, red, blue);
background: conic-gradient(from 0deg, red, blue);
background: repeating-linear-gradient(45deg, red 0px, blue 10px);
```

### Bordures
```css
border: 2px solid black;
border-width: 2px;
border-style: solid;         /* none, solid, dashed, dotted, double */
border-color: black;
border-radius: 10px;         /* Coins arrondis */
border-radius: 10px 20px 30px 40px; /* TL TR BR BL */

/* Bordures individuelles */
border-top: 1px solid red;
border-right: 2px dashed blue;
border-bottom: 3px dotted green;
border-left: 4px double yellow;
```

### Ombres
```css
box-shadow: 5px 5px 10px rgba(0,0,0,0.3);
box-shadow: inset 5px 5px 10px rgba(0,0,0,0.3); /* Ombre intérieure */
text-shadow: 2px 2px 4px rgba(0,0,0,0.5);

/* Ombres multiples */
box-shadow: 
  0 1px 3px rgba(0,0,0,0.12),
  0 1px 2px rgba(0,0,0,0.24);
```

---

## Flexbox

### Container Flex
```css
display: flex;
display: inline-flex;

/* Direction */
flex-direction: row;         /* row, row-reverse, column, column-reverse */

/* Retour à la ligne */
flex-wrap: nowrap;           /* nowrap, wrap, wrap-reverse */

/* Raccourci */
flex-flow: row wrap;         /* flex-direction flex-wrap */

/* Alignement horizontal (axe principal) */
justify-content: flex-start; /* flex-start, flex-end, center, space-between, space-around, space-evenly */

/* Alignement vertical (axe secondaire) */
align-items: stretch;        /* stretch, flex-start, flex-end, center, baseline */

/* Alignement des lignes multiples */
align-content: stretch;      /* stretch, flex-start, flex-end, center, space-between, space-around */

/* Gap (espacement) - nouveauté */
gap: 20px;                   /* Espacement entre les éléments */
row-gap: 20px;               /* Espacement vertical */
column-gap: 10px;            /* Espacement horizontal */
```

### Items Flex
```css
/* Ordre */
order: 0;                    /* Ordre d'affichage */

/* Croissance */
flex-grow: 0;                /* Facteur de croissance */

/* Rétrécissement */
flex-shrink: 1;              /* Facteur de rétrécissement */

/* Base */
flex-basis: auto;            /* Taille de base */

/* Raccourci */
flex: 0 1 auto;              /* flex-grow flex-shrink flex-basis */
flex: 1;                     /* Croissance flexible */

/* Alignement individuel */
align-self: auto;            /* auto, flex-start, flex-end, center, baseline, stretch */
```

---

## Grid

### Container Grid
```css
display: grid;
display: inline-grid;

/* Définition des colonnes */
grid-template-columns: 200px 1fr 2fr;
grid-template-columns: repeat(3, 1fr);
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));

/* Définition des lignes */
grid-template-rows: 100px auto 200px;
grid-template-rows: repeat(3, 100px);

/* Espacement */
gap: 20px;                   /* Espacement général */
row-gap: 20px;               /* Espacement vertical */
column-gap: 10px;            /* Espacement horizontal */
grid-gap: 20px;              /* Ancienne syntaxe (dépréciée) */

/* Alignement du contenu */
justify-content: start;      /* start, end, center, stretch, space-between, space-around, space-evenly */
align-content: start;        /* start, end, center, stretch, space-between, space-around, space-evenly */

/* Alignement des items */
justify-items: stretch;      /* start, end, center, stretch */
align-items: stretch;        /* start, end, center, stretch */

/* Placement automatique */
grid-auto-flow: row;         /* row, column, dense */
grid-auto-rows: 100px;       /* Taille des lignes auto */
grid-auto-columns: 100px;    /* Taille des colonnes auto */

/* Zones nommées */
grid-template-areas:
  "header header header"
  "sidebar main main"
  "footer footer footer";
```

### Items Grid
```css
/* Placement par ligne/colonne */
grid-column-start: 1;
grid-column-end: 3;
grid-row-start: 1;
grid-row-end: 3;

/* Raccourcis */
grid-column: 1 / 3;          /* start / end */
grid-column: 1 / span 2;     /* start / span */
grid-row: 1 / 3;
grid-area: 1 / 1 / 3 / 3;    /* row-start / col-start / row-end / col-end */

/* Zones nommées */
grid-area: header;

/* Alignement individuel */
justify-self: stretch;       /* start, end, center, stretch */
align-self: stretch;         /* start, end, center, stretch */
```

---

## Positionnement

### Position
```css
position: static;            /* Défaut (flux normal) */
position: relative;          /* Relatif à sa position normale */
position: absolute;          /* Relatif au parent positionné */
position: fixed;             /* Relatif au viewport */
position: sticky;            /* Hybride relative/fixed */

/* Décalages */
top: 10px;
right: 20px;
bottom: 30px;
left: 40px;

/* Z-index */
z-index: 10;                 /* Ordre d'empilement */
```

### Float (ancien, éviter)
```css
float: left;                 /* left, right, none */
clear: both;                 /* left, right, both, none */
```

---

## Transformations et animations

### Transformations 2D
```css
transform: translate(50px, 100px);      /* Déplacement */
transform: translateX(50px);
transform: translateY(100px);
transform: rotate(45deg);                /* Rotation */
transform: scale(1.5);                   /* Échelle */
transform: scale(2, 0.5);                /* Échelle X, Y */
transform: scaleX(2);
transform: scaleY(0.5);
transform: skew(10deg, 20deg);           /* Inclinaison */
transform: skewX(10deg);
transform: skewY(20deg);

/* Transformations multiples */
transform: translate(50px, 100px) rotate(45deg) scale(1.5);

/* Origine de la transformation */
transform-origin: center;    /* center, top left, 50% 50%, etc. */
```

### Transformations 3D
```css
transform: translateZ(50px);
transform: translate3d(50px, 100px, 75px);
transform: rotateX(45deg);
transform: rotateY(45deg);
transform: rotateZ(45deg);
transform: rotate3d(1, 1, 1, 45deg);
transform: scaleZ(2);
transform: scale3d(2, 2, 2);
perspective: 1000px;
transform-style: preserve-3d;
backface-visibility: hidden;
```

### Transitions
```css
transition: all 0.3s ease;
transition: opacity 0.3s, transform 0.5s;
transition-property: opacity;    /* all, none, ou propriété */
transition-duration: 0.3s;
transition-timing-function: ease; /* ease, linear, ease-in, ease-out, ease-in-out, cubic-bezier() */
transition-delay: 0.1s;
```

### Animations
```css
/* Définition */
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}

/* Application */
animation: slideIn 0.5s ease-out;
animation-name: slideIn;
animation-duration: 0.5s;
animation-timing-function: ease-out;
animation-delay: 0s;
animation-iteration-count: infinite; /* nombre ou infinite */
animation-direction: normal;         /* normal, reverse, alternate, alternate-reverse */
animation-fill-mode: forwards;       /* none, forwards, backwards, both */
animation-play-state: running;       /* running, paused */
```

---

## Variables CSS

### Déclaration et utilisation
```css
:root {
  /* Déclaration globale */
  --couleur-primaire: #3498db;
  --couleur-secondaire: #2ecc71;
  --espacement: 20px;
  --police-titre: 'Arial', sans-serif;
}

.element {
  /* Utilisation */
  color: var(--couleur-primaire);
  margin: var(--espacement);
  font-family: var(--police-titre);
  
  /* Avec valeur de secours */
  color: var(--couleur-inexistante, #000);
}

.conteneur {
  /* Déclaration locale (scope limité) */
  --espacement-local: 10px;
  padding: var(--espacement-local);
}
```

---

## Media Queries

### Syntaxe de base
```css
/* Largeur minimale */
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}

/* Largeur maximale */
@media (max-width: 767px) {
  .container {
    width: 100%;
  }
}

/* Plage de largeur */
@media (min-width: 768px) and (max-width: 1024px) {
  .container {
    width: 960px;
  }
}

/* Orientation */
@media (orientation: landscape) { }
@media (orientation: portrait) { }

/* Résolution */
@media (min-resolution: 2dppx) { }

/* Préférences utilisateur (nouveauté) */
@media (prefers-color-scheme: dark) { }
@media (prefers-color-scheme: light) { }
@media (prefers-reduced-motion: reduce) { }
@media (prefers-contrast: high) { }

/* Type de média */
@media screen { }
@media print { }

/* Combinaisons */
@media screen and (min-width: 768px) and (orientation: landscape) { }
```

### Points de rupture courants
```css
/* Mobile first */
@media (min-width: 576px) { }   /* Petit mobile */
@media (min-width: 768px) { }   /* Tablette */
@media (min-width: 992px) { }   /* Desktop */
@media (min-width: 1200px) { }  /* Large desktop */
@media (min-width: 1400px) { }  /* Extra large */
```

---

## Pseudo-classes

### État et interaction
```css
:hover { }                   /* Survol */
:active { }                  /* Clic actif */
:focus { }                   /* Focus clavier */
:focus-visible { }           /* Focus visible (nouveauté) */
:focus-within { }            /* Contient un élément focus */
:visited { }                 /* Lien visité */
:link { }                    /* Lien non visité */
:target { }                  /* Cible d'une ancre */
:disabled { }                /* Désactivé */
:enabled { }                 /* Activé */
:checked { }                 /* Case cochée */
:valid { }                   /* Input valide */
:invalid { }                 /* Input invalide */
:required { }                /* Champ requis */
:optional { }                /* Champ optionnel */
```

### Structure
```css
:first-child { }             /* Premier enfant */
:last-child { }              /* Dernier enfant */
:nth-child(2) { }            /* Nième enfant */
:nth-child(odd) { }          /* Enfants impairs */
:nth-child(even) { }         /* Enfants pairs */
:nth-child(3n) { }           /* Tous les 3 éléments */
:nth-child(3n+1) { }         /* Pattern complexe */
:nth-last-child(2) { }       /* Nième en partant de la fin */
:first-of-type { }           /* Premier de son type */
:last-of-type { }            /* Dernier de son type */
:nth-of-type(2) { }          /* Nième de son type */
:only-child { }              /* Enfant unique */
:only-of-type { }            /* Type unique */
:empty { }                   /* Sans enfants */
:not(.classe) { }            /* Négation */
:is(h1, h2, h3) { }          /* Liste (nouveauté) */
:where(h1, h2, h3) { }       /* Liste sans spécificité (nouveauté) */
:has(> img) { }              /* Contient (nouveauté) */
```

---

## Pseudo-éléments

```css
::before { content: "→ "; }  /* Avant le contenu */
::after { content: " ←"; }   /* Après le contenu */
::first-letter { }           /* Première lettre */
::first-line { }             /* Première ligne */
::selection { }              /* Texte sélectionné */
::placeholder { }            /* Placeholder d'input */
::marker { }                 /* Marqueur de liste (nouveauté) */
::backdrop { }               /* Arrière-plan de dialog */
```

---

## Propriétés modernes

### Container Queries (nouveauté)
```css
.container {
  container-type: inline-size; /* inline-size, size, normal */
  container-name: sidebar;
}

@container (min-width: 400px) {
  .card {
    display: grid;
  }
}

@container sidebar (min-width: 300px) {
  .widget {
    font-size: 1.2rem;
  }
}
```

### Subgrid (nouveauté)
```css
.parent {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.enfant {
  display: grid;
  grid-template-columns: subgrid; /* Hérite de la grille parent */
  grid-column: span 2;
}
```

### Aspect Ratio (nouveauté)
```css
.element {
  aspect-ratio: 16 / 9;        /* Ratio largeur/hauteur */
  aspect-ratio: 1;             /* Carré */
}
```

### Scroll Snap
```css
.container {
  scroll-snap-type: x mandatory; /* x, y, both + none, proximity, mandatory */
  overflow-x: scroll;
}

.item {
  scroll-snap-align: start;    /* start, end, center */
  scroll-snap-stop: always;    /* normal, always */
}
```

### Clamp, Min, Max (nouveauté)
```css
/* Responsive fluide */
font-size: clamp(1rem, 2.5vw, 2rem); /* min, préféré, max */
width: min(100%, 800px);             /* Prend la plus petite valeur */
height: max(200px, 50vh);            /* Prend la plus grande valeur */
```

### Filtres
```css
filter: blur(5px);
filter: brightness(1.5);
filter: contrast(1.2);
filter: grayscale(100%);
filter: hue-rotate(90deg);
filter: invert(100%);
filter: opacity(50%);
filter: saturate(200%);
filter: sepia(100%);
filter: drop-shadow(5px 5px 10px rgba(0,0,0,0.3));

/* Filtres multiples */
filter: blur(5px) brightness(1.2) contrast(1.1);

/* Backdrop filter (nouveauté) */
backdrop-filter: blur(10px);
```

### Blend Modes
```css
mix-blend-mode: multiply;    /* normal, multiply, screen, overlay, etc. */
background-blend-mode: multiply;
isolation: isolate;          /* Crée un nouveau contexte de blend */
```

### Object Fit
```css
object-fit: cover;           /* fill, contain, cover, none, scale-down */
object-position: center;     /* Position de l'objet */
```

### Clip Path
```css
clip-path: circle(50%);
clip-path: ellipse(25% 40%);
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
clip-path: inset(10px 20px 30px 40px);
clip-path: url(#clipPath);
```

### Masques
```css
mask-image: url(mask.png);
mask-size: cover;
mask-position: center;
mask-repeat: no-repeat;
```

### Scroll Behavior
```css
scroll-behavior: smooth;     /* smooth, auto */
scroll-margin: 20px;         /* Marge lors du scroll vers élément */
scroll-padding: 20px;        /* Padding du conteneur de scroll */
overscroll-behavior: contain; /* auto, contain, none */
```

### Pointer Events
```css
pointer-events: none;        /* none, auto */
cursor: pointer;             /* default, pointer, text, move, wait, etc. */
user-select: none;           /* none, auto, text, all */
```

### Appearance
```css
appearance: none;            /* Supprime le style natif */
```

### Accent Color (nouveauté)
```css
accent-color: #3498db;       /* Couleur des contrôles (checkbox, radio, etc.) */
```

### Overflow
```css
overflow: visible;           /* visible, hidden, scroll, auto */
overflow-x: auto;
overflow-y: scroll;
overflow: clip;              /* clip (nouveauté) - coupe sans scrollbar */
text-overflow: ellipsis;
```

### Will-change (optimisation)
```css
will-change: transform;      /* Optimisation performance */
will-change: opacity, transform;
```

### Content Visibility (performance, nouveauté)
```css
content-visibility: auto;    /* auto, hidden, visible */
contain-intrinsic-size: 500px; /* Taille estimée pour performance */
```

### Writing Mode
```css
writing-mode: horizontal-tb; /* horizontal-tb, vertical-rl, vertical-lr */
text-orientation: mixed;     /* mixed, upright, sideways */
direction: ltr;              /* ltr, rtl */
```

---

## Unités de mesure

### Absolues
```css
px                           /* Pixels */
pt                           /* Points (1pt = 1/72 pouce) */
cm, mm, in                   /* Centimètres, millimètres, pouces */
```

### Relatives
```css
%                            /* Pourcentage du parent */
em                           /* Relatif à la taille de police de l'élément */
rem                          /* Relatif à la taille de police de :root */
vw, vh                       /* 1% de la largeur/hauteur du viewport */
vmin, vmax                   /* 1% de la plus petite/grande dimension */
ch                           /* Largeur du caractère "0" */
ex                           /* Hauteur du caractère "x" */
lh                           /* Hauteur de ligne (nouveauté) */
rlh                          /* Hauteur de ligne de :root (nouveauté) */
svw, svh                     /* Small viewport (nouveauté) */
lvw, lvh                     /* Large viewport (nouveauté) */
dvw, dvh                     /* Dynamic viewport (nouveauté) */
```

---

## Propriétés de liste

```css
list-style-type: disc;       /* disc, circle, square, decimal, none, etc. */
list-style-position: inside; /* inside, outside */
list-style-image: url(puce.png);
list-style: square inside;   /* Raccourci */
```

---

## Propriétés de tableau

```css
border-collapse: collapse;   /* collapse, separate */
border-spacing: 10px;
caption-side: top;           /* top, bottom */
empty-cells: show;           /* show, hide */
table-layout: auto;          /* auto, fixed */
```

---

## Propriétés d'impression

```css
@media print {
  page-break-before: always; /* auto, always, avoid, left, right */
  page-break-after: always;
  page-break-inside: avoid;
  
  /* Nouveauté */
  break-before: page;
  break-after: page;
  break-inside: avoid;
}
```

---

## Propriétés de colonnes (multi-colonnes)

```css
columns: 3;                  /* Nombre de colonnes */
columns: 200px;              /* Largeur de colonnes */
columns: 3 200px;            /* count width */
column-count: 3;
column-width: 200px;
column-gap: 30px;
column-rule: 2px solid #ccc; /* Comme border */
column-span: all;            /* none, all */
column-fill: balance;        /* auto, balance */
```

---

## Calculs

```css
width: calc(100% - 80px);
height: calc(100vh - 60px);
font-size: calc(1rem + 2vw);
margin: calc(var(--espacement) * 2);
```

---

## Spécificité CSS

```
!important > style inline > #id > .classe > element

Exemples de spécificité :
- element: 0,0,1
- .classe: 0,1,0
- #id: 1,0,0
- element.classe: 0,1,1
- #id .classe element: 1,1,1
```

---

## Bonnes pratiques

1. **Mobile First** : Commencer par le mobile puis ajouter des media queries pour les écrans plus grands
2. **Logical Properties** : Utiliser margin-block, padding-inline, etc. pour le support multilingue
3. **Variables CSS** : Centraliser les valeurs réutilisables
4. **Flexbox vs Grid** : Flexbox pour 1D (ligne ou colonne), Grid pour 2D
5. **Box-sizing** : Utiliser `box-sizing: border-box` globalement
6. **Unités relatives** : Préférer rem/em aux px pour l'accessibilité
7. **Performance** : Éviter les animations sur des propriétés autres que transform et opacity
8. **Container Queries** : Préférer aux media queries pour les composants réutilisables

---

## Reset CSS minimal

```css
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

img, picture, video, canvas, svg {
  display: block;
  max-width: 100%;
}

input, button, textarea, select {
  font: inherit;
}

p, h1, h2, h3, h4, h5, h6 {
  overflow-wrap: break-word;
}
```

---

## Ressources utiles

- **MDN Web Docs** : Documentation complète CSS
- **Can I Use** : Compatibilité navigateurs
- **CSS Tricks** : Guides et astuces
- **Flexbox Froggy** : Apprendre Flexbox en jouant
- **Grid Garden** : Apprendre Grid en jouant

---

*Dernière mise à jour : 2025 - Inclut les dernières spécifications CSS*
