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
let tupleLabeled: [nom: string, age: number] = ["Bob", 25];

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
enum Color {
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
type Success = { type: "success"; data: string };
type Error = { type: "error"; error: Error };
type Result = Success | Error;

function traiter(result: Result) {
  if (result.type === "success") {
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
  privileges: string[];
}

// Interfaces multiples
interface Identifiable { id: number; }
interface Nommable { nom: string; }
interface Entite extends Identifiable, Nommable {}

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
interface Fenetre {
  titre: string;
}
interface Fenetre {
  largeur: number;
}
// Fenetre a maintenant titre et largeur
```

### Type Aliases
```typescript
// Type alias basique
type Point = {
  x: number;
  y: number;
};

// Union type alias
type Result = Success | Failure;

// Intersection type alias
type UtilisateurComplet = Utilisateur & Admin & Metadata;

// Function type alias
type Operation = (a: number, b: number) => number;

// Generic type alias
type Container<T> = { value: T };

// Tuple type alias
type Point3D = [number, number, number];

// Conditional type alias
type NonNullable<T> = T extends null | undefined ? never : T;
```

---

## Classes

### Classes basiques
```typescript
// Classe simple
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hello, I'm ${this.name}`;
  }
}

// Modificateurs d'accès
class Employee {
  public name: string;        // Accessible partout
  private salary: number;     // Accessible uniquement dans la classe
  protected id: number;       // Accessible dans la classe et sous-classes
  readonly company: string;   // Ne peut pas être modifié

  constructor(name: string, salary: number, id: number) {
    this.name = name;
    this.salary = salary;
    this.id = id;
    this.company = "MyCompany";
  }

  getSalary(): number {
    return this.salary; // OK car dans la classe
  }
}

// Propriétés en paramètres (shorthand)
class User {
  constructor(
    public name: string,
    private email: string,
    readonly id: number
  ) {}
}
```

### Héritage
```typescript
// Classe de base
class Animal {
  constructor(public name: string) {}

  move(distance: number): void {
    console.log(`${this.name} moved ${distance}m`);
  }
}

// Classe dérivée
class Dog extends Animal {
  constructor(name: string, public breed: string) {
    super(name); // Appel du constructeur parent
  }

  bark(): void {
    console.log("Woof! Woof!");
  }

  // Override
  move(distance: number): void {
    console.log("Running...");
    super.move(distance);
  }
}
```

### Classes abstraites
```typescript
// Classe abstraite (ne peut pas être instanciée)
abstract class Shape {
  abstract area(): number;
  abstract perimeter(): number;

  describe(): string {
    return `Area: ${this.area()}, Perimeter: ${this.perimeter()}`;
  }
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  area(): number {
    return Math.PI * this.radius ** 2;
  }

  perimeter(): number {
    return 2 * Math.PI * this.radius;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
  }

  area(): number {
    return this.width * this.height;
  }

  perimeter(): number {
    return 2 * (this.width + this.height);
  }
}
```

### Getters & Setters
```typescript
class Temperature {
  private _celsius: number = 0;

  get celsius(): number {
    return this._celsius;
  }

  set celsius(value: number) {
    if (value < -273.15) {
      throw new Error("Temperature below absolute zero!");
    }
    this._celsius = value;
  }

  get fahrenheit(): number {
    return (this._celsius * 9/5) + 32;
  }

  set fahrenheit(value: number) {
    this._celsius = (value - 32) * 5/9;
  }
}

const temp = new Temperature();
temp.celsius = 25;
console.log(temp.fahrenheit); // 77
```

### Propriétés et méthodes statiques
```typescript
class MathUtils {
  static PI: number = 3.14159;
  static readonly E: number = 2.71828;

  static circleArea(radius: number): number {
    return this.PI * radius ** 2;
  }

  static {
    // Static initialization block (TS 4.4+)
    console.log("MathUtils initialized");
  }
}

console.log(MathUtils.PI);
console.log(MathUtils.circleArea(5));
```

### Classes génériques
```typescript
class Box<T> {
  private content: T;

  constructor(value: T) {
    this.content = value;
  }

  getValue(): T {
    return this.content;
  }

  setValue(value: T): void {
    this.content = value;
  }
}

const stringBox = new Box<string>("Hello");
const numberBox = new Box<number>(42);

// Contraintes génériques
class DataStore<T extends { id: number }> {
  private data: T[] = [];

  add(item: T): void {
    this.data.push(item);
  }

  findById(id: number): T | undefined {
    return this.data.find(item => item.id === id);
  }
}
```

### Implements (implémentation d'interface)
```typescript
interface Printable {
  print(): void;
}

interface Saveable {
  save(): void;
}

class Document implements Printable, Saveable {
  constructor(private content: string) {}

  print(): void {
    console.log(this.content);
  }

  save(): void {
    console.log("Saving document...");
  }
}
```

### Private Fields (#) - ES2022
```typescript
class BankAccount {
  #balance: number = 0; // Vraiment privé (runtime)

  deposit(amount: number): void {
    this.#balance += amount;
  }

  getBalance(): number {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
// account.#balance; // Erreur: Property '#balance' is not accessible
```

---

## Generics

### Fonctions génériques
```typescript
// Generic simple
function identity<T>(arg: T): T {
  return arg;
}

let output1 = identity<string>("hello");
let output2 = identity(42); // Type inféré

// Multiple type parameters
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}

const result = pair("age", 30); // [string, number]

// Generic avec contrainte
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person = { name: "Alice", age: 30 };
const name = getProperty(person, "name"); // OK
// const invalid = getProperty(person, "invalid"); // Erreur
```

### Interfaces génériques
```typescript
interface Repository<T> {
  items: T[];
  add(item: T): void;
  find(predicate: (item: T) => boolean): T | undefined;
}

class UserRepository implements Repository<User> {
  items: User[] = [];

  add(user: User): void {
    this.items.push(user);
  }

  find(predicate: (user: User) => boolean): User | undefined {
    return this.items.find(predicate);
  }
}

// Generic interface avec contraintes
interface Comparable<T> {
  compareTo(other: T): number;
}

class Version implements Comparable<Version> {
  constructor(private major: number, private minor: number) {}

  compareTo(other: Version): number {
    if (this.major !== other.major) {
      return this.major - other.major;
    }
    return this.minor - other.minor;
  }
}
```

### Types génériques
```typescript
// Generic type alias
type Result<T, E = Error> = 
  | { success: true; value: T }
  | { success: false; error: E };

function divide(a: number, b: number): Result<number> {
  if (b === 0) {
    return { success: false, error: new Error("Division by zero") };
  }
  return { success: true, value: a / b };
}

// Generic avec valeurs par défaut
type Container<T = string> = {
  value: T;
};

const stringContainer: Container = { value: "hello" }; // T = string par défaut
const numberContainer: Container<number> = { value: 42 };

// Multiple constraints
type Dictionary<K extends string | number, V> = {
  [key in K]: V;
};
```

### Classes génériques avancées
```typescript
// Generic avec contraintes multiples
class Cache<K extends string | number, V extends object> {
  private store = new Map<K, V>();

  set(key: K, value: V): void {
    this.store.set(key, value);
  }

  get(key: K): V | undefined {
    return this.store.get(key);
  }
}

// Generic avec méthodes génériques
class Collection<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  map<U>(fn: (item: T) => U): U[] {
    return this.items.map(fn);
  }

  filter(predicate: (item: T) => boolean): Collection<T> {
    const filtered = new Collection<T>();
    filtered.items = this.items.filter(predicate);
    return filtered;
  }
}
```

### Generic Constraints avancées
```typescript
// Contrainte avec extends
function longest<T extends { length: number }>(a: T, b: T): T {
  return a.length >= b.length ? a : b;
}

const longerString = longest("hello", "hi");
const longerArray = longest([1, 2], [1, 2, 3]);

// Contrainte avec type parameter
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Contrainte avec constructor
function create<T>(Constructor: new () => T): T {
  return new Constructor();
}

class MyClass {
  constructor() {
    console.log("Instance created");
  }
}

const instance = create(MyClass);
```

### Generic Utility Types
```typescript
// Custom utility types
type Nullable<T> = T | null;
type Optional<T> = T | undefined;
type Maybe<T> = T | null | undefined;

// Deep Partial
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Deep Readonly
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};

// Awaited (built-in in TS 4.5+)
type MyAwaited<T> = T extends Promise<infer U> ? MyAwaited<U> : T;
```

---

## Utilitaires de types

### Partial, Required, Readonly
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

// Partial - toutes propriétés optionnelles
type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; }

// Required - toutes propriétés obligatoires
type RequiredUser = Required<PartialUser>;
// { id: number; name: string; email: string; }

// Readonly - toutes propriétés en lecture seule
type ReadonlyUser = Readonly<User>;
// { readonly id: number; readonly name: string; readonly email: string; }
```

### Pick, Omit
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  stock: number;
}

// Pick - sélectionner certaines propriétés
type ProductPreview = Pick<Product, "id" | "name" | "price">;
// { id: number; name: string; price: number; }

// Omit - exclure certaines propriétés
type ProductWithoutStock = Omit<Product, "stock">;
// { id: number; name: string; price: number; description: string; }
```

### Record
```typescript
// Record<Keys, Type> - objet avec clés et type de valeur
type Role = "admin" | "user" | "guest";

type Permissions = Record<Role, string[]>;
// {
//   admin: string[];
//   user: string[];
//   guest: string[];
// }

const permissions: Permissions = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"]
};

// Autre exemple
type PageConfig = Record<string, { title: string; path: string }>;
```

### Extract, Exclude
```typescript
type T1 = "a" | "b" | "c";
type T2 = "a" | "c" | "d";

// Extract - extraire types communs
type Common = Extract<T1, T2>; // "a" | "c"

// Exclude - exclure types
type Diff = Exclude<T1, T2>; // "b"

// Cas pratique
type Primitive = string | number | boolean | null | undefined;
type NonNullablePrimitive = Exclude<Primitive, null | undefined>;
// string | number | boolean
```

### NonNullable
```typescript
type T = string | number | null | undefined;

// NonNullable - exclure null et undefined
type NonNull = NonNullable<T>; // string | number
```

### ReturnType, Parameters
```typescript
// ReturnType - type de retour d'une fonction
function getUser() {
  return { id: 1, name: "Alice", email: "alice@example.com" };
}

type User = ReturnType<typeof getUser>;
// { id: number; name: string; email: string; }

// Parameters - types des paramètres
function createUser(name: string, age: number, active: boolean) {
  return { name, age, active };
}

type CreateUserParams = Parameters<typeof createUser>;
// [string, number, boolean]

// Utilisation
function callCreateUser(...args: CreateUserParams) {
  return createUser(...args);
}
```

### ConstructorParameters, InstanceType
```typescript
class Person {
  constructor(public name: string, public age: number) {}
}

// ConstructorParameters - types des paramètres du constructeur
type PersonParams = ConstructorParameters<typeof Person>;
// [string, number]

// InstanceType - type de l'instance
type PersonInstance = InstanceType<typeof Person>;
// Person
```

### Awaited (TS 4.5+)
```typescript
// Awaited - extrait le type d'une Promise
type StringPromise = Promise<string>;
type AwaitedString = Awaited<StringPromise>; // string

type NestedPromise = Promise<Promise<number>>;
type AwaitedNumber = Awaited<NestedPromise>; // number

// Cas pratique
async function fetchUser() {
  return { id: 1, name: "Alice" };
}

type User = Awaited<ReturnType<typeof fetchUser>>;
// { id: number; name: string; }
```

### ThisType
```typescript
// ThisType - définir le type de 'this' dans un objet
type ObjectDescriptor<Data, Methods> = {
  data?: Data;
  methods?: Methods & ThisType<Data & Methods>;
};

function makeObject<Data, Methods>(
  desc: ObjectDescriptor<Data, Methods>
): Data & Methods {
  const data = desc.data || {};
  const methods = desc.methods || {};
  return { ...data, ...methods } as Data & Methods;
}

const obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx; // OK: this a le bon type
      this.y += dy;
    }
  }
});
```

### Utilitaires personnalisés
```typescript
// Mutable - retirer readonly
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};

// DeepPartial
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// PickByType - sélectionner propriétés par type
type PickByType<T, U> = {
  [K in keyof T as T[K] extends U ? K : never]: T[K];
};

interface Example {
  name: string;
  age: number;
  active: boolean;
  count: number;
}

type NumberProps = PickByType<Example, number>;
// { age: number; count: number; }

// OmitByType - exclure propriétés par type
type OmitByType<T, U> = {
  [K in keyof T as T[K] extends U ? never : K]: T[K];
};

type NonNumberProps = OmitByType<Example, number>;
// { name: string; active: boolean; }
```

---

## Nouveautés TypeScript 5.x

### TypeScript 5.0

#### const Type Parameters
```typescript
// Préserve les types littéraux
function makeArray<const T>(items: T[]): T[] {
  return items;
}

// Sans const
const arr1 = makeArray([1, 2, 3]); // number[]

// Avec const
const arr2 = makeArray([1, 2, 3] as const); // readonly [1, 2, 3]
```

#### Decorators (Stage 3)
```typescript
// Nouveaux decorators standard
function logged(value: Function, context: ClassMethodDecoratorContext) {
  return function(this: any, ...args: any[]) {
    console.log(`Calling ${String(context.name)} with:`, args);
    return value.apply(this, args);
  };
}

class Calculator {
  @logged
  add(a: number, b: number) {
    return a + b;
  }
}
```

#### extends pour configuration
```typescript
// tsconfig.json peut étendre plusieurs fichiers
{
  "extends": ["./base.json", "./strict.json"]
}
```

### TypeScript 5.1

#### Undefined-Returning Functions
```typescript
// Amélioration du return undefined implicite
function doSomething(): undefined {
  // Plus besoin de return explicite
}
```

#### Getters et Setters avec types différents
```typescript
class Thing {
  #value: number = 0;

  get value(): number {
    return this.#value;
  }

  set value(newValue: number | string) {
    if (typeof newValue === "string") {
      this.#value = parseInt(newValue);
    } else {
      this.#value = newValue;
    }
  }
}
```

### TypeScript 5.2

#### using Declarations
```typescript
// Automatic resource cleanup
interface Disposable {
  [Symbol.dispose](): void;
}

function getResource(): Disposable {
  return {
    [Symbol.dispose]() {
      console.log("Resource cleaned up");
    }
  };
}

{
  using resource = getResource();
  // Utilisez resource
} // Automatiquement dispose() appelé
```

#### Decorator Metadata
```typescript
// Accès aux métadonnées des decorators
type Context = {
  kind: string;
  name: string | symbol;
  metadata: Record<PropertyKey, unknown>;
};
```

### TypeScript 5.3

#### Import Attributes
```typescript
// Assertions d'import pour JSON
import data from "./data.json" with { type: "json" };

// Pour CSS Modules
import styles from "./styles.css" with { type: "css" };
```

#### switch(true) Narrowing
```typescript
function area(shape: Circle | Square) {
  switch (true) {
    case "radius" in shape:
      return Math.PI * shape.radius ** 2;
    case "sideLength" in shape:
      return shape.sideLength ** 2;
  }
}
```

### TypeScript 5.4

#### NoInfer Utility Type
```typescript
// Empêcher l'inférence de type
function createStreetLight<C extends string>(
  colors: C[],
  defaultColor?: NoInfer<C>
) {
  // defaultColor doit être un des colors, pas inféré indépendamment
}

createStreetLight(["red", "yellow", "green"], "red"); // OK
// createStreetLight(["red", "yellow", "green"], "blue"); // Erreur
```

#### Object.groupBy et Map.groupBy
```typescript
// Support natif TypeScript
const data = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 25 }
];

const grouped = Object.groupBy(data, (person) => person.age);
// { 25: [Alice, Charlie], 30: [Bob] }
```

### TypeScript 5.5

#### Inferred Type Predicates
```typescript
// Type guards inférés automatiquement
function isString(value: unknown) {
  return typeof value === "string";
}

// TypeScript infère: value is string
const items = [1, "hello", 2, "world"];
const strings = items.filter(isString); // string[]
```

#### Regular Expression Syntax Checking
```typescript
// Vérification de syntaxe des regex
const regex1 = /hello/; // OK
// const regex2 = /hello(/; // Erreur de syntaxe détectée
```

---

## Decorators

### Class Decorators
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

// Decorator avec paramètres
function component(name: string) {
  return function(constructor: Function) {
    console.log(`Registering component: ${name}`);
  };
}

@component("MyComponent")
class MyComponent {}
```

### Method Decorators
```typescript
// Decorator de méthode
function log(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${propertyKey} with:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };

  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}
```

### Property Decorators
```typescript
// Decorator de propriété
function readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false
  });
}

class Person {
  @readonly
  name: string = "Alice";
}

// Decorator avec validation
function min(limit: number) {
  return function(target: any, propertyKey: string) {
    let value: number;

    const getter = () => value;
    const setter = (newVal: number) => {
      if (newVal < limit) {
        throw new Error(`${propertyKey} must be at least ${limit}`);
      }
      value = newVal;
    };

    Object.defineProperty(target, propertyKey, {
      get: getter,
      set: setter
    });
  };
}

class Product {
  @min(0)
  price: number = 0;
}
```

### Parameter Decorators
```typescript
// Decorator de paramètre
function required(
  target: any,
  propertyKey: string,
  parameterIndex: number
) {
  console.log(`Parameter ${parameterIndex} of ${propertyKey} is required`);
}

class UserService {
  createUser(
    @required name: string,
    @required email: string,
    age?: number
  ) {
    // ...
  }
}
```

### Accessor Decorators
```typescript
// Decorator de getter/setter
function configurable(value: boolean) {
  return function(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.configurable = value;
  };
}

class Point {
  private _x: number = 0;

  @configurable(false)
  get x() {
    return this._x;
  }

  set x(value: number) {
    this._x = value;
  }
}
```

### Decorator Factory
```typescript
// Factory pour créer des decorators configurables
function enumerable(value: boolean) {
  return function(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.enumerable = value;
  };
}

class Example {
  @enumerable(false)
  method() {}
}
```

### Composition de Decorators
```typescript
// Plusieurs decorators sur une même cible
function first() {
  console.log("first(): factory evaluated");
  return function(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    console.log("first(): called");
  };
}

function second() {
  console.log("second(): factory evaluated");
  return function(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    console.log("second(): called");
  };
}

class ExampleClass {
  @first()
  @second()
  method() {}
}

// Ordre d'évaluation:
// first(): factory evaluated
// second(): factory evaluated
// second(): called
// first(): called
```

### Metadata avec reflect-metadata
```typescript
import "reflect-metadata";

// Définir des métadonnées
function format(formatString: string) {
  return Reflect.metadata("format", formatString);
}

class Greeter {
  @format("Hello, %s")
  greeting: string = "";
}

// Lire les métadonnées
const formatMetadata = Reflect.getMetadata(
  "format",
  new Greeter(),
  "greeting"
);
console.log(formatMetadata); // "Hello, %s"
```

---

## Modules

### Import/Export ES6
```typescript
// export nommé
export const PI = 3.14;
export function add(a: number, b: number) {
  return a + b;
}
export class Calculator {}

// export par défaut
export default class MyClass {}

// Import nommé
import { PI, add } from "./math";

// Import par défaut
import MyClass from "./myclass";

// Import tout
import * as Math from "./math";

// Import avec alias
import { add as addition } from "./math";

// Re-export
export { add } from "./math";
export * from "./utils";
```

### Type-only Imports/Exports
```typescript
// Type-only import (supprimé au runtime)
import type { User } from "./types";
import { type Admin, normalFunction } from "./types";

// Type-only export
export type { User };
export { type Admin, normalFunction };

// Import d'interfaces (toujours type-only)
import { MyInterface } from "./interfaces";
```

### Dynamic Imports
```typescript
// Import dynamique
async function loadModule() {
  const module = await import("./heavy-module");
  module.doSomething();
}

// Import conditionnel
if (condition) {
  const { feature } = await import("./feature");
  feature.init();
}

// Type d'un import dynamique
type LazyModule = typeof import("./module");
```

### Namespace vs Modules
```typescript
// ❌ Éviter les namespaces (ancien style)
namespace Utils {
  export function formatDate(date: Date): string {
    return date.toISOString();
  }
}

// ✅ Préférer les modules ES6
// utils.ts
export function formatDate(date: Date): string {
  return date.toISOString();
}

// main.ts
import { formatDate } from "./utils";
```

### Module Resolution
```typescript
// tsconfig.json
{
  "compilerOptions": {
    "moduleResolution": "bundler", // ou "node", "node16", "nodenext"
    "baseUrl": "./src",
    "paths": {
      "@models/*": ["models/*"],
      "@utils/*": ["utils/*"],
      "@components/*": ["components/*"]
    }
  }
}

// Utilisation
import { User } from "@models/user";
import { formatDate } from "@utils/date";
```

### Declaration Files (.d.ts)
```typescript
// types.d.ts - Déclarations de types
declare module "my-library" {
  export function doSomething(value: string): number;
  export const VERSION: string;
}

// Déclaration globale
declare global {
  interface Window {
    myGlobalVar: string;
  }
}

// Module ambient
declare module "*.css" {
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
