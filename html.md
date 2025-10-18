# HTML CheatSheet

## Table des matières
1. [Introduction](#introduction)
2. [Structure de base](#structure-de-base)
3. [Balises sémantiques](#balises-sémantiques)
4. [Texte et typographie](#texte-et-typographie)
5. [Listes](#listes)
6. [Liens](#liens)
7. [Images et médias](#images-et-médias)
8. [Tableaux](#tableaux)
9. [Formulaires](#formulaires)
10. [Métadonnées](#métadonnées)
11. [HTML5 moderne](#html5-moderne)
12. [Attributs globaux](#attributs-globaux)
13. [Bonnes pratiques](#bonnes-pratiques)

---

## Introduction

### Qu'est-ce que le HTML ?

HTML (HyperText Markup Language) est le langage de balisage standard pour créer des pages web. Il décrit la **structure** et le **contenu** d'une page web à l'aide de balises.

### Syntaxe de base
```html
<balise>contenu</balise>          <!-- Balise avec contenu -->
<balise attribut="valeur">         <!-- Balise avec attribut -->
<balise />                         <!-- Balise auto-fermante -->

<!-- Commentaire HTML (non visible dans le navigateur) -->
```

---

## Structure de base

### Document HTML minimal
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titre de la page</title>
</head>
<body>
    <!-- Contenu de la page -->
    <h1>Bienvenue</h1>
    <p>Contenu de la page</p>
</body>
</html>
```

### Explication des balises principales

#### `<!DOCTYPE html>`
- Déclaration obligatoire en première ligne
- Indique que c'est un document HTML5
- Sans cette déclaration, le navigateur peut basculer en "mode quirks"

#### `<html>`
- Balise racine qui contient tout le document
- Attribut `lang` : définit la langue du contenu (important pour l'accessibilité et le SEO)
```html
<html lang="fr">    <!-- Français -->
<html lang="en">    <!-- Anglais -->
<html lang="es">    <!-- Espagnol -->
```

#### `<head>`
- Contient les métadonnées du document
- Non visible dans la page
- Informations pour les navigateurs et moteurs de recherche

#### `<body>`
- Contient tout le contenu visible de la page
- Un seul `<body>` par document

---

## Balises sémantiques

### Pourquoi les balises sémantiques ?

Les balises sémantiques décrivent le **sens** et la **structure** du contenu, pas seulement son apparence. Elles sont essentielles pour :

#### 1. **Accessibilité** 🦽
- Les lecteurs d'écran utilisent les balises sémantiques pour permettre aux personnes malvoyantes de naviguer efficacement
- Exemple : un lecteur d'écran peut annoncer "Navigation principale" en détectant `<nav>`, permettant à l'utilisateur de passer directement au contenu principal

#### 2. **SEO (référencement)** 📈
- Les moteurs de recherche comprennent mieux la structure et l'importance du contenu
- `<article>` indique du contenu principal indexable
- `<h1>` à `<h6>` créent une hiérarchie claire pour Google

#### 3. **Maintenance du code** 🔧
- Code plus lisible et compréhensible
- Facilite le travail en équipe
- Plus facile à déboguer

#### 4. **Structure logique** 🏗️
- Sépare clairement les différentes sections
- Meilleure organisation du contenu

### Comparaison : Non sémantique vs Sémantique

#### ❌ Non sémantique (à éviter)
```html
<!-- Difficile à comprendre, pas accessible -->
<div id="header">
    <div id="logo">Mon Site</div>
    <div id="menu">
        <div class="menu-item">Accueil</div>
        <div class="menu-item">À propos</div>
    </div>
</div>

<div id="content">
    <div class="post">
        <div class="title">Mon article</div>
        <div class="text">Contenu de l'article...</div>
    </div>
</div>

<div id="footer">
    <div>© 2025 Mon Site</div>
</div>
```

#### ✅ Sémantique (recommandé)
```html
<!-- Clair, accessible, bien structuré -->
<header>
    <h1>Mon Site</h1>
    <nav>
        <ul>
            <li><a href="#accueil">Accueil</a></li>
            <li><a href="#about">À propos</a></li>
        </ul>
    </nav>
</header>

<main>
    <article>
        <h2>Mon article</h2>
        <p>Contenu de l'article...</p>
    </article>
</main>

<footer>
    <p>&copy; 2025 Mon Site</p>
</footer>
```

### Balises sémantiques principales

#### `<header>`
**Usage** : En-tête d'une page ou d'une section
**Contenu typique** : Logo, titre principal, navigation

```html
<!-- En-tête de page -->
<header>
    <h1>Mon Blog</h1>
    <nav>
        <a href="/">Accueil</a>
        <a href="/about">À propos</a>
    </nav>
</header>

<!-- En-tête d'article -->
<article>
    <header>
        <h2>Titre de l'article</h2>
        <p>Publié le <time datetime="2025-10-18">18 octobre 2025</time></p>
    </header>
    <p>Contenu de l'article...</p>
</article>
```

#### `<nav>`
**Usage** : Navigation principale ou sections de navigation
**Importance** : Aide les lecteurs d'écran à trouver rapidement la navigation

```html
<nav aria-label="Navigation principale">
    <ul>
        <li><a href="/">Accueil</a></li>
        <li><a href="/blog">Blog</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>

<!-- Navigation secondaire (fil d'Ariane) -->
<nav aria-label="Fil d'Ariane">
    <ol>
        <li><a href="/">Accueil</a></li>
        <li><a href="/blog">Blog</a></li>
        <li>Article actuel</li>
    </ol>
</nav>
```

#### `<main>`
**Usage** : Contenu principal unique de la page
**Règle** : Un seul `<main>` par page
**Ne pas inclure** : En-tête, pied de page, navigation répétée

```html
<main>
    <h1>Bienvenue sur mon site</h1>
    <p>Contenu principal de la page...</p>
    
    <section>
        <h2>Nos services</h2>
        <!-- ... -->
    </section>
</main>
```

#### `<article>`
**Usage** : Contenu autonome et réutilisable (article de blog, post, produit)
**Test** : Le contenu a-t-il du sens hors contexte ?

```html
<article>
    <header>
        <h2>Guide complet HTML5</h2>
        <p>Par <a href="/author/alice">Alice Dupont</a></p>
        <time datetime="2025-10-18">18 octobre 2025</time>
    </header>
    
    <p>Introduction de l'article...</p>
    
    <section>
        <h3>Première partie</h3>
        <p>Contenu...</p>
    </section>
    
    <footer>
        <p>Tags : <a href="/tag/html">HTML</a>, <a href="/tag/web">Web</a></p>
    </footer>
</article>
```

#### `<section>`
**Usage** : Section thématique avec un titre
**Différence avec `<div>` : `<section>` a une signification sémantique

```html
<article>
    <h1>Guide du débutant</h1>
    
    <section>
        <h2>Introduction</h2>
        <p>Contenu de l'introduction...</p>
    </section>
    
    <section>
        <h2>Premiers pas</h2>
        <p>Contenu des premiers pas...</p>
    </section>
    
    <section>
        <h2>Conclusion</h2>
        <p>Conclusion...</p>
    </section>
</article>
```

#### `<aside>`
**Usage** : Contenu tangentiellement lié (sidebar, notes, publicités)
**Position** : Souvent latéral mais pas obligatoire

```html
<main>
    <article>
        <h1>Article principal</h1>
        <p>Contenu de l'article...</p>
        
        <!-- Note dans l'article -->
        <aside>
            <h3>Le saviez-vous ?</h3>
            <p>Information complémentaire...</p>
        </aside>
    </article>
</main>

<!-- Sidebar globale -->
<aside>
    <section>
        <h2>Articles récents</h2>
        <ul>
            <li><a href="/article1">Article 1</a></li>
            <li><a href="/article2">Article 2</a></li>
        </ul>
    </section>
</aside>
```

#### `<footer>`
**Usage** : Pied de page d'une section ou de la page
**Contenu typique** : Copyright, liens légaux, informations de contact

```html
<!-- Pied de page global -->
<footer>
    <nav aria-label="Navigation du pied de page">
        <a href="/mentions-legales">Mentions légales</a>
        <a href="/contact">Contact</a>
    </nav>
    <p>&copy; 2025 Mon Site. Tous droits réservés.</p>
</footer>

<!-- Pied d'article -->
<article>
    <h2>Titre de l'article</h2>
    <p>Contenu...</p>
    <footer>
        <p>Auteur : Alice Dupont</p>
        <p>Publié le : 18 octobre 2025</p>
    </footer>
</article>
```

### Balises sémantiques supplémentaires

#### `<figure>` et `<figcaption>`
**Usage** : Image, diagramme, code avec légende

```html
<figure>
    <img src="diagramme.png" alt="Diagramme du processus">
    <figcaption>Figure 1 : Processus de développement</figcaption>
</figure>

<figure>
    <pre><code>
        function hello() {
            console.log("Hello");
        }
    </code></pre>
    <figcaption>Exemple de code JavaScript</figcaption>
</figure>
```

#### `<details>` et `<summary>`
**Usage** : Contenu pliable/dépliable

```html
<details>
    <summary>Voir plus d'informations</summary>
    <p>Contenu caché par défaut qui apparaît au clic</p>
</details>

<details open>
    <summary>FAQ : Comment ça marche ?</summary>
    <p>Réponse à la question...</p>
</details>
```

#### `<mark>`
**Usage** : Texte surligné/mis en évidence

```html
<p>Les balises <mark>sémantiques</mark> sont importantes pour le SEO.</p>
```

#### `<time>`
**Usage** : Date et heure avec format machine-readable

```html
<p>Publié le <time datetime="2025-10-18">18 octobre 2025</time></p>
<p>L'événement commence à <time datetime="2025-10-18T14:30">14h30</time></p>
<p>Durée : <time datetime="PT2H30M">2 heures 30 minutes</time></p>
```

#### `<address>`
**Usage** : Informations de contact

```html
<address>
    <p>Écrit par <a href="mailto:alice@example.com">Alice Dupont</a></p>
    <p>123 Rue de la Paix, Paris</p>
    <p>Tél : <a href="tel:+33123456789">01 23 45 67 89</a></p>
</address>
```

### Structure sémantique complète

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blog - Mon Site</title>
</head>
<body>
    <!-- En-tête global -->
    <header>
        <h1>Mon Blog</h1>
        <nav aria-label="Navigation principale">
            <ul>
                <li><a href="/">Accueil</a></li>
                <li><a href="/articles">Articles</a></li>
                <li><a href="/about">À propos</a></li>
            </ul>
        </nav>
    </header>

    <!-- Contenu principal -->
    <main>
        <!-- Article de blog -->
        <article>
            <header>
                <h2>Introduction au HTML sémantique</h2>
                <p>
                    Par <a href="/author/alice">Alice</a>
                    le <time datetime="2025-10-18">18 octobre 2025</time>
                </p>
            </header>

            <p>Le HTML sémantique améliore l'accessibilité...</p>

            <figure>
                <img src="semantic.png" alt="Schéma des balises sémantiques">
                <figcaption>Les principales balises sémantiques HTML5</figcaption>
            </figure>

            <section>
                <h3>Pourquoi utiliser les balises sémantiques ?</h3>
                <p>Contenu de la section...</p>
            </section>

            <aside>
                <h4>Note importante</h4>
                <p>Information complémentaire...</p>
            </aside>

            <footer>
                <p>Tags : 
                    <a href="/tag/html">HTML</a>, 
                    <a href="/tag/semantic">Sémantique</a>
                </p>
            </footer>
        </article>
    </main>

    <!-- Barre latérale -->
    <aside>
        <section>
            <h2>Articles populaires</h2>
            <ul>
                <li><a href="/article1">Article 1</a></li>
                <li><a href="/article2">Article 2</a></li>
            </ul>
        </section>
    </aside>

    <!-- Pied de page -->
    <footer>
        <nav aria-label="Navigation du pied de page">
            <a href="/mentions-legales">Mentions légales</a>
            <a href="/privacy">Confidentialité</a>
            <a href="/contact">Contact</a>
        </nav>
        <address>
            Contact : <a href="mailto:contact@example.com">contact@example.com</a>
        </address>
        <p>&copy; 2025 Mon Blog. Tous droits réservés.</p>
    </footer>
</body>
</html>
```

---

## Texte et typographie

### Titres (Headings)
```html
<h1>Titre principal (un seul par page)</h1>
<h2>Titre de section</h2>
<h3>Sous-titre</h3>
<h4>Sous-sous-titre</h4>
<h5>Niveau 5</h5>
<h6>Niveau 6 (le plus petit)</h6>
```

**⚠️ Règles importantes :**
- Un seul `<h1>` par page (titre principal)
- Ne pas sauter de niveaux (h1 → h3 directement)
- Utiliser pour la structure, pas le style

**Hiérarchie correcte :**
```html
<h1>Mon Site</h1>
    <h2>Section 1</h2>
        <h3>Sous-section 1.1</h3>
        <h3>Sous-section 1.2</h3>
    <h2>Section 2</h2>
        <h3>Sous-section 2.1</h3>
```

### Paragraphes et sauts
```html
<p>Un paragraphe de texte.</p>

<br>  <!-- Saut de ligne simple -->

<hr>  <!-- Ligne horizontale (séparation thématique) -->
```

### Formatage de texte

#### Emphase et importance
```html
<em>Texte mis en emphase (généralement italique)</em>
<strong>Texte important (généralement gras)</strong>

<i>Italique (termes techniques, pensées)</i>
<b>Gras (sans importance sémantique)</b>

<mark>Texte surligné</mark>
<small>Petit texte (mentions légales, copyright)</small>
```

#### Citations
```html
<blockquote cite="https://source.com">
    <p>Citation longue, séparée du texte principal.</p>
    <footer>— <cite>Auteur</cite></footer>
</blockquote>

<q>Citation courte inline</q>

<cite>Titre d'une œuvre (livre, film, article)</cite>
```

#### Code et préformaté
```html
<code>Code inline</code>

<pre>
Texte préformaté
    Les espaces et sauts de ligne
        sont conservés
</pre>

<pre><code>
function hello() {
    console.log("Hello World");
}
</code></pre>

<kbd>Ctrl + C</kbd>  <!-- Touche clavier -->
<samp>Sortie d'un programme</samp>
<var>x = 10</var>  <!-- Variable mathématique -->
```

#### Modifications et définitions
```html
<del>Texte supprimé</del>
<ins>Texte inséré</ins>

<s>Texte barré (non pertinent)</s>
<u>Texte souligné</u>

<abbr title="HyperText Markup Language">HTML</abbr>

<dfn>Définition d'un terme</dfn>
```

#### Exposants et indices
```html
<p>E = mc<sup>2</sup></p>  <!-- Exposant -->
<p>H<sub>2</sub>O</p>       <!-- Indice -->
```

#### Autres
```html
<ruby>
    漢 <rt>kan</rt>
    字 <rt>ji</rt>
</ruby>

<bdo dir="rtl">Texte de droite à gauche</bdo>

<wbr>  <!-- Suggestion de coupure de mot -->
```

---

## Listes

### Liste non ordonnée
```html
<ul>
    <li>Premier élément</li>
    <li>Deuxième élément</li>
    <li>Troisième élément</li>
</ul>

<!-- Liste imbriquée -->
<ul>
    <li>Élément 1
        <ul>
            <li>Sous-élément 1.1</li>
            <li>Sous-élément 1.2</li>
        </ul>
    </li>
    <li>Élément 2</li>
</ul>
```

### Liste ordonnée
```html
<ol>
    <li>Premier</li>
    <li>Deuxième</li>
    <li>Troisième</li>
</ol>

<!-- Commencer à un numéro différent -->
<ol start="5">
    <li>Cinquième</li>
    <li>Sixième</li>
</ol>

<!-- Types de numérotation -->
<ol type="a">  <!-- a, b, c -->
<ol type="A">  <!-- A, B, C -->
<ol type="i">  <!-- i, ii, iii -->
<ol type="I">  <!-- I, II, III -->
<ol type="1">  <!-- 1, 2, 3 (défaut) -->

<!-- Ordre inversé -->
<ol reversed>
    <li>Trois</li>
    <li>Deux</li>
    <li>Un</li>
</ol>
```

### Liste de définitions
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
    
    <dt>JavaScript</dt>
    <dd>Langage de programmation web</dd>
    <dd>Créé par Brendan Eich en 1995</dd>
</dl>
```

---

## Liens

### Lien de base
```html
<a href="https://example.com">Texte du lien</a>

<!-- Lien dans un nouvel onglet -->
<a href="https://example.com" target="_blank">Lien externe</a>

<!-- Avec relation (sécurité pour target="_blank") -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    Lien sécurisé
</a>
```

### Types de liens

#### Liens internes
```html
<!-- Lien vers une autre page du site -->
<a href="/about.html">À propos</a>
<a href="contact.html">Contact</a>

<!-- Lien vers une ancre -->
<a href="#section1">Aller à la section 1</a>
<h2 id="section1">Section 1</h2>

<!-- Retour en haut -->
<a href="#top">Retour en haut</a>
```

#### Liens spéciaux
```html
<!-- Email -->
<a href="mailto:contact@example.com">Envoyer un email</a>
<a href="mailto:contact@example.com?subject=Demande&body=Bonjour">
    Email avec sujet
</a>

<!-- Téléphone -->
<a href="tel:+33123456789">01 23 45 67 89</a>

<!-- SMS -->
<a href="sms:+33123456789">Envoyer un SMS</a>

<!-- Téléchargement -->
<a href="document.pdf" download>Télécharger le PDF</a>
<a href="image.jpg" download="mon-image.jpg">Télécharger</a>
```

#### Attributs de lien
```html
<!-- title : info-bulle -->
<a href="page.html" title="Description du lien">Lien</a>

<!-- rel : relation -->
<a href="https://example.com" rel="nofollow">Lien nofollow</a>
<a href="next.html" rel="next">Page suivante</a>
<a href="prev.html" rel="prev">Page précédente</a>

<!-- hreflang : langue de la destination -->
<a href="/en/page.html" hreflang="en">English version</a>
```

---

## Images et médias

### Images

#### Image de base
```html
<img src="image.jpg" alt="Description de l'image">

<!-- Avec dimensions (recommandé pour les performances) -->
<img src="image.jpg" alt="Description" width="800" height="600">
```

**⚠️ Attribut `alt` obligatoire :**
- Décrit l'image pour les lecteurs d'écran
- Affiché si l'image ne charge pas
- Important pour le SEO
- Laisser vide (`alt=""`) si image décorative

#### Image responsive
```html
<!-- Différentes tailles selon la largeur d'écran -->
<img 
    src="image-medium.jpg" 
    srcset="image-small.jpg 400w,
            image-medium.jpg 800w,
            image-large.jpg 1200w"
    sizes="(max-width: 400px) 400px,
           (max-width: 800px) 800px,
           1200px"
    alt="Description">

<!-- Différents formats selon le support du navigateur -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <source srcset="image.jpg" type="image/jpeg">
    <img src="image.jpg" alt="Description">
</picture>

<!-- Images différentes selon la taille d'écran -->
<picture>
    <source media="(min-width: 1200px)" srcset="large.jpg">
    <source media="(min-width: 768px)" srcset="medium.jpg">
    <img src="small.jpg" alt="Description">
</picture>
```

#### Formats d'images
```html
<!-- WebP (moderne, bonne compression) -->
<img src="image.webp" alt="Description">

<!-- AVIF (très moderne, meilleure compression) -->
<img src="image.avif" alt="Description">

<!-- Formats classiques -->
<img src="image.jpg" alt="Photo">      <!-- JPEG : photos -->
<img src="logo.png" alt="Logo">        <!-- PNG : logos, transparence -->
<img src="icon.svg" alt="Icône">       <!-- SVG : vectoriel -->
<img src="animation.gif" alt="Anim">   <!-- GIF : animation -->
```

#### Chargement paresseux (lazy loading)
```html
<!-- Ne charge l'image que quand elle entre dans le viewport -->
<img src="image.jpg" alt="Description" loading="lazy">

<!-- Charger immédiatement (défaut) -->
<img src="image.jpg" alt="Description" loading="eager">
```

#### Image map (zones cliquables)
```html
<img src="carte.jpg" alt="Carte" usemap="#carte">

<map name="carte">
    <area shape="rect" coords="0,0,100,100" href="zone1.html" alt="Zone 1">
    <area shape="circle" coords="200,200,50" href="zone2.html" alt="Zone 2">
    <area shape="poly" coords="300,0,400,100,300,200" href="zone3.html" alt="Zone 3">
</map>
```

### Audio
```html
<!-- Audio simple -->
<audio src="audio.mp3" controls></audio>

<!-- Audio avec plusieurs sources -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    Votre navigateur ne supporte pas l'audio.
</audio>

<!-- Attributs -->
<audio 
    src="audio.mp3" 
    controls          <!-- Affiche les contrôles -->
    autoplay          <!-- Lecture automatique (déconseillé) -->
    loop              <!-- Lecture en boucle -->
    muted             <!-- Muet par défaut -->
    preload="auto">   <!-- auto | metadata | none -->
</audio>
```

### Vidéo
```html
<!-- Vidéo simple -->
<video src="video.mp4" controls width="640" height="360"></video>

<!-- Vidéo avec plusieurs sources -->
<video controls width="640" height="360">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <source src="video.ogg" type="video/ogg">
    Votre navigateur ne supporte pas la vidéo.
</video>

<!-- Attributs -->
<video 
    src="video.mp4"
    controls          <!-- Contrôles -->
    autoplay          <!-- Lecture auto (avec muted) -->
    loop              <!-- Boucle -->
    muted             <!-- Muet -->
    poster="thumb.jpg" <!-- Image avant lecture -->
    preload="metadata" <!-- auto | metadata | none -->
    width="640"
    height="360">
</video>

<!-- Avec sous-titres -->
<video controls>
    <source src="video.mp4" type="video/mp4">
    <track 
        kind="subtitles" 
        src="subtitles-fr.vtt" 
        srclang="fr" 
        label="Français"
        default>
    <track 
        kind="subtitles" 
        src="subtitles-en.vtt" 
        srclang="en" 
        label="English">
</video>
```

### iFrame (contenu embarqué)
```html
<!-- Embarquer une page web -->
<iframe src="https://example.com" width="800" height="600"></iframe>

<!-- Vidéo YouTube -->
<iframe 
    width="560" 
    height="315" 
    src="https://www.youtube.com/embed/VIDEO_ID"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
</iframe>

<!-- Google Maps -->
<iframe
    src="https://www.google.com/maps/embed?pb=..."
    width="600"
    height="450"
    style="border:0;"
    allowfullscreen=""
    loading="lazy">
</iframe>

<!-- Avec sandbox (sécurité) -->
<iframe 
    src="page.html" 
    sandbox="allow-scripts allow-same-origin">
</iframe>
```

### SVG inline
```html
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
    <circle cx="50" cy="50" r="40" fill="blue" />
</svg>
```

---

## Tableaux

### Tableau de base
```html
<table>
    <tr>
        <th>Nom</th>
        <th>Âge</th>
        <th>Ville</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>30</td>
        <td>Paris</td>
    </tr>
    <tr>
        <td>Bob</td>
        <td>25</td>
        <td>Lyon</td>
    </tr>
</table>
```

### Tableau structuré (sémantique)
```html
<table>
    <caption>Liste des employés</caption>
    
    <thead>
        <tr>
            <th>Nom</th>
            <th>Prénom</th>
            <th>Poste</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <td>Dupont</td>
            <td>Alice</td>
            <td>Développeuse</td>
        </tr>
        <tr>
            <td>Martin</td>
            <td>Bob</td>
            <td>Designer</td>
        </tr>
    </tbody>
    
    <tfoot>
        <tr>
            <td colspan="3">Total : 2 employés</td>
        </tr>
    </tfoot>
</table>
```

### Fusion de cellules
```html
<table>
    <tr>
        <!-- Fusion horizontale (2 colonnes) -->
        <td colspan="2">Cellule fusionnée</td>
    </tr>
    <tr>
        <td>Colonne 1</td>
        <td>Colonne 2</td>
    </tr>
</table>

<table>
    <tr>
        <!-- Fusion verticale (2 lignes) -->
        <td rowspan="2">Cellule fusionnée</td>
        <td>Ligne 1</td>
    </tr>
    <tr>
        <td>Ligne 2</td>
    </tr>
</table>
```

### Groupement de colonnes
```html
<table>
    <colgroup>
        <col style="background-color: lightblue;">
        <col span="2" style="background-color: lightgreen;">
    </colgroup>
    <tr>
        <th>Nom</th>
        <th>Note 1</th>
        <th>Note 2</th>
    </tr>
    <tr>
        <td>Alice</td>
        <td>15</td>
        <td>18</td>
    </tr>
</table>
```

### Attributs de cellules
```html
<table>
    <tr>
        <!-- Portée de l'en-tête -->
        <th scope="col">Colonne</th>
        <th scope="row">Ligne</th>
        
        <!-- Headers associés -->
        <th id="nom">Nom</th>
    </tr>
    <tr>
        <td headers="nom">Alice</td>
    </tr>
</table>
```

---

## Formulaires

### Formulaire de base
```html
<form action="/submit" method="POST">
    <label for="nom">Nom :</label>
    <input type="text" id="nom" name="nom">
    
    <button type="submit">Envoyer</button>
</form>
```

### Attributs de formulaire
```html
<form 
    action="/submit"           <!-- URL de soumission -->
    method="POST"              <!-- GET ou POST -->
    enctype="multipart/form-data"  <!-- Pour upload de fichiers -->
    autocomplete="on"          <!-- on | off -->
    novalidate                 <!-- Désactive validation HTML5 -->
    target="_blank">           <!-- Où ouvrir la réponse -->
</form>
```

### Types d'input

#### Texte
```html
<!-- Texte simple -->
<input type="text" name="nom" placeholder="Votre nom">

<!-- Mot de passe -->
<input type="password" name="password">

<!-- Email -->
<input type="email" name="email" placeholder="vous@example.com">

<!-- URL -->
<input type="url" name="website" placeholder="https://example.com">

<!-- Téléphone -->
<input type="tel" name="phone" placeholder="+33 1 23 45 67 89">

<!-- Recherche -->
<input type="search" name="q" placeholder="Rechercher...">

<!-- Texte multiligne -->
<textarea name="message" rows="5" cols="40" placeholder="Votre message"></textarea>
```

#### Nombres et dates
```html
<!-- Nombre -->
<input type="number" name="age" min="0" max="120" step="1">

<!-- Range (slider) -->
<input type="range" name="volume" min="0" max="100" value="50">

<!-- Date -->
<input type="date" name="birthday" min="1900-01-01" max="2025-12-31">

<!-- Date et heure -->
<input type="datetime-local" name="meeting">

<!-- Mois -->
<input type="month" name="month">

<!-- Semaine -->
<input type="week" name="week">

<!-- Heure -->
<input type="time" name="time">
```

#### Sélection
```html
<!-- Checkbox -->
<input type="checkbox" id="accept" name="accept" value="yes">
<label for="accept">J'accepte les conditions</label>

<!-- Checkboxes multiples -->
<fieldset>
    <legend>Intérêts :</legend>
    <input type="checkbox" id="sport" name="interests" value="sport">
    <label for="sport">Sport</label>
    
    <input type="checkbox" id="music" name="interests" value="music">
    <label for="music">Musique</label>
</fieldset>

<!-- Radio buttons -->
<fieldset>
    <legend>Genre :</legend>
    <input type="radio" id="homme" name="genre" value="M">
    <label for="homme">Homme</label>
    
    <input type="radio" id="femme" name="genre" value="F">
    <label for="femme">Femme</label>
</fieldset>

<!-- Select (liste déroulante) -->
<select name="pays">
    <option value="">-- Choisir un pays --</option>
    <option value="fr">France</option>
    <option value="be">Belgique</option>
    <option value="ch">Suisse</option>
</select>

<!-- Select avec groupes -->
<select name="voiture">
    <optgroup label="Françaises">
        <option value="peugeot">Peugeot</option>
        <option value="renault">Renault</option>
    </optgroup>
    <optgroup label="Allemandes">
        <option value="bmw">BMW</option>
        <option value="mercedes">Mercedes</option>
    </optgroup>
</select>

<!-- Select multiple -->
<select name="langues" multiple size="4">
    <option value="fr">Français</option>
    <option value="en">Anglais</option>
    <option value="es">Espagnol</option>
    <option value="de">Allemand</option>
</select>
```

#### Fichiers et couleurs
```html
<!-- Upload de fichier -->
<input type="file" name="document">

<!-- Upload multiple -->
<input type="file" name="photos" multiple>

<!-- Accepter certains types -->
<input type="file" name="image" accept="image/*">
<input type="file" name="pdf" accept=".pdf">
<input type="file" name="docs" accept=".doc,.docx,.pdf">

<!-- Couleur -->
<input type="color" name="couleur" value="#ff0000">
```

#### Boutons
```html
<!-- Submit -->
<button type="submit">Envoyer</button>
<input type="submit" value="Envoyer">

<!-- Reset -->
<button type="reset">Réinitialiser</button>
<input type="reset" value="Réinitialiser">

<!-- Button normal -->
<button type="button" onclick="doSomething()">Cliquer</button>
<input type="button" value="Cliquer" onclick="doSomething()">

<!-- Image submit -->
<input type="image" src="submit.png" alt="Envoyer">
```

#### Hidden
```html
<!-- Champ caché -->
<input type="hidden" name="user_id" value="12345">
```

### Attributs des inputs

#### Validation
```html
<!-- Requis -->
<input type="text" name="nom" required>

<!-- Longueur min/max -->
<input type="text" name="username" minlength="3" maxlength="20">

<!-- Pattern (regex) -->
<input type="text" name="code" pattern="[0-9]{5}" title="5 chiffres">

<!-- Min/Max (nombres, dates) -->
<input type="number" name="age" min="18" max="100">

<!-- Email validation automatique -->
<input type="email" name="email" required>

<!-- URL validation automatique -->
<input type="url" name="website" required>
```

#### Autres attributs
```html
<!-- Valeur par défaut -->
<input type="text" name="nom" value="Dupont">

<!-- Placeholder -->
<input type="text" name="search" placeholder="Rechercher...">

<!-- Readonly (lecture seule) -->
<input type="text" name="readonly" value="Non modifiable" readonly>

<!-- Disabled (désactivé) -->
<input type="text" name="disabled" disabled>

<!-- Autofocus -->
<input type="text" name="nom" autofocus>

<!-- Autocomplete -->
<input type="email" name="email" autocomplete="email">
<input type="text" name="nom" autocomplete="name">
<input type="text" name="prenom" autocomplete="given-name">
<input type="text" name="adresse" autocomplete="street-address">

<!-- Size -->
<input type="text" name="code" size="10">

<!-- Multiple (email, file) -->
<input type="email" name="emails" multiple>
```

### Labels et groupement
```html
<!-- Label lié à un input -->
<label for="email">Email :</label>
<input type="email" id="email" name="email">

<!-- Label englobant -->
<label>
    Nom :
    <input type="text" name="nom">
</label>

<!-- Fieldset (groupe de champs) -->
<fieldset>
    <legend>Informations personnelles</legend>
    
    <label for="nom">Nom :</label>
    <input type="text" id="nom" name="nom">
    
    <label for="prenom">Prénom :</label>
    <input type="text" id="prenom" name="prenom">
</fieldset>

<!-- Disabled fieldset -->
<fieldset disabled>
    <legend>Section désactivée</legend>
    <!-- Tous les champs à l'intérieur sont désactivés -->
</fieldset>
```

### Datalist (autocomplétion)
```html
<label for="navigateur">Choisir un navigateur :</label>
<input type="text" id="navigateur" name="navigateur" list="navigateurs">

<datalist id="navigateurs">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Safari">
    <option value="Edge">
    <option value="Opera">
</datalist>
```

### Output
```html
<form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
    <input type="number" id="a" name="a" value="0"> +
    <input type="number" id="b" name="b" value="0"> =
    <output name="result" for="a b">0</output>
</form>
```

### Formulaire complet
```html
<form action="/register" method="POST" enctype="multipart/form-data">
    <fieldset>
        <legend>Inscription</legend>
        
        <!-- Nom -->
        <label for="nom">Nom * :</label>
        <input type="text" id="nom" name="nom" required minlength="2">
        
        <!-- Email -->
        <label for="email">Email * :</label>
        <input type="email" id="email" name="email" required>
        
        <!-- Mot de passe -->
        <label for="password">Mot de passe * :</label>
        <input type="password" id="password" name="password" 
               required minlength="8" 
               pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
               title="Au moins 8 caractères avec majuscule, minuscule et chiffre">
        
        <!-- Date de naissance -->
        <label for="birthday">Date de naissance :</label>
        <input type="date" id="birthday" name="birthday" 
               min="1900-01-01" max="2007-12-31">
        
        <!-- Genre -->
        <fieldset>
            <legend>Genre :</legend>
            <input type="radio" id="homme" name="genre" value="M">
            <label for="homme">Homme</label>
            
            <input type="radio" id="femme" name="genre" value="F">
            <label for="femme">Femme</label>
            
            <input type="radio" id="autre" name="genre" value="O">
            <label for="autre">Autre</label>
        </fieldset>
        
        <!-- Intérêts -->
        <fieldset>
            <legend>Intérêts :</legend>
            <input type="checkbox" id="sport" name="interests" value="sport">
            <label for="sport">Sport</label>
            
            <input type="checkbox" id="tech" name="interests" value="tech">
            <label for="tech">Technologie</label>
            
            <input type="checkbox" id="art" name="interests" value="art">
            <label for="art">Art</label>
        </fieldset>
        
        <!-- Pays -->
        <label for="pays">Pays :</label>
        <select id="pays" name="pays">
            <option value="">-- Choisir --</option>
            <option value="fr">France</option>
            <option value="be">Belgique</option>
            <option value="ch">Suisse</option>
        </select>
        
        <!-- Biographie -->
        <label for="bio">Biographie :</label>
        <textarea id="bio" name="bio" rows="5" cols="40" 
                  placeholder="Parlez-nous de vous..." maxlength="500"></textarea>
        
        <!-- Photo de profil -->
        <label for="photo">Photo de profil :</label>
        <input type="file" id="photo" name="photo" accept="image/*">
        
        <!-- CGU -->
        <input type="checkbox" id="cgu" name="cgu" required>
        <label for="cgu">J'accepte les conditions générales *</label>
        
        <!-- Boutons -->
        <button type="submit">S'inscrire</button>
        <button type="reset">Réinitialiser</button>
    </fieldset>
</form>
```

---

## Métadonnées

### Balises meta

#### Encodage et viewport
```html
<!-- Encodage des caractères (obligatoire) -->
<meta charset="UTF-8">

<!-- Viewport pour responsive design (obligatoire mobile) -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

#### SEO
```html
<!-- Description (affichée dans les résultats de recherche) -->
<meta name="description" content="Description de la page (150-160 caractères)">

<!-- Mots-clés (moins important aujourd'hui) -->
<meta name="keywords" content="html, tutoriel, développement web">

<!-- Auteur -->
<meta name="author" content="Alice Dupont">

<!-- Robots des moteurs de recherche -->
<meta name="robots" content="index, follow">
<meta name="robots" content="noindex, nofollow">
<meta name="googlebot" content="index, follow">
```

#### Open Graph (réseaux sociaux)
```html
<!-- Facebook, LinkedIn -->
<meta property="og:title" content="Titre de la page">
<meta property="og:description" content="Description de la page">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">
<meta property="og:site_name" content="Nom du site">
<meta property="og:locale" content="fr_FR">

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@username">
<meta name="twitter:title" content="Titre de la page">
<meta name="twitter:description" content="Description">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

#### Autres meta
```html
<!-- Rafraîchissement/redirection -->
<meta http-equiv="refresh" content="30">  <!-- Rafraîchir après 30s -->
<meta http-equiv="refresh" content="0;url=https://example.com">  <!-- Rediriger -->

<!-- Compatibilité IE -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">

<!-- Thème mobile -->
<meta name="theme-color" content="#007bff">

<!-- Application web -->
<meta name="application-name" content="Mon App">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
```

### Link

#### CSS
```html
<link rel="stylesheet" href="styles.css">
<link rel="stylesheet" href="print.css" media="print">
<link rel="stylesheet" href="mobile.css" media="(max-width: 768px)">
```

#### Favicon
```html
<link rel="icon" href="favicon.ico" type="image/x-icon">
<link rel="icon" href="favicon.png" type="image/png">
<link rel="icon" href="favicon.svg" type="image/svg+xml">

<!-- Différentes tailles -->
<link rel="icon" sizes="16x16" href="favicon-16.png">
<link rel="icon" sizes="32x32" href="favicon-32.png">
<link rel="icon" sizes="192x192" href="favicon-192.png">

<!-- Apple Touch Icon -->
<link rel="apple-touch-icon" href="apple-touch-icon.png">

<!-- Manifest (PWA) -->
<link rel="manifest" href="manifest.json">
```

#### Relations
```html
<!-- Page canonique (SEO) -->
<link rel="canonical" href="https://example.com/page">

<!-- Versions alternatives -->
<link rel="alternate" href="https://example.com/en/page" hreflang="en">
<link rel="alternate" href="https://example.com/es/page" hreflang="es">

<!-- RSS/Atom -->
<link rel="alternate" type="application/rss+xml" href="feed.rss" title="RSS Feed">

<!-- Préchargement -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="script.js" as="script">

<!-- Préconnexion -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="dns-prefetch" href="https://fonts.googleapis.com">

<!-- Prefetch -->
<link rel="prefetch" href="page2.html">

<!-- Auteur -->
<link rel="author" href="humans.txt">

<!-- Licence -->
<link rel="license" href="copyright.html">
```

### Base
```html
<!-- URL de base pour les liens relatifs -->
<base href="https://example.com/">
<base target="_blank">
```

### Title
```html
<!-- Titre de la page (obligatoire) -->
<title>Titre de la page - Nom du site</title>

<!-- Bonnes pratiques -->
<!-- - 50-60 caractères max pour le SEO -->
<!-- - Mettre le titre de la page avant le nom du site -->
<!-- - Éviter "Accueil" seul, être descriptif -->
```

### Script
```html
<!-- JavaScript externe -->
<script src="script.js"></script>

<!-- JavaScript inline -->
<script>
    console.log("Hello World");
</script>

<!-- Attributs -->
<script src="script.js" defer></script>      <!-- Exécuté après parsing HTML -->
<script src="script.js" async></script>      <!-- Exécuté dès disponible -->
<script type="module" src="module.js"></script>  <!-- Module ES6 -->
<script nomodule src="fallback.js"></script>  <!-- Pour navigateurs anciens -->

<!-- Type -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "WebSite",
    "name": "Mon Site"
}
</script>
```

### Style inline
```html
<!-- CSS dans le head (éviter si possible) -->
<style>
    body {
        font-family: Arial, sans-serif;
    }
</style>
```

---

## HTML5 moderne

### Balises HTML5 nouvelles

#### Canvas
```html
<!-- Dessiner avec JavaScript -->
<canvas id="myCanvas" width="800" height="600">
    Votre navigateur ne supporte pas canvas.
</canvas>

<script>
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = 'red';
    ctx.fillRect(10, 10, 100, 100);
</script>
```

#### Progress et Meter
```html
<!-- Barre de progression -->
<progress value="70" max="100">70%</progress>

<!-- Jauge -->
<meter value="0.6">60%</meter>
<meter value="70" min="0" max="100" low="30" high="80" optimum="100">70/100</meter>
```

#### Dialog
```html
<!-- Boîte de dialogue modale -->
<dialog id="myDialog">
    <h2>Titre de la boîte</h2>
    <p>Contenu de la boîte de dialogue</p>
    <button onclick="document.getElementById('myDialog').close()">Fermer</button>
</dialog>

<button onclick="document.getElementById('myDialog').showModal()">
    Ouvrir le dialog
</button>
```

#### Template
```html
<!-- Template HTML (non rendu par défaut) -->
<template id="myTemplate">
    <div class="item">
        <h3></h3>
        <p></p>
    </div>
</template>

<script>
    const template = document.getElementById('myTemplate');
    const clone = template.content.cloneNode(true);
    // Modifier et insérer le clone dans le DOM
</script>
```

#### Slot (Web Components)
```html
<template id="user-card">
    <div class="card">
        <slot name="username">Nom par défaut</slot>
        <slot name="email"></slot>
    </div>
</template>
```

### Attributs HTML5

#### Data attributes
```html
<!-- Stocker des données personnalisées -->
<div 
    data-user-id="12345" 
    data-role="admin" 
    data-created-at="2025-10-18">
    Contenu
</div>

<script>
    const element = document.querySelector('div');
    console.log(element.dataset.userId);     // "12345"
    console.log(element.dataset.role);       // "admin"
    console.log(element.dataset.createdAt);  // "2025-10-18"
</script>
```

#### Contenteditable
```html
<!-- Rendre le contenu éditable -->
<div contenteditable="true">
    Ce texte peut être modifié par l'utilisateur
</div>

<p contenteditable="false">Non éditable</p>
```

#### Draggable
```html
<!-- Élément déplaçable par drag & drop -->
<div draggable="true" ondragstart="drag(event)">
    Déplacez-moi
</div>

<div ondrop="drop(event)" ondragover="allowDrop(event)">
    Zone de dépôt
</div>
```

#### Spellcheck
```html
<!-- Vérification orthographique -->
<textarea spellcheck="true">Texte avec vérification</textarea>
<input type="text" spellcheck="false">
```

#### Translate
```html
<!-- Indiquer si le contenu doit être traduit -->
<p translate="no">Brand Name</p>
<p translate="yes">Texte traduisible</p>
```

#### Hidden
```html
<!-- Cacher un élément -->
<div hidden>Contenu caché</div>
```

#### Inputmode
```html
<!-- Type de clavier sur mobile -->
<input type="text" inputmode="numeric">    <!-- Clavier numérique -->
<input type="text" inputmode="tel">        <!-- Clavier téléphone -->
<input type="text" inputmode="email">      <!-- Clavier email -->
<input type="text" inputmode="url">        <!-- Clavier URL -->
<input type="text" inputmode="search">     <!-- Clavier recherche -->
```

---

## Attributs globaux

### Attributs communs à toutes les balises

#### id et class
```html
<!-- ID unique -->
<div id="header">...</div>

<!-- Classes multiples -->
<div class="container centered large">...</div>
```

#### title
```html
<!-- Info-bulle au survol -->
<abbr title="HyperText Markup Language">HTML</abbr>
<button title="Cliquez pour envoyer">Envoyer</button>
```

#### lang
```html
<!-- Langue du contenu -->
<html lang="fr">
<p lang="en">This is in English</p>
<blockquote lang="es">Esto está en español</blockquote>
```

#### dir
```html
<!-- Direction du texte -->
<p dir="ltr">Texte de gauche à droite</p>
<p dir="rtl">نص من اليمين إلى اليسار</p>
<p dir="auto">Direction automatique</p>
```

#### tabindex
```html
<!-- Ordre de navigation au clavier -->
<button tabindex="1">Premier</button>
<button tabindex="2">Deuxième</button>
<button tabindex="3">Troisième</button>

<!-- Rendre focusable -->
<div tabindex="0">Peut recevoir le focus</div>

<!-- Retirer du flux de tabulation -->
<button tabindex="-1">Non accessible par Tab</button>
```

#### accesskey
```html
<!-- Raccourci clavier -->
<button accesskey="s">Enregistrer (Alt+S)</button>
<a href="#content" accesskey="c">Contenu (Alt+C)</a>
```

#### ARIA (Accessible Rich Internet Applications)
```html
<!-- Rôles -->
<div role="navigation">...</div>
<button role="button">...</button>

<!-- Labels -->
<button aria-label="Fermer">X</button>
<div aria-labelledby="title">
    <h2 id="title">Titre</h2>
</div>

<!-- Description -->
<button aria-describedby="help">?</button>
<div id="help">Texte d'aide</div>

<!-- États -->
<button aria-pressed="true">Activé</button>
<button aria-expanded="false">Replié</button>
<div aria-hidden="true">Caché aux lecteurs d'écran</div>
<input aria-required="true">
<div aria-live="polite">Contenu qui se met à jour</div>

<!-- Propriétés -->
<input aria-invalid="true">
<div aria-busy="true">Chargement...</div>
<button aria-disabled="true">Désactivé</button>
```

---

## Bonnes pratiques

### Structure du document

#### ✅ À faire
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Description pertinente">
    <title>Titre descriptif - Nom du site</title>
    
    <!-- CSS en premier -->
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Structure sémantique claire -->
    <header>
        <nav>...</nav>
    </header>
    
    <main>
        <article>...</article>
    </main>
    
    <footer>...</footer>
    
    <!-- JavaScript en fin de body -->
    <script src="script.js"></script>
</body>
</html>
```

#### ❌ À éviter
```html
<!-- Pas de DOCTYPE -->
<html>
<head>
    <!-- Pas de charset -->
    <!-- Pas de viewport -->
    <title>Accueil</title>  <!-- Titre non descriptif -->
</head>
<body>
    <!-- Divitis - abus de div -->
    <div id="header">
        <div id="nav">
            <div class="item">...</div>
        </div>
    </div>
    
    <!-- Pas de structure sémantique -->
</body>
</html>
```

### Validation HTML

#### Utiliser le validateur W3C
- https://validator.w3.org/
- Vérifier régulièrement votre HTML
- Corriger les erreurs et avertissements

#### Erreurs courantes à éviter
```html
<!-- ❌ Balises non fermées -->
<p>Paragraphe
<div>

<!-- ✅ Bien fermer -->
<p>Paragraphe</p>
<div></div>

<!-- ❌ Attributs sans guillemets -->
<img src=image.jpg alt=Description>

<!-- ✅ Toujours avec guillemets -->
<img src="image.jpg" alt="Description">

<!-- ❌ Imbrication incorrecte -->
<p><div>Texte</div></p>

<!-- ✅ Block dans block, inline dans inline -->
<div><p>Texte</p></div>

<!-- ❌ Attributs en double -->
<img src="image.jpg" alt="Image" alt="Photo">

<!-- ✅ Un seul attribut -->
<img src="image.jpg" alt="Image">
```

### SEO et Performance

#### Optimisation des images
```html
<!-- ✅ Toujours un alt -->
<img src="photo.jpg" alt="Description précise de la photo">

<!-- ✅ Dimensions pour éviter le layout shift -->
<img src="photo.jpg" alt="Photo" width="800" height="600">

<!-- ✅ Lazy loading pour images hors viewport -->
<img src="photo.jpg" alt="Photo" loading="lazy">

<!-- ✅ Formats modernes avec fallback -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="Photo">
</picture>
```

#### Structure des titres
```html
<!-- ✅ Hiérarchie logique -->
<h1>Titre principal</h1>
<h2>Section 1</h2>
<h3>Sous-section 1.1</h3>
<h3>Sous-section 1.2</h3>
<h2>Section 2</h2>

<!-- ❌ Niveaux sautés -->
<h1>Titre</h1>
<h3>Sous-titre</h3>  <!-- On a sauté h2 -->
```

#### Liens et navigation
```html
<!-- ✅ Textes de liens descriptifs -->
<a href="/article">Lire l'article sur le HTML sémantique</a>

<!-- ❌ Textes génériques -->
<a href="/article">Cliquez ici</a>
<a href="/article">En savoir plus</a>

<!-- ✅ Links externes sécurisés -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    Lien externe
</a>
```

### Sémantique et lisibilité

#### Choisir la bonne balise
```html
<!-- ✅ Utiliser les balises sémantiques appropriées -->
<nav>           <!-- Navigation -->
<article>       <!-- Contenu autonome -->
<section>       <!-- Section thématique -->
<aside>         <!-- Contenu tangentiel -->
<header>        <!-- En-tête -->
<footer>        <!-- Pied de page -->
<main>          <!-- Contenu principal -->

<!-- ❌ Divitis (abus de div) -->
<div class="navigation">
<div class="content">
<div class="sidebar">

<!-- ✅ Strong vs b, em vs i -->
<strong>Important</strong>  <!-- Importance sémantique -->
<b>Gras visuel</b>         <!-- Style sans importance -->

<em>Emphase</em>           <!-- Emphase sémantique -->
<i>Italique</i>            <!-- Style sans emphase -->
```

#### Commentaires utiles
```html
<!-- ✅ Commentaires explicatifs -->
<!-- Section des articles de blog -->
<section class="blog-posts">
    ...
</section>

<!-- Fin de la section des articles -->

<!-- ❌ Commentaires inutiles ou obsolètes -->
<!-- <div>Code commenté non supprimé</div> -->
<!-- TODO: à faire depuis 2020 -->
```

### Formulaires

#### Labels obligatoires
```html
<!-- ✅ Toujours lier label et input -->
<label for="email">Email :</label>
<input type="email" id="email" name="email">

<!-- ❌ Input sans label -->
<input type="email" name="email" placeholder="Email">

<!-- ✅ Alternative : label englobant -->
<label>
    Email :
    <input type="email" name="email">
</label>
```

#### Validation côté client
```html
<!-- ✅ Utiliser les attributs de validation -->
<input type="email" name="email" required>
<input type="text" name="phone" pattern="[0-9]{10}" title="10 chiffres">
<input type="number" name="age" min="18" max="100">

<!-- Mais toujours valider côté serveur aussi! -->
```

### Organisation du code

#### Indentation et lisibilité
```html
<!-- ✅ Code indenté et organisé -->
<article>
    <header>
        <h2>Titre</h2>
        <p>Metadata</p>
    </header>
    <p>Contenu</p>
</article>

<!-- ❌ Code non indenté -->
<article>
<header>
<h2>Titre</h2>
</header>
</article>
```

#### Minuscules pour les balises
```html
<!-- ✅ Minuscules (HTML5) -->
<div class="container">
    <p>Texte</p>
</div>

<!-- ❌ Majuscules (ancien XHTML) -->
<DIV CLASS="container">
    <P>Texte</P>
</DIV>
```

### Compatibilité

#### Préfixes et fallbacks
```html
<!-- ✅ Toujours prévoir un fallback -->
<video controls>
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    Votre navigateur ne supporte pas la vidéo HTML5.
    <a href="video.mp4">Télécharger la vidéo</a>
</video>
```

#### Tester sur plusieurs navigateurs
- Chrome/Edge (Chromium)
- Firefox
- Safari
- Version mobile de chaque

---

## Ressources

### Documentation officielle
- [MDN Web Docs (Mozilla)](https://developer.mozilla.org/fr/docs/Web/HTML)
- [W3C HTML Specification](https://html.spec.whatwg.org/)
- [Can I Use](https://caniuse.com/) - Compatibilité des fonctionnalités

### Outils de validation
- [W3C Validator](https://validator.w3.org/)
- [HTML5 Validator](https://html5.validator.nu/)

### Outils SEO
- [Google Search Console](https://search.google.com/search-console)
- [Lighthouse (Chrome DevTools)](https://developers.google.com/web/tools/lighthouse)

### Guides et tutoriels
- [HTML.com](https://html.com/)
- [W3Schools](https://www.w3schools.com/html/)
- [HTML Reference](https://htmlreference.io/)

---

## Checklist HTML

### Avant de publier

- [ ] DOCTYPE déclaré
- [ ] Langue définie (`<html lang="fr">`)
- [ ] Charset UTF-8
- [ ] Viewport pour responsive
- [ ] Titre descriptif (<60 caractères)
- [ ] Meta description (150-160 caractères)
- [ ] Favicon présent
- [ ] Structure sémantique (header, main, footer, etc.)
- [ ] Un seul `<h1>` par page
- [ ] Hiérarchie des titres logique
- [ ] Toutes les images ont un `alt`
- [ ] Tous les formulaires ont des labels
- [ ] Liens externes avec `rel="noopener noreferrer"`
- [ ] HTML validé (W3C Validator)
- [ ] Testé sur différents navigateurs
- [ ] Performance testée (Lighthouse)
- [ ] Open Graph pour réseaux sociaux

---
