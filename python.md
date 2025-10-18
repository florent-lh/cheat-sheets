# Python CheatSheet

## Table des matières
1. [Bases et Syntaxe](#bases-et-syntaxe)
2. [Variables et Types](#variables-et-types)
3. [Opérateurs](#opérateurs)
4. [Structures de contrôle](#structures-de-contrôle)
5. [Fonctions](#fonctions)
6. [Structures de données](#structures-de-données)
7. [Compréhensions](#compréhensions)
8. [Programmation Orientée Objet](#programmation-orientée-objet)
9. [Nouveautés Python 3.8+](#nouveautés-python-38)
10. [Type Hints](#type-hints)
11. [Gestion des erreurs](#gestion-des-erreurs)
12. [Fichiers et I/O](#fichiers-et-io)
13. [Modules et Packages](#modules-et-packages)
14. [Décorateurs](#décorateurs)
15. [Context Managers](#context-managers)
16. [Async/Await](#asyncawait)
17. [Fonctions utiles](#fonctions-utiles)

---

## Bases et Syntaxe

### Commentaires
```python
# Commentaire sur une ligne

"""
Commentaire sur
plusieurs lignes
(docstring)
"""

'''
Aussi un commentaire
sur plusieurs lignes
'''
```

### Indentation
```python
# Python utilise l'indentation (4 espaces recommandés)
if True:
    print("Indenté")
    if True:
        print("Double indentation")
```

### Affichage
```python
# Print
print("Hello World")
print("Hello", "World")  # Espace automatique
print("Hello", "World", sep=", ", end="!\n")

# f-strings (Python 3.6+)
nom = "Alice"
age = 30
print(f"Je m'appelle {nom} et j'ai {age} ans")
print(f"{nom=}")  # Python 3.8+ affiche "nom='Alice'"
print(f"{2 + 2 = }")  # Affiche "2 + 2 = 4"

# Format
print("Je m'appelle {} et j'ai {} ans".format(nom, age))
print("Je m'appelle {0} et j'ai {1} ans".format(nom, age))
print("Je m'appelle {nom} et j'ai {age} ans".format(nom=nom, age=age))

# Formatage avancé
print(f"{3.14159:.2f}")  # 3.14
print(f"{42:05d}")       # 00042
print(f"{1000000:,}")    # 1,000,000

# % formatting (ancien, mais toujours utilisé)
print("Je m'appelle %s et j'ai %d ans" % (nom, age))
```

### Import
```python
import math
import math as m
from math import pi, sqrt
from math import *  # Déconseillé
from . import module  # Import relatif
from .. import parent_module
```

### Pass, del
```python
# Pass (instruction vide)
def fonction_vide():
    pass

# Del (supprimer variable)
x = 10
del x
```

---

## Variables et Types

### Variables
```python
# Déclaration (pas de mot-clé)
variable = "valeur"
nombre = 42
_variable = "ok"  # Underscore autorisé

# Multiple assignment
x, y, z = 1, 2, 3
x = y = z = 0

# Swap
x, y = y, x

# Unpacking
a, *b, c = [1, 2, 3, 4, 5]  # a=1, b=[2,3,4], c=5

# Constantes (convention)
PI = 3.14159
MAX_SIZE = 100
```

### Types primitifs

#### Nombres
```python
# Integer
entier = 42
grand = 1_000_000  # Python 3.6+ (lisibilité)
hexa = 0xFF
octal = 0o77
binaire = 0b1010

# Float
decimal = 3.14
scientifique = 1.2e-3
inf = float('inf')
neg_inf = float('-inf')
nan = float('nan')

# Complex
complexe = 3 + 4j
reel = complexe.real      # 3.0
imaginaire = complexe.imag  # 4.0

# Boolean
vrai = True
faux = False

# None
vide = None
```

#### Strings
```python
# Déclaration
texte = "double quotes"
texte = 'simple quotes'

# Multi-lignes
multi = """Texte sur
plusieurs lignes"""

multi = '''Autre
multi-lignes'''

# Raw strings (pas d'échappement)
chemin = r"C:\Users\nom"

# Bytes
octets = b"Hello"
octets = bytes("Hello", "utf-8")

# f-strings (Python 3.6+)
nom = "Alice"
message = f"Bonjour {nom}"
calcul = f"2 + 2 = {2 + 2}"

# Indexation et slicing
texte = "Python"
texte[0]      # 'P'
texte[-1]     # 'n'
texte[0:3]    # 'Pyt'
texte[:3]     # 'Pyt'
texte[3:]     # 'hon'
texte[::2]    # 'Pto' (pas de 2)
texte[::-1]   # 'nohtyP' (inversé)
```

### Vérification de types
```python
# type()
type(42)        # <class 'int'>
type("hello")   # <class 'str'>
type([1, 2])    # <class 'list'>

# isinstance()
isinstance(42, int)           # True
isinstance("hello", str)      # True
isinstance([1, 2], (list, tuple))  # True

# Conversion
int("42")       # 42
float("3.14")   # 3.14
str(42)         # "42"
bool(1)         # True
list("abc")     # ['a', 'b', 'c']
tuple([1, 2])   # (1, 2)
set([1, 2, 2])  # {1, 2}
```

---

## Opérateurs

### Opérateurs arithmétiques
```python
a = 10
b = 3

a + b   # 13  Addition
a - b   # 7   Soustraction
a * b   # 30  Multiplication
a / b   # 3.333... Division (float)
a // b  # 3   Division entière
a % b   # 1   Modulo
a ** b  # 1000 Exponentiation

# Opérateurs d'affectation
a += 5  # a = a + 5
a -= 3  # a = a - 3
a *= 2  # a = a * 2
a /= 4  # a = a / 4
a //= 2 # a = a // 2
a %= 3  # a = a % 3
a **= 2 # a = a ** 2

# Walrus operator := (Python 3.8+)
if (n := len([1, 2, 3])) > 2:
    print(f"Liste de {n} éléments")

# Dans une compréhension
[y for x in data if (y := f(x)) > 0]
```

### Opérateurs de comparaison
```python
# Égalité
5 == 5      # True
5 != 6      # True

# Comparaisons
10 > 5      # True
10 < 5      # False
10 >= 10    # True
10 <= 5     # False

# Chaînage
1 < 5 < 10  # True
x == y == z # Tous égaux

# Identité
x is y      # Même objet
x is not y  # Pas le même objet
x is None   # Vérifier None
```

### Opérateurs logiques
```python
# and, or, not
True and False  # False
True or False   # True
not True        # False

# Court-circuit
True or fonction_jamais_appelée()
False and fonction_jamais_appelée()

# any, all
any([False, True, False])   # True (au moins un)
all([True, True, True])     # True (tous)
```

### Opérateurs sur les séquences
```python
# Appartenance
"a" in "abc"        # True
"d" not in "abc"    # True
2 in [1, 2, 3]      # True

# Concaténation
[1, 2] + [3, 4]     # [1, 2, 3, 4]
"hello" + "world"   # "helloworld"

# Répétition
[1, 2] * 3          # [1, 2, 1, 2, 1, 2]
"abc" * 3           # "abcabcabc"
```

### Opérateurs bit à bit
```python
a = 60  # 0011 1100
b = 13  # 0000 1101

a & b   # 12  AND
a | b   # 61  OR
a ^ b   # 49  XOR
~a      # -61 NOT
a << 2  # 240 Left shift
a >> 2  # 15  Right shift
```

---

## Structures de contrôle

### Conditions

#### if...elif...else
```python
if condition:
    # code
elif autre_condition:
    # autre code
else:
    # code par défaut

# Inline if (ternaire)
resultat = "pair" if x % 2 == 0 else "impair"

# Inline if sans else (walrus)
if (valeur := fonction()) is not None:
    print(valeur)
```

### Match/Case (Python 3.10+)
```python
# Match simple
match status:
    case 200:
        return "OK"
    case 404:
        return "Not Found"
    case 500:
        return "Server Error"
    case _:
        return "Unknown"

# Match avec conditions
match point:
    case (0, 0):
        print("Origine")
    case (0, y):
        print(f"Axe Y à {y}")
    case (x, 0):
        print(f"Axe X à {x}")
    case (x, y):
        print(f"Point ({x}, {y})")

# Match avec types
match value:
    case int():
        print("Entier")
    case str():
        print("String")
    case list():
        print("Liste")

# Match avec garde
match point:
    case (x, y) if x == y:
        print("Diagonale")
    case (x, y):
        print("Autre point")

# Match avec classes
match command:
    case Command(action="quit"):
        quit()
    case Command(action="move", direction=dir):
        move(dir)
```

### Boucles

#### for
```python
# Itération sur séquence
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

for item in [1, 2, 3]:
    print(item)

# Avec index (enumerate)
for index, value in enumerate(['a', 'b', 'c']):
    print(f"{index}: {value}")

# Avec début personnalisé
for index, value in enumerate(['a', 'b', 'c'], start=1):
    print(f"{index}: {value}")

# Itération sur dictionnaire
for key in dict:
    print(key)

for key, value in dict.items():
    print(f"{key}: {value}")

# Itération sur plusieurs listes (zip)
for x, y in zip([1, 2, 3], ['a', 'b', 'c']):
    print(f"{x} - {y}")

# Range avec pas
for i in range(0, 10, 2):  # 0, 2, 4, 6, 8
    print(i)

# Range inversé
for i in range(10, 0, -1):  # 10, 9, ..., 1
    print(i)

# Unpacking dans for
for x, y, z in [(1, 2, 3), (4, 5, 6)]:
    print(x, y, z)
```

#### while
```python
i = 0
while i < 5:
    print(i)
    i += 1

# While avec else (exécuté si pas de break)
while i < 5:
    print(i)
    i += 1
else:
    print("Boucle terminée normalement")
```

### Contrôle de flux
```python
# break (sortir de la boucle)
for i in range(10):
    if i == 5:
        break

# continue (passer à l'itération suivante)
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)

# pass (instruction vide)
for i in range(10):
    pass

# else avec boucles (exécuté si pas de break)
for i in range(5):
    if i == 10:
        break
else:
    print("Pas de break")
```

---

## Fonctions

### Déclaration de fonctions
```python
def saluer(nom):
    return f"Bonjour {nom}"

# Sans return (retourne None)
def afficher():
    print("Hello")

# Return multiple
def coordonnees():
    return 10, 20

x, y = coordonnees()

# Docstring
def fonction():
    """
    Description de la fonction.
    
    Args:
        param1: Description
        
    Returns:
        Description du retour
    """
    pass
```

### Paramètres

#### Paramètres par défaut
```python
def saluer(nom="Invité", langue="fr"):
    return f"Bonjour {nom}"

saluer()               # "Bonjour Invité"
saluer("Alice")        # "Bonjour Alice"
saluer(langue="en")    # Arguments nommés
```

#### *args et **kwargs
```python
# *args (arguments positionnels variables)
def somme(*nombres):
    return sum(nombres)

somme(1, 2, 3, 4)  # 10

# **kwargs (arguments nommés variables)
def info(**details):
    for key, value in details.items():
        print(f"{key}: {value}")

info(nom="Alice", age=30, ville="Paris")

# Combinaison
def fonction(arg1, arg2, *args, kwarg1="default", **kwargs):
    pass

# Unpacking
args = [1, 2, 3]
kwargs = {"a": 1, "b": 2}
fonction(*args, **kwargs)
```

#### Positional-only et Keyword-only (Python 3.8+)
```python
# Positional-only (avant /)
def fonction(a, b, /, c, d):
    # a et b doivent être positionnels
    # c et d peuvent être positionnels ou nommés
    pass

fonction(1, 2, 3, 4)
fonction(1, 2, c=3, d=4)
# fonction(a=1, b=2, c=3, d=4)  # Erreur!

# Keyword-only (après *)
def fonction(a, b, *, c, d):
    # a et b peuvent être positionnels ou nommés
    # c et d doivent être nommés
    pass

fonction(1, 2, c=3, d=4)
# fonction(1, 2, 3, 4)  # Erreur!

# Combinaison
def fonction(a, /, b, *, c):
    # a: positional-only
    # b: flexible
    # c: keyword-only
    pass
```

### Lambda functions
```python
# Fonction anonyme
carre = lambda x: x ** 2
addition = lambda x, y: x + y

# Utilisation courante avec map, filter, sorted
nombres = [1, 2, 3, 4, 5]
carres = list(map(lambda x: x ** 2, nombres))
pairs = list(filter(lambda x: x % 2 == 0, nombres))
personnes = sorted(personnes, key=lambda p: p['age'])
```

### Fonctions imbriquées et closures
```python
def externe(x):
    def interne(y):
        return x + y
    return interne

ajouter_5 = externe(5)
print(ajouter_5(3))  # 8

# Closure avec nonlocal
def compteur():
    count = 0
    def incrementer():
        nonlocal count
        count += 1
        return count
    return incrementer

c = compteur()
print(c())  # 1
print(c())  # 2
```

### Générateurs
```python
# yield
def generer_nombres(n):
    for i in range(n):
        yield i

# Utilisation
for nombre in generer_nombres(5):
    print(nombre)

# Expression génératrice
carres = (x ** 2 for x in range(10))

# yield from (Python 3.3+)
def generer_tout():
    yield from range(3)
    yield from [3, 4, 5]

# Générateur avec send
def echo():
    while True:
        valeur = yield
        print(f"Reçu: {valeur}")

gen = echo()
next(gen)  # Démarrer
gen.send("Hello")
```

### Récursion
```python
def factorielle(n):
    if n <= 1:
        return 1
    return n * factorielle(n - 1)

# Avec cache (Python 3.9+)
from functools import cache

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# lru_cache (Python 3.2+)
from functools import lru_cache

@lru_cache(maxsize=128)
def fonction_couteuse(n):
    # Calculs complexes
    return result
```

---

## Structures de données

### Listes (Lists)
```python
# Création
liste = [1, 2, 3]
vide = []
mixed = [1, "deux", 3.0, [4, 5]]

# Accès
liste[0]       # Premier élément
liste[-1]      # Dernier élément
liste[1:3]     # Slice [2, 3]
liste[::2]     # Tous les 2 éléments
liste[::-1]    # Inversé

# Modification
liste[0] = 10
liste[1:3] = [20, 30]

# Ajout
liste.append(4)           # Ajouter à la fin
liste.insert(0, 0)        # Insérer à l'index
liste.extend([5, 6])      # Étendre avec une liste
liste += [7, 8]           # Concaténation

# Suppression
liste.remove(3)           # Supprimer première occurrence
element = liste.pop()     # Supprimer et retourner dernier
element = liste.pop(0)    # Supprimer à l'index
del liste[0]              # Supprimer à l'index
del liste[1:3]            # Supprimer slice
liste.clear()             # Vider la liste

# Recherche
liste.index(3)            # Index de la première occurrence
liste.count(3)            # Nombre d'occurrences
3 in liste                # Appartenance

# Tri
liste.sort()              # Tri en place
liste.sort(reverse=True)  # Tri décroissant
liste.sort(key=len)       # Tri avec clé
sorted(liste)             # Nouvelle liste triée

# Reverse
liste.reverse()           # Inverser en place
reversed(liste)           # Iterator inversé

# Copie
copie = liste.copy()      # Copie superficielle
copie = liste[:]          # Autre méthode
copie = list(liste)       # Autre méthode

import copy
deep = copy.deepcopy(liste)  # Copie profonde

# Autres
len(liste)                # Longueur
min(liste)                # Minimum
max(liste)                # Maximum
sum(liste)                # Somme
```

### Tuples
```python
# Création
tuple_ex = (1, 2, 3)
singleton = (1,)          # Virgule obligatoire
vide = ()
sans_parentheses = 1, 2, 3

# Immutable
# tuple_ex[0] = 10  # Erreur!

# Unpacking
a, b, c = (1, 2, 3)
a, *b, c = (1, 2, 3, 4, 5)  # a=1, b=[2,3,4], c=5

# Named tuples
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)
print(p[0], p[1])

# Ou avec defaults (Python 3.7+)
Point = namedtuple('Point', ['x', 'y'], defaults=[0, 0])
```

### Sets
```python
# Création
ensemble = {1, 2, 3}
vide = set()  # {} crée un dict!
from_list = set([1, 2, 2, 3])  # {1, 2, 3}

# Ajout/Suppression
ensemble.add(4)
ensemble.update([5, 6])
ensemble.remove(3)      # Erreur si n'existe pas
ensemble.discard(3)     # Pas d'erreur
element = ensemble.pop()  # Supprimer arbitraire
ensemble.clear()

# Opérations ensemblistes
a = {1, 2, 3}
b = {3, 4, 5}

a | b            # Union {1, 2, 3, 4, 5}
a & b            # Intersection {3}
a - b            # Différence {1, 2}
a ^ b            # Différence symétrique {1, 2, 4, 5}

a.union(b)
a.intersection(b)
a.difference(b)
a.symmetric_difference(b)

# Tests
a.issubset(b)
a.issuperset(b)
a.isdisjoint(b)

# Frozen set (immutable)
frozen = frozenset([1, 2, 3])
```

### Dictionnaires (Dicts)
```python
# Création
dict_ex = {"nom": "Alice", "age": 30}
vide = {}
dict_ex = dict(nom="Alice", age=30)

# Accès
dict_ex["nom"]            # "Alice"
dict_ex.get("nom")        # "Alice"
dict_ex.get("ville", "Paris")  # Valeur par défaut

# Modification
dict_ex["age"] = 31
dict_ex["ville"] = "Paris"  # Ajouter nouvelle clé

# Suppression
del dict_ex["age"]
valeur = dict_ex.pop("nom")
valeur = dict_ex.pop("inconnu", "default")
dict_ex.clear()

# Méthodes
dict_ex.keys()            # Vue des clés
dict_ex.values()          # Vue des valeurs
dict_ex.items()           # Vue des paires (clé, valeur)

# Itération
for key in dict_ex:
    print(key)

for key, value in dict_ex.items():
    print(f"{key}: {value}")

# Fusion (Python 3.9+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = dict1 | dict2

# Update
dict1.update(dict2)
dict1 |= dict2  # Python 3.9+

# setdefault
dict_ex.setdefault("ville", "Paris")  # Ajoute si n'existe pas

# fromkeys
keys = ["a", "b", "c"]
nouveau = dict.fromkeys(keys, 0)  # {"a": 0, "b": 0, "c": 0}

# Dictionnaire ordonné (par défaut depuis Python 3.7+)
# collections.OrderedDict pour versions antérieures

# defaultdict
from collections import defaultdict

dd = defaultdict(list)
dd["key"].append(1)  # Pas d'erreur même si key n'existe pas

dd = defaultdict(int)
dd["count"] += 1  # Initialise à 0 automatiquement

# Counter
from collections import Counter

compteur = Counter([1, 2, 2, 3, 3, 3])
# Counter({3: 3, 2: 2, 1: 1})

compteur.most_common(2)  # [(3, 3), (2, 2)]
compteur.update([1, 1])  # Incrémenter
```

### Autres structures
```python
# deque (double-ended queue)
from collections import deque

dq = deque([1, 2, 3])
dq.append(4)        # Droite
dq.appendleft(0)    # Gauche
dq.pop()            # Droite
dq.popleft()        # Gauche
dq.rotate(1)        # Rotation

# ChainMap
from collections import ChainMap

dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
combined = ChainMap(dict1, dict2)
combined["a"]  # 1 (cherche dans dict1 puis dict2)

# heapq (tas)
import heapq

heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)
min_elem = heapq.heappop(heap)  # 1

# Les 3 plus petits/grands
heapq.nsmallest(3, liste)
heapq.nlargest(3, liste)
```

---

## Compréhensions

### List Comprehension
```python
# Basique
carres = [x**2 for x in range(10)]

# Avec condition
pairs = [x for x in range(10) if x % 2 == 0]

# Condition if-else
valeurs = [x if x > 0 else 0 for x in range(-5, 5)]

# Imbriquée
matrice = [[i+j for j in range(3)] for i in range(3)]

# Flatten
liste_2d = [[1, 2], [3, 4], [5, 6]]
flat = [x for row in liste_2d for x in row]

# Avec walrus (Python 3.8+)
[y for x in data if (y := f(x)) is not None]
```

### Dict Comprehension
```python
# Basique
carres = {x: x**2 for x in range(5)}

# Avec condition
pairs = {x: x**2 for x in range(10) if x % 2 == 0}

# Inverser dictionnaire
original = {"a": 1, "b": 2}
inverse = {v: k for k, v in original.items()}

# Filtrer
filtered = {k: v for k, v in dict_ex.items() if v > 10}
```

### Set Comprehension
```python
# Basique
carres = {x**2 for x in range(10)}

# Avec condition
pairs = {x for x in range(10) if x % 2 == 0}
```

### Generator Expression
```python
# Ne crée pas la liste en mémoire
carres = (x**2 for x in range(1000000))

# Utilisation
for carre in carres:
    print(carre)

# Conversion
liste = list(carres)
```

---

## Programmation Orientée Objet

### Classes de base
```python
class Personne:
    # Variable de classe
    espece = "Homo sapiens"
    
    # Constructeur
    def __init__(self, nom, age):
        # Variables d'instance
        self.nom = nom
        self.age = age
        self._protege = "protégé"  # Convention
        self.__prive = "privé"      # Name mangling
    
    # Méthode
    def saluer(self):
        return f"Bonjour, je suis {self.nom}"
    
    # Méthode de classe
    @classmethod
    def from_birth_year(cls, nom, annee_naissance):
        age = 2025 - annee_naissance
        return cls(nom, age)
    
    # Méthode statique
    @staticmethod
    def est_majeur(age):
        return age >= 18
    
    # Représentation
    def __repr__(self):
        return f"Personne('{self.nom}', {self.age})"
    
    def __str__(self):
        return f"{self.nom}, {self.age} ans"

# Instanciation
alice = Personne("Alice", 30)
print(alice.saluer())
print(Personne.espece)

# Méthode de classe
bob = Personne.from_birth_year("Bob", 1995)

# Méthode statique
Personne.est_majeur(20)
```

### Propriétés (Properties)
```python
class Personne:
    def __init__(self, nom, age):
        self._nom = nom
        self._age = age
    
    # Getter
    @property
    def nom(self):
        return self._nom
    
    # Setter
    @nom.setter
    def nom(self, valeur):
        if not valeur:
            raise ValueError("Le nom ne peut pas être vide")
        self._nom = valeur
    
    # Deleter
    @nom.deleter
    def nom(self):
        del self._nom
    
    # Propriété en lecture seule
    @property
    def majeur(self):
        return self._age >= 18

# Utilisation
p = Personne("Alice", 30)
print(p.nom)      # Appelle le getter
p.nom = "Bob"     # Appelle le setter
del p.nom         # Appelle le deleter
```

### Héritage
```python
class Etudiant(Personne):
    def __init__(self, nom, age, ecole):
        super().__init__(nom, age)  # Appel constructeur parent
        self.ecole = ecole
    
    # Override
    def saluer(self):
        return f"{super().saluer()} et j'étudie à {self.ecole}"
    
    # Nouvelle méthode
    def etudier(self):
        return f"{self.nom} étudie"

# Héritage multiple
class A:
    def methode(self):
        print("A")

class B:
    def methode(self):
        print("B")

class C(A, B):
    pass

# MRO (Method Resolution Order)
print(C.__mro__)
```

### Méthodes spéciales (dunder methods)
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # Représentation
    def __repr__(self):
        return f"Point({self.x}, {self.y})"
    
    def __str__(self):
        return f"({self.x}, {self.y})"
    
    # Opérateurs
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        return Point(self.x * scalar, self.y * scalar)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __lt__(self, other):
        return (self.x**2 + self.y**2) < (other.x**2 + other.y**2)
    
    # Longueur
    def __len__(self):
        return int((self.x**2 + self.y**2)**0.5)
    
    # Accès par index
    def __getitem__(self, index):
        if index == 0:
            return self.x
        elif index == 1:
            return self.y
        raise IndexError()
    
    def __setitem__(self, index, value):
        if index == 0:
            self.x = value
        elif index == 1:
            self.y = value
        else:
            raise IndexError()
    
    # Callable
    def __call__(self):
        return f"Point at ({self.x}, {self.y})"
    
    # Context manager
    def __enter__(self):
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        pass

# Utilisation
p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = p1 + p2
print(p1[0])  # 1
p1()  # Callable
```

### Dataclasses (Python 3.7+)
```python
from dataclasses import dataclass, field

@dataclass
class Personne:
    nom: str
    age: int
    email: str = "inconnu"
    
    def saluer(self):
        return f"Bonjour, je suis {self.nom}"

# Génère automatiquement __init__, __repr__, __eq__
alice = Personne("Alice", 30)
print(alice)

# Avec options
@dataclass(frozen=True)  # Immutable
class Point:
    x: int
    y: int

@dataclass(order=True)  # Ajoute __lt__, __le__, etc.
class Personne:
    nom: str
    age: int

# Field avec default factory
@dataclass
class Classe:
    etudiants: list = field(default_factory=list)
    notes: dict = field(default_factory=dict)

# Field avec metadata
@dataclass
class Utilisateur:
    nom: str = field(metadata={"description": "Nom de l'utilisateur"})
```

### Classes abstraites
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def faire_bruit(self):
        pass
    
    @abstractmethod
    def se_deplacer(self):
        pass
    
    # Méthode concrète
    def dormir(self):
        return "Zzz..."

class Chien(Animal):
    def faire_bruit(self):
        return "Woof!"
    
    def se_deplacer(self):
        return "Le chien marche"

# animal = Animal()  # Erreur!
chien = Chien()
```

### Slots (optimisation mémoire)
```python
class Point:
    __slots__ = ['x', 'y']
    
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Pas de __dict__, moins de mémoire
# Pas d'attributs dynamiques
```

---

## Nouveautés Python 3.8+

### Python 3.8

#### Walrus Operator :=
```python
# Assignment expression
if (n := len(liste)) > 10:
    print(f"Liste de {n} éléments")

# Dans une boucle while
while (ligne := fichier.readline()):
    print(ligne)

# Dans list comprehension
[y for x in data if (y := f(x)) is not None]

# Éviter calculs redondants
donnees = [y := f(x), y**2, y**3]
```

#### Positional-Only Parameters
```python
def fonction(a, b, /, c, d):
    # a et b doivent être positionnels
    pass

fonction(1, 2, 3, 4)
fonction(1, 2, c=3, d=4)
```

#### f-strings avec =
```python
x = 10
print(f"{x=}")  # "x=10"
print(f"{x + 5=}")  # "x + 5=15"
```

### Python 3.9

#### Dict Merge Operators
```python
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}

# Merge
merged = dict1 | dict2

# Update
dict1 |= dict2
```

#### Type Hinting Improvements
```python
# Type hints génériques sans import
def fonction(liste: list[int]) -> list[str]:
    return [str(x) for x in liste]

def process(data: dict[str, int]) -> tuple[str, int]:
    pass

# Avant Python 3.9, il fallait:
from typing import List, Dict, Tuple

def fonction(liste: List[int]) -> List[str]:
    pass
```

#### str Methods
```python
# removeprefix, removesuffix
texte = "HelloWorld"
texte.removeprefix("Hello")  # "World"
texte.removesuffix("World")  # "Hello"
```

### Python 3.10

#### Match Statement
```python
# Structural pattern matching
match status:
    case 200:
        return "OK"
    case 404:
        return "Not Found"
    case _:
        return "Unknown"

# Pattern matching avancé
match point:
    case (0, 0):
        print("Origine")
    case (x, 0):
        print(f"Sur axe X: {x}")
    case (0, y):
        print(f"Sur axe Y: {y}")
    case (x, y) if x == y:
        print(f"Sur diagonale: {x}")
    case (x, y):
        print(f"Point: ({x}, {y})")
```

#### Union Types with |
```python
# Nouvelle syntaxe pour Union
def fonction(param: int | float) -> str | None:
    pass

# Équivalent à:
from typing import Union, Optional
def fonction(param: Union[int, float]) -> Optional[str]:
    pass
```

#### Better Error Messages
```python
# Messages d'erreur plus précis avec suggestions
# Exemple: SyntaxError pointe vers l'endroit exact
```

### Python 3.11

#### Exception Groups
```python
# Lever plusieurs exceptions
raise ExceptionGroup("Multiple errors", [
    ValueError("Erreur 1"),
    TypeError("Erreur 2")
])

# Capturer
try:
    raise ExceptionGroup("errors", [ValueError("1"), TypeError("2")])
except* ValueError as e:
    print("ValueError caught")
except* TypeError as e:
    print("TypeError caught")
```

#### Task Groups (asyncio)
```python
import asyncio

async def main():
    async with asyncio.TaskGroup() as tg:
        task1 = tg.create_task(coro1())
        task2 = tg.create_task(coro2())
```

#### Self Type
```python
from typing import Self

class Calculatrice:
    def ajouter(self, x: int) -> Self:
        return self
```

### Python 3.12

#### Type Parameter Syntax
```python
# Nouvelle syntaxe pour les génériques
def fonction[T](valeur: T) -> T:
    return valeur

class Boite[T]:
    def __init__(self, contenu: T):
        self.contenu = contenu
```

#### f-strings améliorés
```python
# Quotes imbriquées plus faciles
nom = "Alice"
print(f"Bonjour {f"{nom.upper()}"}")

# Multi-lignes plus simples
message = f"""
    Nom: {nom}
    Age: {age}
"""
```

---

## Type Hints

### Types de base
```python
# Types simples
def fonction(x: int, y: str) -> bool:
    return len(y) > x

# Types composés (Python 3.9+)
def process(data: list[int]) -> dict[str, int]:
    return {str(x): x for x in data}

# Avant Python 3.9
from typing import List, Dict
def process(data: List[int]) -> Dict[str, int]:
    pass
```

### Types avancés
```python
from typing import (
    Optional, Union, Any, Callable, 
    TypeVar, Generic, Protocol
)

# Optional (peut être None)
def fonction(x: Optional[int] = None) -> str:
    pass

# Python 3.10+
def fonction(x: int | None = None) -> str:
    pass

# Union
def fonction(x: Union[int, str]) -> Union[float, bool]:
    pass

# Python 3.10+
def fonction(x: int | str) -> float | bool:
    pass

# Any (n'importe quel type)
def fonction(x: Any) -> Any:
    pass

# Callable
def apply(func: Callable[[int, int], int], x: int, y: int) -> int:
    return func(x, y)

# TypeVar (générique)
T = TypeVar('T')

def premier_element(liste: list[T]) -> T:
    return liste[0]

# Generic
from typing import Generic

class Boite(Generic[T]):
    def __init__(self, contenu: T):
        self.contenu = contenu
    
    def get(self) -> T:
        return self.contenu

# Protocol (duck typing)
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None:
        ...

def render(obj: Drawable) -> None:
    obj.draw()

# Literal
from typing import Literal

def fonction(mode: Literal["r", "w", "a"]) -> None:
    pass

# TypedDict
from typing import TypedDict

class Personne(TypedDict):
    nom: str
    age: int
    email: str

def traiter(p: Personne) -> None:
    print(p["nom"])

# Final
from typing import Final

MAX_SIZE: Final = 100
MAX_SIZE = 200  # Erreur de type (mypy)

# NewType
from typing import NewType

UserId = NewType('UserId', int)

def get_user(user_id: UserId) -> str:
    pass

user_id = UserId(12345)
```

### Type Guards (Python 3.10+)
```python
from typing import TypeGuard

def is_string_list(val: list[object]) -> TypeGuard[list[str]]:
    return all(isinstance(x, str) for x in val)

def process(data: list[object]) -> None:
    if is_string_list(data):
        # data est maintenant list[str]
        for item in data:
            print(item.upper())
```

---

## Gestion des erreurs

### Try...Except...Finally
```python
try:
    # Code qui peut échouer
    resultat = 10 / 0
except ZeroDivisionError:
    print("Division par zéro!")
except ValueError as e:
    print(f"Erreur de valeur: {e}")
except (TypeError, KeyError):
    print("TypeError ou KeyError")
except Exception as e:
    # Catch all
    print(f"Erreur: {e}")
else:
    # Exécuté si pas d'exception
    print("Succès")
finally:
    # Toujours exécuté
    print("Nettoyage")

# Exception sans variable (Python 3.8+)
try:
    pass
except ValueError:
    pass
```

### Lever des exceptions
```python
# raise
raise ValueError("Message d'erreur")

# Re-raise
try:
    pass
except Exception as e:
    print(f"Erreur: {e}")
    raise  # Re-lever la même exception

# raise from (chaînage)
try:
    pass
except ValueError as e:
    raise TypeError("Nouvelle erreur") from e
```

### Exceptions personnalisées
```python
class MonErreur(Exception):
    """Exception personnalisée"""
    pass

class ValidationError(Exception):
    def __init__(self, field, message):
        self.field = field
        self.message = message
        super().__init__(f"{field}: {message}")

raise ValidationError("email", "Format invalide")
```

### Assertions
```python
assert condition, "Message d'erreur"

# Désactiver avec python -O script.py
assert x > 0, "x doit être positif"
```

### Context Manager pour exceptions
```python
from contextlib import suppress

# Ignorer exceptions spécifiques
with suppress(FileNotFoundError):
    os.remove("fichier_inexistant.txt")
```

---

## Fichiers et I/O

### Lecture de fichiers
```python
# Lecture complète
with open("fichier.txt", "r", encoding="utf-8") as f:
    contenu = f.read()

# Lecture ligne par ligne
with open("fichier.txt", "r") as f:
    for ligne in f:
        print(ligne.strip())

# Lecture dans une liste
with open("fichier.txt", "r") as f:
    lignes = f.readlines()

# Lecture d'une ligne
with open("fichier.txt", "r") as f:
    premiere_ligne = f.readline()

# Sans context manager (déconseillé)
f = open("fichier.txt", "r")
contenu = f.read()
f.close()
```

### Écriture de fichiers
```python
# Écriture (écrase)
with open("fichier.txt", "w", encoding="utf-8") as f:
    f.write("Contenu\n")
    f.writelines(["ligne1\n", "ligne2\n"])

# Append (ajouter)
with open("fichier.txt", "a") as f:
    f.write("Nouvelle ligne\n")

# Lecture et écriture
with open("fichier.txt", "r+") as f:
    contenu = f.read()
    f.write("Ajout")
```

### Modes d'ouverture
```python
"r"   # Lecture (défaut)
"w"   # Écriture (écrase)
"a"   # Append
"x"   # Création exclusive (erreur si existe)
"r+"  # Lecture et écriture
"w+"  # Écriture et lecture (écrase)
"a+"  # Append et lecture

"rb"  # Binaire lecture
"wb"  # Binaire écriture
```

### Manipulation de fichiers
```python
import os
import shutil
from pathlib import Path

# os module
os.path.exists("fichier.txt")
os.path.isfile("fichier.txt")
os.path.isdir("dossier")
os.path.getsize("fichier.txt")
os.path.join("dossier", "fichier.txt")

# Opérations
os.remove("fichier.txt")
os.rename("ancien.txt", "nouveau.txt")
os.mkdir("dossier")
os.makedirs("path/to/dossier", exist_ok=True)
os.rmdir("dossier")  # Doit être vide

# Lister
os.listdir("dossier")
os.walk("dossier")  # Récursif

# shutil
shutil.copy("source.txt", "dest.txt")
shutil.copytree("source_dir", "dest_dir")
shutil.move("source.txt", "dest.txt")
shutil.rmtree("dossier")  # Supprime récursivement

# pathlib (moderne, recommandé)
path = Path("dossier/fichier.txt")

path.exists()
path.is_file()
path.is_dir()
path.stat().st_size

path.read_text()
path.write_text("contenu")
path.read_bytes()
path.write_bytes(b"contenu")

path.parent        # Dossier parent
path.name          # Nom du fichier
path.stem          # Nom sans extension
path.suffix        # Extension
path.parts         # Tuple des parties

# Opérations
path.mkdir(parents=True, exist_ok=True)
path.rmdir()
path.unlink()      # Supprimer fichier
path.rename("nouveau.txt")

# Glob
list(path.glob("*.txt"))
list(path.rglob("*.txt"))  # Récursif

# Joindre chemins
path = Path("dossier") / "sous-dossier" / "fichier.txt"
```

### Fichiers temporaires
```python
import tempfile

# Fichier temporaire
with tempfile.TemporaryFile(mode='w+') as f:
    f.write("contenu")
    f.seek(0)
    print(f.read())

# Named temporary file
with tempfile.NamedTemporaryFile(mode='w+', delete=False) as f:
    print(f.name)
    f.write("contenu")

# Dossier temporaire
with tempfile.TemporaryDirectory() as tmpdir:
    print(f"Dossier: {tmpdir}")
    # Utiliser tmpdir
```

### JSON
```python
import json

# Encoder
data = {"nom": "Alice", "age": 30}
json_string = json.dumps(data)
json_pretty = json.dumps(data, indent=2, ensure_ascii=False)

# Décoder
data = json.loads(json_string)

# Fichiers
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)

with open("data.json", "r") as f:
    data = json.load(f)
```

### CSV
```python
import csv

# Lire
with open("data.csv", "r") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# Lire avec dictionnaire
with open("data.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["nom"], row["age"])

# Écrire
with open("data.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["nom", "age"])
    writer.writerows([["Alice", 30], ["Bob", 25]])

# Écrire avec dictionnaire
with open("data.csv", "w", newline='') as f:
    fieldnames = ["nom", "age"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({"nom": "Alice", "age": 30})
```

### Pickle (sérialisation Python)
```python
import pickle

# Sérialiser
data = {"nom": "Alice", "liste": [1, 2, 3]}
with open("data.pkl", "wb") as f:
    pickle.dump(data, f)

# Désérialiser
with open("data.pkl", "rb") as f:
    data = pickle.load(f)
```

---

## Modules et Packages

### Import
```python
# Import module
import math
print(math.pi)

# Import avec alias
import numpy as np

# Import spécifique
from math import pi, sqrt
from math import *  # Déconseillé

# Import relatif (dans un package)
from . import module
from .. import parent_module
from .subpackage import module
```

### Créer un module
```python
# fichier: mon_module.py
def fonction():
    return "Hello"

CONSTANTE = 42

# Utilisation
import mon_module
print(mon_module.fonction())
print(mon_module.CONSTANTE)
```

### Créer un package
```
mon_package/
    __init__.py
    module1.py
    module2.py
    sous_package/
        __init__.py
        module3.py
```

```python
# mon_package/__init__.py
from .module1 import fonction1
from .module2 import fonction2

__all__ = ['fonction1', 'fonction2']

# Utilisation
import mon_package
mon_package.fonction1()
```

### __name__ et __main__
```python
# Dans un module
def fonction():
    pass

# Exécuté seulement si lancé directement
if __name__ == "__main__":
    fonction()
    print("Module lancé directement")
```

### Modules standards utiles
```python
# sys
import sys
sys.argv        # Arguments ligne de commande
sys.path        # Chemins de recherche modules
sys.version     # Version Python
sys.exit()      # Quitter

# os
import os
os.environ      # Variables d'environnement
os.getcwd()     # Dossier courant
os.chdir()      # Changer dossier

# datetime
from datetime import datetime, date, time, timedelta

maintenant = datetime.now()
date_obj = date(2025, 10, 18)
delta = timedelta(days=7)

# random
import random
random.random()          # [0, 1)
random.randint(1, 10)    # Entier entre 1 et 10
random.choice([1,2,3])   # Élément aléatoire
random.shuffle(liste)    # Mélanger
random.sample(liste, 3)  # 3 éléments aléatoires

# itertools
import itertools

itertools.count(10)           # 10, 11, 12, ...
itertools.cycle([1,2,3])      # 1, 2, 3, 1, 2, 3, ...
itertools.repeat(10, 3)       # 10, 10, 10
itertools.chain([1,2], [3,4]) # 1, 2, 3, 4
itertools.combinations([1,2,3], 2)  # Combinaisons
itertools.permutations([1,2,3])     # Permutations

# functools
from functools import reduce, lru_cache, partial

reduce(lambda x, y: x + y, [1,2,3,4])  # Somme

@lru_cache(maxsize=128)
def fonction_couteuse(n):
    pass

double = partial(pow, 2)  # Fixe premier argument

# collections (voir section Structures de données)

# re (expressions régulières)
import re

pattern = r'\d+'
re.match(pattern, "123abc")
re.search(pattern, "abc123")
re.findall(pattern, "123 abc 456")
re.sub(pattern, "X", "123 abc 456")
```

---

## Décorateurs

### Décorateurs de fonctions
```python
# Décorateur simple
def mon_decorateur(func):
    def wrapper(*args, **kwargs):
        print("Avant")
        resultat = func(*args, **kwargs)
        print("Après")
        return resultat
    return wrapper

@mon_decorateur
def fonction():
    print("Fonction")

# Équivalent à:
# fonction = mon_decorateur(fonction)

# Avec functools.wraps (préserve métadonnées)
from functools import wraps

def mon_decorateur(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

# Décorateur avec arguments
def repeter(fois):
    def decorateur(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(fois):
                resultat = func(*args, **kwargs)
            return resultat
        return wrapper
    return decorateur

@repeter(3)
def dire_bonjour():
    print("Bonjour")

# Décorateur de classe
def singleton(cls):
    instances = {}
    @wraps(cls)
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class MaClasse:
    pass
```

### Décorateurs intégrés
```python
class MaClasse:
    # Méthode statique
    @staticmethod
    def methode_statique():
        pass
    
    # Méthode de classe
    @classmethod
    def methode_classe(cls):
        pass
    
    # Propriété
    @property
    def valeur(self):
        return self._valeur
    
    @valeur.setter
    def valeur(self, val):
        self._valeur = val

# Cache
from functools import cache, lru_cache

@cache  # Python 3.9+
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

@lru_cache(maxsize=128)
def fonction_couteuse(n):
    pass
```

### Décorateurs empilés
```python
@decorateur1
@decorateur2
@decorateur3
def fonction():
    pass

# Équivalent à:
# fonction = decorateur1(decorateur2(decorateur3(fonction)))
```

---

## Context Managers

### With statement
```python
# Fichiers
with open("fichier.txt", "r") as f:
    contenu = f.read()
# f est automatiquement fermé

# Multiple context managers
with open("in.txt", "r") as fin, open("out.txt", "w") as fout:
    contenu = fin.read()
    fout.write(contenu)
```

### Créer un context manager

#### Avec classe
```python
class MonContextManager:
    def __enter__(self):
        print("Entrée")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Sortie")
        # Retourner True pour supprimer l'exception
        return False

with MonContextManager() as cm:
    print("Dans le contexte")
```

#### Avec contextlib
```python
from contextlib import contextmanager

@contextmanager
def mon_context_manager():
    print("Entrée")
    try:
        yield "valeur"
    finally:
        print("Sortie")

with mon_context_manager() as valeur:
    print(f"Valeur: {valeur}")
```

### Context managers utiles
```python
from contextlib import suppress, redirect_stdout, ExitStack

# Supprimer exceptions
with suppress(FileNotFoundError, KeyError):
    os.remove("inexistant.txt")

# Rediriger stdout
import io
f = io.StringIO()
with redirect_stdout(f):
    print("Capturé")
output = f.getvalue()

# ExitStack (context managers dynamiques)
with ExitStack() as stack:
    files = [stack.enter_context(open(f, 'r')) for f in fichiers]
    # Tous les fichiers sont fermés à la sortie
```

---

## Async/Await

### Coroutines de base
```python
import asyncio

# Définir une coroutine
async def dire_bonjour():
    print("Bonjour")
    await asyncio.sleep(1)
    print("Au revoir")

# Exécuter
asyncio.run(dire_bonjour())

# Plusieurs coroutines
async def main():
    await asyncio.gather(
        dire_bonjour(),
        autre_coroutine(),
        encore_une()
    )

asyncio.run(main())
```

### Async avec retour
```python
async def obtenir_donnees(url):
    # Simulation requête
    await asyncio.sleep(1)
    return f"Données de {url}"

async def main():
    # Sequential
    resultat1 = await obtenir_donnees("url1")
    resultat2 = await obtenir_donnees("url2")
    
    # Concurrent
    resultats = await asyncio.gather(
        obtenir_donnees("url1"),
        obtenir_donnees("url2"),
        obtenir_donnees("url3")
    )
    print(resultats)

asyncio.run(main())
```

### Tasks
```python
async def main():
    # Créer task
    task1 = asyncio.create_task(coro1())
    task2 = asyncio.create_task(coro2())
    
    # Attendre
    await task1
    await task2
    
    # Ou attendre tous
    await asyncio.gather(task1, task2)
    
    # Task groups (Python 3.11+)
    async with asyncio.TaskGroup() as tg:
        task1 = tg.create_task(coro1())
        task2 = tg.create_task(coro2())
```

### Async iterators
```python
class AsyncIterator:
    def __init__(self, limit):
        self.limit = limit
        self.count = 0
    
    def __aiter__(self):
        return self
    
    async def __anext__(self):
        if self.count >= self.limit:
            raise StopAsyncIteration
        await asyncio.sleep(0.1)
        self.count += 1
        return self.count

async def main():
    async for i in AsyncIterator(5):
        print(i)

# Async comprehension
async def main():
    result = [i async for i in AsyncIterator(5)]
```

### Async context managers
```python
class AsyncContextManager:
    async def __aenter__(self):
        await asyncio.sleep(0.1)
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        await asyncio.sleep(0.1)

async def main():
    async with AsyncContextManager() as cm:
        print("Dans le contexte")
```

---

## Fonctions utiles

### Built-in Functions

#### Conversions
```python
int("42")           # String vers int
float("3.14")       # String vers float
str(42)             # Vers string
bool(1)             # Vers boolean
list("abc")         # Vers liste
tuple([1, 2])       # Vers tuple
set([1, 2, 2])      # Vers set
dict([("a", 1)])    # Vers dict

chr(65)             # Code vers caractère ('A')
ord('A')            # Caractère vers code (65)
hex(255)            # Vers hexa ('0xff')
bin(10)             # Vers binaire ('0b1010')
oct(8)              # Vers octal ('0o10')
```

#### Mathématiques
```python
abs(-5)             # Valeur absolue
round(3.7)          # Arrondi
round(3.14159, 2)   # 2 décimales
pow(2, 3)           # Puissance
divmod(10, 3)       # (3, 1) quotient et reste

sum([1, 2, 3])      # Somme
min([1, 2, 3])      # Minimum
max([1, 2, 3])      # Maximum
```

#### Itérables
```python
# all, any
all([True, True, True])   # True si tous
any([False, True, False]) # True si au moins un

# enumerate
for i, val in enumerate(['a', 'b', 'c']):
    print(i, val)

# zip
for x, y in zip([1, 2], ['a', 'b']):
    print(x, y)

# map
carres = map(lambda x: x**2, [1, 2, 3])

# filter
pairs = filter(lambda x: x % 2 == 0, [1, 2, 3, 4])

# reversed
for i in reversed([1, 2, 3]):
    print(i)

# sorted
sorted([3, 1, 2])
sorted(["c", "a", "b"])
sorted(personnes, key=lambda p: p['age'])

# range
range(5)           # 0 à 4
range(1, 5)        # 1 à 4
range(0, 10, 2)    # 0, 2, 4, 6, 8

# len
len([1, 2, 3])     # Longueur
```

#### Informations
```python
type(var)           # Type
isinstance(var, int)  # Vérifier type
id(var)             # Identité (adresse mémoire)
dir(obj)            # Attributs et méthodes
help(fonction)      # Documentation
vars(obj)           # __dict__
callable(obj)       # Est-ce callable
hasattr(obj, 'attr')  # A l'attribut
getattr(obj, 'attr', default)  # Obtenir attribut
setattr(obj, 'attr', value)    # Définir attribut
delattr(obj, 'attr')           # Supprimer attribut
```

#### Autres
```python
input("Message: ")  # Lire input utilisateur
print(*args, sep=' ', end='\n')

open(file, mode)    # Ouvrir fichier

eval("2 + 2")       # Évaluer expression
exec("x = 5")       # Exécuter code

compile(source, filename, mode)  # Compiler code

repr(obj)           # Représentation
ascii(obj)          # Représentation ASCII

hash(obj)           # Hash
```

### String methods
```python
# Casse
s.upper()           # MAJUSCULES
s.lower()           # minuscules
s.capitalize()      # Première lettre majuscule
s.title()           # Chaque Mot Commence Majuscule
s.swapcase()        # Inverse la casse
s.casefold()        # Lowercase agressif (pour comparaisons)

# Recherche
s.find("sub")       # Index (-1 si non trouvé)
s.rfind("sub")      # Index depuis la fin
s.index("sub")      # Index (ValueError si non trouvé)
s.count("sub")      # Nombre d'occurrences
s.startswith("pre")
s.endswith("suf")

# Modification
s.replace("old", "new")
s.replace("old", "new", 1)  # Remplacer 1 fois
s.strip()           # Supprimer espaces début/fin
s.lstrip()          # Gauche
s.rstrip()          # Droite
s.strip(",.!")      # Caractères personnalisés

# Split/Join
s.split()           # Diviser par espaces
s.split(",")        # Diviser par délimiteur
s.rsplit(",", 1)    # Split depuis la fin
s.splitlines()      # Diviser par lignes
",".join(liste)     # Joindre

# Formatage
s.center(10)        # Centrer
s.ljust(10)         # Aligner gauche
s.rjust(10)         # Aligner droite
s.zfill(5)          # Remplir de zéros: "00042"

# Vérifications
s.isalpha()         # Que des lettres
s.isdigit()         # Que des chiffres
s.isalnum()         # Lettres ou chiffres
s.isspace()         # Que des espaces
s.islower()
s.isupper()
s.istitle()
```

### Collections methods
```python
# Liste
liste.append(x)
liste.extend(iterable)
liste.insert(i, x)
liste.remove(x)
liste.pop([i])
liste.clear()
liste.index(x)
liste.count(x)
liste.sort()
liste.reverse()
liste.copy()

# Dict
d.keys()
d.values()
d.items()
d.get(key, default)
d.pop(key)
d.popitem()
d.setdefault(key, default)
d.update(other_dict)
d.clear()

# Set
s.add(x)
s.update(iterable)
s.remove(x)
s.discard(x)
s.pop()
s.clear()
s.union(other)
s.intersection(other)
s.difference(other)
s.symmetric_difference(other)
s.issubset(other)
s.issuperset(other)
```

---

## Bonnes pratiques

### PEP 8 (Style Guide)
```python
# Nommage
variable_name        # snake_case pour variables et fonctions
ClassName            # PascalCase pour classes
CONSTANT_NAME        # SCREAMING_SNAKE_CASE pour constantes
_private_var         # Underscore pour "privé"
__name_mangling      # Double underscore pour name mangling

# Indentation: 4 espaces

# Longueur de ligne: 79 caractères (max 99)

# Imports
# - En haut du fichier
# - Imports stdlib, puis tiers, puis locaux
# - Un import par ligne (sauf from)

import os
import sys

from math import pi, sqrt

import numpy as np

from .local import module

# Espaces
# - Autour des opérateurs: x = 1 + 2
# - Après virgules: [1, 2, 3]
# - Pas d'espaces: func(arg)

# Quotes
# - Simple ou double, être consistant
# - Triple quotes pour docstrings

# Docstrings
def fonction(arg1, arg2):
    """
    Description courte.
    
    Description longue si nécessaire.
    
    Args:
        arg1: Description
        arg2: Description
        
    Returns:
        Description du retour
        
    Raises:
        ValueError: Si arg1 invalide
    """
    pass
```

### Type Checking
```python
# Utiliser mypy pour vérifier les types
# pip install mypy
# mypy script.py

# Utiliser type hints
def fonction(x: int, y: str) -> bool:
    return len(y) > x
```

### Environnements virtuels
```bash
# Créer environnement virtuel
python -m venv venv

# Activer
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Désactiver
deactivate

# Requirements
pip freeze > requirements.txt
pip install -r requirements.txt
```

### Tests
```python
# unittest
import unittest

class TestFonctions(unittest.TestCase):
    def test_addition(self):
        self.assertEqual(1 + 1, 2)
    
    def test_division(self):
        with self.assertRaises(ZeroDivisionError):
            1 / 0

if __name__ == '__main__':
    unittest.main()

# pytest (recommandé)
def test_addition():
    assert 1 + 1 == 2

def test_division():
    import pytest
    with pytest.raises(ZeroDivisionError):
        1 / 0

# Lancer: pytest
```

### Logging
```python
import logging

# Configuration
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    filename='app.log'
)

logger = logging.getLogger(__name__)

# Niveaux
logger.debug("Debug message")
logger.info("Info message")
logger.warning("Warning message")
logger.error("Error message")
logger.critical("Critical message")
```

---

## Ressources

- [Documentation officielle Python](https://docs.python.org/3/)
- [PEP 8 - Style Guide](https://pep8.org/)
- [Python Package Index (PyPI)](https://pypi.org/)
- [Real Python](https://realpython.com/)
- [Python.org](https://www.python.org/)

---

