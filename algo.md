# Algo CheatSheet

## Table des matières
1. [Qu'est-ce qu'un algorithme ?](#quest-ce-quun-algorithme)
2. [Mise en forme d'un algorithme](#mise-en-forme-dun-algorithme)
3. [Variables et types](#variables-et-types)
4. [Instructions de base](#instructions-de-base)
5. [Structures conditionnelles](#structures-conditionnelles)
6. [Structures itératives (boucles)](#structures-itératives-boucles)
7. [Tableaux](#tableaux)
8. [Algorithmes de tri](#algorithmes-de-tri)
9. [Complexité](#complexité)
10. [Récursivité](#récursivité)

---

## Qu'est-ce qu'un algorithme ?

### Définition
Un **algorithme** est une procédure qui, une fois appliquée, amène à la solution d'un problème. C'est une suite finie et non ambiguë d'instructions permettant de résoudre un problème.

### Exemples dans la vie courante
- Une recette de cuisine
- Un mode d'emploi
- Un itinéraire routier
- Une notice de montage

### Algorithme vs Programme
- **Algorithme** : description abstraite de la méthode de résolution
- **Programme** : traduction de l'algorithme dans un langage compréhensible par la machine
- **Processus** : exécution effective du programme

### Qualités d'un bon algorithme
- **Correct** : produit le résultat attendu
- **Efficace** : s'exécute rapidement
- **Lisible** : facile à comprendre
- **Général** : fonctionne pour tous les cas valides

---

## Mise en forme d'un algorithme

### Structure générale (patron)

```
Algorithme NomAlgorithme
Début
    ... actions
Fin
```

### Exemple simple

```
Algorithme Salutation
Début
    Écrire('Bonjour !')
    ALaLigne
Fin
```

### Avec des paramètres et variables

```
Algorithme NomAlgorithme (paramètres...)
Variable
    nom_variable : type
Début
    ... actions
Fin
```

### Exemple complet

```
Algorithme CalculerSurface (longueur, largeur : réel)
Variable
    surface : réel
Début
    surface ← longueur × largeur
    Écrire('La surface est : ')
    Écrire(surface)
    ALaLigne
Fin
```

### Instructions de sortie
- **Écrire(texte)** : affiche du texte ou une valeur
- **ALaLigne** : passe à la ligne suivante

### Commentaires
Les commentaires sont placés entre accolades `{ commentaire }` pour expliquer le code.

```
Algorithme Exemple
Début
    { Ceci est un commentaire }
    Écrire('Hello')
Fin
```

---

## Variables et types

### Qu'est-ce qu'une variable ?
Une variable possède :
- Un **nom** (identifiant)
- Un **type** (nature des données)
- Une **valeur** (contenu)

### Déclaration de variables

```
Variable
    age : entier
    nom : texte
    prix : réel
    estValide : booléen
```

### Types de base

| Type | Description | Exemples |
|------|-------------|----------|
| **entier** | Nombre sans décimale | -5, 0, 42, 1000 |
| **réel** | Nombre avec décimale | 3.14, -2.5, 0.0 |
| **booléen** | Vrai ou Faux | vrai, faux |
| **caractère** | Un seul caractère | 'a', 'Z', '7', '@' |
| **texte** (chaîne) | Suite de caractères | "Bonjour", "Alice" |

### Affectation

L'affectation utilise le symbole **←** (flèche vers la gauche).

```
age ← 25
nom ← "Marie"
prix ← 19.99
estValide ← vrai
```

⚠️ **Important** : L'affectation n'est pas une équation mathématique !

```
x ← x + 1    { On lit : "x prend la valeur de x + 1" }
```

### Expressions et opérateurs

**Opérateurs arithmétiques :**
- Addition : `+`
- Soustraction : `-`
- Multiplication : `×` ou `*`
- Division : `/`
- Division entière : `div`
- Modulo (reste) : `mod`
- Puissance : `^`

**Opérateurs de comparaison :**
- Égal : `=`
- Différent : `≠`
- Plus petit : `<`
- Plus grand : `>`
- Plus petit ou égal : `≤`
- Plus grand ou égal : `≥`

**Opérateurs logiques :**
- ET : `ET` ou `AND`
- OU : `OU` ou `OR`
- NON : `NON` ou `NOT`

### Exemples d'expressions

```
resultat ← (a + b) × 2
moyenne ← (note1 + note2 + note3) / 3
reste ← nombre mod 10
estMajeur ← age ≥ 18
condition ← (x > 0) ET (x < 100)
```

---

## Instructions de base

### Affichage (sortie)

```
Écrire('Texte à afficher')
Écrire(variable)
ALaLigne    { Retour à la ligne }
```

**Exemple :**
```
Algorithme AfficherMessage
Variable
    nom : texte
Début
    nom ← "Alice"
    Écrire('Bonjour ')
    Écrire(nom)
    Écrire(' !')
    ALaLigne
Fin
```
**Sortie :** `Bonjour Alice !`

### Saisie (entrée)

```
Lire(variable)
```

**Exemple :**
```
Algorithme DemanderAge
Variable
    age : entier
Début
    Écrire('Quel est votre âge ? ')
    Lire(age)
    Écrire('Vous avez ')
    Écrire(age)
    Écrire(' ans.')
    ALaLigne
Fin
```

---

## Structures conditionnelles

### Si Alors Sinon

```
Si condition Alors
    instructions1
Sinon
    instructions2
Fin Si
```

**Exemple : Déterminer si un nombre est pair**
```
Algorithme TestParite (n : entier)
Début
    Si (n mod 2 = 0) Alors
        Écrire('Le nombre est pair')
    Sinon
        Écrire('Le nombre est impair')
    Fin Si
Fin
```

### Si Alors (sans Sinon)

```
Si condition Alors
    instructions
Fin Si
```

**Exemple : Vérifier la majorité**
```
Algorithme VerifierMajorite (age : entier)
Début
    Si (age ≥ 18) Alors
        Écrire('Vous êtes majeur')
    Fin Si
Fin
```

### Si multiples (Si Sinon Si)

```
Si condition1 Alors
    instructions1
Sinon Si condition2 Alors
    instructions2
Sinon Si condition3 Alors
    instructions3
Sinon
    instructions_defaut
Fin Si
```

**Exemple : Attribution d'une mention**
```
Algorithme AttribuerMention (note : réel)
Début
    Si (note ≥ 16) Alors
        Écrire('Très bien')
    Sinon Si (note ≥ 14) Alors
        Écrire('Bien')
    Sinon Si (note ≥ 12) Alors
        Écrire('Assez bien')
    Sinon Si (note ≥ 10) Alors
        Écrire('Passable')
    Sinon
        Écrire('Insuffisant')
    Fin Si
Fin
```

### Selon Cas (équivalent du switch)

```
Selon variable
    Cas valeur1 : instructions1
    Cas valeur2 : instructions2
    Cas valeur3 : instructions3
    Défaut : instructions_defaut
Fin Selon
```

**Exemple : Jour de la semaine**
```
Algorithme NomJour (jour : entier)
Début
    Selon jour
        Cas 1 : Écrire('Lundi')
        Cas 2 : Écrire('Mardi')
        Cas 3 : Écrire('Mercredi')
        Cas 4 : Écrire('Jeudi')
        Cas 5 : Écrire('Vendredi')
        Cas 6 : Écrire('Samedi')
        Cas 7 : Écrire('Dimanche')
        Défaut : Écrire('Jour invalide')
    Fin Selon
Fin
```

---

## Structures itératives (boucles)

### Boucle Pour

Utilisée quand on connaît à l'avance le nombre d'itérations.

```
Pour variable ← début À fin Faire
    instructions
Fin Pour
```

**Exemple : Afficher les nombres de 1 à 10**
```
Algorithme CompterJusque10
Variable
    i : entier
Début
    Pour i ← 1 À 10 Faire
        Écrire(i)
        ALaLigne
    Fin Pour
Fin
```

**Exemple : Table de multiplication**
```
Algorithme TableMultiplication (n : entier)
Variable
    i : entier
Début
    Pour i ← 1 À 10 Faire
        Écrire(n)
        Écrire(' × ')
        Écrire(i)
        Écrire(' = ')
        Écrire(n × i)
        ALaLigne
    Fin Pour
Fin
```

**Boucle décroissante :**
```
Pour i ← 10 À 1 Faire
    { Compte de 10 à 1 }
Fin Pour
```

### Boucle Tant Que

Utilisée quand on ne connaît pas le nombre d'itérations à l'avance. La condition est testée **avant** chaque itération.

```
Tant Que condition Faire
    instructions
Fin Tant Que
```

**Exemple : Compter jusqu'à 100**
```
Algorithme CompteJusque100
Variable
    i : entier
Début
    i ← 1
    Tant Que (i ≤ 100) Faire
        Écrire(i)
        ALaLigne
        i ← i + 1
    Fin Tant Que
Fin
```

**Exemple : Deviner un nombre**
```
Algorithme DevinerNombre
Variable
    secret, essai : entier
Début
    secret ← 42
    essai ← 0
    
    Tant Que (essai ≠ secret) Faire
        Écrire('Proposez un nombre : ')
        Lire(essai)
        
        Si (essai < secret) Alors
            Écrire('Trop petit !')
        Sinon Si (essai > secret) Alors
            Écrire('Trop grand !')
        Fin Si
    Fin Tant Que
    
    Écrire('Bravo ! Vous avez trouvé !')
Fin
```

### Boucle Répéter Jusqu'à

La condition est testée **après** chaque itération. Le corps de la boucle est donc exécuté au moins une fois.

```
Répéter
    instructions
Jusqu'à condition
```

**Exemple : Menu interactif**
```
Algorithme MenuPrincipal
Variable
    choix : entier
Début
    Répéter
        Écrire('=== MENU ===')
        ALaLigne
        Écrire('1. Option A')
        ALaLigne
        Écrire('2. Option B')
        ALaLigne
        Écrire('3. Quitter')
        ALaLigne
        Écrire('Votre choix : ')
        Lire(choix)
    Jusqu'à (choix = 3)
    
    Écrire('Au revoir !')
Fin
```

### Différence Tant Que vs Répéter

```
{ Tant Que : peut ne JAMAIS s'exécuter }
i ← 10
Tant Que (i < 5) Faire
    Écrire(i)    { Ne s'affiche jamais }
Fin Tant Que

{ Répéter : s'exécute AU MOINS une fois }
i ← 10
Répéter
    Écrire(i)    { S'affiche une fois : 10 }
Jusqu'à (i < 5)
```

### Comparaison des trois boucles

**Même algorithme avec les trois types de boucles :**

```
{ Version Pour }
Algorithme Compte100Pour
Variable i : entier
Début
    Pour i ← 1 À 100 Faire
        Écrire(i)
        ALaLigne
    Fin Pour
Fin

{ Version Tant Que }
Algorithme Compte100TantQue
Variable i : entier
Début
    i ← 1
    Tant Que (i ≤ 100) Faire
        Écrire(i)
        ALaLigne
        i ← i + 1
    Fin Tant Que
Fin

{ Version Répéter }
Algorithme Compte100Repeter
Variable i : entier
Début
    i ← 1
    Répéter
        Écrire(i)
        ALaLigne
        i ← i + 1
    Jusqu'À (i > 100)
Fin
```

**Quand utiliser quelle boucle ?**
- **Pour** : nombre d'itérations connu à l'avance
- **Tant Que** : condition à vérifier avant chaque itération
- **Répéter** : au moins une exécution garantie

---

## Tableaux

### Définition
Un tableau est une structure de données qui stocke plusieurs éléments du même type, accessibles par leur position (indice).

### Déclaration

```
Variable
    notes : tableau[1..30] de réel
    noms : tableau[1..50] de texte
```

### Accès aux éléments

```
notes[1] ← 15.5    { Premier élément }
notes[5] ← 12.0    { Cinquième élément }
x ← notes[3]       { Lecture }
```

⚠️ **Attention** : Les indices commencent généralement à 1 (selon les conventions).

### Fonctions sur les tableaux

- **taille(tableau)** : retourne le nombre d'éléments
- **créer_tableau(n)** : crée un tableau de taille n

### Parcours d'un tableau

```
Algorithme AfficherTableau (t : tableau d'entiers)
Variable
    i : entier
Début
    Pour i ← 1 À taille(t) Faire
        Écrire(t[i])
        Écrire(' ')
    Fin Pour
    ALaLigne
Fin
```

### Exemples d'algorithmes sur les tableaux

**Calculer la somme des éléments :**
```
Algorithme SommeTableau (t : tableau d'entiers)
Variable
    i, somme : entier
Début
    somme ← 0
    Pour i ← 1 À taille(t) Faire
        somme ← somme + t[i]
    Fin Pour
    Retourner somme
Fin
```

**Trouver le maximum :**
```
Algorithme Maximum (t : tableau d'entiers)
{ Recherche le plus grand élément du tableau }
Variable
    i, max : entier
Début
    max ← t[1]    { On suppose que le tableau n'est pas vide }
    
    Pour i ← 2 À taille(t) Faire
        Si (t[i] > max) Alors
            max ← t[i]
        Fin Si
    Fin Pour
    
    Retourner max
Fin
```

**Trouver la position du minimum :**
```
Algorithme PositionMinimum (t : tableau d'entiers ; debut, fin : entier)
{ Retourne la position du plus petit élément entre debut et fin }
Variable
    i, imin : entier
Début
    imin ← debut
    
    Pour i ← debut+1 À fin Faire
        Si (t[i] < t[imin]) Alors
            imin ← i
        Fin Si
    Fin Pour
    
    Retourner imin
Fin
```

### Recherche dans un tableau

**Recherche linéaire (simple) :**
```
Algorithme RechercheLineaire (e : entier ; t : tableau d'entiers)
{ Indique si l'élément e est présent dans le tableau }
Variable
    i : entier
Début
    i ← 1
    Tant Que (i ≤ taille(t)) ET (t[i] ≠ e) Faire
        i ← i + 1
    Fin Tant Que
    
    Si (i > taille(t)) Alors
        Écrire('Élément non trouvé')
        Retourner -1
    Sinon
        Écrire('Élément trouvé à la position ')
        Écrire(i)
        Retourner i
    Fin Si
Fin
```

**Recherche dans un tableau trié :**
```
Algorithme RechercheTrie (e : entier ; t : tableau trié d'entiers)
Variable
    i : entier
Début
    i ← 1
    { On s'arrête si on dépasse l'élément cherché }
    Tant Que (i ≤ taille(t)) ET (t[i] < e) Faire
        i ← i + 1
    Fin Tant Que
    
    Si (i ≤ taille(t)) ET (t[i] = e) Alors
        Écrire('Trouvé à la position ')
        Écrire(i)
        Retourner i
    Sinon
        Écrire('Non trouvé')
        Retourner -1
    Fin Si
Fin
```

**Recherche dichotomique (tableau trié) :**
```
Algorithme RechercheDichotomique (e : entier ; t : tableau trié d'entiers)
{ Recherche rapide dans un tableau trié }
Variable
    gauche, droite, milieu : entier
    trouve : booléen
Début
    gauche ← 1
    droite ← taille(t)
    trouve ← faux
    
    Tant Que (gauche ≤ droite) ET NON(trouve) Faire
        milieu ← (gauche + droite) div 2
        
        Si (t[milieu] = e) Alors
            trouve ← vrai
        Sinon Si (e < t[milieu]) Alors
            droite ← milieu - 1
        Sinon
            gauche ← milieu + 1
        Fin Si
    Fin Tant Que
    
    Si trouve Alors
        Écrire('Trouvé à la position ')
        Écrire(milieu)
        Retourner milieu
    Sinon
        Écrire('Non trouvé')
        Retourner -1
    Fin Si
Fin
```

**Complexité :**
- Recherche linéaire : **O(n)**
- Recherche dichotomique : **O(log n)** (beaucoup plus rapide !)

### Insertion dans un tableau trié

```
Algorithme Insertion (t : tableau d'entiers ; n, e : entier)
{ Insère l'élément e à sa place dans un tableau trié de n éléments }
Variable
    i : entier
Début
    i ← n
    { Décaler les éléments vers la droite }
    Tant Que (i > 0) ET (t[i] > e) Faire
        t[i+1] ← t[i]
        i ← i - 1
    Fin Tant Que
    
    { Insérer l'élément à sa place }
    t[i+1] ← e
Fin
```

---

## Algorithmes de tri

### Algorithme d'échange

Utilitaire pour échanger deux éléments d'un tableau :

```
Algorithme Échange (t : tableau ; i, j : entier)
{ Échange le contenu des cases i et j }
Variable
    temp : même type que t
Début
    temp ← t[i]
    t[i] ← t[j]
    t[j] ← temp
Fin
```

### Tri par insertion

**Principe :** Comme trier des cartes à jouer : on insère chaque élément à sa place dans la partie déjà triée.

```
Algorithme TriInsertion (t : tableau d'entiers)
{ Trie le tableau par ordre croissant }
Variable
    i : entier
Début
    Pour i ← 2 À taille(t) Faire
        { Insérer t[i] à sa place dans t[1..i-1] }
        Insertion(t, i-1, t[i])
    Fin Pour
Fin
```

**Exemple d'exécution :**
```
Tableau initial : [5, 2, 4, 1, 3]

i=2 : [2, 5, 4, 1, 3]  { 2 inséré avant 5 }
i=3 : [2, 4, 5, 1, 3]  { 4 inséré entre 2 et 5 }
i=4 : [1, 2, 4, 5, 3]  { 1 inséré en première position }
i=5 : [1, 2, 3, 4, 5]  { 3 inséré entre 2 et 4 }
```

**Complexité :** O(n²)

### Tri par sélection (extraction)

**Principe :** Trouver le minimum, le mettre en première position, recommencer avec le reste.

```
Algorithme TriSelection (t : tableau d'entiers)
{ Trie le tableau par ordre croissant }
Variable
    i, imin : entier
Début
    Pour i ← 1 À taille(t)-1 Faire
        { Trouver le minimum dans t[i..n] }
        imin ← PositionMinimum(t, i, taille(t))
        
        { Échanger avec la position i }
        Échange(t, i, imin)
    Fin Pour
Fin
```

**Exemple d'exécution :**
```
Tableau initial : [5, 2, 4, 1, 3]

i=1 : [1, 2, 4, 5, 3]  { min=1, échange avec position 1 }
i=2 : [1, 2, 4, 5, 3]  { min=2, déjà en place }
i=3 : [1, 2, 3, 5, 4]  { min=3, échange avec position 3 }
i=4 : [1, 2, 3, 4, 5]  { min=4, échange avec position 4 }
```

**Complexité :** O(n²)

### Tri à bulles

**Principe :** Comparer les éléments adjacents et les échanger s'ils sont dans le mauvais ordre. Les plus grands "remontent" comme des bulles.

```
Algorithme TriBulles (t : tableau d'entiers)
{ Trie le tableau par ordre croissant }
Variable
    i, j : entier
Début
    Pour i ← 1 À taille(t)-1 Faire
        Pour j ← 1 À taille(t)-i Faire
            Si (t[j] > t[j+1]) Alors
                Échange(t, j, j+1)
            Fin Si
        Fin Pour
    Fin Pour
Fin
```

**Exemple d'exécution :**
```
Tableau initial : [5, 2, 4, 1, 3]

Passage 1 : [2, 4, 1, 3, 5]  { 5 remonte à la fin }
Passage 2 : [2, 1, 3, 4, 5]  { 4 remonte }
Passage 3 : [1, 2, 3, 4, 5]  { 3 remonte }
Passage 4 : [1, 2, 3, 4, 5]  { déjà trié }
```

**Complexité :** O(n²)

**Version optimisée** (s'arrête si plus d'échange) :
```
Algorithme TriBullesOptimise (t : tableau d'entiers)
Variable
    i, j : entier
    echange : booléen
Début
    Pour i ← 1 À taille(t)-1 Faire
        echange ← faux
        
        Pour j ← 1 À taille(t)-i Faire
            Si (t[j] > t[j+1]) Alors
                Échange(t, j, j+1)
                echange ← vrai
            Fin Si
        Fin Pour
        
        { Si aucun échange, le tableau est trié }
        Si NON(echange) Alors
            Sortir de la boucle
        Fin Si
    Fin Pour
Fin
```

### Tri rapide (Quick Sort)

**Principe :** Diviser pour régner
1. Choisir un pivot
2. Placer les éléments < pivot à gauche, > pivot à droite
3. Trier récursivement les deux parties

```
Algorithme TriRapide (t : tableau d'entiers ; debut, fin : entier)
Variable
    pivot : entier
Début
    Si (debut < fin) Alors
        { Placer le pivot à sa position finale }
        pivot ← Partitionner(t, debut, fin)
        
        { Trier récursivement les deux parties }
        TriRapide(t, debut, pivot-1)
        TriRapide(t, pivot+1, fin)
    Fin Si
Fin

Algorithme Partitionner (t : tableau d'entiers ; debut, fin : entier)
Variable
    gauche, droite : entier
Début
    gauche ← debut + 1
    droite ← fin
    
    Tant Que (gauche ≤ droite) Faire
        { Trouver un élément à droite < pivot }
        Tant Que (t[droite] > t[debut]) Faire
            droite ← droite - 1
        Fin Tant Que
        
        { Trouver un élément à gauche ≥ pivot }
        Tant Que (gauche ≤ fin) ET (t[gauche] ≤ t[debut]) Faire
            gauche ← gauche + 1
        Fin Tant Que
        
        { Échanger si nécessaire }
        Si (gauche < droite) Alors
            Échange(t, gauche, droite)
            droite ← droite - 1
            gauche ← gauche + 1
        Fin Si
    Fin Tant Que
    
    { Placer le pivot à sa position finale }
    Échange(t, debut, droite)
    Retourner droite
Fin
```

**Complexité :**
- Moyenne : **O(n log n)** (très efficace !)
- Pire cas : O(n²) (rare)

### Comparaison des tris

| Algorithme | Complexité | Avantages | Inconvénients |
|------------|------------|-----------|---------------|
| **Insertion** | O(n²) | Simple, efficace sur petits tableaux ou presque triés | Lent sur grands tableaux |
| **Sélection** | O(n²) | Nombre minimal d'échanges | Toujours O(n²) même si déjà trié |
| **Bulles** | O(n²) | Simple à comprendre | Lent, peu utilisé en pratique |
| **Rapide** | O(n log n) moyen | Très rapide en pratique | Pire cas O(n²) |

---

## Complexité

### Qu'est-ce que la complexité ?

La **complexité** mesure l'efficacité d'un algorithme en fonction de la taille des données (notée **n**).

On distingue :
- **Complexité temporelle** : temps d'exécution
- **Complexité spatiale** : mémoire utilisée

### Notation Big O

La notation **O(...)** décrit l'ordre de grandeur de la complexité.

| Notation | Nom | Exemple |
|----------|-----|---------|
| O(1) | Constante | Accès à un élément de tableau |
| O(log n) | Logarithmique | Recherche dichotomique |
| O(n) | Linéaire | Parcours de tableau |
| O(n log n) | Linéarithmique | Tri rapide (moyen) |
| O(n²) | Quadratique | Tri à bulles |
| O(2ⁿ) | Exponentielle | Tours de Hanoï |

### Exemples de calcul

**Algorithme simple (O(1)) :**
```
Algorithme AccesTableau (t : tableau ; i : entier)
Début
    Retourner t[i]    { Une seule opération }
Fin
```
**Complexité : O(1)** - temps constant

**Parcours de tableau (O(n)) :**
```
Algorithme SommeTableau (t : tableau d'entiers)
Variable i, somme : entier
Début
    somme ← 0
    Pour i ← 1 À n Faire    { n itérations }
        somme ← somme + t[i]
    Fin Pour
    Retourner somme
Fin
```
**Complexité : O(n)** - n opérations

**Boucles imbriquées (O(n²)) :**
```
Algorithme TriBulles (t : tableau d'entiers)
Variable i, j : entier
Début
    Pour i ← 1 À n Faire        { n itérations }
        Pour j ← 1 À n Faire    { n itérations }
            { opérations }
        Fin Pour
    Fin Pour
Fin
```
**Complexité : O(n²)** - n × n opérations

### Complexité au mieux, moyen, pire

Pour l'algorithme **Maximum** (recherche du plus grand élément) :

**Nombre d'affectations :**
- **Au mieux :** 1 (le max est en première position)
- **En moyenne :** log(n) 
- **Au pire :** n (tableau trié croissant, max à la fin)

**Nombre de comparaisons :**
- Toujours : n-1 (on compare avec tous les éléments)

### Ordre de croissance

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

**Illustration avec n = 1000 :**
- O(1) : 1 opération
- O(log n) : ~10 opérations
- O(n) : 1 000 opérations
- O(n log n) : ~10 000 opérations
- O(n²) : 1 000 000 opérations
- O(2ⁿ) : ~10³⁰⁰ opérations (impossible !)

### Problèmes avec complexité exponentielle

**Tours de Hanoï :**
```
Algorithme Hanoi (n : entier ; source, destination, auxiliaire : texte)
Début
    Si (n = 1) Alors
        Écrire('Déplacer disque de ')
        Écrire(source)
        Écrire(' vers ')
        Écrire(destination)
    Sinon
        Hanoi(n-1, source, auxiliaire, destination)
        Écrire('Déplacer disque de ')
        Écrire(source)
        Écrire(' vers ')
        Écrire(destination)
        Hanoi(n-1, auxiliaire, destination, source)
    Fin Si
Fin
```

**Nombre de déplacements :** 2ⁿ - 1  
**Complexité : O(2ⁿ)** - exponentielle

Avec 64 disques, il faudrait ~18 milliards de milliards d'années !

### Loi de Moore et complexité

**Loi de Moore :** La puissance des processeurs double tous les 18 mois.

**Impact selon la complexité :**

| Complexité | Si on double la vitesse | Amélioration |
|------------|------------------------|--------------|
| O(n) | On peut traiter 2×n données | ×2 |
| O(n²) | On peut traiter √2×n données | ×1.4 |
| O(2ⁿ) | On peut traiter n+1 données | +1 seulement ! |

**Conclusion :** Pour les problèmes exponentiels, même des ordinateurs plus rapides n'aident pas beaucoup !

---

## Récursivité

### Définition

Une fonction est **récursive** si elle s'appelle elle-même.

### Structure d'un algorithme récursif

```
Algorithme Recursif (paramètre)
Début
    Si cas_de_base Alors
        Retourner solution_simple
    Sinon
        Retourner Recursif(problème_plus_petit)
    Fin Si
Fin
```

### Éléments essentiels

1. **Cas de base** : condition d'arrêt
2. **Appel récursif** : sur un problème plus petit
3. **Progression** : on doit se rapprocher du cas de base

### Exemples classiques

**Factorielle :**
```
Algorithme Factorielle (n : entier)
Début
    Si (n ≤ 1) Alors
        Retourner 1          { Cas de base }
    Sinon
        Retourner n × Factorielle(n-1)    { Appel récursif }
    Fin Si
Fin
```

**Calculs :**
- Factorielle(5) = 5 × Factorielle(4)
- = 5 × 4 × Factorielle(3)
- = 5 × 4 × 3 × Factorielle(2)
- = 5 × 4 × 3 × 2 × Factorielle(1)
- = 5 × 4 × 3 × 2 × 1 = 120

**Puissance :**
```
Algorithme Puissance (x : réel ; n : entier)
{ Calcule x à la puissance n }
Début
    Si (n = 0) Alors
        Retourner 1         { Cas de base }
    Sinon
        Retourner x × Puissance(x, n-1)
    Fin Si
Fin
```

**Somme des n premiers entiers :**
```
Algorithme SommeN (n : entier)
{ Calcule 1 + 2 + ... + n }
Début
    Si (n = 0) Alors
        Retourner 0
    Sinon
        Retourner n + SommeN(n-1)
    Fin Si
Fin
```

**Suite de Fibonacci :**
```
Algorithme Fibonacci (n : entier)
{ Calcule le n-ième terme de la suite de Fibonacci }
Début
    Si (n ≤ 1) Alors
        Retourner n
    Sinon
        Retourner Fibonacci(n-1) + Fibonacci(n-2)
    Fin Si
Fin
```

⚠️ **Attention :** Cette version est inefficace (complexité exponentielle) car elle recalcule plusieurs fois les mêmes valeurs.

### Récursivité sur les chaînes

**Inverser une chaîne :**
```
Algorithme InverserChaine (s : texte)
Début
    Si (longueur(s) ≤ 1) Alors
        Retourner s
    Sinon
        Retourner InverserChaine(sous_chaine(s, 2, fin)) + s[1]
    Fin Si
Fin
```

### Récursivité vs Itération

**Version récursive - Compter jusqu'à 100 :**
```
Algorithme CompteRecursif (n : entier)
Début
    Si (n ≤ 100) Alors
        Écrire(n)
        ALaLigne
        CompteRecursif(n+1)
    Fin Si
Fin
```

**Version itérative :**
```
Algorithme CompteIteratif
Variable i : entier
Début
    Pour i ← 1 À 100 Faire
        Écrire(i)
        ALaLigne
    Fin Pour
Fin
```

| Critère | Récursivité | Itération |
|---------|-------------|-----------|
| Lisibilité | Souvent plus claire | Plus verbeuse |
| Performance | Plus lente (appels de fonction) | Plus rapide |
| Mémoire | Utilise la pile d'appels | Mémoire constante |
| Utilisation | Structures arborescentes, problèmes naturellement récursifs | Boucles simples |

---

## Conseils pratiques

### Méthodologie de résolution

1. **Comprendre le problème**
   - Lire l'énoncé plusieurs fois
   - Identifier les entrées et sorties
   - Faire des exemples concrets à la main

2. **Concevoir l'algorithme**
   - Décomposer en sous-problèmes
   - Choisir les structures de données appropriées
   - Commencer simple, optimiser ensuite

3. **Écrire l'algorithme**
   - Respecter la syntaxe
   - Nommer les variables de manière explicite
   - Commenter les parties complexes

4. **Tester**
   - Cas simples
   - Cas limites (tableau vide, n=0, etc.)
   - Cas généraux

### Erreurs courantes à éviter

❌ **Oublier d'initialiser les variables**
```
somme ← 0    { Toujours initialiser ! }
```

❌ **Confusion entre = et ←**
```
x = 5     { Comparaison }
x ← 5     { Affectation }
```

❌ **Boucle infinie**
```
i ← 0
Tant Que (i < 10) Faire
    Écrire(i)
    { Oubli de i ← i+1 : boucle infinie ! }
Fin Tant Que
```

❌ **Débordement de tableau**
```
Pour i ← 1 À taille(t)+1 Faire    { Erreur ! }
    t[i] ← 0
Fin Pour
```

❌ **Absence de cas de base en récursivité**
```
Algorithme MauvaiseRecursivite (n : entier)
Début
    { Pas de cas de base : erreur ! }
    Retourner MauvaiseRecursivite(n-1)
Fin
```

### Cas de test importants

- **Tableau vide** : taille = 0
- **Un élément** : taille = 1
- **Tous identiques** : [5, 5, 5, 5]
- **Déjà trié** : [1, 2, 3, 4, 5]
- **Trié inverse** : [5, 4, 3, 2, 1]
- **Valeurs limites** : 0, négatifs, très grands nombres

---



