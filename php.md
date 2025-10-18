# PHP Cheat Sheet

## Table des matières
1. [Bases et Syntaxe](#bases-et-syntaxe)
2. [Variables et Types](#variables-et-types)
3. [Opérateurs](#opérateurs)
4. [Structures de contrôle](#structures-de-contrôle)
5. [Fonctions](#fonctions)
6. [Tableaux (Arrays)](#tableaux-arrays)
7. [Programmation Orientée Objet](#programmation-orientée-objet)
8. [Nouveautés PHP 8.x](#nouveautés-php-8x)
9. [Gestion des erreurs](#gestion-des-erreurs)
10. [Manipulation de fichiers](#manipulation-de-fichiers)
11. [Base de données](#base-de-données)
12. [Sessions et Cookies](#sessions-et-cookies)
13. [Fonctions utiles](#fonctions-utiles)

---

## Bases et Syntaxe

### Tags PHP
```php
<?php
// Code PHP
?>

<?php
// Fichier PHP pur - pas de tag de fermeture recommandé

// Tag court (si short_open_tag activé)
<? echo "Hello"; ?>

// Echo court (toujours disponible)
<?= "Hello" ?>
```

### Commentaires
```php
<?php
// Commentaire sur une ligne

# Aussi un commentaire sur une ligne

/*
 * Commentaire
 * sur plusieurs
 * lignes
 */

/**
 * Commentaire de documentation
 * @param string $name
 * @return void
 */
```

### Affichage
```php
<?php
echo "Hello World";
echo "Hello", " ", "World"; // Multiple arguments

print "Hello World"; // Retourne 1

// Variables dans les strings
$name = "Alice";
echo "Bonjour $name";
echo "Bonjour {$name}";
echo 'Bonjour $name'; // Pas d\'interpolation avec quotes simples

// Printf
printf("Bonjour %s", $name);
printf("Nombre: %d, Float: %.2f", 42, 3.14159);

// Var_dump pour debug
var_dump($variable);
print_r($array);
```

---

## Variables et Types

### Déclaration de variables
```php
<?php
$variable = "valeur"; // Commence par $
$nombre = 42;
$_variable = "ok"; // Underscore autorisé
// $2variable = "error"; // Ne peut pas commencer par un chiffre

// Variables variables
$nom = "variable";
$$nom = "valeur"; // $variable = "valeur"

// Constantes
define("MA_CONSTANTE", "valeur");
const AUTRE_CONSTANTE = "valeur"; // PHP 5.3+

// Constantes magiques
__LINE__      // Numéro de ligne
__FILE__      // Chemin du fichier
__DIR__       // Dossier du fichier
__FUNCTION__  // Nom de la fonction
__CLASS__     // Nom de la classe
__METHOD__    // Nom de la méthode
__NAMESPACE__ // Nom du namespace
```

### Types de données

#### Scalaires
```php
<?php
// String
$string = "texte";
$string = 'texte';
$heredoc = <<<EOT
Texte sur
plusieurs lignes
avec variables $var
EOT;

$nowdoc = <<<'EOT'
Texte sans
interpolation de $var
EOT;

// Integer
$int = 42;
$hex = 0x1A;    // Hexadécimal
$octal = 0123;  // Octal
$binary = 0b1010; // Binaire
$underscore = 1_000_000; // PHP 7.4+ (lisibilité)

// Float
$float = 3.14;
$scientific = 1.2e3;
$underscore_float = 1_234.567; // PHP 7.4+

// Boolean
$vrai = true;
$faux = false;

// Null
$null = null;
```

#### Types composés
```php
<?php
// Array
$array = array(1, 2, 3);
$array = [1, 2, 3]; // PHP 5.4+

// Object
$obj = new stdClass();
$obj->propriete = "valeur";

// Callable
$callable = function() { return "hello"; };

// Resource (fichiers, connexions DB, etc.)
$file = fopen("file.txt", "r");

// Mixed (PHP 8.0+)
function test(mixed $param): mixed {}
```

### Vérification de types
```php
<?php
// is_* fonctions
is_string($var);
is_int($var);
is_float($var);
is_bool($var);
is_array($var);
is_object($var);
is_null($var);
is_numeric($var);
is_callable($var);
is_resource($var);

// gettype
gettype($var); // "string", "integer", etc.

// Type casting
(string) $var;
(int) $var;
(float) $var;
(bool) $var;
(array) $var;
(object) $var;

// Type juggling
$result = "10" + 5; // 15 (string converti en int)
```

### Type declarations (PHP 7+)
```php
<?php
// Scalar types (strict_types=1 pour mode strict)
declare(strict_types=1);

function addition(int $a, int $b): int {
    return $a + $b;
}

// Nullable types (PHP 7.1+)
function test(?string $param): ?int {
    return $param ? strlen($param) : null;
}

// Union types (PHP 8.0+)
function process(int|float $number): int|float {
    return $number * 2;
}

// Intersection types (PHP 8.1+)
function test(Countable&ArrayAccess $param) {}

// Never type (PHP 8.1+)
function terminate(): never {
    exit;
}

// Mixed type (PHP 8.0+)
function anything(mixed $param): mixed {
    return $param;
}
```

---

## Opérateurs

### Opérateurs arithmétiques
```php
<?php
$a = 10;
$b = 3;

$a + $b;  // 13 Addition
$a - $b;  // 7  Soustraction
$a * $b;  // 30 Multiplication
$a / $b;  // 3.333... Division
$a % $b;  // 1  Modulo
$a ** $b; // 1000 Exponentiation (PHP 5.6+)

$a++;     // Post-incrémentation
++$a;     // Pré-incrémentation
$a--;     // Post-décrémentation
--$a;     // Pré-décrémentation

// Integer division (PHP 8.0+)
intdiv(10, 3); // 3
```

### Opérateurs d'affectation
```php
<?php
$x = 10;
$x += 5;   // $x = $x + 5
$x -= 3;   // $x = $x - 3
$x *= 2;   // $x = $x * 2
$x /= 4;   // $x = $x / 4
$x %= 3;   // $x = $x % 3
$x **= 2;  // $x = $x ** 2
$x .= "text"; // Concaténation

// Null coalescing assignment (PHP 7.4+)
$x ??= 10; // $x = $x ?? 10
```

### Opérateurs de comparaison
```php
<?php
// Égalité
5 == "5";   // true (avec conversion)
5 === "5";  // false (strict)
5 != "5";   // false
5 !== "5";  // true
5 <> "5";   // false (alternative à !=)

// Comparaisons
10 > 5;     // true
10 < 5;     // false
10 >= 10;   // true
10 <= 5;    // false

// Spaceship operator (PHP 7+)
1 <=> 2;    // -1 (inférieur)
2 <=> 2;    // 0  (égal)
3 <=> 2;    // 1  (supérieur)
```

### Opérateurs logiques
```php
<?php
$a = true;
$b = false;

$a && $b;   // false (AND)
$a and $b;  // false (AND, priorité basse)
$a || $b;   // true  (OR)
$a or $b;   // true  (OR, priorité basse)
!$a;        // false (NOT)
$a xor $b;  // true  (XOR)
```

### Opérateurs sur les strings
```php
<?php
$a = "Hello";
$b = "World";

$a . " " . $b;  // "Hello World" (concaténation)
$a .= " World"; // $a = $a . " World"
```

### Opérateurs sur les tableaux
```php
<?php
$a = [1, 2];
$b = [3, 4];

$a + $b;    // Union
$a == $b;   // Égalité
$a === $b;  // Égalité stricte
$a != $b;   // Différent
$a !== $b;  // Différent strict
```

### Null coalescing (PHP 7+)
```php
<?php
$value = $var ?? "default"; // Si $var est null ou n'existe pas

// Chaîné
$value = $a ?? $b ?? $c ?? "default";

// Null safe operator (PHP 8.0+)
$result = $obj?->method()?->property;
```

### Opérateur ternaire
```php
<?php
$result = $condition ? "vrai" : "faux";

// Ternaire court (PHP 5.3+)
$result = $var ?: "default"; // Si $var est falsy
```

---

## Structures de contrôle

### Conditions

#### if...elseif...else
```php
<?php
if ($condition) {
    // code
} elseif ($autre) {
    // autre code
} else {
    // défaut
}

// Syntaxe alternative
if ($condition):
    // code
elseif ($autre):
    // autre
else:
    // défaut
endif;
```

#### switch
```php
<?php
switch ($var) {
    case 1:
        echo "Un";
        break;
    case 2:
    case 3:
        echo "Deux ou Trois";
        break;
    default:
        echo "Autre";
}

// Match expression (PHP 8.0+)
$result = match($var) {
    1 => "Un",
    2, 3 => "Deux ou Trois",
    default => "Autre"
};

// Match avec conditions
$result = match(true) {
    $age < 18 => "Mineur",
    $age < 65 => "Adulte",
    default => "Sénior"
};
```

### Boucles

#### for
```php
<?php
for ($i = 0; $i < 10; $i++) {
    echo $i;
}

// Syntaxe alternative
for ($i = 0; $i < 10; $i++):
    echo $i;
endfor;
```

#### while
```php
<?php
$i = 0;
while ($i < 10) {
    echo $i;
    $i++;
}

// Syntaxe alternative
while ($i < 10):
    echo $i;
    $i++;
endwhile;
```

#### do...while
```php
<?php
$i = 0;
do {
    echo $i;
    $i++;
} while ($i < 10);
```

#### foreach
```php
<?php
// Tableau indexé
foreach ($array as $value) {
    echo $value;
}

// Tableau associatif
foreach ($array as $key => $value) {
    echo "$key => $value";
}

// Par référence
foreach ($array as &$value) {
    $value *= 2;
}
unset($value); // Bonne pratique

// Syntaxe alternative
foreach ($array as $value):
    echo $value;
endforeach;
```

### Contrôle de flux
```php
<?php
break;       // Sort de la boucle
continue;    // Passe à l'itération suivante
break 2;     // Sort de 2 niveaux de boucles
continue 2;  // Continue 2 niveaux au-dessus
return;      // Sort de la fonction
exit;        // Termine le script
die();       // Alias de exit

// goto (déconseillé)
goto label;
label:
echo "Code";
```

---

## Fonctions

### Déclaration de fonctions
```php
<?php
function saluer($nom) {
    return "Bonjour $nom";
}

// Appel
echo saluer("Alice");
```

### Paramètres

#### Paramètres par défaut
```php
<?php
function saluer($nom = "Invité", $langue = "fr") {
    return "Bonjour $nom";
}

saluer(); // "Bonjour Invité"
saluer("Alice"); // "Bonjour Alice"
```

#### Type hinting (PHP 7+)
```php
<?php
function addition(int $a, int $b): int {
    return $a + $b;
}

function traiter(array $data, ?string $option = null): bool {
    return true;
}

// Union types (PHP 8.0+)
function process(int|float|string $value): mixed {
    return $value;
}
```

#### Passage par référence
```php
<?php
function incrementer(&$valeur) {
    $valeur++;
}

$x = 5;
incrementer($x);
echo $x; // 6
```

#### Variadic functions (PHP 5.6+)
```php
<?php
function somme(...$nombres) {
    return array_sum($nombres);
}

somme(1, 2, 3, 4); // 10

// Unpacking (PHP 5.6+)
$array = [1, 2, 3];
somme(...$array);

// Named arguments (PHP 8.0+)
function creerUtilisateur($nom, $age, $email) {
    // ...
}

creerUtilisateur(
    email: "alice@example.com",
    nom: "Alice",
    age: 30
);
```

### Fonctions anonymes (Closures)
```php
<?php
$saluer = function($nom) {
    return "Bonjour $nom";
};

echo $saluer("Alice");

// Closure avec use
$multiplicateur = 2;
$multiplier = function($n) use ($multiplicateur) {
    return $n * $multiplicateur;
};

// Par référence
$multiplier = function($n) use (&$multiplicateur) {
    return $n * $multiplicateur++;
};

// Arrow functions (PHP 7.4+)
$double = fn($x) => $x * 2;
$saluer = fn($nom) => "Bonjour $nom";

// Avec variables du scope parent (auto use)
$multiplicateur = 2;
$multiplier = fn($n) => $n * $multiplicateur;
```

### Fonctions de première classe (PHP 8.1+)
```php
<?php
class Calculatrice {
    public function addition($a, $b) {
        return $a + $b;
    }
}

// Créer une closure à partir d'une méthode
$calc = new Calculatrice();
$additionner = $calc->addition(...);
$additionner(2, 3); // 5

// Fonction statique
$fonction = strlen(...);
$fonction("hello"); // 5
```

### Fonctions générateurs
```php
<?php
function genererNombres($max) {
    for ($i = 0; $i < $max; $i++) {
        yield $i;
    }
}

foreach (genererNombres(5) as $nombre) {
    echo $nombre;
}

// Yield avec clé
function genererPaires() {
    yield "a" => 1;
    yield "b" => 2;
}

// Yield from (PHP 7+)
function genererTout() {
    yield from [1, 2, 3];
    yield from genererNombres(3);
}
```

---

## Tableaux (Arrays)

### Création de tableaux
```php
<?php
// Tableau indexé
$fruits = array("Pomme", "Banane", "Orange");
$fruits = ["Pomme", "Banane", "Orange"]; // PHP 5.4+

// Tableau associatif
$personne = array(
    "nom" => "Alice",
    "age" => 30
);
$personne = [
    "nom" => "Alice",
    "age" => 30
];

// Tableau multidimensionnel
$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
```

### Accès et modification
```php
<?php
// Accès
$fruits[0];           // Premier élément
$fruits[count($fruits) - 1]; // Dernier
$personne["nom"];     // Par clé

// Modification
$fruits[0] = "Fraise";
$personne["age"] = 31;

// Ajout
$fruits[] = "Kiwi";   // À la fin
array_push($fruits, "Mangue");
array_unshift($fruits, "Cerise"); // Au début

// Suppression
unset($fruits[0]);
array_pop($fruits);   // Dernier
array_shift($fruits); // Premier
```

### Fonctions de tableaux

#### Informations
```php
<?php
count($array);        // Nombre d'éléments
sizeof($array);       // Alias de count
empty($array);        // Vide ou non
isset($array[$key]);  // Clé existe
in_array($value, $array); // Valeur existe
array_key_exists($key, $array); // Clé existe
```

#### Manipulation
```php
<?php
// Fusion
$result = array_merge($arr1, $arr2);
$result = $arr1 + $arr2; // Conserve les clés

// Slice
$slice = array_slice($array, 1, 3); // Offset 1, longueur 3

// Splice (modifie le tableau)
array_splice($array, 1, 2, ["new"]); // Remplace

// Split
$chunks = array_chunk($array, 2); // Divise en morceaux

// Unique
$unique = array_unique($array);

// Reverse
$reversed = array_reverse($array);

// Flip (échange clés et valeurs)
$flipped = array_flip($array);

// Keys et values
$keys = array_keys($array);
$values = array_values($array);

// Combine
$combined = array_combine($keys, $values);

// Pad
$padded = array_pad($array, 5, 0); // Remplit jusqu'à 5 éléments
```

#### Tri
```php
<?php
// Tri de base
sort($array);         // Croissant (ré-indexe)
rsort($array);        // Décroissant
asort($array);        // Croissant (garde clés)
arsort($array);       // Décroissant (garde clés)
ksort($array);        // Par clés croissant
krsort($array);       // Par clés décroissant

// Tri naturel
natsort($array);      // Tri naturel
natcasesort($array);  // Tri naturel insensible à la casse

// Tri personnalisé
usort($array, function($a, $b) {
    return $a <=> $b;
});

// Tri multidimensionnel
array_multisort($array1, SORT_ASC, $array2, SORT_DESC);

// Shuffle
shuffle($array);      // Mélange aléatoire
```

#### Recherche et filtre
```php
<?php
// Recherche
$key = array_search($value, $array);
array_key_first($array); // PHP 7.3+
array_key_last($array);  // PHP 7.3+

// Filter
$filtered = array_filter($array, function($item) {
    return $item > 5;
});

// Map
$mapped = array_map(function($item) {
    return $item * 2;
}, $array);

// Map avec plusieurs tableaux
$result = array_map(function($a, $b) {
    return $a + $b;
}, $array1, $array2);

// Reduce
$sum = array_reduce($array, function($carry, $item) {
    return $carry + $item;
}, 0);

// Walk (modifie en place)
array_walk($array, function(&$value, $key) {
    $value *= 2;
});
```

#### Extraction
```php
<?php
// Extract variables from array
extract($array); // Crée des variables

// Compact (inverse de extract)
$nom = "Alice";
$age = 30;
$array = compact("nom", "age"); // ["nom" => "Alice", "age" => 30]

// List (destructuration)
list($a, $b, $c) = [1, 2, 3];
[$a, $b, $c] = [1, 2, 3]; // PHP 7.1+

// Avec clés (PHP 7.1+)
["nom" => $nom, "age" => $age] = $personne;

// Skip elements
[, , $c] = [1, 2, 3]; // $c = 3
```

#### Opérations ensemblistes
```php
<?php
// Différence
$diff = array_diff($arr1, $arr2);
$diff = array_diff_key($arr1, $arr2);

// Intersection
$inter = array_intersect($arr1, $arr2);
$inter = array_intersect_key($arr1, $arr2);

// Union
$union = $arr1 + $arr2;
```

#### Spread operator (PHP 7.4+)
```php
<?php
$arr1 = [1, 2, 3];
$arr2 = [4, 5, 6];
$merged = [...$arr1, ...$arr2]; // [1, 2, 3, 4, 5, 6]

// Avec clés (PHP 8.1+)
$arr = ["a" => 1, "b" => 2];
$merged = [...$arr, "c" => 3];
```

---

## Programmation Orientée Objet

### Classes et objets

#### Déclaration de classe
```php
<?php
class Personne {
    // Propriétés
    public $nom;
    protected $age;
    private $email;
    
    // Constante
    const ESPECE = "Homo sapiens";
    
    // Constructeur
    public function __construct($nom, $age, $email) {
        $this->nom = $nom;
        $this->age = $age;
        $this->email = $email;
    }
    
    // Méthode
    public function saluer() {
        return "Bonjour, je suis {$this->nom}";
    }
    
    // Getter
    public function getAge() {
        return $this->age;
    }
    
    // Setter
    public function setAge($age) {
        if ($age > 0) {
            $this->age = $age;
        }
    }
    
    // Méthode statique
    public static function getEspece() {
        return self::ESPECE;
    }
    
    // Destructeur
    public function __destruct() {
        // Nettoyage
    }
}

// Instanciation
$alice = new Personne("Alice", 30, "alice@example.com");
echo $alice->saluer();
echo Personne::ESPECE;
echo Personne::getEspece();
```

#### Visibilité
```php
<?php
class Exemple {
    public $public;       // Accessible partout
    protected $protected; // Classe et enfants
    private $private;     // Seulement cette classe
    
    public function methodePublique() {}
    protected function methodeProtegee() {}
    private function methodePrivee() {}
}
```

#### Héritage
```php
<?php
class Etudiant extends Personne {
    private $ecole;
    
    public function __construct($nom, $age, $email, $ecole) {
        parent::__construct($nom, $age, $email);
        $this->ecole = $ecole;
    }
    
    // Override
    public function saluer() {
        return parent::saluer() . " et j'étudie à {$this->ecole}";
    }
    
    // Final (ne peut pas être overridée)
    final public function etudier() {
        return "J'étudie";
    }
}

// Classe finale (ne peut pas être héritée)
final class ClasseFinale {
    // ...
}
```

#### Classe abstraite
```php
<?php
abstract class Animal {
    abstract public function faireDuBruit();
    
    public function dormir() {
        return "Zzz...";
    }
}

class Chien extends Animal {
    public function faireDuBruit() {
        return "Woof!";
    }
}
```

#### Interfaces
```php
<?php
interface Volant {
    public function voler();
}

interface Nageur {
    public function nager();
}

class Canard implements Volant, Nageur {
    public function voler() {
        return "Le canard vole";
    }
    
    public function nager() {
        return "Le canard nage";
    }
}
```

#### Traits (PHP 5.4+)
```php
<?php
trait Logger {
    public function log($message) {
        echo "[LOG] $message\n";
    }
}

trait Timer {
    public function startTimer() {
        $this->startTime = time();
    }
}

class MaClasse {
    use Logger, Timer;
    
    // Résolution de conflit
    use TraitA, TraitB {
        TraitA::methode insteadof TraitB;
        TraitB::methode as methodeB;
    }
}

$obj = new MaClasse();
$obj->log("Message");
```

#### Propriétés et méthodes magiques
```php
<?php
class MaClasse {
    private $data = [];
    
    // Getter magique
    public function __get($name) {
        return $this->data[$name] ?? null;
    }
    
    // Setter magique
    public function __set($name, $value) {
        $this->data[$name] = $value;
    }
    
    // isset magique
    public function __isset($name) {
        return isset($this->data[$name]);
    }
    
    // unset magique
    public function __unset($name) {
        unset($this->data[$name]);
    }
    
    // Appel de méthode magique
    public function __call($name, $arguments) {
        echo "Méthode $name appelée";
    }
    
    // Appel de méthode statique magique
    public static function __callStatic($name, $arguments) {
        echo "Méthode statique $name appelée";
    }
    
    // toString
    public function __toString() {
        return "MaClasse instance";
    }
    
    // Invoke (objet comme fonction)
    public function __invoke($param) {
        return "Invoqué avec $param";
    }
    
    // Clone
    public function __clone() {
        // Logique de clonage
    }
    
    // Serialize
    public function __serialize(): array {
        return $this->data;
    }
    
    public function __unserialize(array $data): void {
        $this->data = $data;
    }
}

$obj = new MaClasse();
$obj->propriete = "valeur"; // __set
echo $obj->propriete;       // __get
echo $obj;                  // __toString
$obj("param");              // __invoke
```

#### Namespaces (PHP 5.3+)
```php
<?php
namespace MonProjet\Models;

class Utilisateur {
    // ...
}

// Utilisation
use MonProjet\Models\Utilisateur;
use MonProjet\Models\Utilisateur as User;
use function MonProjet\Utils\helper;
use const MonProjet\Constants\VERSION;

$user = new Utilisateur();
$user = new \MonProjet\Models\Utilisateur(); // Nom complet

// Namespace actuel
namespace MonProjet {
    // Code
}

namespace {
    // Namespace global
}
```

#### Type hinting pour objets
```php
<?php
function traiter(Personne $personne): Personne {
    return $personne;
}

// Self, parent, static
class Base {
    public function getInstance(): self {
        return new self();
    }
    
    public function getStatic(): static {
        return new static();
    }
}
```

---

## Nouveautés PHP 8.x

### Named Arguments (PHP 8.0)
```php
<?php
function creerUtilisateur($nom, $age, $email, $actif = true) {
    // ...
}

// Arguments nommés (ordre n'a pas d'importance)
creerUtilisateur(
    email: "alice@example.com",
    nom: "Alice",
    age: 30
);
```

### Attributes (PHP 8.0)
```php
<?php
#[Route("/api/users")]
class UserController {
    #[Route("/create", method: "POST")]
    #[Authorize("admin")]
    public function create() {
        // ...
    }
}

// Définir un attribute
#[Attribute]
class Route {
    public function __construct(
        public string $path,
        public string $method = "GET"
    ) {}
}

// Lire les attributes
$reflection = new ReflectionClass(UserController::class);
$attributes = $reflection->getAttributes(Route::class);
```

### Constructor Property Promotion (PHP 8.0)
```php
<?php
// Ancien
class Personne {
    public string $nom;
    public int $age;
    
    public function __construct(string $nom, int $age) {
        $this->nom = $nom;
        $this->age = $age;
    }
}

// Nouveau (PHP 8.0+)
class Personne {
    public function __construct(
        public string $nom,
        public int $age
    ) {}
}
```

### Union Types (PHP 8.0)
```php
<?php
function process(int|float $number): int|float {
    return $number * 2;
}

function test(string|null $param): string|null {
    return $param;
}

// Ou utiliser ?string pour nullable
function test(?string $param): ?string {
    return $param;
}
```

### Match Expression (PHP 8.0)
```php
<?php
// Plus strict que switch (===)
$result = match($value) {
    1 => "Un",
    2 => "Deux",
    3, 4 => "Trois ou Quatre",
    default => "Autre"
};

// Avec expressions
$result = match(true) {
    $age < 18 => "Mineur",
    $age >= 18 && $age < 65 => "Adulte",
    default => "Sénior"
};

// Retourne une valeur (pas de break nécessaire)
```

### Nullsafe Operator (PHP 8.0)
```php
<?php
// Ancien
$pays = null;
if ($utilisateur !== null) {
    $adresse = $utilisateur->getAdresse();
    if ($adresse !== null) {
        $pays = $adresse->getPays();
    }
}

// Nouveau
$pays = $utilisateur?->getAdresse()?->getPays();
```

### Enumerations (PHP 8.1)
```php
<?php
// Enum simple
enum Statut {
    case Brouillon;
    case Publie;
    case Archive;
}

$statut = Statut::Publie;

// Backed enum
enum Statut: string {
    case Brouillon = 'draft';
    case Publie = 'published';
    case Archive = 'archived';
}

$statut = Statut::Publie;
echo $statut->value; // "published"
$statut = Statut::from('published');

// Avec méthodes
enum Statut: string {
    case Brouillon = 'draft';
    case Publie = 'published';
    
    public function libelle(): string {
        return match($this) {
            self::Brouillon => "Brouillon",
            self::Publie => "Publié",
        };
    }
}
```

### Readonly Properties (PHP 8.1)
```php
<?php
class Utilisateur {
    public function __construct(
        public readonly string $nom,
        public readonly int $age
    ) {}
}

$user = new Utilisateur("Alice", 30);
// $user->nom = "Bob"; // Erreur!

// Readonly class (PHP 8.2)
readonly class Configuration {
    public function __construct(
        public string $host,
        public int $port
    ) {}
}
```

### Intersection Types (PHP 8.1)
```php
<?php
function process(Countable&ArrayAccess $data): void {
    // $data doit implémenter les deux interfaces
}
```

### Never Type (PHP 8.1)
```php
<?php
function terminate(): never {
    exit;
}

function throwError(): never {
    throw new Exception("Erreur");
}
```

### First-class Callable Syntax (PHP 8.1)
```php
<?php
$fn = strlen(...);
$fn("hello"); // 5

// Méthode
$fn = $object->method(...);

// Méthode statique
$fn = MyClass::staticMethod(...);
```

### New in Initializers (PHP 8.1)
```php
<?php
class Service {
    public function __construct(
        private Logger $logger = new Logger()
    ) {}
}
```

### Final Class Constants (PHP 8.1)
```php
<?php
class Parent {
    final public const CONSTANTE = "valeur";
}

class Enfant extends Parent {
    // public const CONSTANTE = "autre"; // Erreur!
}
```

### Readonly Classes (PHP 8.2)
```php
<?php
readonly class Config {
    public function __construct(
        public string $host,
        public int $port
    ) {}
}
```

### Disjunctive Normal Form (DNF) Types (PHP 8.2)
```php
<?php
function process((A&B)|C $param): void {
    // $param est soit (A ET B) soit C
}
```

### Constants in Traits (PHP 8.2)
```php
<?php
trait MonTrait {
    public const CONSTANTE = "valeur";
}
```

### Dynamic Properties Deprecated (PHP 8.2)
```php
<?php
// Déprécié en 8.2
$obj = new stdClass();
$obj->nouvelle = "valeur"; // Warning

// Utiliser #[AllowDynamicProperties]
#[AllowDynamicProperties]
class MaClasse {
    // Propriétés dynamiques autorisées
}
```

### Fetch Enum Properties in const expressions (PHP 8.3)
```php
<?php
enum Statut: string {
    case Actif = 'actif';
}

class Config {
    const STATUT_DEFAULT = Statut::Actif->value; // PHP 8.3+
}
```

### Override Attribute (PHP 8.3)
```php
<?php
class Parent {
    public function methode(): void {}
}

class Enfant extends Parent {
    #[Override]
    public function methode(): void {
        // S'assure que la méthode existe dans le parent
    }
}
```

### Typed Class Constants (PHP 8.3)
```php
<?php
class Config {
    public const string HOST = "localhost";
    public const int PORT = 3306;
}
```

---

## Gestion des erreurs

### Try...Catch...Finally
```php
<?php
try {
    // Code qui peut échouer
    throw new Exception("Erreur");
} catch (Exception $e) {
    echo "Erreur: " . $e->getMessage();
    echo $e->getFile();
    echo $e->getLine();
    echo $e->getTrace();
    echo $e->getTraceAsString();
} finally {
    // Toujours exécuté
    echo "Nettoyage";
}

// Multiple catch
try {
    // ...
} catch (InvalidArgumentException $e) {
    // Gérer InvalidArgumentException
} catch (RuntimeException $e) {
    // Gérer RuntimeException
} catch (Exception $e) {
    // Gérer toutes les autres
}

// Catch multiple types (PHP 7.1+)
try {
    // ...
} catch (InvalidArgumentException | RuntimeException $e) {
    // Gérer les deux types
}

// Non-capturing catches (PHP 8.0+)
try {
    // ...
} catch (Exception) {
    // Pas besoin de la variable
}
```

### Exceptions personnalisées
```php
<?php
class MonException extends Exception {
    public function __construct($message, $code = 0, Throwable $previous = null) {
        parent::__construct($message, $code, $previous);
    }
    
    public function __toString() {
        return __CLASS__ . ": [{$this->code}]: {$this->message}\n";
    }
}

throw new MonException("Erreur personnalisée");
```

### Hiérarchie des exceptions
```php
<?php
// Exception de base
class AppException extends Exception {}

// Exceptions spécifiques
class ValidationException extends AppException {}
class DatabaseException extends AppException {}
class AuthException extends AppException {}

try {
    throw new ValidationException("Validation échouée");
} catch (ValidationException $e) {
    // Gérer validation
} catch (AppException $e) {
    // Gérer autres exceptions de l'app
}
```

### Error Handling
```php
<?php
// set_error_handler
set_error_handler(function($errno, $errstr, $errfile, $errline) {
    throw new ErrorException($errstr, 0, $errno, $errfile, $errline);
});

// set_exception_handler
set_exception_handler(function($exception) {
    echo "Exception non gérée: " . $exception->getMessage();
});

// Erreurs fatales (PHP 7+)
register_shutdown_function(function() {
    $error = error_get_last();
    if ($error !== null && $error['type'] === E_ERROR) {
        // Gérer l'erreur fatale
    }
});

// Error (PHP 7+)
try {
    undefinedFunction();
} catch (Error $e) {
    echo "Erreur: " . $e->getMessage();
}

// Throwable (PHP 7+) - catch tout
try {
    // ...
} catch (Throwable $e) {
    // Catch Exception ET Error
}
```

### Assert
```php
<?php
assert($value > 0, "La valeur doit être positive");

// Avec exception (PHP 7+)
assert($value > 0, new InvalidArgumentException("Invalide"));

// Configuration
assert_options(ASSERT_ACTIVE, 1);
assert_options(ASSERT_WARNING, 0);
assert_options(ASSERT_EXCEPTION, 1);
```

---

## Manipulation de fichiers

### Lecture de fichiers
```php
<?php
// Lire tout le fichier
$contenu = file_get_contents("fichier.txt");

// Lire dans un tableau (ligne par ligne)
$lignes = file("fichier.txt");
$lignes = file("fichier.txt", FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);

// Lecture avec fopen
$handle = fopen("fichier.txt", "r");
if ($handle) {
    while (($ligne = fgets($handle)) !== false) {
        echo $ligne;
    }
    fclose($handle);
}

// Lire un nombre de caractères
$data = fread($handle, 1024);

// Lire jusqu'à la fin
$contenu = fread($handle, filesize("fichier.txt"));

// Lire caractère par caractère
$char = fgetc($handle);

// Lire CSV
$handle = fopen("data.csv", "r");
while (($data = fgetcsv($handle, 1000, ",")) !== false) {
    print_r($data);
}
fclose($handle);
```

### Écriture de fichiers
```php
<?php
// Écrire tout le contenu (écrase)
file_put_contents("fichier.txt", "Contenu");

// Ajouter à la fin
file_put_contents("fichier.txt", "Contenu", FILE_APPEND);

// Écriture avec fopen
$handle = fopen("fichier.txt", "w");
fwrite($handle, "Contenu");
fputs($handle, "Autre ligne\n"); // Alias de fwrite
fclose($handle);

// Écrire CSV
$handle = fopen("data.csv", "w");
fputcsv($handle, ["col1", "col2", "col3"]);
fclose($handle);
```

### Modes d'ouverture
```php
<?php
"r"   // Lecture seule
"r+"  // Lecture/écriture, début du fichier
"w"   // Écriture seule, écrase le fichier
"w+"  // Lecture/écriture, écrase le fichier
"a"   // Écriture seule, ajout à la fin
"a+"  // Lecture/écriture, ajout à la fin
"x"   // Création et écriture, erreur si existe
"x+"  // Création et lecture/écriture
"c"   // Écriture seule, ne tronque pas
"c+"  // Lecture/écriture, ne tronque pas
```

### Manipulation de fichiers
```php
<?php
// Vérifications
file_exists("fichier.txt");
is_file("fichier.txt");
is_dir("dossier");
is_readable("fichier.txt");
is_writable("fichier.txt");
is_executable("script.sh");

// Informations
filesize("fichier.txt");
filemtime("fichier.txt"); // Dernière modification
fileatime("fichier.txt"); // Dernier accès
filectime("fichier.txt"); // Changement inode
filetype("fichier.txt");
mime_content_type("fichier.txt");

// Opérations
copy("source.txt", "destination.txt");
rename("ancien.txt", "nouveau.txt");
unlink("fichier.txt"); // Supprimer
chmod("fichier.txt", 0644);

// Création de dossier
mkdir("dossier", 0755, true); // true = récursif

// Supprimer dossier
rmdir("dossier"); // Doit être vide

// Lister dossier
$fichiers = scandir("dossier");
$fichiers = glob("*.txt"); // Avec pattern

// Iterator
$dir = new DirectoryIterator("dossier");
foreach ($dir as $fileinfo) {
    if (!$fileinfo->isDot()) {
        echo $fileinfo->getFilename();
    }
}

// Recursive iterator
$iterator = new RecursiveIteratorIterator(
    new RecursiveDirectoryIterator("dossier")
);
foreach ($iterator as $file) {
    if ($file->isFile()) {
        echo $file->getPathname();
    }
}
```

### Chemins
```php
<?php
// Informations sur le chemin
basename("/path/to/file.txt");      // "file.txt"
dirname("/path/to/file.txt");       // "/path/to"
pathinfo("/path/to/file.txt");      // Array avec dirname, basename, extension, filename

$info = pathinfo("/path/to/file.txt");
$info['dirname'];    // "/path/to"
$info['basename'];   // "file.txt"
$info['extension'];  // "txt"
$info['filename'];   // "file"

// Chemin absolu
realpath("../file.txt");

// Dossier de travail
getcwd();
chdir("/nouveau/dossier");

// Dossier temporaire
sys_get_temp_dir();
tempnam(sys_get_temp_dir(), "prefix");
```

### Upload de fichiers
```php
<?php
// Vérifier l'upload
if (isset($_FILES['fichier'])) {
    $fichier = $_FILES['fichier'];
    
    // Informations
    $nom = $fichier['name'];
    $type = $fichier['type'];
    $taille = $fichier['size'];
    $tmp = $fichier['tmp_name'];
    $erreur = $fichier['error'];
    
    // Vérifications
    if ($erreur === UPLOAD_ERR_OK) {
        // Vérifier le type MIME
        $finfo = new finfo(FILEINFO_MIME_TYPE);
        $mimeType = $finfo->file($tmp);
        
        if ($mimeType === "image/jpeg") {
            // Déplacer le fichier
            $destination = "uploads/" . basename($nom);
            move_uploaded_file($tmp, $destination);
        }
    }
}

// Codes d'erreur
UPLOAD_ERR_OK;         // 0 - Pas d'erreur
UPLOAD_ERR_INI_SIZE;   // 1 - Taille > upload_max_filesize
UPLOAD_ERR_FORM_SIZE;  // 2 - Taille > MAX_FILE_SIZE (formulaire)
UPLOAD_ERR_PARTIAL;    // 3 - Téléchargement partiel
UPLOAD_ERR_NO_FILE;    // 4 - Aucun fichier téléchargé
```

---

## Base de données

### MySQLi
```php
<?php
// Connexion
$mysqli = new mysqli("localhost", "user", "password", "database");

if ($mysqli->connect_error) {
    die("Erreur de connexion: " . $mysqli->connect_error);
}

// Requête simple
$result = $mysqli->query("SELECT * FROM users");

// Fetch
while ($row = $result->fetch_assoc()) {
    echo $row['name'];
}

// Fetch all
$rows = $result->fetch_all(MYSQLI_ASSOC);

// Prepared statements (protection SQL injection)
$stmt = $mysqli->prepare("SELECT * FROM users WHERE id = ?");
$stmt->bind_param("i", $id); // i=integer, s=string, d=double, b=blob
$id = 1;
$stmt->execute();
$result = $stmt->get_result();
$user = $result->fetch_assoc();

// Insert
$stmt = $mysqli->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
$stmt->bind_param("ss", $name, $email);
$name = "Alice";
$email = "alice@example.com";
$stmt->execute();

$lastId = $mysqli->insert_id;

// Update
$stmt = $mysqli->prepare("UPDATE users SET name = ? WHERE id = ?");
$stmt->bind_param("si", $name, $id);
$stmt->execute();

$affected = $stmt->affected_rows;

// Delete
$stmt = $mysqli->prepare("DELETE FROM users WHERE id = ?");
$stmt->bind_param("i", $id);
$stmt->execute();

// Transactions
$mysqli->begin_transaction();
try {
    $mysqli->query("INSERT INTO ...");
    $mysqli->query("UPDATE ...");
    $mysqli->commit();
} catch (Exception $e) {
    $mysqli->rollback();
    throw $e;
}

// Fermeture
$stmt->close();
$mysqli->close();
```

### PDO (PHP Data Objects)
```php
<?php
// Connexion
try {
    $pdo = new PDO(
        "mysql:host=localhost;dbname=database;charset=utf8mb4",
        "user",
        "password",
        [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
            PDO::ATTR_EMULATE_PREPARES => false
        ]
    );
} catch (PDOException $e) {
    die("Erreur de connexion: " . $e->getMessage());
}

// Requête simple
$stmt = $pdo->query("SELECT * FROM users");
$users = $stmt->fetchAll();

// Prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id");
$stmt->execute(['id' => 1]);
$user = $stmt->fetch();

// Named parameters
$stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (:name, :email)");
$stmt->execute([
    'name' => "Alice",
    'email' => "alice@example.com"
]);

// Positional parameters
$stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
$stmt->execute(["Alice", "alice@example.com"]);

// Fetch modes
$stmt->fetch(PDO::FETCH_ASSOC);  // Tableau associatif
$stmt->fetch(PDO::FETCH_NUM);    // Tableau indexé
$stmt->fetch(PDO::FETCH_BOTH);   // Les deux
$stmt->fetch(PDO::FETCH_OBJ);    // Objet
$stmt->fetch(PDO::FETCH_CLASS, "User"); // Instance de classe

// Fetch all
$users = $stmt->fetchAll();
$names = $stmt->fetchAll(PDO::FETCH_COLUMN, 0); // Première colonne

// Fetch en boucle
while ($row = $stmt->fetch()) {
    echo $row['name'];
}

// Last insert ID
$lastId = $pdo->lastInsertId();

// Row count
$count = $stmt->rowCount();

// Transactions
$pdo->beginTransaction();
try {
    $pdo->exec("INSERT INTO ...");
    $pdo->exec("UPDATE ...");
    $pdo->commit();
} catch (Exception $e) {
    $pdo->rollBack();
    throw $e;
}

// Quote (échapper les valeurs - utiliser prepared statements plutôt)
$safe = $pdo->quote($value);

// Autres bases de données
// PostgreSQL
$pdo = new PDO("pgsql:host=localhost;dbname=database", "user", "password");

// SQLite
$pdo = new PDO("sqlite:/path/to/database.db");

// SQL Server
$pdo = new PDO("sqlsrv:Server=localhost;Database=database", "user", "password");
```

---

## Sessions et Cookies

### Sessions
```php
<?php
// Démarrer la session
session_start();

// Configurer avant session_start()
ini_set('session.cookie_lifetime', 3600);
ini_set('session.gc_maxlifetime', 3600);

// Options (PHP 7+)
session_start([
    'cookie_lifetime' => 3600,
    'cookie_secure' => true,
    'cookie_httponly' => true,
    'cookie_samesite' => 'Lax'
]);

// Stocker des données
$_SESSION['user_id'] = 123;
$_SESSION['username'] = "alice";

// Lire des données
$userId = $_SESSION['user_id'] ?? null;

// Vérifier
if (isset($_SESSION['user_id'])) {
    echo "Utilisateur connecté";
}

// Supprimer une variable
unset($_SESSION['user_id']);

// Détruire la session
session_destroy();

// Régénérer l'ID (sécurité)
session_regenerate_id(true);

// Nom de la session
session_name("MON_APP");

// ID de session
$sessionId = session_id();

// Informations sur la session
session_status(); // PHP_SESSION_DISABLED, PHP_SESSION_NONE, PHP_SESSION_ACTIVE

// Session avec base de données (custom handler)
class DatabaseSessionHandler implements SessionHandlerInterface {
    public function open($savePath, $sessionName) {}
    public function close() {}
    public function read($id) {}
    public function write($id, $data) {}
    public function destroy($id) {}
    public function gc($maxlifetime) {}
}

$handler = new DatabaseSessionHandler();
session_set_save_handler($handler, true);
```

### Cookies
```php
<?php
// Créer un cookie
setcookie("nom", "valeur", [
    'expires' => time() + 3600,
    'path' => '/',
    'domain' => 'example.com',
    'secure' => true,
    'httponly' => true,
    'samesite' => 'Lax' // PHP 7.3+
]);

// Anciennes versions
setcookie("nom", "valeur", time() + 3600, "/", "example.com", true, true);

// Lire un cookie
$value = $_COOKIE['nom'] ?? null;

// Supprimer un cookie
setcookie("nom", "", time() - 3600, "/");

// Cookie avec tableau
setcookie("user[name]", "Alice");
setcookie("user[email]", "alice@example.com");
// Accessible via $_COOKIE['user']['name']

// Attributs SameSite
'Lax'    // Default, protège contre CSRF
'Strict' // Plus strict
'None'   // Permet cross-site (nécessite Secure)
```

---

## Fonctions utiles

### Strings

#### Manipulation
```php
<?php
strlen($str);              // Longueur
mb_strlen($str);           // Longueur (multibyte)

strtoupper($str);          // Majuscules
strtolower($str);          // Minuscules
ucfirst($str);             // Première lettre majuscule
ucwords($str);             // Première lettre de chaque mot
lcfirst($str);             // Première lettre minuscule

trim($str);                // Supprimer espaces début/fin
ltrim($str);               // Gauche
rtrim($str);               // Droite
trim($str, ",");           // Caractères personnalisés

str_pad($str, 10, "0", STR_PAD_LEFT);  // Remplir
str_repeat($str, 3);       // Répéter

substr($str, 0, 5);        // Sous-chaîne
mb_substr($str, 0, 5);     // Multibyte
str_split($str, 2);        // Diviser en morceaux

strrev($str);              // Inverser

// Recherche
strpos($str, "needle");           // Position de la première occurrence
strrpos($str, "needle");          // Position de la dernière occurrence
stripos($str, "needle");          // Insensible à la casse
strripos($str, "needle");         // Insensible à la casse, dernière
str_contains($str, "needle");     // PHP 8.0+
str_starts_with($str, "prefix");  // PHP 8.0+
str_ends_with($str, "suffix");    // PHP 8.0+

// Remplacement
str_replace("old", "new", $str);
str_ireplace("old", "new", $str); // Insensible
strtr($str, "abc", "123");        // Traduire caractères
strtr($str, ["old" => "new"]);    // Traduire strings

// Conversion
str_split($str);           // String vers array
implode(",", $array);      // Array vers string
join(",", $array);         // Alias de implode
explode(",", $str);        // String vers array

// Comparaison
strcmp($str1, $str2);      // Sensible (0 si égal)
strcasecmp($str1, $str2);  // Insensible
strnatcmp($str1, $str2);   // Naturel ("2" < "10")

// Encodage
htmlspecialchars($str);              // Échapper HTML
htmlspecialchars_decode($str);       // Décoder HTML
htmlentities($str);                  // Convertir en entités HTML
html_entity_decode($str);            // Décoder entités
strip_tags($str);                    // Supprimer tags HTML
strip_tags($str, "<p><a>");          // Garder certains tags

// URL
urlencode($str);           // Encoder URL
urldecode($str);           // Décoder URL
rawurlencode($str);        // RFC 3986
rawurldecode($str);
http_build_query($array);  // Array vers query string
parse_url($url);           // Parser URL

// Hash et cryptage
md5($str);                 // MD5 (ne pas utiliser pour mots de passe)
sha1($str);                // SHA1 (ne pas utiliser pour mots de passe)
hash('sha256', $str);      // Hash avec algorithme
password_hash($str, PASSWORD_DEFAULT); // Pour mots de passe
password_verify($str, $hash); // Vérifier mot de passe

// Base64
base64_encode($str);
base64_decode($str);

// Formatage
sprintf("Bonjour %s", $nom);
printf("Nombre: %d", 42);
vsprintf("Format %s", ["valeur"]);

number_format(1234.5678, 2, ',', ' '); // "1 234,57"

// Parsing
parse_str("a=1&b=2", $output); // Query string vers array
sscanf("123 456", "%d %d", $a, $b); // Parser format
```

### Tableaux (voir section dédiée)

### Nombres
```php
<?php
// Math de base
abs(-5);               // Valeur absolue
ceil(4.3);             // Arrondi supérieur
floor(4.8);            // Arrondi inférieur
round(4.5);            // Arrondi standard
round(4.567, 2);       // 2 décimales

max(1, 2, 3);          // Maximum
min(1, 2, 3);          // Minimum
max($array);           // Maximum d'un tableau

pow(2, 3);             // Puissance
sqrt(16);              // Racine carrée
exp(1);                // e^x
log(10);               // Logarithme naturel
log10(100);            // Logarithme base 10

// Trigonométrie
sin($x);
cos($x);
tan($x);
asin($x);
acos($x);
atan($x);
deg2rad($deg);         // Degrés vers radians
rad2deg($rad);         // Radians vers degrés

// Random
rand();                // Nombre aléatoire
rand(1, 100);          // Entre 1 et 100
mt_rand();             // Mersenne Twister (plus rapide)
random_int(1, 100);    // Cryptographiquement sûr (PHP 7+)
random_bytes(16);      // Bytes aléatoires (PHP 7+)

// Constantes
M_PI;                  // π
M_E;                   // e
M_SQRT2;               // √2
PHP_INT_MAX;           // Entier maximum
PHP_INT_MIN;           // Entier minimum
PHP_FLOAT_MAX;         // Float maximum

// Conversion
intval($var);          // Vers entier
floatval($var);        // Vers float
is_nan($var);          // Est NaN
is_finite($var);       // Est fini
is_infinite($var);     // Est infini
```

### Date et heure
```php
<?php
// Timestamp
time();                // Timestamp actuel
microtime(true);       // Avec microsecondes

// Formatage
date("Y-m-d H:i:s");   // Format personnalisé
date("d/m/Y");         // "18/10/2025"

// Formats courants
"Y"   // Année (4 chiffres)
"y"   // Année (2 chiffres)
"m"   // Mois (01-12)
"n"   // Mois (1-12)
"M"   // Mois (abrégé)
"F"   // Mois (complet)
"d"   // Jour (01-31)
"j"   // Jour (1-31)
"D"   // Jour semaine (abrégé)
"l"   // Jour semaine (complet)
"H"   // Heure (00-23)
"h"   // Heure (01-12)
"i"   // Minutes
"s"   // Secondes
"A"   // AM/PM

// Parsing
strtotime("2025-10-18");
strtotime("+1 day");
strtotime("next Monday");
strtotime("last Friday");

// DateTime (orienté objet)
$date = new DateTime();
$date = new DateTime("2025-10-18");
$date = new DateTime("now", new DateTimeZone("Europe/Paris"));

$date->format("Y-m-d H:i:s");
$date->setDate(2025, 10, 18);
$date->setTime(14, 30, 0);

// Modification
$date->modify("+1 day");
$date->modify("-2 weeks");
$date->add(new DateInterval("P1D"));      // +1 jour
$date->sub(new DateInterval("PT2H30M"));  // -2h30

// Comparaison
$date1 < $date2;
$date1->diff($date2);  // DateInterval

// Intervalle
$interval = new DateInterval("P1Y2M3DT4H5M6S"); // 1 an, 2 mois, 3 jours, 4h, 5min, 6sec

// Période
$period = new DatePeriod(
    new DateTime("2025-01-01"),
    new DateInterval("P1D"),
    new DateTime("2025-01-10")
);

foreach ($period as $date) {
    echo $date->format("Y-m-d");
}

// Timezone
$tz = new DateTimeZone("Europe/Paris");
$date->setTimezone($tz);
date_default_timezone_set("Europe/Paris");
date_default_timezone_get();

// Immutable (PHP 5.5+)
$date = new DateTimeImmutable();
$newDate = $date->modify("+1 day"); // Retourne un nouvel objet
```

### JSON
```php
<?php
// Encoder
$json = json_encode($array);
$json = json_encode($array, JSON_PRETTY_PRINT);
$json = json_encode($array, JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);

// Options
JSON_PRETTY_PRINT;           // Formaté
JSON_UNESCAPED_UNICODE;      // UTF-8
JSON_UNESCAPED_SLASHES;      // Ne pas échapper /
JSON_NUMERIC_CHECK;          // Convertir strings numériques
JSON_FORCE_OBJECT;           // Forcer objet
JSON_THROW_ON_ERROR;         // Exception au lieu d'erreur (PHP 7.3+)

// Décoder
$array = json_decode($json);        // Objet
$array = json_decode($json, true);  // Array associatif

// Avec exception (PHP 7.3+)
try {
    $data = json_decode($json, true, 512, JSON_THROW_ON_ERROR);
} catch (JsonException $e) {
    echo $e->getMessage();
}

// Erreurs
json_last_error();           // Code d'erreur
json_last_error_msg();       // Message d'erreur

// Codes d'erreur
JSON_ERROR_NONE;
JSON_ERROR_DEPTH;
JSON_ERROR_STATE_MISMATCH;
JSON_ERROR_CTRL_CHAR;
JSON_ERROR_SYNTAX;
```

### Serialization
```php
<?php
// Serialize
$serialized = serialize($data);

// Unserialize
$data = unserialize($serialized);

// Options (PHP 7+)
$data = unserialize($serialized, ['allowed_classes' => ['MyClass']]);
$data = unserialize($serialized, ['allowed_classes' => false]); // Que stdClass
```

### Variables
```php
<?php
// Vérifications
isset($var);           // Défini et non null
empty($var);           // Vide (false, 0, "", null, [], etc.)
is_null($var);         // Est null

// Information
var_dump($var);        // Dump détaillé
print_r($var);         // Affichage lisible
var_export($var);      // Code PHP valide
get_defined_vars();    // Toutes les variables définies

// Conversion
settype($var, "integer");
gettype($var);

// Destruction
unset($var);
```

### Output Control
```php
<?php
// Bufferisation
ob_start();
echo "Contenu";
$contenu = ob_get_contents();
ob_end_clean();  // Nettoyer sans afficher
ob_end_flush();  // Afficher et nettoyer

// Niveau de bufferisation
ob_get_level();

// Callback
ob_start(function($buffer) {
    return strtoupper($buffer);
});
```

### HTTP Headers
```php
<?php
// Envoyer un header
header("Content-Type: application/json");
header("Location: /page.php");
header("HTTP/1.1 404 Not Found");

// Headers multiples
header("Set-Cookie: name=value");
header("Set-Cookie: other=value", false); // false = ne pas remplacer

// Vérifier si headers envoyés
if (!headers_sent()) {
    header("Location: /");
}

// Codes HTTP
http_response_code(404);
http_response_code(200);

// Obtenir headers reçus
$headers = getallheaders();
```

### Fonctions système
```php
<?php
// Exécuter commande
exec("ls -la", $output, $returnVar);
shell_exec("ls -la");
system("ls -la");
passthru("ls -la");

// Informations système
phpinfo();
php_uname();
phpversion();
get_loaded_extensions();

// Configuration PHP
ini_get("upload_max_filesize");
ini_set("display_errors", "1");
get_cfg_var("upload_max_filesize");

// Environnement
getenv("PATH");
putenv("MY_VAR=value");
$_ENV['PATH'];
$_SERVER['HTTP_HOST'];

// Sleep
sleep(1);          // Secondes
usleep(1000000);   // Microsecondes
time_nanosleep(0, 500000000); // Nanosecondes
```

### Email
```php
<?php
// mail() basique
mail("to@example.com", "Sujet", "Message");

// Avec headers
$headers = "From: sender@example.com\r\n";
$headers .= "Reply-To: reply@example.com\r\n";
$headers .= "Content-Type: text/html; charset=UTF-8\r\n";

mail("to@example.com", "Sujet", $message, $headers);

// Pour emails plus complexes, utiliser une bibliothèque comme PHPMailer
```

### Filtrage et validation
```php
<?php
// Filter
filter_var($email, FILTER_VALIDATE_EMAIL);
filter_var($url, FILTER_VALIDATE_URL);
filter_var($ip, FILTER_VALIDATE_IP);
filter_var($int, FILTER_VALIDATE_INT);
filter_var($float, FILTER_VALIDATE_FLOAT);
filter_var($bool, FILTER_VALIDATE_BOOLEAN);

// Sanitize
filter_var($str, FILTER_SANITIZE_STRING);
filter_var($email, FILTER_SANITIZE_EMAIL);
filter_var($url, FILTER_SANITIZE_URL);
filter_var($int, FILTER_SANITIZE_NUMBER_INT);
filter_var($float, FILTER_SANITIZE_NUMBER_FLOAT);

// Filter input
filter_input(INPUT_GET, 'param', FILTER_VALIDATE_INT);
filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);

// Multiple
filter_var_array($_GET, [
    'id' => FILTER_VALIDATE_INT,
    'email' => FILTER_VALIDATE_EMAIL
]);
```

---

## Bonnes pratiques

### PSR Standards
```php
<?php
// PSR-1: Basic Coding Standard
// - Utiliser <?php ou <?=
// - UTF-8 sans BOM
// - Séparer logique et affichage

// PSR-2/PSR-12: Coding Style
// - Indentation: 4 espaces
// - Accolades: Nouvelles lignes pour classes et fonctions
// - Espaces autour des opérateurs

// PSR-4: Autoloading
namespace Vendor\Package;

class ClassName {
    // ...
}

// PSR-3: Logger Interface
interface LoggerInterface {
    public function log($level, $message, array $context = []);
}

// PSR-7: HTTP Message
// PSR-15: HTTP Handlers
```

### Sécurité
```php
<?php
// Toujours utiliser prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$id]);

// Échapper l'output HTML
echo htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8');

// Valider les inputs
$email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);

// CSRF Protection
session_start();
if (empty($_SESSION['token'])) {
    $_SESSION['token'] = bin2hex(random_bytes(32));
}

// Vérifier le token
if (!hash_equals($_SESSION['token'], $_POST['token'])) {
    die('CSRF token invalide');
}

// Password hashing
$hash = password_hash($password, PASSWORD_DEFAULT);
if (password_verify($password, $hash)) {
    // OK
}

// Ne jamais afficher les erreurs en production
ini_set('display_errors', '0');
ini_set('log_errors', '1');
ini_set('error_log', '/path/to/error.log');

// Headers de sécurité
header("X-Content-Type-Options: nosniff");
header("X-Frame-Options: DENY");
header("X-XSS-Protection: 1; mode=block");
header("Content-Security-Policy: default-src 'self'");
```

### Performance
```php
<?php
// Utiliser opcache
// Dans php.ini:
opcache.enable=1
opcache.memory_consumption=128
opcache.max_accelerated_files=10000

// Éviter les require/include répétés
require_once 'file.php';

// Utiliser isset() au lieu de array_key_exists() quand possible
isset($array[$key]); // Plus rapide

// String concatenation
// Utiliser .= au lieu de créer de nouveaux strings
$str = "";
for ($i = 0; $i < 1000; $i++) {
    $str .= $i; // Mieux que $str = $str . $i;
}

// Ou utiliser un array et implode
$parts = [];
for ($i = 0; $i < 1000; $i++) {
    $parts[] = $i;
}
$str = implode('', $parts);

// foreach plus rapide que for avec count()
foreach ($array as $item) {} // Mieux que for ($i = 0; $i < count($array); $i++)

// Utiliser ++$i au lieu de $i++ quand possible
for ($i = 0; $i < 1000; ++$i) {}
```

### Autoloading (PSR-4)
```php
<?php
// composer.json
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}

// Utilisation
require 'vendor/autoload.php';

use App\Models\User;
$user = new User();

// Autoloader manuel
spl_autoload_register(function ($class) {
    $file = str_replace('\\', '/', $class) . '.php';
    if (file_exists($file)) {
        require $file;
    }
});
```

---

## Ressources

- [Documentation officielle PHP](https://www.php.net/manual/fr/)
- [PHP: The Right Way](https://phptherightway.com/)
- [PSR Standards](https://www.php-fig.org/psr/)
- [Composer](https://getcomposer.org/)
- [Packagist](https://packagist.org/)

---
