# TypeScript CheatSheet 

## Table des matières
1. [Types de base](#types-de-base)
2. [Types avancés](#types-avancés)
3. [Interfaces & Types](#interfaces--types)
4. [Classes](#classes)
5. [Generics](#generics)
6. [Utilitaires de types](#utilitaires-de-types)
7. [Nouveautés TypeScript 5.x](#nouveautés-typescript-5x)
8. [Decorators](#decorators)
9. [Modules](#modules)
10. [Configuration](#configuration)

---

## Types de base

### Types primitifs
```typescript
// String
let nom: string = "Alice";

// Number
let age: number = 25;
let hex: number = 0xf00d;
let binary: number = 0b1010;

// Boolean
let estActif: boolean = true;

// Null & Undefined
let n: null = null;
let u: undefined = undefined;

// Symbol
let sym: symbol = Symbol("clé");

// BigInt
let grand: bigint = 100n;
```

### Any, Unknown, Never, Void
```typescript
// any - éviter autant que possible
let quelqueChose: any = 4;
quelqueChose = "maintenant une string";

// unknown - type safe any
let valeur: unknown = 4;
if (typeof valeur === "string") {
  console.log(valeur.toUpperCase()); // OK après vérification
}

// never - ne retourne jamais
function erreur(message: string): never {
  throw new Error(message);
}

// void - pas de valeur de retour
function log(message: string): void {
  console.log(message);
}
```

### Arrays & Tuples
```typescript
// Arrays
let nombres: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];
let readonly: ReadonlyArray<number> = [1, 2, 3];

// Tuples
let tuple: [string, number] = ["age", 30];
let tupleLabelé: [nom: string, age: number] = ["Bob", 25];

// Tuple avec rest
let tupleRest: [string, ...number[]] = ["hello", 1, 2, 3];

// Tuple readonly
let tupleRO: readonly [string, number] = ["x", 10];
```

### Enums
```typescript
// Enum numérique
enum Direction {
  Haut = 1,
  Bas,
  Gauche,
  Droite
}

// Enum string
enum Couleur {
  Rouge = "ROUGE",
  Vert = "VERT",
  Bleu = "BLEU"
}

// Const enum (optimisé)
const enum Status {
  Actif = "ACTIF",
  Inactif = "INACTIF"
}

// Enum hétérogène (déconseillé)
enum Mixte {
  Non = 0,
  Oui = "OUI"
}
```

---

## Types avancés

### Union & Intersection
```typescript
// Union types
type ID = string | number;
let userId: ID = 123;
userId = "abc123";

function afficher(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}

// Intersection types
type Nom = { nom: string };
type Age = { age: number };
type Personne = Nom & Age;

let personne: Personne = { nom: "Alice", age: 30 };
```

### Literal Types
```typescript
// String literals
type Direction = "nord" | "sud" | "est" | "ouest";
let dir: Direction = "nord";

// Numeric literals
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;

// Boolean literals
type Vrai = true;

// Template literal types
type EmailLocale = `${string}@${string}.${string}`;
let email: EmailLocale = "test@example.com";
```

### Type Guards
```typescript
// typeof
function example(x: string | number) {
  if (typeof x === "string") {
    return x.toUpperCase();
  }
  return x.toFixed(2);
}

// instanceof
class Chat { miauler() {} }
class Chien { aboyer() {} }

function faireBruit(animal: Chat | Chien) {
  if (animal instanceof Chat) {
    animal.miauler();
  } else {
    animal.aboyer();
  }
}

// in operator
type Poisson = { nager: () => void };
type Oiseau = { voler: () => void };

function bouger(animal: Poisson | Oiseau) {
  if ("nager" in animal) {
    animal.nager();
  } else {
    animal.voler();
  }
}

// Type predicate
function estString(value: unknown): value is string {
  return typeof value === "string";
}

// Discriminated unions
type Succès = { type: "succès"; data: string };
type Erreur = { type: "erreur"; error: Error };
type Résultat = Succès | Erreur;

function traiter(result: Résultat) {
  if (result.type === "succès") {
    console.log(result.data);
  } else {
    console.log(result.error);
  }
}
```

### Index Signatures
```typescript
// Index signature simple
interface StringDictionary {
  [key: string]: string;
}

// Index signature avec propriétés connues
interface MixedDictionary {
  [key: string]: number;
  length: number; // doit être du même type
}

// Multiple index signatures
interface MultiIndex {
  [key: string]: number | string;
  [key: number]: number;
}

// Record utility
type Dictionary = Record<string, number>;
```

### Conditional Types
```typescript
// Basic conditional
type IsString<T> = T extends string ? true : false;

// Distributive conditional types
type ToArray<T> = T extends any ? T[] : never;
type StrOrNum = ToArray<string | number>; // string[] | number[]

// infer keyword
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
type UnpackPromise<T> = T extends Promise<infer U> ? U : T;

// Conditional chains
type TypeName<T> = 
  T extends string ? "string" :
  T extends number ? "number" :
  T extends boolean ? "boolean" :
  "object";
```

### Mapped Types
```typescript
// Basic mapped type
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// Optional mapped type
type Partial<T> = {
  [P in keyof T]?: T[P];
};

// Modifier de mapping
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};

type Required<T> = {
  [P in keyof T]-?: T[P];
};

// Key remapping
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

// Filtered keys
type PickByType<T, U> = {
  [K in keyof T as T[K] extends U ? K : never]: T[K];
};
```

---

## Interfaces & Types

### Interfaces
```typescript
// Interface basique
interface Utilisateur {
  id: number;
  nom: string;
  email?: string; // optionnel
  readonly dateCreation: Date; // readonly
}

// Extension d'interface
interface Admin extends Utilisateur {
  privilèges: string[];
}

// Interfaces multiples
interface Identifiable { id: number; }
interface Nommable { nom: string; }
interface Entité extends Identifiable, Nommable {}

// Interface pour fonctions
interface Calculatrice {
  (a: number, b: number): number;
}

const additionner: Calculatrice = (x, y) => x + y;

// Interface hybride
interface Compteur {
  (debut: number): string;
  interval: number;
  reset(): void;
}

// Declaration merging
interface Fenêtre {
  titre: string;
}
interface Fenêtre {
  largeur: number;
}
// Fenêtre a maintenant titre et largeur
```

### Type Aliases
```typescript
// Type alias basique
type Point = {
  x: number;
  y: number;
};

// Union type alias
type Résultat = Succès | Échec;

// Intersection type alias
type UtilisateurComplet = Utilisateur & Admin & Metadata;

// Function type alias
type Opération = (a: number, b: number) => number;

// Generic type alias
type Container<T> = { value: T };

// Recursive type alias
type JSONValue = 
  | string
  | number
  | boolean
  | null
  | JSONValue[]
  | { [key: string]: JSONValue };

// Conditional type alias
type NonNullable<T> = T extends null | undefined ? never : T;
```

### Interface vs Type
```typescript
// ✅ Interface peut être étendue
interface Animal { nom: string; }
interface Chien extends Animal { race: string; }

// ✅ Type peut utiliser unions/intersections
type Pet = Chien | Chat;

// ✅ Interface supporte declaration merging
interface Utilisateur { nom: string; }
interface Utilisateur { age: number; }

// ✅ Type peut créer des aliases pour primitifs
type ID = string | number;

// ⚠️ Préférer interface pour objets publics
// ⚠️ Préférer type pour unions/utilitaires complexes
```

---

## Classes

### Classes de base
```typescript
class Personne {
  // Propriétés
  nom: string;
  age: number;
  private _secret: string; // private
  protected familyName: string; // protected
  readonly id: number; // readonly

  // Constructeur
  constructor(nom: string, age: number) {
    this.nom = nom;
    this.age = age;
    this.id = Date.now();
  }

  // Méthode
  présenter(): string {
    return `Je suis ${this.nom}, ${this.age} ans`;
  }

  // Getter
  get secret(): string {
    return this._secret;
  }

  // Setter
  set secret(value: string) {
    this._secret = value;
  }

  // Méthode statique
  static créer(nom: string): Personne {
    return new Personne(nom, 0);
  }
}

// Shorthand constructor
class Point {
  constructor(
    public x: number,
    public y: number,
    private id: string = "default"
  ) {}
}
```

### Héritage et Polymorphisme
```typescript
// Classe abstraite
abstract class Forme {
  abstract calculerAire(): number;
  
  décrire(): string {
    return `Aire: ${this.calculerAire()}`;
  }
}

class Cercle extends Forme {
  constructor(private rayon: number) {
    super();
  }

  calculerAire(): number {
    return Math.PI * this.rayon ** 2;
  }

  // Override
  override décrire(): string {
    return `Cercle - ${super.décrire()}`;
  }
}

// Implements
interface Drawable {
  dessiner(): void;
}

class Rectangle extends Forme implements Drawable {
  constructor(
    private largeur: number,
    private hauteur: number
  ) {
    super();
  }

  calculerAire(): number {
    return this.largeur * this.hauteur;
  }

  dessiner(): void {
    console.log("Dessin rectangle");
  }
}
```

### Modificateurs d'accès
```typescript
class BankAccount {
  public readonly accountNumber: string; // accessible partout
  protected balance: number; // accessible dans classe et sous-classes
  private #pin: number; // vraiment privé (JavaScript private)

  constructor(accountNumber: string, initialBalance: number, pin: number) {
    this.accountNumber = accountNumber;
    this.balance = initialBalance;
    this.#pin = pin;
  }

  // Méthode privée
  private validatePin(pin: number): boolean {
    return this.#pin === pin;
  }

  public withdraw(amount: number, pin: number): boolean {
    if (!this.validatePin(pin)) return false;
    if (this.balance >= amount) {
      this.balance -= amount;
      return true;
    }
    return false;
  }
}
```

---

## Generics

### Fonctions génériques
```typescript
// Generic basique
function identité<T>(arg: T): T {
  return arg;
}

let output = identité<string>("hello");
let inferred = identité(42); // type inféré

// Multiple type parameters
function paire<T, U>(premier: T, second: U): [T, U] {
  return [premier, second];
}

// Generic constraints
function longueur<T extends { length: number }>(arg: T): number {
  return arg.length;
}

// Using type parameters in constraints
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Generic avec valeur par défaut
function créerArray<T = string>(length: number, value: T): T[] {
  return Array(length).fill(value);
}
```

### Classes génériques
```typescript
// Classe générique
class Boîte<T> {
  constructor(private contenu: T) {}

  getContenu(): T {
    return this.contenu;
  }

  setContenu(contenu: T): void {
    this.contenu = contenu;
  }
}

let boîteString = new Boîte<string>("hello");
let boîteNumber = new Boîte<number>(42);

// Generic avec contraintes
class Collection<T extends { id: number }> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  findById(id: number): T | undefined {
    return this.items.find(item => item.id === id);
  }
}
```

### Interfaces génériques
```typescript
// Interface générique
interface Réponse<T> {
  data: T;
  status: number;
  message: string;
}

let userResponse: Réponse<Utilisateur> = {
  data: { id: 1, nom: "Alice" },
  status: 200,
  message: "OK"
};

// Generic avec fonction
interface Transformer<T, U> {
  (input: T): U;
}

const numberToString: Transformer<number, string> = (n) => n.toString();

// Generic interface avec index signature
interface Dictionary<T> {
  [key: string]: T;
}

let nombres: Dictionary<number> = {
  un: 1,
  deux: 2
};
```

### Types génériques avancés
```typescript
// Conditional generic type
type TypeOrArray<T> = T extends any[] ? T : T[];

// Generic mapped type
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

// Generic avec infer
type FunctionReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

// Recursive generic type
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Variadic tuple types
type Concat<T extends any[], U extends any[]> = [...T, ...U];
type Result = Concat<[1, 2], [3, 4]>; // [1, 2, 3, 4]
```

---

## Utilitaires de types

### Utilitaires de base
```typescript
// Partial - rend toutes les propriétés optionnelles
type PartialUser = Partial<Utilisateur>;

// Required - rend toutes les propriétés requises
type RequiredUser = Required<Partial<Utilisateur>>;

// Readonly - rend toutes les propriétés readonly
type ReadonlyUser = Readonly<Utilisateur>;

// Record - crée un type objet
type PageInfo = Record<'home' | 'about' | 'contact', { title: string }>;

// Pick - sélectionne certaines propriétés
type UserPreview = Pick<Utilisateur, 'nom' | 'email'>;

// Omit - exclut certaines propriétés
type UserWithoutId = Omit<Utilisateur, 'id'>;

// Exclude - exclut des types d'une union
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"

// Extract - extrait des types d'une union
type T1 = Extract<"a" | "b" | "c", "a" | "f">; // "a"

// NonNullable - exclut null et undefined
type T2 = NonNullable<string | number | undefined>; // string | number
```

### Utilitaires de fonctions
```typescript
// ReturnType - extrait le type de retour
type T3 = ReturnType<() => string>; // string

// Parameters - extrait les types des paramètres
type T4 = Parameters<(a: string, b: number) => void>; // [string, number]

// ConstructorParameters - paramètres du constructeur
class C {
  constructor(a: string, b: number) {}
}
type T5 = ConstructorParameters<typeof C>; // [string, number]

// InstanceType - type d'instance
type T6 = InstanceType<typeof C>; // C

// ThisParameterType - type du this
function toHex(this: Number) {
  return this.toString(16);
}
type T7 = ThisParameterType<typeof toHex>; // Number

// OmitThisParameter - enlève le paramètre this
type T8 = OmitThisParameter<typeof toHex>; // () => string
```

### Utilitaires de strings
```typescript
// Uppercase - convertit en majuscules
type T9 = Uppercase<"hello">; // "HELLO"

// Lowercase - convertit en minuscules
type T10 = Lowercase<"HELLO">; // "hello"

// Capitalize - première lettre en majuscule
type T11 = Capitalize<"hello">; // "Hello"

// Uncapitalize - première lettre en minuscule
type T12 = Uncapitalize<"Hello">; // "hello"

// Exemple pratique
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

interface Person {
  name: string;
  age: number;
}

type PersonGetters = Getters<Person>;
// { getName: () => string; getAge: () => number; }
```

### Utilitaires personnalisés
```typescript
// DeepReadonly
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object 
    ? DeepReadonly<T[P]> 
    : T[P];
};

// Awaited (built-in depuis TS 4.5)
type A = Awaited<Promise<string>>; // string
type B = Awaited<Promise<Promise<number>>>; // number

// PickByValue - sélectionne par type de valeur
type PickByValue<T, V> = {
  [K in keyof T as T[K] extends V ? K : never]: T[K];
};

// RequireAtLeastOne
type RequireAtLeastOne<T, Keys extends keyof T = keyof T> = 
  Pick<T, Exclude<keyof T, Keys>> 
  & {
    [K in Keys]-?: Required<Pick<T, K>> & Partial<Pick<T, Exclude<Keys, K>>>;
  }[Keys];

// RequireOnlyOne
type RequireOnlyOne<T, Keys extends keyof T = keyof T> = 
  Pick<T, Exclude<keyof T, Keys>> 
  & {
    [K in Keys]-?: Required<Pick<T, K>> & Partial<Record<Exclude<Keys, K>, undefined>>;
  }[Keys];
```

---

## Nouveautés TypeScript 5.x

### TypeScript 5.0

#### Decorators (Stage 3)
```typescript
// Decorator de classe
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
}

// Decorator de méthode
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Appel de ${propertyKey} avec`, args);
    return originalMethod.apply(this, args);
  };
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

// Auto-accessor decorators
class Person {
  accessor name: string = "";
}
```

#### const Type Parameters
```typescript
// Force les inférences littérales
function makeArray<const T>(arr: T[]): T[] {
  return arr;
}

const arr1 = makeArray([1, 2, 3]); // type: number[]
const arr2 = makeArray([1, 2, 3] as const); // type: readonly [1, 2, 3]

// Avec const type parameter
function makeArrayConst<const T>(arr: T[]): T[] {
  return arr;
}

const arr3 = makeArrayConst([1, 2, 3]); // type: (1 | 2 | 3)[]
```

#### Enums comme Union Types
```typescript
// TypeScript 5.0 améliore les enums
enum LogLevel {
  ERROR,
  WARN,
  INFO,
  DEBUG
}

// Peut être utilisé comme union
function log(level: LogLevel, message: string) {
  // ...
}

// Type narrowing amélioré
function check(level: LogLevel) {
  if (level === LogLevel.ERROR) {
    // level est maintenant LogLevel.ERROR
  }
}
```

### TypeScript 5.1

#### Undefined-Returning Functions in Void Context
```typescript
// Avant: erreur, maintenant: OK
function doSomething(callback: () => void) {
  callback();
}

doSomething(() => {
  return undefined; // OK maintenant
});
```

#### Unrelated Types for getters and setters
```typescript
class Thing {
  #size = 0;

  // getter retourne un type
  get size(): number {
    return this.#size;
  }

  // setter accepte un type différent
  set size(value: number | string | boolean) {
    const num = Number(value);
    if (!Number.isFinite(num)) {
      this.#size = 0;
      return;
    }
    this.#size = num;
  }
}
```

### TypeScript 5.2

#### using Declarations
```typescript
// Gestion automatique des ressources
interface Disposable {
  [Symbol.dispose](): void;
}

function doWork() {
  using file = getFileHandle(); // dispose() appelé automatiquement
  // ... utiliser file
} // file.dispose() appelé ici

// Async disposal
interface AsyncDisposable {
  [Symbol.asyncDispose](): Promise<void>;
}

async function doAsyncWork() {
  await using connection = await getConnection();
  // ... utiliser connection
} // await connection.asyncDispose() appelé ici
```

#### Decorator Metadata
```typescript
// Métadonnées pour decorators
type Metadata = {
  name: string;
  version: number;
};

function setMetadata(metadata: Metadata) {
  return (target: any, context: ClassDecoratorContext) => {
    context.metadata[context.name] = metadata;
  };
}

@setMetadata({ name: "MyClass", version: 1 })
class MyClass {}
```

### TypeScript 5.3

#### Import Attributes
```typescript
// Import avec attributs
import data from "./data.json" with { type: "json" };

// Import de modules
import styles from "./styles.css" with { type: "css" };
```

#### Narrowing avec switch(true)
```typescript
function processValue(value: string | number) {
  switch (true) {
    case typeof value === "string":
      // value est string ici
      console.log(value.toUpperCase());
      break;
    case typeof value === "number":
      // value est number ici
      console.log(value.toFixed(2));
      break;
  }
}
```

### TypeScript 5.4

#### NoInfer Utility Type
```typescript
// Empêche l'inférence de type
function createStreetLight<C extends string>(
  colors: C[],
  defaultColor?: NoInfer<C>
) {
  // ...
}

createStreetLight(["red", "yellow", "green"], "red"); // OK
createStreetLight(["red", "yellow", "green"], "blue"); // Erreur
```

#### Preserve Narrowing in Closures
```typescript
// Meilleure préservation du narrowing
function processValue(value: string | number) {
  if (typeof value === "string") {
    // value est string
    setTimeout(() => {
      // value est toujours string ici maintenant
      console.log(value.toUpperCase());
    }, 100);
  }
}
```

### TypeScript 5.5

#### Inferred Type Predicates
```typescript
// Les type predicates sont maintenant inférés
function isString(value: unknown) {
  return typeof value === "string";
}

// TypeScript infère maintenant: value is string
function processValue(value: unknown) {
  if (isString(value)) {
    // value est string ici
    console.log(value.toUpperCase());
  }
}

// Fonctionne avec filter
const values: unknown[] = [1, "hello", 2, "world"];
const strings = values.filter(isString); // string[]
```

#### Regular Expression Syntax Checking
```typescript
// Vérification de syntaxe regex
const regex1 = /[a-z]/; // OK
const regex2 = /[z-a]/; // Erreur: range invalide
const regex3 = /(?<name>\w+)/; // OK - named group
```

---

## Decorators

### Class Decorators
```typescript
function Component(config: { selector: string }) {
  return function<T extends { new(...args: any[]): {} }>(constructor: T) {
    return class extends constructor {
      selector = config.selector;
    };
  };
}

@Component({ selector: 'app-root' })
class AppComponent {
  title = "Mon App";
}
```

### Method Decorators
```typescript
function Memoize(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const original = descriptor.value;
  const cache = new Map();

  descriptor.value = function(...args: any[]) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = original.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

class Calculator {
  @Memoize
  fibonacci(n: number): number {
    if (n <= 1) return n;
    return this.fibonacci(n - 1) + this.fibonacci(n - 2);
  }
}
```

### Property Decorators
```typescript
function MinLength(length: number) {
  return function(target: any, propertyKey: string) {
    let value: string;

    const getter = () => value;
    const setter = (newVal: string) => {
      if (newVal.length < length) {
        throw new Error(`${propertyKey} doit avoir au moins ${length} caractères`);
      }
      value = newVal;
    };

    Object.defineProperty(target, propertyKey, {
      get: getter,
      set: setter,
      enumerable: true,
      configurable: true
    });
  };
}

class User {
  @MinLength(3)
  username: string;
}
```

### Parameter Decorators
```typescript
function Required(
  target: Object,
  propertyKey: string | symbol,
  parameterIndex: number
) {
  const existingRequiredParameters: number[] = 
    Reflect.getOwnMetadata("required", target, propertyKey) || [];
  
  existingRequiredParameters.push(parameterIndex);
  
  Reflect.defineMetadata(
    "required",
    existingRequiredParameters,
    target,
    propertyKey
  );
}

class UserService {
  createUser(@Required name: string, age?: number) {
    // ...
  }
}
```

---

## Modules

### Export & Import
```typescript
// ===== Export =====

// Named exports
export const PI = 3.14;
export function addition(a: number, b: number): number {
  return a + b;
}
export class Utilisateur {}

// Export groupé
const A = 1;
const B = 2;
export { A, B };

// Export avec alias
export { A as ConstanteA, B as ConstanteB };

// Re-export
export { Utilisateur as User } from './utilisateur';
export * from './utils';
export * as utils from './utils';

// Default export
export default class Application {}

// ===== Import =====

// Named imports
import { PI, addition } from './math';

// Import avec alias
import { Utilisateur as User } from './utilisateur';

// Import tout
import * as math from './math';

// Import default
import Application from './app';

// Import default + named
import React, { useState, useEffect } from 'react';

// Import pour side-effects
import './polyfills';

// Dynamic import
const module = await import('./module');

// Import type (supprimé à la compilation)
import type { User } from './types';
import { type Config, data } from './config';
```

### Module Resolution
```typescript
// Triple-slash directives
/// <reference path="./types.d.ts" />
/// <reference types="node" />

// Module augmentation
declare module 'express' {
  interface Request {
    user?: User;
  }
}

// Global augmentation
declare global {
  interface Window {
    myApp: App;
  }
}

// Ambient modules
declare module '*.css' {
  const content: { [className: string]: string };
  export default content;
}

declare module '*.jpg' {
  const content: string;
  export default content;
}
```

### Namespace (déconseillé, préférer modules ES6)
```typescript
namespace Geometry {
  export interface Point {
    x: number;
    y: number;
  }

  export function distance(p1: Point, p2: Point): number {
    return Math.sqrt(
      Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2)
    );
  }
}

// Utilisation
const p1: Geometry.Point = { x: 0, y: 0 };
const p2: Geometry.Point = { x: 3, y: 4 };
const dist = Geometry.distance(p1, p2);
```

---

## Configuration

### tsconfig.json
```json
{
  "compilerOptions": {
    // Language & Environment
    "target": "ES2022",                    // Version JavaScript cible
    "lib": ["ES2022", "DOM"],              // Bibliothèques incluses
    "jsx": "react-jsx",                    // Mode JSX
    "experimentalDecorators": true,        // Decorators experimentaux
    "emitDecoratorMetadata": true,         // Métadonnées decorators
    "useDefineForClassFields": true,       // Sémantique moderne des champs

    // Modules
    "module": "ESNext",                    // Système de modules
    "moduleResolution": "bundler",         // Résolution moderne (Bundler)
    "resolveJsonModule": true,             // Import JSON
    "allowImportingTsExtensions": true,    // Import .ts dans .ts
    "noEmit": true,                        // Pas d'émission (pour bundler)
    
    // JavaScript Support
    "allowJs": true,                       // Accepter fichiers JS
    "checkJs": false,                      // Vérifier fichiers JS
    
    // Emit
    "declaration": true,                   // Générer .d.ts
    "declarationMap": true,                // Source maps pour .d.ts
    "sourceMap": true,                     // Source maps
    "outDir": "./dist",                    // Dossier de sortie
    "removeComments": true,                // Enlever commentaires
    "importHelpers": true,                 // Importer helpers de tslib
    "downlevelIteration": true,            // Iteration pour anciennes versions
    
    // Interop Constraints
    "isolatedModules": true,               // Chaque fichier est un module
    "allowSyntheticDefaultImports": true,  // Import default synthétique
    "esModuleInterop": true,               // Interop ESM/CommonJS
    "forceConsistentCasingInFileNames": true, // Casse cohérente
    
    // Type Checking
    "strict": true,                        // Mode strict (active tout ci-dessous)
    "noImplicitAny": true,                 // Pas de any implicite
    "strictNullChecks": true,              // Vérification null/undefined
    "strictFunctionTypes": true,           // Vérification types fonctions
    "strictBindCallApply": true,           // Vérification bind/call/apply
    "strictPropertyInitialization": true,  // Init propriétés dans constructeur
    "noImplicitThis": true,                // Pas de this implicite
    "alwaysStrict": true,                  // Mode strict JS
    
    // Additional Checks
    "noUnusedLocals": true,                // Erreur variables non utilisées
    "noUnusedParameters": true,            // Erreur paramètres non utilisés
    "noImplicitReturns": true,             // Return explicite
    "noFallthroughCasesInSwitch": true,    // Pas de fallthrough dans switch
    "noUncheckedIndexedAccess": true,      // Index access retourne T | undefined
    "allowUnusedLabels": false,            // Pas de labels non utilisés
    "allowUnreachableCode": false,         // Pas de code inaccessible
    
    // Completeness
    "skipLibCheck": true                   // Skip vérif .d.ts node_modules
  },
  
  "include": [
    "src/**/*"
  ],
  
  "exclude": [
    "node_modules",
    "dist",
    "**/*.spec.ts"
  ],
  
  // Project References
  "references": [
    { "path": "./packages/core" }
  ]
}
```

### Configuration par environnement
```json
// tsconfig.base.json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022"
  }
}

// tsconfig.json (dev)
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "sourceMap": true,
    "noEmit": true
  }
}

// tsconfig.prod.json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "sourceMap": false,
    "removeComments": true,
    "declaration": true
  }
}
```

### Types de déclaration (.d.ts)
```typescript
// types/global.d.ts
declare global {
  interface Window {
    myAPI: {
      version: string;
      init: () => void;
    };
  }
  
  const __DEV__: boolean;
  const __VERSION__: string;
}

export {};

// types/modules.d.ts
declare module '*.module.css' {
  const classes: { [key: string]: string };
  export default classes;
}

declare module '*.svg' {
  import { FC, SVGProps } from 'react';
  const content: FC<SVGProps<SVGSVGElement>>;
  export default content;
}

// types/custom.d.ts
declare namespace MyLib {
  interface Config {
    apiKey: string;
    timeout: number;
  }
  
  function init(config: Config): void;
  function getData(): Promise<any>;
}

// Module ambient
declare module 'my-untyped-library' {
  export function doSomething(value: string): number;
  export const VERSION: string;
}
```

---

## Best Practices

### 1. Utiliser le mode strict
```typescript
// ✅ Bon
// tsconfig.json: "strict": true

// ❌ Éviter
let value; // any implicite
```

### 2. Préférer interfaces pour objets publics
```typescript
// ✅ Bon - interface pour API publique
export interface User {
  id: number;
  name: string;
}

// ✅ Bon - type pour unions/utilitaires
export type Status = 'pending' | 'active' | 'inactive';
export type Result<T> = Success<T> | Error;
```

### 3. Utiliser unknown au lieu de any
```typescript
// ✅ Bon
function processValue(value: unknown) {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
}

// ❌ Éviter
function processValue(value: any) {
  return value.toUpperCase(); // Pas de vérification
}
```

### 4. Type guards pour narrowing
```typescript
// ✅ Bon
function isError(value: unknown): value is Error {
  return value instanceof Error;
}

if (isError(result)) {
  console.log(result.message); // OK
}
```

### 5. Utiliser const assertions
```typescript
// ✅ Bon - type littéral préservé
const config = {
  endpoint: 'https://api.example.com',
  timeout: 5000
} as const;

// ❌ Éviter - type élargi
const config = {
  endpoint: 'https://api.example.com',
  timeout: 5000
};
```

### 6. Nommer les génériques de façon descriptive
```typescript
// ✅ Bon
interface Repository<Entity> {
  find(id: string): Promise<Entity>;
  save(entity: Entity): Promise<void>;
}

// ❌ Éviter (sauf convention établie)
interface Repository<T> {
  find(id: string): Promise<T>;
  save(t: T): Promise<void>;
}
```

### 7. Éviter les assertions de type
```typescript
// ✅ Bon - type guard
if (typeof value === 'string') {
  console.log(value.toUpperCase());
}

// ❌ Éviter - assertion
console.log((value as string).toUpperCase());
```

### 8. Utiliser readonly quand approprié
```typescript
// ✅ Bon
interface Config {
  readonly apiKey: string;
  readonly endpoints: readonly string[];
}

function processConfig(config: Config) {
  // config.apiKey = "new"; // Erreur
}
```

---

## Raccourcis & Astuces

### Raccourcis de syntaxe
```typescript
// Optional chaining
const value = obj?.property?.nested;

// Nullish coalescing
const result = value ?? defaultValue;

// Non-null assertion (à éviter)
const element = document.getElementById('app')!;

// Template literal types
type EventName = `on${Capitalize<string>}`;

// Satisfies operator (TS 4.9+)
const config = {
  host: 'localhost',
  port: 8080
} satisfies Config;
```

### Astuces de typage
```typescript
// Extraire type d'un objet
const person = { name: "Alice", age: 30 };
type Person = typeof person;

// Type d'un élément de tableau
const colors = ["red", "green", "blue"] as const;
type Color = typeof colors[number]; // "red" | "green" | "blue"

// Type de clés d'objet
type PersonKeys = keyof Person; // "name" | "age"

// Type de valeurs d'objet
type PersonValues = Person[keyof Person]; // string | number

// Type de retour de fonction
function getUser() {
  return { id: 1, name: "Alice" };
}
type User = ReturnType<typeof getUser>;
```

---

## Ressources

- **Documentation officielle**: https://www.typescriptlang.org/docs/
- **Playground**: https://www.typescriptlang.org/play
- **Type Challenges**: https://github.com/type-challenges/type-challenges
- **TypeScript Deep Dive**: https://basarat.gitbook.io/typescript/
- **Nouvelles releases**: https://devblogs.microsoft.com/typescript/
