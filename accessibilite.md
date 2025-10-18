# Accessibilité CheatSheet

## Table des matières
- [Introduction à l'accessibilité](#introduction-à-laccessibilité)
- [Sémantique HTML](#sémantique-html)
- [Attributs ARIA](#attributs-aria)
- [Formulaires accessibles](#formulaires-accessibles)
- [Navigation et structure](#navigation-et-structure)
- [Images et médias](#images-et-médias)
- [Liens et boutons](#liens-et-boutons)
- [Tableaux accessibles](#tableaux-accessibles)
- [Navigation au clavier](#navigation-au-clavier)
- [Focus management](#focus-management)
- [Contenu dynamique](#contenu-dynamique)
- [Couleurs et contrastes](#couleurs-et-contrastes)
- [Bonnes pratiques](#bonnes-pratiques)
- [Outils et tests](#outils-et-tests)

---

## Introduction à l'accessibilité

### Qu'est-ce que l'accessibilité web ?
L'accessibilité web garantit que les sites et applications sont utilisables par tous, y compris les personnes en situation de handicap (visuel, auditif, moteur, cognitif).

### WCAG (Web Content Accessibility Guidelines)

#### Niveaux de conformité
- **A** : Niveau minimum (essentiel)
- **AA** : Niveau recommandé (standard légal dans beaucoup de pays)
- **AAA** : Niveau optimal (le plus élevé)

#### Principes POUR
1. **Perceptible** : L'information doit être perceptible
2. **Opérable** : L'interface doit être utilisable
3. **Compréhensible** : L'information doit être compréhensible
4. **Robuste** : Le contenu doit être interprétable par tous les agents utilisateurs

---

## Sémantique HTML

### Structure de document

#### ✅ Bon (sémantique)
```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Titre descriptif de la page</title>
</head>
<body>
  <header>
    <nav aria-label="Navigation principale">
      <!-- Navigation -->
    </nav>
  </header>
  
  <main>
    <article>
      <h1>Titre principal</h1>
      <section>
        <h2>Sous-titre</h2>
        <p>Contenu...</p>
      </section>
    </article>
    
    <aside>
      <h2>Informations complémentaires</h2>
      <!-- Contenu secondaire -->
    </aside>
  </main>
  
  <footer>
    <p>© 2025 - Informations de pied de page</p>
  </footer>
</body>
</html>
```

#### ❌ Mauvais (non sémantique)
```html
<div class="header">
  <div class="nav">
    <!-- Navigation -->
  </div>
</div>

<div class="content">
  <div class="title">Titre</div>
  <div class="text">Contenu...</div>
</div>

<div class="footer">
  <div>© 2025</div>
</div>
```

### Hiérarchie des titres

#### ✅ Bon
```html
<h1>Titre principal de la page</h1>
  <h2>Section 1</h2>
    <h3>Sous-section 1.1</h3>
    <h3>Sous-section 1.2</h3>
  <h2>Section 2</h2>
    <h3>Sous-section 2.1</h3>
```

#### ❌ Mauvais
```html
<h1>Titre principal</h1>
<h3>Sauter un niveau</h3>
<h2>Ordre incorrect</h2>
<h1>Plusieurs h1</h1>
```

**Règles :**
- Un seul `<h1>` par page
- Ne pas sauter de niveaux (h1 → h3)
- Respecter l'ordre hiérarchique
- Ne pas utiliser les titres pour le style (utiliser CSS)

### Balises sémantiques

```html
<!-- Structure -->
<header>      <!-- En-tête de page ou section -->
<nav>         <!-- Navigation -->
<main>        <!-- Contenu principal (un seul par page) -->
<article>     <!-- Contenu autonome et réutilisable -->
<section>     <!-- Section thématique -->
<aside>       <!-- Contenu complémentaire -->
<footer>      <!-- Pied de page ou section -->

<!-- Contenu -->
<figure>      <!-- Contenu illustratif -->
  <img src="image.jpg" alt="Description">
  <figcaption>Légende de l'image</figcaption>
</figure>

<time datetime="2025-10-18">18 octobre 2025</time>
<mark>Texte surligné</mark>
<abbr title="HyperText Markup Language">HTML</abbr>
<address>     <!-- Coordonnées de contact -->
  <a href="mailto:contact@example.com">Contact</a>
</address>
```

---

## Attributs ARIA

### Qu'est-ce qu'ARIA ?
**ARIA** (Accessible Rich Internet Applications) améliore l'accessibilité des contenus dynamiques et des composants complexes.

### Règles d'utilisation ARIA

1. **Utiliser HTML sémantique d'abord** : Ne pas utiliser ARIA si HTML suffit
2. **Ne pas changer la sémantique native** : Ne pas ajouter ARIA inutilement
3. **Tous les contrôles interactifs doivent être utilisables au clavier**
4. **Ne pas utiliser `role="presentation"` ou `aria-hidden="true"` sur des éléments focusables**
5. **Tous les éléments interactifs doivent avoir un nom accessible**

### Rôles ARIA principaux

```html
<!-- Landmarks (régions) -->
<div role="banner">        <!-- Équivalent à <header> -->
<div role="navigation">    <!-- Équivalent à <nav> -->
<div role="main">          <!-- Équivalent à <main> -->
<div role="complementary"> <!-- Équivalent à <aside> -->
<div role="contentinfo">   <!-- Équivalent à <footer> -->
<div role="search">        <!-- Formulaire de recherche -->
<div role="form">          <!-- Formulaire -->

<!-- Widgets -->
<div role="button">        <!-- Bouton personnalisé -->
<div role="checkbox">      <!-- Case à cocher -->
<div role="dialog">        <!-- Boîte de dialogue -->
<div role="alert">         <!-- Message d'alerte -->
<div role="alertdialog">   <!-- Dialogue d'alerte -->
<div role="menu">          <!-- Menu déroulant -->
<div role="menuitem">      <!-- Élément de menu -->
<div role="tab">           <!-- Onglet -->
<div role="tabpanel">      <!-- Panneau d'onglet -->
<div role="tooltip">       <!-- Info-bulle -->

<!-- Structure -->
<div role="list">          <!-- Liste -->
<div role="listitem">      <!-- Élément de liste -->
<div role="article">       <!-- Article -->
<div role="region">        <!-- Région générique -->
```

### États et propriétés ARIA

#### États (dynamiques)
```html
<!-- État d'expansion -->
<button aria-expanded="false">Menu</button>
<button aria-expanded="true">Menu</button>

<!-- État de sélection -->
<div role="tab" aria-selected="true">Onglet actif</div>
<div role="tab" aria-selected="false">Onglet inactif</div>

<!-- État d'activation -->
<button aria-pressed="true">Activé</button>
<button aria-pressed="false">Désactivé</button>

<!-- État de visibilité -->
<div aria-hidden="true">Caché aux lecteurs d'écran</div>
<div aria-hidden="false">Visible</div>

<!-- État de désactivation -->
<button aria-disabled="true">Désactivé</button>

<!-- État occupé -->
<div aria-busy="true">Chargement...</div>

<!-- État de vérification -->
<div role="checkbox" aria-checked="true">Coché</div>
<div role="checkbox" aria-checked="false">Non coché</div>
<div role="checkbox" aria-checked="mixed">Partiellement coché</div>

<!-- État invalide -->
<input aria-invalid="true">
<input aria-invalid="false">
```

#### Propriétés (statiques)
```html
<!-- Label -->
<button aria-label="Fermer la fenêtre">×</button>

<!-- Labelled by -->
<h2 id="dialog-title">Confirmer l'action</h2>
<div role="dialog" aria-labelledby="dialog-title">
  <!-- Contenu -->
</div>

<!-- Described by -->
<input 
  type="email" 
  aria-describedby="email-hint"
  required
>
<span id="email-hint">Format : exemple@domaine.com</span>

<!-- Live regions -->
<div aria-live="polite">Message mis à jour</div>
<div aria-live="assertive">Alerte urgente</div>
<div aria-live="off">Pas d'annonce</div>

<!-- Atomic -->
<div aria-live="polite" aria-atomic="true">
  <!-- Tout le contenu est annoncé ensemble -->
</div>

<!-- Relevant -->
<div aria-live="polite" aria-relevant="additions">
  <!-- Annonce seulement les ajouts -->
</div>
<div aria-live="polite" aria-relevant="removals">
  <!-- Annonce seulement les suppressions -->
</div>
<div aria-live="polite" aria-relevant="text">
  <!-- Annonce les changements de texte -->
</div>
<div aria-live="polite" aria-relevant="all">
  <!-- Annonce tous les changements -->
</div>

<!-- Contrôles -->
<button aria-controls="panel-1">Afficher le panneau</button>
<div id="panel-1">Contenu du panneau</div>

<!-- Requis -->
<input aria-required="true">

<!-- Plage de valeurs -->
<div 
  role="slider" 
  aria-valuemin="0" 
  aria-valuemax="100" 
  aria-valuenow="50"
  aria-valuetext="50 pourcent"
>
</div>

<!-- Niveau hiérarchique -->
<div role="heading" aria-level="2">Titre niveau 2</div>

<!-- Autocomplete -->
<input 
  type="text" 
  aria-autocomplete="list"
  aria-controls="suggestions"
>

<!-- Modal -->
<div role="dialog" aria-modal="true">
  <!-- Contenu du modal -->
</div>
```

### Exemples de composants avec ARIA

#### Accordéon
```html
<div class="accordion">
  <h3>
    <button 
      id="accordion-btn-1"
      aria-expanded="false" 
      aria-controls="accordion-panel-1"
    >
      Section 1
    </button>
  </h3>
  <div 
    id="accordion-panel-1" 
    role="region"
    aria-labelledby="accordion-btn-1"
    hidden
  >
    <p>Contenu de la section 1</p>
  </div>
</div>
```

#### Onglets
```html
<div class="tabs">
  <div role="tablist" aria-label="Sections">
    <button 
      role="tab" 
      aria-selected="true" 
      aria-controls="panel-1"
      id="tab-1"
    >
      Onglet 1
    </button>
    <button 
      role="tab" 
      aria-selected="false" 
      aria-controls="panel-2"
      id="tab-2"
      tabindex="-1"
    >
      Onglet 2
    </button>
  </div>
  
  <div role="tabpanel" id="panel-1" aria-labelledby="tab-1">
    <p>Contenu de l'onglet 1</p>
  </div>
  <div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>
    <p>Contenu de l'onglet 2</p>
  </div>
</div>
```

#### Modal / Dialog
```html
<div 
  role="dialog" 
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
>
  <h2 id="dialog-title">Titre du dialogue</h2>
  <p id="dialog-desc">Description du dialogue</p>
  
  <button aria-label="Fermer le dialogue">×</button>
  
  <div>
    <!-- Contenu -->
  </div>
  
  <div>
    <button>Annuler</button>
    <button>Confirmer</button>
  </div>
</div>
```

#### Menu déroulant
```html
<div class="menu-container">
  <button 
    aria-haspopup="true" 
    aria-expanded="false"
    aria-controls="menu-1"
    id="menu-button"
  >
    Options
  </button>
  
  <ul 
    role="menu" 
    id="menu-1"
    aria-labelledby="menu-button"
    hidden
  >
    <li role="none">
      <button role="menuitem">Option 1</button>
    </li>
    <li role="none">
      <button role="menuitem">Option 2</button>
    </li>
    <li role="none">
      <button role="menuitem">Option 3</button>
    </li>
  </ul>
</div>
```

---

## Formulaires accessibles

### Labels et inputs

#### ✅ Bon
```html
<!-- Méthode 1 : Label avec for -->
<label for="prenom">Prénom</label>
<input type="text" id="prenom" name="prenom" required>

<!-- Méthode 2 : Label englobant -->
<label>
  Nom
  <input type="text" name="nom" required>
</label>

<!-- Avec description -->
<label for="email">Email</label>
<input 
  type="email" 
  id="email" 
  name="email" 
  aria-describedby="email-hint"
  required
>
<span id="email-hint">Format : exemple@domaine.com</span>

<!-- Avec message d'erreur -->
<label for="telephone">Téléphone</label>
<input 
  type="tel" 
  id="telephone" 
  name="telephone" 
  aria-describedby="tel-error"
  aria-invalid="true"
  required
>
<span id="tel-error" role="alert">
  Le numéro de téléphone est invalide
</span>
```

#### ❌ Mauvais
```html
<!-- Pas de label -->
<input type="text" placeholder="Prénom">

<!-- Placeholder comme label -->
<input type="text" placeholder="Entrez votre nom">

<!-- Label non associé -->
<label>Nom</label>
<input type="text" name="nom">
```

### Groupes de champs

```html
<fieldset>
  <legend>Informations personnelles</legend>
  
  <label for="nom">Nom</label>
  <input type="text" id="nom" name="nom" required>
  
  <label for="prenom">Prénom</label>
  <input type="text" id="prenom" name="prenom" required>
</fieldset>

<fieldset>
  <legend>Civilité</legend>
  
  <label>
    <input type="radio" name="civilite" value="mr" required>
    Monsieur
  </label>
  
  <label>
    <input type="radio" name="civilite" value="mme" required>
    Madame
  </label>
</fieldset>
```

### Cases à cocher et boutons radio

```html
<!-- Cases à cocher -->
<fieldset>
  <legend>Centres d'intérêt</legend>
  
  <label>
    <input type="checkbox" name="interets" value="sport">
    Sport
  </label>
  
  <label>
    <input type="checkbox" name="interets" value="musique">
    Musique
  </label>
  
  <label>
    <input type="checkbox" name="interets" value="lecture">
    Lecture
  </label>
</fieldset>

<!-- Boutons radio -->
<fieldset>
  <legend>Abonnement à la newsletter</legend>
  
  <label>
    <input type="radio" name="newsletter" value="oui" required>
    Oui
  </label>
  
  <label>
    <input type="radio" name="newsletter" value="non" required>
    Non
  </label>
</fieldset>
```

### Select et autocomplete

```html
<!-- Select simple -->
<label for="pays">Pays</label>
<select id="pays" name="pays" required>
  <option value="">-- Choisir un pays --</option>
  <option value="fr">France</option>
  <option value="be">Belgique</option>
  <option value="ch">Suisse</option>
</select>

<!-- Select avec groupes -->
<label for="categorie">Catégorie</label>
<select id="categorie" name="categorie">
  <option value="">-- Choisir --</option>
  <optgroup label="Fruits">
    <option value="pomme">Pomme</option>
    <option value="banane">Banane</option>
  </optgroup>
  <optgroup label="Légumes">
    <option value="carotte">Carotte</option>
    <option value="tomate">Tomate</option>
  </optgroup>
</select>

<!-- Autocomplete natif -->
<label for="nom">Nom</label>
<input 
  type="text" 
  id="nom" 
  name="nom" 
  autocomplete="family-name"
>

<label for="email">Email</label>
<input 
  type="email" 
  id="email" 
  name="email" 
  autocomplete="email"
>

<!-- Liste de suggestions -->
<label for="ville">Ville</label>
<input 
  type="text" 
  id="ville" 
  name="ville"
  list="villes"
  autocomplete="off"
>
<datalist id="villes">
  <option value="Paris">
  <option value="Lyon">
  <option value="Marseille">
</datalist>
```

### Messages d'erreur

```html
<!-- Erreur au niveau du champ -->
<div>
  <label for="email">Email</label>
  <input 
    type="email" 
    id="email" 
    name="email" 
    aria-describedby="email-error"
    aria-invalid="true"
    required
  >
  <span id="email-error" role="alert" class="error">
    ⚠️ Veuillez entrer une adresse email valide
  </span>
</div>

<!-- Erreur globale au formulaire -->
<form aria-labelledby="form-title">
  <h2 id="form-title">Inscription</h2>
  
  <!-- Message d'erreur global -->
  <div role="alert" aria-live="polite">
    <h3>Erreurs dans le formulaire :</h3>
    <ul>
      <li><a href="#email">L'email est requis</a></li>
      <li><a href="#password">Le mot de passe doit contenir 8 caractères</a></li>
    </ul>
  </div>
  
  <!-- Champs du formulaire -->
</form>
```

### Champs requis

```html
<!-- Méthode 1 : Attribut required -->
<label for="nom">Nom (requis)</label>
<input type="text" id="nom" name="nom" required>

<!-- Méthode 2 : ARIA -->
<label for="prenom">Prénom <span aria-label="requis">*</span></label>
<input type="text" id="prenom" name="prenom" aria-required="true">

<!-- Légende pour les champs requis -->
<p>Les champs marqués d'un <span aria-label="requis">*</span> sont obligatoires</p>
```

---

## Navigation et structure

### Skip links (liens d'évitement)

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <style>
    .skip-link {
      position: absolute;
      top: -40px;
      left: 0;
      background: #000;
      color: white;
      padding: 8px;
      z-index: 100;
    }
    .skip-link:focus {
      top: 0;
    }
  </style>
</head>
<body>
  <a href="#main-content" class="skip-link">
    Aller au contenu principal
  </a>
  <a href="#navigation" class="skip-link">
    Aller à la navigation
  </a>
  
  <nav id="navigation">
    <!-- Navigation -->
  </nav>
  
  <main id="main-content">
    <!-- Contenu principal -->
  </main>
</body>
</html>
```

### Navigation principale

```html
<nav aria-label="Navigation principale">
  <ul>
    <li><a href="/" aria-current="page">Accueil</a></li>
    <li><a href="/services">Services</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>

<!-- Si plusieurs navigations -->
<nav aria-label="Navigation principale">
  <!-- Menu principal -->
</nav>

<nav aria-label="Navigation secondaire">
  <!-- Menu secondaire -->
</nav>

<nav aria-label="Fil d'Ariane">
  <ol>
    <li><a href="/">Accueil</a></li>
    <li><a href="/blog">Blog</a></li>
    <li aria-current="page">Article</li>
  </ol>
</nav>
```

### Pagination

```html
<nav aria-label="Pagination">
  <ul>
    <li>
      <a href="?page=1" aria-label="Page précédente">
        ← Précédent
      </a>
    </li>
    <li>
      <a href="?page=1">1</a>
    </li>
    <li>
      <a href="?page=2" aria-current="page">2</a>
    </li>
    <li>
      <a href="?page=3">3</a>
    </li>
    <li>
      <a href="?page=3" aria-label="Page suivante">
        Suivant →
      </a>
    </li>
  </ul>
</nav>
```

---

## Images et médias

### Images

#### ✅ Bon
```html
<!-- Image informative -->
<img src="chat.jpg" alt="Chat roux dormant sur un coussin">

<!-- Image décorative -->
<img src="decoration.png" alt="" role="presentation">
<!-- OU -->
<img src="decoration.png" alt="">

<!-- Image avec texte adjacent -->
<figure>
  <img src="graphique.png" alt="Graphique montrant une augmentation de 50%">
  <figcaption>
    Évolution des ventes de 2020 à 2025
  </figcaption>
</figure>

<!-- Image complexe avec description longue -->
<img 
  src="infographie.png" 
  alt="Infographie sur le changement climatique"
  aria-describedby="infographie-desc"
>
<div id="infographie-desc">
  <h3>Description détaillée</h3>
  <p>L'infographie montre que...</p>
</div>

<!-- Image cliquable -->
<a href="/produit">
  <img src="produit.jpg" alt="Voir les détails du produit X">
</a>

<!-- Logo -->
<img src="logo.png" alt="Nom de l'entreprise - Retour à l'accueil">
```

#### ❌ Mauvais
```html
<!-- Pas de alt -->
<img src="photo.jpg">

<!-- Alt non descriptif -->
<img src="chat.jpg" alt="image">
<img src="produit.jpg" alt="photo.jpg">

<!-- Alt redondant avec le texte adjacent -->
<img src="chat.jpg" alt="Chat">
<p>Chat</p>
```

### Images SVG

```html
<!-- SVG décoratif -->
<svg aria-hidden="true" focusable="false">
  <!-- Contenu SVG -->
</svg>

<!-- SVG informatif -->
<svg role="img" aria-labelledby="svg-title">
  <title id="svg-title">Description de l'image</title>
  <!-- Contenu SVG -->
</svg>

<!-- Icône SVG dans un bouton -->
<button>
  <svg aria-hidden="true" focusable="false">
    <use href="#icon-search"></use>
  </svg>
  <span>Rechercher</span>
</button>

<!-- Si l'icône est seule -->
<button aria-label="Rechercher">
  <svg aria-hidden="true" focusable="false">
    <use href="#icon-search"></use>
  </svg>
</button>
```

### Vidéos

```html
<!-- Vidéo avec sous-titres -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track 
    kind="subtitles" 
    src="subtitles-fr.vtt" 
    srclang="fr" 
    label="Français"
    default
  >
  <track 
    kind="subtitles" 
    src="subtitles-en.vtt" 
    srclang="en" 
    label="English"
  >
  <track 
    kind="descriptions" 
    src="descriptions.vtt" 
    srclang="fr" 
    label="Audiodescription"
  >
  <p>Votre navigateur ne supporte pas la vidéo HTML5.</p>
</video>

<!-- Transcription textuelle -->
<video controls aria-describedby="video-transcript">
  <source src="video.mp4" type="video/mp4">
</video>
<details id="video-transcript">
  <summary>Transcription de la vidéo</summary>
  <p>Texte complet de la vidéo...</p>
</details>
```

### Audio

```html
<audio controls>
  <source src="podcast.mp3" type="audio/mp3">
  <source src="podcast.ogg" type="audio/ogg">
  <track 
    kind="captions" 
    src="captions.vtt" 
    srclang="fr" 
    label="Français"
  >
  <p>Votre navigateur ne supporte pas l'élément audio.</p>
</audio>

<!-- Avec transcription -->
<audio controls aria-describedby="audio-transcript">
  <source src="interview.mp3" type="audio/mp3">
</audio>
<details id="audio-transcript">
  <summary>Transcription de l'interview</summary>
  <p>Texte complet...</p>
</details>
```

### Iframe

```html
<!-- Iframe avec titre descriptif -->
<iframe 
  src="https://www.youtube.com/embed/..." 
  title="Tutoriel CSS Grid - Partie 1"
  allowfullscreen
>
</iframe>

<!-- Carte Google Maps -->
<iframe 
  src="https://maps.google.com/..." 
  title="Localisation de notre bureau à Paris"
  loading="lazy"
>
</iframe>
```

---

## Liens et boutons

### Liens

#### ✅ Bon
```html
<!-- Lien descriptif -->
<a href="/article">Lire l'article complet sur l'accessibilité</a>

<!-- Lien avec contexte -->
<article>
  <h2>Titre de l'article</h2>
  <p>Résumé...</p>
  <a href="/article">Lire la suite</a>
</article>

<!-- Lien externe -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  Visiter le site externe
  <span class="visually-hidden">(s'ouvre dans un nouvel onglet)</span>
</a>

<!-- Téléchargement -->
<a href="/document.pdf" download>
  Télécharger le guide (PDF, 2 Mo)
</a>

<!-- Lien email -->
<a href="mailto:contact@example.com">
  Nous contacter par email
</a>

<!-- Lien téléphone -->
<a href="tel:+33123456789">
  Appeler le +33 1 23 45 67 89
</a>
```

#### ❌ Mauvais
```html
<!-- Lien non descriptif -->
<a href="/article">Cliquez ici</a>
<a href="/page">En savoir plus</a>

<!-- Lien vide -->
<a href="/page"></a>

<!-- Plusieurs liens identiques sans contexte -->
<a href="/article1">Lire</a>
<a href="/article2">Lire</a>
<a href="/article3">Lire</a>
```

### Boutons

#### ✅ Bon
```html
<!-- Bouton simple -->
<button type="button">Enregistrer</button>

<!-- Bouton de soumission -->
<button type="submit">Envoyer le formulaire</button>

<!-- Bouton avec icône et texte -->
<button type="button">
  <svg aria-hidden="true"><!-- Icône --></svg>
  Supprimer
</button>

<!-- Bouton avec icône seule -->
<button type="button" aria-label="Fermer">
  <svg aria-hidden="true"><!-- Icône fermeture --></svg>
</button>

<!-- Bouton toggle -->
<button 
  type="button" 
  aria-pressed="false"
  aria-label="Activer le mode sombre"
>
  Mode sombre
</button>
```

#### ❌ Mauvais
```html
<!-- Div comme bouton -->
<div onclick="doSomething()">Cliquer</div>

<!-- Lien comme bouton -->
<a href="#" onclick="doSomething()">Cliquer</a>

<!-- Bouton sans texte ni label -->
<button>
  <svg><!-- Icône --></svg>
</button>
```

### Différence entre lien et bouton

**Utiliser un lien (`<a>`)** :
- Pour naviguer vers une autre page ou section
- Pour télécharger un fichier
- Si l'action a une URL

**Utiliser un bouton (`<button>`)** :
- Pour déclencher une action (ouvrir un modal, soumettre un formulaire)
- Pour des interactions JavaScript
- Si pas de navigation

---

## Tableaux accessibles

### Tableau simple

```html
<table>
  <caption>Horaires d'ouverture</caption>
  <thead>
    <tr>
      <th scope="col">Jour</th>
      <th scope="col">Horaires</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Lundi</th>
      <td>9h - 18h</td>
    </tr>
    <tr>
      <th scope="row">Mardi</th>
      <td>9h - 18h</td>
    </tr>
    <tr>
      <th scope="row">Mercredi</th>
      <td>Fermé</td>
    </tr>
  </tbody>
</table>
```

### Tableau complexe

```html
<table>
  <caption>Résultats trimestriels par région</caption>
  <thead>
    <tr>
      <th scope="col" rowspan="2">Région</th>
      <th scope="col" colspan="3">2024</th>
    </tr>
    <tr>
      <th scope="col">T1</th>
      <th scope="col">T2</th>
      <th scope="col">T3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Nord</th>
      <td headers="nord t1">100K</td>
      <td headers="nord t2">120K</td>
      <td headers="nord t3">150K</td>
    </tr>
    <tr>
      <th scope="row">Sud</th>
      <td headers="sud t1">80K</td>
      <td headers="sud t2">90K</td>
      <td headers="sud t3">95K</td>
    </tr>
  </tbody>
</table>
```

### Tableau de données vs tableau de mise en page

#### ✅ Bon (tableau de données)
```html
<table>
  <caption>Prix des produits</caption>
  <thead>
    <tr>
      <th scope="col">Produit</th>
      <th scope="col">Prix</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Pomme</th>
      <td>2€</td>
    </tr>
  </tbody>
</table>
```

#### ❌ Mauvais (tableau pour la mise en page)
```html
<!-- Ne jamais faire -->
<table>
  <tr>
    <td>Menu</td>
    <td>Contenu</td>
    <td>Sidebar</td>
  </tr>
</table>
```

**Note** : Utiliser CSS Grid ou Flexbox pour la mise en page, jamais de tableaux.

---

## Navigation au clavier

### Ordre de tabulation

```html
<!-- L'ordre de tabulation suit l'ordre du DOM -->
<button>Bouton 1</button>
<button>Bouton 2</button>
<button>Bouton 3</button>

<!-- ❌ Ne pas utiliser tabindex positif -->
<button tabindex="3">Dernier</button>
<button tabindex="1">Premier</button>
<button tabindex="2">Deuxième</button>

<!-- ✅ Bon usage de tabindex -->
<div tabindex="0" role="button">Élément focusable</div>
<div tabindex="-1">Non focusable au clavier, mais via JS</div>
```

### Raccourcis clavier

| Touche | Action |
|--------|--------|
| Tab | Élément suivant |
| Shift + Tab | Élément précédent |
| Entrée | Activer lien/bouton |
| Espace | Activer bouton/checkbox |
| Flèches | Navigation dans menus, tabs, etc. |
| Échap | Fermer modal/menu |
| Home/End | Début/fin de liste |

### Accesskey (à éviter)

```html
<!-- ⚠️ À éviter : peut entrer en conflit avec les raccourcis du navigateur -->
<a href="#content" accesskey="c">Contenu</a>

<!-- ✅ Mieux : instructions visibles -->
<button>Rechercher <kbd>Ctrl+K</kbd></button>
```

---

## Focus management

### Styles de focus

```css
/* ❌ Ne JAMAIS faire */
*:focus {
  outline: none;
}

/* ✅ Personnaliser le focus de manière visible */
:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Focus pour clavier seulement (nouveauté CSS) */
:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Supprimer focus pour souris, garder pour clavier */
:focus:not(:focus-visible) {
  outline: none;
}
```

### Focus dans les composants

```javascript
// Modal : piéger le focus
const modal = document.querySelector('[role="dialog"]');
const focusableElements = modal.querySelectorAll(
  'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
);
const firstElement = focusableElements[0];
const lastElement = focusableElements[focusableElements.length - 1];

// Focus sur le premier élément à l'ouverture
firstElement.focus();

// Piéger le focus dans le modal
modal.addEventListener('keydown', (e) => {
  if (e.key === 'Tab') {
    if (e.shiftKey && document.activeElement === firstElement) {
      e.preventDefault();
      lastElement.focus();
    } else if (!e.shiftKey && document.activeElement === lastElement) {
      e.preventDefault();
      firstElement.focus();
    }
  }
  
  // Fermer avec Échap
  if (e.key === 'Escape') {
    closeModal();
  }
});

// Restaurer le focus après fermeture
const triggerElement = document.activeElement;
// ... ouverture modal
// À la fermeture :
triggerElement.focus();
```

### Skip links visibles au focus

```html
<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: white;
  padding: 8px;
  z-index: 100;
  text-decoration: none;
}

.skip-link:focus {
  top: 0;
}
</style>

<a href="#main" class="skip-link">
  Aller au contenu principal
</a>
```

---

## Contenu dynamique

### Live regions

```html
<!-- Notification polie -->
<div 
  role="status" 
  aria-live="polite"
  aria-atomic="true"
>
  Article ajouté au panier
</div>

<!-- Alerte urgente -->
<div 
  role="alert" 
  aria-live="assertive"
  aria-atomic="true"
>
  Erreur : Le paiement a échoué
</div>

<!-- Log (historique) -->
<div role="log" aria-live="polite">
  <!-- Messages de chat -->
</div>

<!-- Timer -->
<div role="timer" aria-live="off">
  <span aria-atomic="true">5:00</span>
</div>

<!-- Barre de progression -->
<div 
  role="progressbar" 
  aria-valuenow="60" 
  aria-valuemin="0" 
  aria-valuemax="100"
  aria-label="Progression du téléchargement"
>
  60%
</div>
```

### Chargement asynchrone

```html
<!-- Bouton de chargement -->
<button aria-busy="true" aria-live="polite">
  <span class="spinner" aria-hidden="true"></span>
  Chargement...
</button>

<!-- Après chargement -->
<button aria-busy="false">
  Chargé
</button>

<!-- Skeleton screen -->
<div aria-busy="true" aria-label="Chargement du contenu">
  <div class="skeleton"></div>
  <div class="skeleton"></div>
</div>

<!-- Contenu chargé -->
<div aria-busy="false">
  <!-- Contenu réel -->
</div>
```

### Infinite scroll

```html
<!-- Annoncer le chargement -->
<main>
  <div id="content">
    <!-- Articles -->
  </div>
  
  <div 
    role="status" 
    aria-live="polite" 
    aria-atomic="true"
    class="visually-hidden"
  >
    Chargement de plus d'articles...
  </div>
</main>

<!-- Bouton alternatif pour charger plus -->
<button id="load-more">
  Charger plus d'articles
</button>
```

---

## Couleurs et contrastes

### Ratios de contraste WCAG

**Niveau AA (minimum)** :
- Texte normal : ratio de 4.5:1
- Texte large (≥18pt ou ≥14pt gras) : ratio de 3:1
- Éléments d'interface : ratio de 3:1

**Niveau AAA (optimal)** :
- Texte normal : ratio de 7:1
- Texte large : ratio de 4.5:1

### Exemples

```html
<!-- ✅ Bon contraste -->
<style>
  .good-contrast {
    color: #000000; /* Noir */
    background: #ffffff; /* Blanc */
    /* Ratio: 21:1 */
  }
  
  .good-contrast-2 {
    color: #ffffff;
    background: #0066cc;
    /* Ratio: 4.5:1 (AA) */
  }
</style>

<!-- ❌ Mauvais contraste -->
<style>
  .bad-contrast {
    color: #cccccc; /* Gris clair */
    background: #ffffff; /* Blanc */
    /* Ratio: 1.6:1 (insuffisant) */
  }
</style>
```

### Ne pas utiliser la couleur seule

```html
<!-- ❌ Mauvais : seule la couleur indique l'erreur -->
<input type="email" class="error">
<style>
  .error {
    border-color: red;
  }
</style>

<!-- ✅ Bon : couleur + icône + texte -->
<div>
  <input 
    type="email" 
    class="error"
    aria-invalid="true"
    aria-describedby="email-error"
  >
  <span id="email-error">
    ⚠️ L'email est invalide
  </span>
</div>
<style>
  .error {
    border: 2px solid red;
    border-left: 4px solid red;
  }
</style>
```

---

## Bonnes pratiques

### HTML valide

```html
<!-- Toujours utiliser DOCTYPE -->
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Titre de la page</title>
</head>
<body>
  <!-- Contenu -->
</body>
</html>
```

### Langue du document

```html
<!-- Langue principale -->
<html lang="fr">

<!-- Changement de langue dans le contenu -->
<p>
  Bonjour en français.
  <span lang="en">Hello in English.</span>
  <span lang="es">Hola en español.</span>
</p>
```

### Titres de page

```html
<!-- ✅ Bon : descriptif et unique -->
<title>Résultats de recherche pour "accessibilité" - Mon Site</title>

<!-- ❌ Mauvais : générique -->
<title>Page</title>
<title>Accueil</title>
```

### Texte masqué visuellement mais accessible

```css
/* Classe pour cacher visuellement mais garder accessible */
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Visible au focus (pour skip links) */
.visually-hidden:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

### Responsive et zoom

```html
<!-- Permettre le zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- ❌ Ne JAMAIS bloquer le zoom -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

```css
/* Utiliser des unités relatives */
body {
  font-size: 16px; /* Base */
}

h1 {
  font-size: 2rem; /* Relatif à la base */
}

/* Support du zoom texte */
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

### Animations et mouvements

```css
/* Respecter les préférences utilisateur */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Animation avec pause */
.carousel {
  animation: slide 10s infinite;
}

.carousel:hover,
.carousel:focus-within {
  animation-play-state: paused;
}
```

### Autocomplete sur les formulaires

```html
<!-- Attributs autocomplete recommandés -->
<input type="text" name="name" autocomplete="name">
<input type="email" name="email" autocomplete="email">
<input type="tel" name="phone" autocomplete="tel">
<input type="text" name="address" autocomplete="street-address">
<input type="text" name="city" autocomplete="address-level2">
<input type="text" name="postal" autocomplete="postal-code">
<input type="text" name="country" autocomplete="country">
<input type="password" name="password" autocomplete="current-password">
<input type="password" name="new-password" autocomplete="new-password">
```

---

## Outils et tests

### Outils de validation

1. **Validateur HTML W3C** : https://validator.w3.org/
2. **WAVE (WebAIM)** : Extension navigateur pour tester l'accessibilité
3. **axe DevTools** : Extension Chrome/Firefox
4. **Lighthouse** : Intégré dans Chrome DevTools
5. **Pa11y** : Outil en ligne de commande
6. **NVDA/JAWS** : Lecteurs d'écran pour Windows
7. **VoiceOver** : Lecteur d'écran macOS/iOS
8. **TalkBack** : Lecteur d'écran Android

### Extensions navigateur

- **WAVE Evaluation Tool**
- **axe DevTools**
- **Accessibility Insights**
- **HeadingsMap**
- **Landmark Navigation**
- **Colour Contrast Analyser**

### Tests manuels essentiels

#### ✅ Checklist de base

- [ ] Naviguer uniquement au clavier (Tab, Shift+Tab)
- [ ] Vérifier que tous les éléments interactifs sont focusables
- [ ] Tester avec un lecteur d'écran
- [ ] Vérifier les contrastes de couleurs
- [ ] Tester avec zoom à 200%
- [ ] Désactiver CSS et vérifier la structure
- [ ] Tester sur mobile
- [ ] Vérifier les alternatives textuelles des images
- [ ] Tester les formulaires avec erreurs
- [ ] Vérifier la hiérarchie des titres

### Raccourcis lecteur d'écran (NVDA/JAWS)

| Raccourci | Action |
|-----------|--------|
| H | Titre suivant |
| Shift + H | Titre précédent |
| D | Landmark suivant |
| B | Bouton suivant |
| F | Formulaire suivant |
| T | Tableau suivant |
| G | Graphique suivant |
| K | Lien suivant |
| Insert + F7 | Liste des éléments |

---

## Ressources et documentation

### Standards et guidelines

- **WCAG 2.1** : https://www.w3.org/WAI/WCAG21/quickref/
- **RGAA (France)** : https://www.numerique.gouv.fr/publications/rgaa-accessibilite/
- **WAI-ARIA** : https://www.w3.org/TR/wai-aria/
- **HTML Living Standard** : https://html.spec.whatwg.org/

### Ressources d'apprentissage

- **MDN Accessibility** : Documentation Mozilla
- **WebAIM** : Articles et guides
- **A11y Project** : Checklist et bonnes pratiques
- **Inclusive Components** : Patterns de composants accessibles
- **Deque University** : Cours en ligne

### Communauté

- **#a11y** : Hashtag Twitter
- **A11y Slack** : Communauté Slack
- **Forums WebAIM** : Discussion et support

---

## Checklist rapide d'accessibilité

### HTML

- [ ] DOCTYPE déclaré
- [ ] Attribut `lang` sur `<html>`
- [ ] Structure sémantique (header, nav, main, footer)
- [ ] Un seul `<main>` et un seul `<h1>`
- [ ] Hiérarchie des titres respectée
- [ ] Skip links présents

### Formulaires

- [ ] Tous les inputs ont un label associé
- [ ] Champs requis indiqués
- [ ] Messages d'erreur descriptifs
- [ ] Groupes de champs avec `<fieldset>` et `<legend>`

### Images

- [ ] Attribut `alt` sur toutes les images
- [ ] `alt=""` pour images décoratives
- [ ] Descriptions alternatives pertinentes

### Navigation

- [ ] Navigation possible au clavier uniquement
- [ ] Indicateur de focus visible
- [ ] Ordre de tabulation logique
- [ ] `aria-current` sur page active

### Contrastes

- [ ] Ratio 4.5:1 pour texte normal
- [ ] Ratio 3:1 pour texte large
- [ ] Ne pas utiliser couleur seule pour l'information

### ARIA

- [ ] ARIA utilisé seulement si nécessaire
- [ ] Rôles, états et propriétés appropriés
- [ ] Live regions pour contenu dynamique

### Responsive

- [ ] Viewport configuré sans bloquer le zoom
- [ ] Support du zoom jusqu'à 200%
- [ ] Contenu lisible sur mobile

---

