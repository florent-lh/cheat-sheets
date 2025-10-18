# JavaScript Cheat Sheet Complet

## Table des matières
1. [Variables et Types de données](#variables-et-types-de-données)
2. [Opérateurs](#opérateurs)
3. [Structures de contrôle](#structures-de-contrôle)
4. [Fonctions](#fonctions)
5. [Objets](#objets)
6. [Tableaux (Arrays)](#tableaux-arrays)
7. [Classes](#classes)
8. [Asynchrone](#asynchrone)
9. [DOM Manipulation](#dom-manipulation)
10. [Modules](#modules)
11. [Gestion d'erreurs](#gestion-derreurs)
12. [Méthodes utiles](#méthodes-utiles)

---

## Variables et Types de données

### Déclaration de variables
```javascript
var ancienne = "éviter"; // Portée fonction, hoisting
let variable = "moderne"; // Portée bloc, réassignable
const constante = "immuable"; // Portée bloc, non réassignable
```

### Types primitifs
```javascript
// String
let texte = "Bonjour";
let template = `Hello ${texte}`; // Template literals

// Number
let entier = 42;
let decimal = 3.14;
let negatif = -10;
let infini = Infinity;

// Boolean
let vrai = true;
let faux = false;

// Undefined
let nonDefini;
console.log(nonDefini); // undefined

// Null
let vide = null;

// Symbol (ES6)
let symbole = Symbol("description");

// BigInt (ES2020)
let grand = 9007199254740991n;
```

### Vérification de types
```javascript
typeof "hello"; // "string"
typeof 42; // "number"
typeof true; // "boolean"
typeof undefined; // "undefined"
typeof null; // "object" (bug historique)
typeof {}; // "object"
typeof []; // "object"
typeof function(){}; // "function"

// Vérification avancée
Array.isArray([]); // true
null === null; // true pour vérifier null
```

---

## Opérateurs

### Opérateurs arithmétiques
```javascript
let a = 10, b = 3;

a + b;   // 13 (Addition)
a - b;   // 7  (Soustraction)
a * b;   // 30 (Multiplication)
a / b;   // 3.333... (Division)
a % b;   // 1  (Modulo)
a ** b;  // 1000 (Exponentiation ES2016)

a++;     // Incrémentation
a--;     // Décrémentation
```

### Opérateurs de comparaison
```javascript
// Égalité (avec conversion de type)
5 == "5";   // true
5 != "6";   // true

// Égalité stricte (sans conversion)
5 === "5";  // false
5 !== "6";  // true

// Comparaisons
10 > 5;     // true
10 < 5;     // false
10 >= 10;   // true
10 <= 5;    // false
```

### Opérateurs logiques
```javascript
let x = true, y = false;

x && y;     // false (ET logique)
x || y;     // true  (OU logique)
!x;         // false (NON logique)

// Court-circuit
true || console.log("non exécuté");
false && console.log("non exécuté");

// Nullish coalescing (ES2020)
null ?? "défaut";      // "défaut"
undefined ?? "défaut"; // "défaut"
0 ?? "défaut";         // 0
```

### Opérateurs d'affectation
```javascript
let x = 10;
x += 5;  // x = x + 5  (15)
x -= 3;  // x = x - 3  (12)
x *= 2;  // x = x * 2  (24)
x /= 4;  // x = x / 4  (6)
x %= 4;  // x = x % 4  (2)
x **= 3; // x = x ** 3 (8)

// Affectation logique (ES2021)
x ||= 5;  // x = x || 5
x &&= 5;  // x = x && 5
x ??= 5;  // x = x ?? 5
```

### Opérateur ternaire
```javascript
let age = 18;
let statut = age >= 18 ? "adulte" : "mineur";
```

---

## Structures de contrôle

### Conditions

#### if...else
```javascript
if (condition) {
    // code si vrai
} else if (autreCondition) {
    // autre code
} else {
    // code par défaut
}
```

#### switch
```javascript
switch (valeur) {
    case 1:
        console.log("Un");
        break;
    case 2:
        console.log("Deux");
        break;
    default:
        console.log("Autre");
}
```

### Boucles

#### for
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

#### while
```javascript
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}
```

#### do...while
```javascript
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);
```

#### for...of (itérables)
```javascript
let arr = [1, 2, 3];
for (let valeur of arr) {
    console.log(valeur);
}
```

#### for...in (propriétés d'objets)
```javascript
let obj = {a: 1, b: 2};
for (let cle in obj) {
    console.log(cle, obj[cle]);
}
```

### Contrôle de flux
```javascript
break;    // Sort de la boucle
continue; // Passe à l'itération suivante
return;   // Sort d'une fonction
```

---

## Fonctions

### Déclaration de fonction
```javascript
function saluer(nom) {
    return `Bonjour ${nom}`;
}
```

### Expression de fonction
```javascript
const saluer = function(nom) {
    return `Bonjour ${nom}`;
};
```

### Fonction flèche (Arrow function ES6)
```javascript
const saluer = (nom) => `Bonjour ${nom}`;
const additionner = (a, b) => a + b;
const multiple = (x) => {
    let resultat = x * 2;
    return resultat;
};

// Sans paramètre
const direBonjour = () => "Bonjour";
```

### Paramètres par défaut
```javascript
function saluer(nom = "Invité") {
    return `Bonjour ${nom}`;
}
```

### Rest parameters
```javascript
function somme(...nombres) {
    return nombres.reduce((acc, n) => acc + n, 0);
}
somme(1, 2, 3, 4); // 10
```

### Callback
```javascript
function traiter(callback) {
    callback();
}
traiter(() => console.log("Traité"));
```

### Fonctions d'ordre supérieur
```javascript
function multiplier(facteur) {
    return function(nombre) {
        return nombre * facteur;
    };
}
const double = multiplier(2);
double(5); // 10
```

### IIFE (Immediately Invoked Function Expression)
```javascript
(function() {
    console.log("Exécuté immédiatement");
})();
```

---

## Objets

### Création d'objets
```javascript
// Littéral
let personne = {
    nom: "Alice",
    age: 30,
    saluer: function() {
        return `Bonjour, je suis ${this.nom}`;
    }
};

// Avec new Object
let obj = new Object();
obj.propriete = "valeur";

// Object.create
let proto = {type: "Mammifère"};
let chien = Object.create(proto);
```

### Accès aux propriétés
```javascript
personne.nom;           // Notation par point
personne["age"];        // Notation par crochets
personne["cle-avec-tiret"];

// Propriété calculée
let cle = "nom";
personne[cle];          // "Alice"
```

### Shorthand properties (ES6)
```javascript
let nom = "Bob";
let age = 25;
let personne = {nom, age}; // {nom: "Bob", age: 25}
```

### Méthodes raccourcies (ES6)
```javascript
let obj = {
    methode() {
        return "valeur";
    }
};
```

### Destructuration
```javascript
let {nom, age} = personne;
let {nom: nouveauNom} = personne; // Renommer
let {ville = "Paris"} = personne; // Valeur par défaut
```

### Spread operator
```javascript
let obj1 = {a: 1, b: 2};
let obj2 = {...obj1, c: 3}; // {a: 1, b: 2, c: 3}
```

### Méthodes utiles
```javascript
Object.keys(obj);        // Tableau des clés
Object.values(obj);      // Tableau des valeurs
Object.entries(obj);     // Tableau [clé, valeur]
Object.assign({}, obj);  // Copier un objet
Object.freeze(obj);      // Rendre immuable
Object.seal(obj);        // Empêcher ajout/suppression
```

### Optional chaining (ES2020)
```javascript
obj?.propriete?.sousPropriete;
obj?.methode?.();
```

---

## Tableaux (Arrays)

### Création
```javascript
let arr = [1, 2, 3];
let arr2 = new Array(1, 2, 3);
let vide = new Array(5); // 5 emplacements vides
```

### Accès et modification
```javascript
arr[0];           // Premier élément
arr[arr.length - 1]; // Dernier élément
arr[1] = 10;      // Modification
```

### Méthodes de modification

#### Ajout/Suppression
```javascript
arr.push(4);         // Ajoute à la fin
arr.pop();           // Supprime le dernier
arr.unshift(0);      // Ajoute au début
arr.shift();         // Supprime le premier
arr.splice(1, 2);    // Supprime 2 éléments à partir de l'index 1
arr.splice(1, 0, "x"); // Insère "x" à l'index 1
```

#### Copie et concaténation
```javascript
let copie = arr.slice();      // Copie superficielle
let copie2 = arr.slice(1, 3); // Sous-tableau
let concat = arr.concat([4, 5]); // Concaténer
let spread = [...arr, 4, 5];  // Avec spread
```

### Méthodes d'itération

#### forEach
```javascript
arr.forEach((element, index) => {
    console.log(index, element);
});
```

#### map
```javascript
let doubles = arr.map(x => x * 2);
```

#### filter
```javascript
let pairs = arr.filter(x => x % 2 === 0);
```

#### reduce
```javascript
let somme = arr.reduce((acc, x) => acc + x, 0);
let produit = arr.reduce((acc, x) => acc * x, 1);
```

#### find / findIndex
```javascript
let premier = arr.find(x => x > 5);
let index = arr.findIndex(x => x > 5);
```

#### some / every
```javascript
arr.some(x => x > 5);   // Au moins un élément
arr.every(x => x > 0);  // Tous les éléments
```

#### sort
```javascript
arr.sort();                    // Tri alphabétique
arr.sort((a, b) => a - b);     // Tri numérique croissant
arr.sort((a, b) => b - a);     // Tri décroissant
```

#### reverse
```javascript
arr.reverse(); // Inverse l'ordre
```

### Méthodes de recherche
```javascript
arr.indexOf(2);          // Index de la première occurrence
arr.lastIndexOf(2);      // Index de la dernière occurrence
arr.includes(2);         // true si présent
```

### Méthodes modernes (ES2019+)
```javascript
arr.flat();              // Aplatit un niveau
arr.flat(2);             // Aplatit 2 niveaux
arr.flatMap(x => [x, x * 2]); // map puis flat
```

### Destructuration
```javascript
let [a, b, ...reste] = [1, 2, 3, 4, 5];
// a = 1, b = 2, reste = [3, 4, 5]
```

---

## Classes

### Déclaration de classe (ES6)
```javascript
class Personne {
    constructor(nom, age) {
        this.nom = nom;
        this.age = age;
    }
    
    saluer() {
        return `Bonjour, je suis ${this.nom}`;
    }
    
    // Getter
    get info() {
        return `${this.nom}, ${this.age} ans`;
    }
    
    // Setter
    set nouveauNom(nom) {
        this.nom = nom;
    }
    
    // Méthode statique
    static espece() {
        return "Homo sapiens";
    }
}

let alice = new Personne("Alice", 30);
```

### Héritage
```javascript
class Etudiant extends Personne {
    constructor(nom, age, ecole) {
        super(nom, age); // Appel du constructeur parent
        this.ecole = ecole;
    }
    
    etudier() {
        return `${this.nom} étudie à ${this.ecole}`;
    }
    
    // Override
    saluer() {
        return super.saluer() + ` et j'étudie à ${this.ecole}`;
    }
}
```

### Propriétés privées (ES2022)
```javascript
class Compte {
    #solde = 0; // Propriété privée
    
    deposer(montant) {
        this.#solde += montant;
    }
    
    get solde() {
        return this.#solde;
    }
}
```

---

## Asynchrone

### Callbacks
```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback("données");
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});
```

### Promises
```javascript
// Création
let promesse = new Promise((resolve, reject) => {
    if (succes) {
        resolve("Résultat");
    } else {
        reject("Erreur");
    }
});

// Utilisation
promesse
    .then(resultat => console.log(resultat))
    .catch(erreur => console.error(erreur))
    .finally(() => console.log("Terminé"));

// Chaînage
fetch(url)
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

### Promise.all / Promise.race
```javascript
// Attendre toutes les promesses
Promise.all([promesse1, promesse2, promesse3])
    .then(resultats => console.log(resultats));

// Première promesse résolue
Promise.race([promesse1, promesse2])
    .then(resultat => console.log(resultat));

// Promise.allSettled (ES2020)
Promise.allSettled([p1, p2, p3])
    .then(resultats => console.log(resultats));

// Promise.any (ES2021)
Promise.any([p1, p2, p3])
    .then(resultat => console.log(resultat));
```

### Async/Await (ES2017)
```javascript
async function fetchData() {
    try {
        let response = await fetch(url);
        let data = await response.json();
        return data;
    } catch (error) {
        console.error(error);
    }
}

// Utilisation
fetchData().then(data => console.log(data));

// Await dans une boucle
for (let item of items) {
    await traiter(item);
}

// Parallèle avec Promise.all
let resultats = await Promise.all([
    fetch(url1),
    fetch(url2),
    fetch(url3)
]);
```

---

## DOM Manipulation

### Sélection d'éléments
```javascript
// Ancien
document.getElementById("id");
document.getElementsByClassName("classe");
document.getElementsByTagName("div");

// Moderne (recommandé)
document.querySelector("#id");           // Premier élément
document.querySelectorAll(".classe");    // NodeList
```

### Création et modification
```javascript
// Créer un élément
let div = document.createElement("div");

// Modifier le contenu
element.textContent = "Texte";
element.innerHTML = "<span>HTML</span>";

// Attributs
element.getAttribute("href");
element.setAttribute("href", "url");
element.removeAttribute("href");
element.hasAttribute("href");

// Classes
element.classList.add("classe");
element.classList.remove("classe");
element.classList.toggle("classe");
element.classList.contains("classe");

// Styles
element.style.color = "red";
element.style.backgroundColor = "blue";
```

### Manipulation du DOM
```javascript
// Ajouter
parent.appendChild(enfant);
parent.append(enfant1, enfant2);      // Plusieurs éléments
parent.prepend(enfant);                // Au début
element.before(nouveau);               // Avant
element.after(nouveau);                // Après

// Supprimer
element.remove();
parent.removeChild(enfant);

// Remplacer
element.replaceWith(nouveau);
parent.replaceChild(nouveau, ancien);

// Cloner
let clone = element.cloneNode(true);  // true = deep clone
```

### Événements
```javascript
// Ajouter un écouteur
element.addEventListener("click", (e) => {
    console.log("Cliqué!", e);
});

// Supprimer un écouteur
element.removeEventListener("click", fonction);

// Événements courants
"click", "dblclick"
"mouseenter", "mouseleave", "mousemove"
"keydown", "keyup", "keypress"
"submit", "change", "input", "focus", "blur"
"load", "DOMContentLoaded"

// Propagation
e.stopPropagation();      // Arrête la propagation
e.preventDefault();        // Empêche l'action par défaut

// Délégation d'événements
parent.addEventListener("click", (e) => {
    if (e.target.matches(".item")) {
        console.log("Item cliqué");
    }
});
```

### Traverser le DOM
```javascript
element.parentElement;
element.children;              // HTMLCollection
element.firstElementChild;
element.lastElementChild;
element.nextElementSibling;
element.previousElementSibling;
element.closest(".classe");    // Ancêtre le plus proche
```

---

## Modules

### Export
```javascript
// Export nommé
export const variable = "valeur";
export function fonction() {}
export class Classe {}

// Export par défaut
export default function() {}

// Export multiple
export {var1, var2, fonction};

// Renommer lors de l'export
export {original as nouveau};
```

### Import
```javascript
// Import nommé
import {variable, fonction} from "./module.js";

// Import par défaut
import maFonction from "./module.js";

// Import tout
import * as Module from "./module.js";

// Renommer lors de l'import
import {original as nouveau} from "./module.js";

// Import dynamique
import("./module.js")
    .then(module => {
        module.fonction();
    });

// Async import
let module = await import("./module.js");
```

---

## Gestion d'erreurs

### try...catch...finally
```javascript
try {
    // Code qui peut échouer
    throw new Error("Erreur personnalisée");
} catch (error) {
    console.error(error.message);
    console.error(error.stack);
} finally {
    // Toujours exécuté
    console.log("Nettoyage");
}
```

### Types d'erreurs
```javascript
new Error("Message");
new TypeError("Type incorrect");
new ReferenceError("Variable non définie");
new SyntaxError("Syntaxe invalide");
new RangeError("Hors limites");
```

### Erreurs personnalisées
```javascript
class MonErreur extends Error {
    constructor(message) {
        super(message);
        this.name = "MonErreur";
    }
}

throw new MonErreur("Problème spécifique");
```

---

## Méthodes utiles

### String
```javascript
str.length;                    // Longueur
str.charAt(0);                 // Caractère à l'index
str.indexOf("x");              // Index de la première occurrence
str.lastIndexOf("x");          // Index de la dernière occurrence
str.includes("x");             // Contient la sous-chaîne
str.startsWith("x");           // Commence par
str.endsWith("x");             // Finit par
str.slice(0, 5);               // Sous-chaîne
str.substring(0, 5);           // Sous-chaîne (similaire)
str.substr(0, 5);              // Sous-chaîne (déprécié)
str.toUpperCase();             // Majuscules
str.toLowerCase();             // Minuscules
str.trim();                    // Supprimer espaces début/fin
str.trimStart() / trimEnd();   // Un seul côté
str.repeat(3);                 // Répéter
str.replace("a", "b");         // Remplacer première occurrence
str.replaceAll("a", "b");      // Remplacer toutes (ES2021)
str.split(",");                // Diviser en tableau
str.padStart(10, "0");         // Remplir au début
str.padEnd(10, "0");           // Remplir à la fin
str.match(/regex/);            // Regex
str.search(/regex/);           // Index de regex
```

### Number
```javascript
Number.parseInt("42");         // Convertir en entier
Number.parseFloat("3.14");     // Convertir en décimal
Number.isNaN(value);           // Est NaN
Number.isFinite(value);        // Est fini
Number.isInteger(value);       // Est entier
num.toFixed(2);                // Arrondir à 2 décimales
num.toPrecision(3);            // 3 chiffres significatifs
num.toString();                // Convertir en string
num.toExponential();           // Notation exponentielle
```

### Math
```javascript
Math.PI;                       // 3.14159...
Math.E;                        // 2.71828...
Math.abs(-5);                  // Valeur absolue
Math.ceil(4.3);                // Arrondi supérieur
Math.floor(4.8);               // Arrondi inférieur
Math.round(4.5);               // Arrondi standard
Math.trunc(4.8);               // Partie entière
Math.max(1, 2, 3);             // Maximum
Math.min(1, 2, 3);             // Minimum
Math.pow(2, 3);                // Puissance
Math.sqrt(16);                 // Racine carrée
Math.random();                 // Nombre aléatoire [0, 1)
Math.sign(-5);                 // Signe (-1, 0, 1)
```

### Date
```javascript
let maintenant = new Date();
let date = new Date("2024-01-01");
let date2 = new Date(2024, 0, 1); // Mois de 0 à 11

date.getFullYear();            // Année
date.getMonth();               // Mois (0-11)
date.getDate();                // Jour du mois (1-31)
date.getDay();                 // Jour de la semaine (0-6)
date.getHours();               // Heures
date.getMinutes();             // Minutes
date.getSeconds();             // Secondes
date.getMilliseconds();        // Millisecondes
date.getTime();                // Timestamp (ms depuis 1970)

date.setFullYear(2025);        // Modifier année
date.toISOString();            // Format ISO
date.toLocaleDateString();     // Format local
Date.now();                    // Timestamp actuel
```

### JSON
```javascript
JSON.stringify(obj);           // Objet → JSON string
JSON.stringify(obj, null, 2);  // Avec indentation
JSON.parse(jsonString);        // JSON string → Objet
```

### Console
```javascript
console.log("Message");
console.error("Erreur");
console.warn("Avertissement");
console.info("Information");
console.table([{a: 1}, {a: 2}]); // Tableau formaté
console.time("label");           // Démarrer chronomètre
console.timeEnd("label");        // Arrêter chronomètre
console.clear();                 // Nettoyer la console
console.assert(condition, "msg"); // Assertion
```

### Timers
```javascript
setTimeout(() => {}, 1000);    // Exécuter après délai
setInterval(() => {}, 1000);   // Exécuter périodiquement
clearTimeout(id);              // Annuler setTimeout
clearInterval(id);             // Annuler setInterval
```

### Local Storage
```javascript
localStorage.setItem("cle", "valeur");
localStorage.getItem("cle");
localStorage.removeItem("cle");
localStorage.clear();

// Session Storage (similaire mais temporaire)
sessionStorage.setItem("cle", "valeur");
```

### Regex
```javascript
let regex = /pattern/flags;
let regex2 = new RegExp("pattern", "flags");

// Flags
"g" // global
"i" // insensible à la casse
"m" // multiline
"s" // dotAll
"u" // unicode

// Méthodes
regex.test(str);              // true/false
regex.exec(str);              // Détails du match
str.match(regex);             // Matches
str.matchAll(regex);          // Tous les matches (itérateur)
str.replace(regex, "remplacement");
```

### Set (ES6)
```javascript
let set = new Set([1, 2, 3]);
set.add(4);
set.has(2);                   // true
set.delete(2);
set.size;                     // Nombre d'éléments
set.clear();

for (let val of set) {
    console.log(val);
}
```

### Map (ES6)
```javascript
let map = new Map();
map.set("cle", "valeur");
map.get("cle");
map.has("cle");
map.delete("cle");
map.size;
map.clear();

for (let [cle, valeur] of map) {
    console.log(cle, valeur);
}
```

### WeakSet et WeakMap
```javascript
// Ne permettent que des objets comme clés
// Permettent le garbage collection
let weakSet = new WeakSet();
let weakMap = new WeakMap();
```

---

## Fonctionnalités ES6+

### Template Literals
```javascript
let nom = "Alice";
let message = `Bonjour ${nom}!`;
let multi = `Ligne 1
Ligne 2
Ligne 3`;
```

### Destructuration avancée
```javascript
// Imbriquée
let {a: {b}} = {a: {b: 1}};

// Avec reste
let {a, ...reste} = {a: 1, b: 2, c: 3};

// Paramètres
function f({a, b = 2}) {
    return a + b;
}
```

### Valeurs par défaut
```javascript
function f(a = 1, b = 2) {
    return a + b;
}
```

### Computed property names
```javascript
let cle = "nom";
let obj = {
    [cle]: "Alice",
    [`${cle}Complet`]: "Alice Dupont"
};
```

### Symboles
```javascript
let sym = Symbol("description");
let obj = {
    [sym]: "valeur"
};
```

### Iterators et Generators
```javascript
// Generator
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

let g = gen();
g.next(); // {value: 1, done: false}

// Iterator personnalisé
let obj = {
    [Symbol.iterator]() {
        let i = 0;
        return {
            next: () => ({
                value: i++,
                done: i > 3
            })
        };
    }
};

for (let val of obj) {
    console.log(val); // 0, 1, 2
}
```

### Proxy (ES6)
```javascript
let obj = {nom: "Alice"};
let proxy = new Proxy(obj, {
    get(target, prop) {
        console.log(`Lecture de ${prop}`);
        return target[prop];
    },
    set(target, prop, value) {
        console.log(`Écriture de ${prop} = ${value}`);
        target[prop] = value;
        return true;
    }
});
```

### Reflect (ES6)
```javascript
Reflect.get(obj, "prop");
Reflect.set(obj, "prop", "valeur");
Reflect.has(obj, "prop");
Reflect.deleteProperty(obj, "prop");
```

---

## Bonnes pratiques

### Conventions de nommage
```javascript
// Variables et fonctions: camelCase
let maVariable;
function maFonction() {}

// Classes: PascalCase
class MaClasse {}

// Constantes: UPPER_SNAKE_CASE
const MA_CONSTANTE = "valeur";

// Privé: préfixer avec _
let _prive;
```

### Comparaisons strictes
```javascript
// Toujours utiliser === et !==
if (x === 5) {}
if (y !== null) {}
```

### Éviter var
```javascript
// Préférer const par défaut, let si nécessaire
const constante = "valeur";
let variable = "autre";
```

### Arrow functions et this
```javascript
// Les arrow functions n'ont pas leur propre this
let obj = {
    valeur: 42,
    methode: function() {
        setTimeout(() => {
            console.log(this.valeur); // 42
        }, 100);
    }
};
```

---

## Performance

### Éviter les fuites mémoire
```javascript
// Nettoyer les listeners
element.removeEventListener("click", handler);

// Nettoyer les timers
clearInterval(interval);
clearTimeout(timeout);

// Éviter les références circulaires
```

### Optimisations
```javascript
// Cache les sélections DOM
let element = document.querySelector("#id");

// Utiliser documentFragment pour DOM multiple
let fragment = document.createDocumentFragment();
items.forEach(item => {
    fragment.appendChild(createItem(item));
});
parent.appendChild(fragment);

// Debouncing
function debounce(func, delay) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => func.apply(this, args), delay);
    };
}

// Throttling
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

---

## Patterns utiles

### Module Pattern
```javascript
const Module = (function() {
    let prive = "privé";
    
    return {
        public: "public",
        methode() {
            return prive;
        }
    };
})();
```

### Singleton
```javascript
const Singleton = (function() {
    let instance;
    
    function createInstance() {
        return {
            propriete: "valeur"
        };
    }
    
    return {
        getInstance() {
            if (!instance) {
                instance = createInstance();
            }
            return instance;
        }
    };
})();
```

### Observer Pattern
```javascript
class Observable {
    constructor() {
        this.observers = [];
    }
    
    subscribe(fn) {
        this.observers.push(fn);
    }
    
    unsubscribe(fn) {
        this.observers = this.observers.filter(f => f !== fn);
    }
    
    notify(data) {
        this.observers.forEach(fn => fn(data));
    }
}
```

---

## Ressources

- [MDN Web Docs](https://developer.mozilla.org/fr/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [ECMAScript Specifications](https://tc39.es/ecma262/)

---

*Créé avec ❤️ pour la communauté JavaScript*

