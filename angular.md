# Angular CheatSheet 

## Table des matières
- [Installation](#installation)
- [Nouveautés Angular 17+](#nouveautés-angular-17)
- [Structure de projet](#structure-de-projet)
- [Standalone Components](#standalone-components)
- [Components](#components)
- [Templates](#templates)
- [Directives](#directives)
- [Control Flow (Nouveau)](#control-flow-nouveau)
- [Signals](#signals)
- [Services & Dependency Injection](#services--dependency-injection)
- [RxJS & Observables](#rxjs--observables)
- [Routing](#routing)
- [Forms](#forms)
- [HttpClient](#httpclient)
- [Pipes](#pipes)
- [Lifecycle Hooks](#lifecycle-hooks)
- [ViewChild & ContentChild](#viewchild--contentchild)
- [Animations](#animations)
- [Testing](#testing)
- [Performance](#performance)
- [Best Practices](#best-practices)

---

## Installation

### Angular CLI

```bash
# Installer Angular CLI globalement
npm install -g @angular/cli

# Vérifier la version
ng version

# Créer un nouveau projet
ng new my-app

# Options lors de la création
ng new my-app --routing --style=scss --standalone

# Servir l'application
cd my-app
ng serve

# Ouvrir dans le navigateur
# http://localhost:4200
```

### Commandes CLI essentielles

```bash
# Générer un composant
ng generate component mon-composant
ng g c mon-composant
ng g c mon-composant --standalone  # Standalone component

# Générer un service
ng generate service mon-service
ng g s mon-service

# Générer un module
ng generate module mon-module
ng g m mon-module

# Générer une directive
ng generate directive ma-directive
ng g d ma-directive

# Générer un pipe
ng generate pipe mon-pipe
ng g p mon-pipe

# Générer un guard
ng generate guard mon-guard
ng g g mon-guard

# Générer une interface
ng generate interface mon-interface
ng g i mon-interface

# Build pour production
ng build --configuration production
ng build --prod

# Tests
ng test           # Unit tests
ng e2e            # End-to-end tests

# Linter
ng lint

# Mise à jour
ng update
ng update @angular/core @angular/cli
```

---

## Nouveautés Angular 17+

### ⭐ Control Flow Syntax (Angular 17+)

```typescript
// ✅ NOUVEAU - Syntaxe native
@if (condition) {
  <p>Affiché si vrai</p>
} @else {
  <p>Affiché si faux</p>
}

// ✅ NOUVEAU - @for
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
} @empty {
  <p>Aucun élément</p>
}

// ✅ NOUVEAU - @switch
@switch (value) {
  @case ('A') {
    <p>Case A</p>
  }
  @case ('B') {
    <p>Case B</p>
  }
  @default {
    <p>Default</p>
  }
}

// ❌ ANCIEN - Directives structurelles
<p *ngIf="condition; else elseBlock">Affiché si vrai</p>
<ng-template #elseBlock>
  <p>Affiché si faux</p>
</ng-template>

<li *ngFor="let item of items; trackBy: trackByFn">
  {{ item.name }}
</li>
```

### ⭐ Deferrable Views (Angular 17+)

```typescript
// Chargement lazy automatique
@defer (on viewport) {
  <heavy-component />
} @placeholder {
  <p>Chargement...</p>
} @loading (minimum 1s) {
  <spinner />
} @error {
  <p>Erreur de chargement</p>
}

// Triggers disponibles
@defer (on idle) { }           // Quand le navigateur est idle
@defer (on immediate) { }      // Immédiatement
@defer (on timer(2s)) { }      // Après un délai
@defer (on hover) { }          // Au survol
@defer (on interaction) { }    // Sur interaction
@defer (on viewport) { }       // Quand visible dans le viewport
```

### ⭐ Signals (Angular 16+)

```typescript
import { signal, computed, effect } from '@angular/core';

// Signal de base
const count = signal(0);

// Lecture
console.log(count());

// Écriture
count.set(1);
count.update(value => value + 1);

// Computed signal
const doubleCount = computed(() => count() * 2);

// Effect
effect(() => {
  console.log(`Count is: ${count()}`);
});
```

### ⭐ Standalone Components (Angular 14+, par défaut en 17+)

```typescript
// Plus besoin de NgModule !
@Component({
  selector: 'app-my-component',
  standalone: true,
  imports: [CommonModule, FormsModule, MyOtherComponent],
  template: `<h1>Hello</h1>`
})
export class MyComponent { }
```

---

## Structure de projet

```
my-app/
├── src/
│   ├── app/
│   │   ├── components/
│   │   │   └── header/
│   │   │       ├── header.component.ts
│   │   │       ├── header.component.html
│   │   │       ├── header.component.scss
│   │   │       └── header.component.spec.ts
│   │   ├── services/
│   │   │   └── user.service.ts
│   │   ├── models/
│   │   │   └── user.model.ts
│   │   ├── guards/
│   │   │   └── auth.guard.ts
│   │   ├── interceptors/
│   │   │   └── auth.interceptor.ts
│   │   ├── directives/
│   │   │   └── highlight.directive.ts
│   │   ├── pipes/
│   │   │   └── custom.pipe.ts
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.scss
│   │   ├── app.config.ts          # ⭐ Nouveau (standalone)
│   │   └── app.routes.ts          # ⭐ Nouveau (routing)
│   ├── assets/
│   ├── environments/
│   │   ├── environment.ts
│   │   └── environment.prod.ts
│   ├── index.html
│   ├── main.ts
│   └── styles.scss
├── angular.json
├── package.json
├── tsconfig.json
└── README.md
```

---

## Standalone Components

### Configuration de l'application (main.ts)

```typescript
// main.ts - ⭐ Approche moderne
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));
```

### Configuration (app.config.ts)

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { provideAnimations } from '@angular/platform-browser/animations';
import { routes } from './app.routes';
import { authInterceptor } from './interceptors/auth.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient(
      withInterceptors([authInterceptor])
    ),
    provideAnimations(),
    // Autres providers
  ]
};
```

### Composant Standalone

```typescript
// my-component.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { ChildComponent } from './child/child.component';

@Component({
  selector: 'app-my-component',
  standalone: true,
  imports: [
    CommonModule,      // ngIf, ngFor, pipes...
    FormsModule,       // ngModel
    ChildComponent     // Autres composants
  ],
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.scss']
})
export class MyComponent {
  title = 'Mon Composant';
}
```

---

## Components

### Component de base

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.scss'],
  // Ou inline
  // template: `<h1>{{ title }}</h1>`,
  // styles: [`h1 { color: blue; }`]
})
export class ExampleComponent implements OnInit {
  // Propriétés
  title = 'Mon titre';
  count = 0;
  items: string[] = ['A', 'B', 'C'];

  // Constructor avec DI
  constructor(
    private myService: MyService
  ) { }

  // Lifecycle hook
  ngOnInit(): void {
    console.log('Component initialized');
  }

  // Méthodes
  increment(): void {
    this.count++;
  }

  handleClick(event: MouseEvent): void {
    console.log('Clicked', event);
  }
}
```

### Input & Output

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  standalone: true,
  template: `
    <h2>{{ title }}</h2>
    <p>Count: {{ count }}</p>
    <button (click)="increment()">+</button>
  `
})
export class ChildComponent {
  // Input - Recevoir des données du parent
  @Input() title = '';
  @Input() count = 0;
  
  // Input avec alias
  @Input('userName') name = '';
  
  // Input required ⭐ Angular 16+
  @Input({ required: true }) id!: string;
  
  // Input avec transformation ⭐ Angular 16+
  @Input({ transform: booleanAttribute }) disabled = false;

  // Output - Émettre des événements vers le parent
  @Output() countChange = new EventEmitter<number>();
  @Output() delete = new EventEmitter<void>();

  increment(): void {
    this.count++;
    this.countChange.emit(this.count);
  }

  onDelete(): void {
    this.delete.emit();
  }
}

// Parent component
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [ChildComponent],
  template: `
    <app-child
      [title]="parentTitle"
      [count]="parentCount"
      (countChange)="handleCountChange($event)"
      (delete)="handleDelete()"
    />
  `
})
export class ParentComponent {
  parentTitle = 'Mon titre';
  parentCount = 0;

  handleCountChange(newCount: number): void {
    this.parentCount = newCount;
  }

  handleDelete(): void {
    console.log('Delete triggered');
  }
}
```

### Two-way binding avec ngModel

```typescript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-example',
  standalone: true,
  imports: [FormsModule],
  template: `
    <input [(ngModel)]="name" />
    <p>Hello {{ name }}</p>
  `
})
export class ExampleComponent {
  name = 'Angular';
}
```

### Two-way binding personnalisé

```typescript
// child.component.ts
@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <button (click)="increment()">{{ value }}</button>
  `
})
export class CounterComponent {
  @Input() value = 0;
  @Output() valueChange = new EventEmitter<number>();

  increment(): void {
    this.value++;
    this.valueChange.emit(this.value);
  }
}

// parent.component.ts
@Component({
  template: `<app-counter [(value)]="count" />`
})
export class ParentComponent {
  count = 0;
}
```

### Component avec Signals ⭐

```typescript
import { Component, signal, computed } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <p>Count: {{ count() }}</p>
    <p>Double: {{ doubleCount() }}</p>
    <button (click)="increment()">+</button>
  `
})
export class CounterComponent {
  // Signal
  count = signal(0);
  
  // Computed signal
  doubleCount = computed(() => this.count() * 2);

  increment(): void {
    this.count.update(value => value + 1);
  }
}
```

---

## Templates

### Interpolation

```html
<!-- Texte -->
<p>{{ title }}</p>
<p>{{ 1 + 1 }}</p>
<p>{{ getTitle() }}</p>

<!-- HTML (attention XSS!) -->
<div [innerHTML]="htmlContent"></div>
```

### Property Binding

```html
<!-- Attribut HTML -->
<img [src]="imageUrl" [alt]="imageAlt">
<button [disabled]="isDisabled">Click</button>

<!-- Propriété DOM -->
<input [value]="name">

<!-- Classe CSS -->
<div [class.active]="isActive"></div>
<div [class]="'my-class another-class'"></div>
<div [class]="classObject"></div>

<!-- Style -->
<div [style.color]="color"></div>
<div [style.font-size.px]="fontSize"></div>
<div [style]="styleObject"></div>

<!-- Attributs data -->
<div [attr.data-id]="userId"></div>
<div [attr.aria-label]="label"></div>
```

### Event Binding

```html
<!-- Click -->
<button (click)="handleClick()">Click</button>
<button (click)="handleClick($event)">Click</button>

<!-- Input -->
<input (input)="onInput($event)">
<input (change)="onChange($event)">
<input (keyup)="onKeyUp($event)">
<input (keyup.enter)="onEnter()">

<!-- Focus -->
<input (focus)="onFocus()" (blur)="onBlur()">

<!-- Mouse -->
<div (mouseenter)="onMouseEnter()" (mouseleave)="onMouseLeave()"></div>

<!-- Custom events -->
<app-child (customEvent)="handleCustomEvent($event)"></app-child>
```

### Two-way Binding

```html
<!-- ngModel -->
<input [(ngModel)]="name">

<!-- Équivalent à -->
<input [ngModel]="name" (ngModelChange)="name = $event">

<!-- Custom two-way binding -->
<app-counter [(value)]="count"></app-counter>
```

### Template Reference Variables

```html
<!-- Référence à un élément -->
<input #nameInput type="text">
<button (click)="nameInput.focus()">Focus</button>
<p>{{ nameInput.value }}</p>

<!-- Référence à un composant -->
<app-child #childComponent></app-child>
<button (click)="childComponent.someMethod()">Call method</button>

<!-- Référence dans ngFor -->
<div *ngFor="let item of items; let i = index">
  <input #itemInput>
</div>
```

---

## Directives

### Directives structurelles (Ancien)

```html
<!-- *ngIf -->
<p *ngIf="condition">Affiché si vrai</p>
<p *ngIf="condition; else elseBlock">Si vrai</p>
<ng-template #elseBlock>
  <p>Si faux</p>
</ng-template>

<!-- *ngFor -->
<li *ngFor="let item of items">{{ item }}</li>

<!-- Variables locales ngFor -->
<li *ngFor="let item of items; let i = index; let first = first; let last = last; let even = even; let odd = odd">
  {{ i }} - {{ item }} - First: {{ first }} - Last: {{ last }}
</li>

<!-- trackBy pour performance -->
<li *ngFor="let item of items; trackBy: trackByFn">{{ item.name }}</li>

trackByFn(index: number, item: any): any {
  return item.id;
}

<!-- *ngSwitch -->
<div [ngSwitch]="value">
  <p *ngSwitchCase="'A'">Case A</p>
  <p *ngSwitchCase="'B'">Case B</p>
  <p *ngSwitchDefault>Default</p>
</div>
```

### Directives d'attribut

```html
<!-- ngClass -->
<div [ngClass]="'class1 class2'"></div>
<div [ngClass]="['class1', 'class2']"></div>
<div [ngClass]="{ 'active': isActive, 'disabled': isDisabled }"></div>

<!-- ngStyle -->
<div [ngStyle]="{ 'color': color, 'font-size': fontSize + 'px' }"></div>

<!-- ngModel -->
<input [(ngModel)]="name">
```

### Directive personnalisée

```typescript
// highlight.directive.ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @Input() appHighlight = 'yellow';
  @Input() defaultColor = 'transparent';

  constructor(private el: ElementRef) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(this.defaultColor);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

// Utilisation
<p appHighlight="yellow">Hover me!</p>
<p [appHighlight]="color" defaultColor="lightgray">Hover me!</p>
```

### Directive structurelle personnalisée

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]',
  standalone: true
})
export class UnlessDirective {
  @Input() set appUnless(condition: boolean) {
    if (!condition) {
      this.viewContainer.createEmbeddedView(this.templateRef);
    } else {
      this.viewContainer.clear();
    }
  }

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) { }
}

// Utilisation
<p *appUnless="condition">Affiché si faux</p>
```

---

## Control Flow (Nouveau)

### @if / @else ⭐ Angular 17+

```typescript
// Condition simple
@if (user) {
  <p>Hello {{ user.name }}</p>
}

// Avec else
@if (isLoggedIn) {
  <p>Welcome back!</p>
} @else {
  <p>Please login</p>
}

// Else if
@if (status === 'loading') {
  <spinner />
} @else if (status === 'error') {
  <error-message />
} @else {
  <content />
}

// Conditions complexes
@if (user && user.isAdmin) {
  <admin-panel />
}
```

### @for ⭐ Angular 17+

```typescript
// Boucle simple avec track
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
}

// Avec index
@for (item of items; track item.id; let i = $index) {
  <li>{{ i + 1 }}. {{ item.name }}</li>
}

// Variables disponibles
@for (item of items; track item.id; 
  let i = $index;      // index
  let f = $first;      // premier élément
  let l = $last;       // dernier élément
  let e = $even;       // index pair
  let o = $odd;        // index impair
  let c = $count       // nombre total
) {
  <li [class.first]="f" [class.last]="l">
    {{ i + 1 }}/{{ c }}: {{ item.name }}
  </li>
}

// @empty - affiché si la liste est vide
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
} @empty {
  <p>Aucun élément trouvé</p>
}

// Track par index (quand pas d'identifiant unique)
@for (item of items; track $index) {
  <li>{{ item }}</li>
}
```

### @switch ⭐ Angular 17+

```typescript
@switch (status) {
  @case ('pending') {
    <pending-badge />
  }
  @case ('approved') {
    <approved-badge />
  }
  @case ('rejected') {
    <rejected-badge />
  }
  @default {
    <unknown-badge />
  }
}

// Avec expressions
@switch (user.role) {
  @case ('admin') {
    <admin-dashboard />
  }
  @case ('user') {
    <user-dashboard />
  }
  @default {
    <guest-view />
  }
}
```

### @defer (Lazy Loading) ⭐ Angular 17+

```typescript
// Basic defer
@defer {
  <heavy-component />
}

// Avec placeholder (avant le chargement)
@defer {
  <heavy-component />
} @placeholder {
  <div>Contenu temporaire...</div>
}

// Avec loading (pendant le chargement)
@defer {
  <heavy-component />
} @loading {
  <spinner />
} @placeholder {
  <div>Cliquez pour charger</div>
}

// Avec error
@defer {
  <heavy-component />
} @error {
  <p>Erreur lors du chargement</p>
}

// Complet
@defer {
  <heavy-component />
} @loading (minimum 1s; after 100ms) {
  <spinner />
} @placeholder (minimum 500ms) {
  <skeleton-loader />
} @error {
  <error-message />
}

// Triggers
@defer (on viewport) { }           // Visible dans le viewport
@defer (on idle) { }               // Navigateur idle
@defer (on immediate) { }          // Immédiatement
@defer (on timer(2s)) { }          // Après 2 secondes
@defer (on hover) { }              // Au survol
@defer (on interaction) { }        // Sur interaction (click, focus...)
@defer (on hover(triggerRef)) {    // Au survol d'un élément spécifique
  <component />
}

// Triggers multiples
@defer (on hover; on timer(3s)) {
  <component />
}

// Prefetch
@defer (on viewport; prefetch on idle) {
  <heavy-component />
}
```

---

## Signals

### Création et utilisation ⭐ Angular 16+

```typescript
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-signals-demo',
  standalone: true,
  template: `
    <h2>Count: {{ count() }}</h2>
    <h2>Double: {{ doubleCount() }}</h2>
    <button (click)="increment()">+</button>
    <button (click)="decrement()">-</button>
    <button (click)="reset()">Reset</button>
  `
})
export class SignalsDemoComponent {
  // Signal primitif
  count = signal(0);
  
  // Signal objet
  user = signal({ name: 'John', age: 30 });

  // Computed signal (dérivé)
  doubleCount = computed(() => this.count() * 2);
  
  // Computed avec plusieurs dépendances
  summary = computed(() => 
    `${this.user().name} has count ${this.count()}`
  );

  // Effect (side effect)
  constructor() {
    effect(() => {
      console.log(`Count changed to: ${this.count()}`);
    });
  }

  // Méthodes
  increment() {
    // set - remplace la valeur
    this.count.set(this.count() + 1);
  }

  decrement() {
    // update - modifie basé sur la valeur actuelle
    this.count.update(value => value - 1);
  }

  reset() {
    this.count.set(0);
  }

  updateUser() {
    // Pour les objets, utiliser update
    this.user.update(user => ({
      ...user,
      age: user.age + 1
    }));
  }
}
```

### Signals avancés

```typescript
import { signal, computed, effect, untracked } from '@angular/core';

export class AdvancedSignals {
  // Signal array
  items = signal<string[]>([]);

  addItem(item: string) {
    this.items.update(items => [...items, item]);
  }

  removeItem(index: number) {
    this.items.update(items => items.filter((_, i) => i !== index));
  }

  // Computed avec logique complexe
  filteredItems = computed(() => {
    const items = this.items();
    return items.filter(item => item.length > 3);
  });

  // Effect avec cleanup
  constructor() {
    effect((onCleanup) => {
      const subscription = someObservable.subscribe();
      
      onCleanup(() => {
        subscription.unsubscribe();
      });
    });
  }

  // untracked - lire sans créer de dépendance
  logCurrentValue() {
    effect(() => {
      console.log('Count:', this.count());
      
      // untracked ne crée pas de dépendance
      const otherValue = untracked(() => this.otherSignal());
      console.log('Other:', otherValue);
    });
  }
}
```

### Signal inputs ⭐ Angular 17.1+

```typescript
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-child',
  standalone: true,
  template: `
    <h2>{{ title() }}</h2>
    <p>Required: {{ requiredValue() }}</p>
    <button (click)="handleClick()">Click</button>
  `
})
export class ChildComponent {
  // Signal input
  title = input<string>('Default Title');
  
  // Required signal input
  requiredValue = input.required<number>();
  
  // Signal input avec transformation
  disabled = input(false, {
    transform: (value: boolean | string) => {
      return typeof value === 'string' ? value === '' : value;
    }
  });

  // Signal output
  clicked = output<void>();

  handleClick() {
    this.clicked.emit();
  }
}

// Usage dans le parent
@Component({
  template: `
    <app-child
      [title]="'Mon Titre'"
      [requiredValue]="42"
      (clicked)="onClicked()"
    />
  `
})
export class ParentComponent {
  onClicked() {
    console.log('Clicked!');
  }
}
```

### Signals avec RxJS

```typescript
import { Component, signal } from '@angular/core';
import { toObservable, toSignal } from '@angular/core/rxjs-interop';
import { interval } from 'rxjs';

@Component({
  selector: 'app-signals-rxjs',
  standalone: true,
  template: `
    <p>Count: {{ count() }}</p>
    <p>Timer: {{ timer() }}</p>
  `
})
export class SignalsRxjsComponent {
  count = signal(0);

  // Convertir signal en Observable
  count$ = toObservable(this.count);

  // Convertir Observable en Signal
  timer = toSignal(interval(1000), { initialValue: 0 });

  constructor() {
    // S'abonner à l'Observable dérivé du signal
    this.count$.subscribe(value => {
      console.log('Count changed:', value);
    });
  }
}
```

---

## Services & Dependency Injection

### Service de base

```typescript
// user.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Service singleton disponible partout
})
export class UserService {
  private users: User[] = [];

  constructor() { }

  getUsers(): User[] {
    return this.users;
  }

  addUser(user: User): void {
    this.users.push(user);
  }

  getUserById(id: number): User | undefined {
    return this.users.find(u => u.id === id);
  }
}
```

### Injection dans un composant

```typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from './services/user.service';

@Component({
  selector: 'app-users',
  standalone: true,
  template: `
    @for (user of users; track user.id) {
      <p>{{ user.name }}</p>
    }
  `
})
export class UsersComponent implements OnInit {
  users: User[] = [];

  // Injection par constructor
  constructor(private userService: UserService) { }

  ngOnInit(): void {
    this.users = this.userService.getUsers();
  }
}
```

### Inject function ⭐ Angular 14+

```typescript
import { Component, inject, OnInit } from '@angular/core';
import { UserService } from './services/user.service';

@Component({
  selector: 'app-users',
  standalone: true,
  template: `...`
})
export class UsersComponent implements OnInit {
  // Nouvelle syntaxe avec inject()
  private userService = inject(UserService);
  users: User[] = [];

  ngOnInit(): void {
    this.users = this.userService.getUsers();
  }
}
```

### Providers

```typescript
// Différents scopes de providers

// 1. Root - Service singleton global
@Injectable({
  providedIn: 'root'
})
export class GlobalService { }

// 2. Component - Une instance par composant
@Component({
  selector: 'app-example',
  providers: [LocalService]  // Instance locale
})
export class ExampleComponent { }

// 3. Module (approche classique)
@NgModule({
  providers: [ModuleService]
})
export class FeatureModule { }

// 4. Standalone - app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [
    MyService,
    { provide: API_URL, useValue: 'https://api.example.com' }
  ]
};
```

### Injection tokens

```typescript
import { InjectionToken } from '@angular/core';

// Définir un token
export const API_URL = new InjectionToken<string>('api.url');

// Fournir une valeur
export const appConfig: ApplicationConfig = {
  providers: [
    { provide: API_URL, useValue: 'https://api.example.com' }
  ]
};

// Injecter
@Component({
  selector: 'app-example',
  standalone: true
})
export class ExampleComponent {
  private apiUrl = inject(API_URL);

  constructor() {
    console.log(this.apiUrl); // 'https://api.example.com'
  }
}
```

### Factory providers

```typescript
// Service factory
export function userServiceFactory(http: HttpClient, config: AppConfig) {
  return new UserService(http, config.apiUrl);
}

// Provider
{
  provide: UserService,
  useFactory: userServiceFactory,
  deps: [HttpClient, AppConfig]
}

// Ou avec inject
export function createUserService() {
  const http = inject(HttpClient);
  const config = inject(AppConfig);
  return new UserService(http, config.apiUrl);
}

{
  provide: UserService,
  useFactory: createUserService
}
```

### Service avec Signals ⭐

```typescript
import { Injectable, signal, computed } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class CounterService {
  // Private signal
  private _count = signal(0);
  
  // Public readonly signal
  count = this._count.asReadonly();
  
  // Computed
  doubleCount = computed(() => this._count() * 2);

  increment() {
    this._count.update(c => c + 1);
  }

  decrement() {
    this._count.update(c => c - 1);
  }

  reset() {
    this._count.set(0);
  }
}
```

---

## RxJS & Observables

### Basics

```typescript
import { Observable, of, from, interval } from 'rxjs';
import { map, filter, take, debounceTime, switchMap } from 'rxjs/operators';

// Créer un Observable
const observable$ = new Observable(subscriber => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  subscriber.complete();
});

// of - créer depuis des valeurs
const numbers$ = of(1, 2, 3, 4, 5);

// from - créer depuis un array, promise, etc.
const fromArray$ = from([1, 2, 3]);
const fromPromise$ = from(fetch('/api/data'));

// interval - émettre à intervalle régulier
const timer$ = interval(1000);

// S'abonner
observable$.subscribe({
  next: value => console.log(value),
  error: err => console.error(err),
  complete: () => console.log('Complete')
});
```

### Opérateurs communs

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subject, takeUntil } from 'rxjs';

@Component({
  selector: 'app-rxjs-example',
  standalone: true,
  template: `...`
})
export class RxjsExampleComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit() {
    // map - transformer les valeurs
    of(1, 2, 3)
      .pipe(
        map(x => x * 2),
        takeUntil(this.destroy$)
      )
      .subscribe(value => console.log(value)); // 2, 4, 6

    // filter - filtrer les valeurs
    of(1, 2, 3, 4, 5)
      .pipe(
        filter(x => x % 2 === 0),
        takeUntil(this.destroy$)
      )
      .subscribe(value => console.log(value)); // 2, 4

    // debounceTime - attendre avant d'émettre
    input$
      .pipe(
        debounceTime(300),
        takeUntil(this.destroy$)
      )
      .subscribe(value => console.log(value));

    // switchMap - switch vers un nouvel Observable
    searchTerm$
      .pipe(
        debounceTime(300),
        switchMap(term => this.http.get(`/api/search?q=${term}`)),
        takeUntil(this.destroy$)
      )
      .subscribe(results => console.log(results));

    // take - prendre n valeurs puis complete
    interval(1000)
      .pipe(take(5))
      .subscribe(value => console.log(value)); // 0, 1, 2, 3, 4

    // catchError - gérer les erreurs
    this.http.get('/api/data')
      .pipe(
        catchError(error => {
          console.error(error);
          return of([]); // Valeur par défaut
        }),
        takeUntil(this.destroy$)
      )
      .subscribe(data => console.log(data));
  }

  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

### Subjects

```typescript
import { Subject, BehaviorSubject, ReplaySubject, AsyncSubject } from 'rxjs';

// Subject - simple
const subject = new Subject<number>();
subject.next(1);
subject.subscribe(value => console.log('Sub1:', value));
subject.next(2); // Sub1: 2

// BehaviorSubject - garde la dernière valeur
const behaviorSubject = new BehaviorSubject<number>(0);
behaviorSubject.subscribe(value => console.log('BS1:', value)); // BS1: 0
behaviorSubject.next(1); // BS1: 1
behaviorSubject.subscribe(value => console.log('BS2:', value)); // BS2: 1

// ReplaySubject - rejoue les n dernières valeurs
const replaySubject = new ReplaySubject<number>(2);
replaySubject.next(1);
replaySubject.next(2);
replaySubject.next(3);
replaySubject.subscribe(value => console.log('RS:', value)); // RS: 2, RS: 3

// AsyncSubject - émet la dernière valeur seulement au complete
const asyncSubject = new AsyncSubject<number>();
asyncSubject.next(1);
asyncSubject.next(2);
asyncSubject.subscribe(value => console.log('AS:', value));
asyncSubject.next(3);
asyncSubject.complete(); // AS: 3
```

### Service avec Observable

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

interface User {
  id: number;
  name: string;
}

@Injectable({
  providedIn: 'root'
})
export class UserStateService {
  private userSubject = new BehaviorSubject<User | null>(null);
  
  // Observable public
  user$: Observable<User | null> = this.userSubject.asObservable();

  setUser(user: User) {
    this.userSubject.next(user);
  }

  clearUser() {
    this.userSubject.next(null);
  }

  getUser(): User | null {
    return this.userSubject.value;
  }
}
```

### Async Pipe

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-async-example',
  standalone: true,
  imports: [CommonModule],
  template: `
    <!-- Async pipe s'abonne et se désabonne automatiquement -->
    @if (user$ | async; as user) {
      <p>{{ user.name }}</p>
    }

    @for (item of items$ | async; track item.id) {
      <li>{{ item.name }}</li>
    }
  `
})
export class AsyncExampleComponent {
  user$: Observable<User>;
  items$: Observable<Item[]>;

  constructor(private userService: UserService) {
    this.user$ = this.userService.getUser();
    this.items$ = this.userService.getItems();
  }
}
```

---

## Routing

### Configuration des routes (app.routes.ts) ⭐

```typescript
// app.routes.ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { authGuard } from './guards/auth.guard';

export const routes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'about',
    component: AboutComponent
  },
  {
    path: 'user/:id',
    component: UserComponent
  },
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [authGuard]
  },
  {
    path: 'products',
    loadComponent: () => import('./products/products.component')
      .then(m => m.ProductsComponent)
  },
  {
    path: 'dashboard',
    loadChildren: () => import('./dashboard/dashboard.routes')
      .then(m => m.DASHBOARD_ROUTES)
  },
  {
    path: '**',
    redirectTo: ''
  }
];
```

### Configuration dans app.config.ts

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(
      routes,
      withComponentInputBinding()  // ⭐ Route params comme inputs
    )
  ]
};
```

### RouterLink et RouterOutlet

```html
<!-- app.component.html -->
<nav>
  <a routerLink="/">Home</a>
  <a routerLink="/about">About</a>
  <a [routerLink]="['/user', userId]">User</a>
  
  <!-- RouterLinkActive -->
  <a routerLink="/about" routerLinkActive="active">About</a>
  <a routerLink="/" routerLinkActive="active" [routerLinkActiveOptions]="{ exact: true }">Home</a>
</nav>

<!-- Outlet pour afficher les composants -->
<router-outlet />
```

### Navigation programmatique

```typescript
import { Component, inject } from '@angular/core';
import { Router, ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `
    <button (click)="goToAbout()">Go to About</button>
    <button (click)="goToUser()">Go to User</button>
    <button (click)="goBack()">Back</button>
  `
})
export class ExampleComponent {
  private router = inject(Router);
  private route = inject(ActivatedRoute);

  goToAbout() {
    this.router.navigate(['/about']);
  }

  goToUser() {
    // Avec paramètres
    this.router.navigate(['/user', 123]);
    
    // Avec query params
    this.router.navigate(['/search'], {
      queryParams: { q: 'angular' }
    });
    
    // Navigation relative
    this.router.navigate(['../sibling'], { relativeTo: this.route });
  }

  goBack() {
    // Navigation dans l'historique
    window.history.back();
  }
}
```

### Route Params et Query Params

```typescript
import { Component, OnInit, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user',
  standalone: true,
  template: `
    <h2>User {{ userId }}</h2>
    <p>Query: {{ query }}</p>
  `
})
export class UserComponent implements OnInit {
  private route = inject(ActivatedRoute);
  userId: string | null = null;
  query: string | null = null;

  ngOnInit() {
    // Route params
    this.userId = this.route.snapshot.paramMap.get('id');
    
    // Query params
    this.query = this.route.snapshot.queryParamMap.get('q');
    
    // Observable (pour réagir aux changements)
    this.route.paramMap.subscribe(params => {
      this.userId = params.get('id');
    });

    this.route.queryParamMap.subscribe(queryParams => {
      this.query = queryParams.get('q');
    });
  }
}
```

### Route Inputs ⭐ Angular 16+

```typescript
// Avec withComponentInputBinding() dans app.config.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user',
  standalone: true,
  template: `<h2>User {{ id }}</h2>`
})
export class UserComponent {
  // Route params automatiquement bindés aux inputs
  @Input() id?: string;
  
  // Query params aussi
  @Input() q?: string;
}

// Route: /user/123?q=search
// id = '123', q = 'search'
```

### Guards (Fonction) ⭐ Angular 15+

```typescript
// auth.guard.ts
import { inject } from '@angular/core';
import { Router, type CanActivateFn } from '@angular/router';
import { AuthService } from './auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true;
  }

  // Rediriger vers login
  return router.parseUrl('/login');
};

// Utilisation
{
  path: 'admin',
  component: AdminComponent,
  canActivate: [authGuard]
}
```

### Autres types de Guards

```typescript
// CanDeactivate - Confirmer avant de quitter
export const canDeactivateGuard: CanDeactivateFn<ComponentType> = (component) => {
  if (component.hasUnsavedChanges()) {
    return confirm('Vous avez des modifications non sauvegardées. Quitter ?');
  }
  return true;
};

// CanMatch - Charger ou non une route lazy
export const canMatchGuard: CanMatchFn = (route, segments) => {
  const authService = inject(AuthService);
  return authService.hasPermission('admin');
};

// Resolve - Charger des données avant d'afficher la route
export const userResolver: ResolveFn<User> = (route, state) => {
  const userService = inject(UserService);
  const userId = route.paramMap.get('id')!;
  return userService.getUser(userId);
};

// Utilisation
{
  path: 'user/:id',
  component: UserComponent,
  resolve: { user: userResolver }
}
```

### Routes imbriquées

```typescript
// app.routes.ts
export const routes: Routes = [
  {
    path: 'dashboard',
    component: DashboardComponent,
    children: [
      {
        path: '',
        component: DashboardHomeComponent
      },
      {
        path: 'profile',
        component: ProfileComponent
      },
      {
        path: 'settings',
        component: SettingsComponent
      }
    ]
  }
];

// dashboard.component.html
<h1>Dashboard</h1>
<nav>
  <a routerLink="profile">Profile</a>
  <a routerLink="settings">Settings</a>
</nav>
<router-outlet /> <!-- Routes enfants affichées ici -->
```

---

## Forms

### Template-driven Forms

```typescript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-template-form',
  standalone: true,
  imports: [FormsModule, CommonModule],
  template: `
    <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
      <!-- Input simple -->
      <input
        type="text"
        name="username"
        [(ngModel)]="user.username"
        required
        minlength="3"
        #username="ngModel"
      >
      
      <!-- Messages d'erreur -->
      @if (username.invalid && (username.dirty || username.touched)) {
        <div class="error">
          @if (username.errors?.['required']) {
            <p>Le nom est requis</p>
          }
          @if (username.errors?.['minlength']) {
            <p>Minimum 3 caractères</p>
          }
        </div>
      }

      <!-- Email -->
      <input
        type="email"
        name="email"
        [(ngModel)]="user.email"
        required
        email
        #email="ngModel"
      >

      <!-- Select -->
      <select name="country" [(ngModel)]="user.country">
        <option value="">Choisir un pays</option>
        <option value="fr">France</option>
        <option value="us">USA</option>
      </select>

      <!-- Checkbox -->
      <input
        type="checkbox"
        name="subscribe"
        [(ngModel)]="user.subscribe"
      >

      <!-- Radio -->
      <label>
        <input type="radio" name="gender" value="male" [(ngModel)]="user.gender">
        Homme
      </label>
      <label>
        <input type="radio" name="gender" value="female" [(ngModel)]="user.gender">
        Femme
      </label>

      <button type="submit" [disabled]="userForm.invalid">
        Submit
      </button>
    </form>

    <!-- Debug -->
    <pre>{{ user | json }}</pre>
    <p>Form valid: {{ userForm.valid }}</p>
  `
})
export class TemplateFormComponent {
  user = {
    username: '',
    email: '',
    country: '',
    subscribe: false,
    gender: ''
  };

  onSubmit(form: any) {
    if (form.valid) {
      console.log('Form submitted', this.user);
    }
  }
}
```

### Reactive Forms

```typescript
import { Component, OnInit } from '@angular/core';
import { ReactiveFormsModule, FormBuilder, FormGroup, FormControl, Validators } from '@angular/forms';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-reactive-form',
  standalone: true,
  imports: [ReactiveFormsModule, CommonModule],
  template: `
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
      <!-- Input -->
      <input formControlName="username">
      
      @if (username.invalid && (username.dirty || username.touched)) {
        <div class="error">
          @if (username.errors?.['required']) {
            <p>Le nom est requis</p>
          }
          @if (username.errors?.['minlength']) {
            <p>Minimum {{ username.errors?.['minlength'].requiredLength }} caractères</p>
          }
        </div>
      }

      <input type="email" formControlName="email">
      <input type="password" formControlName="password">

      <!-- FormGroup imbriqué -->
      <div formGroupName="address">
        <input formControlName="street" placeholder="Rue">
        <input formControlName="city" placeholder="Ville">
        <input formControlName="zipCode" placeholder="Code postal">
      </div>

      <!-- FormArray -->
      <div formArrayName="phones">
        @for (phone of phones.controls; track $index; let i = $index) {
          <div [formGroupName]="i">
            <input formControlName="number">
            <button type="button" (click)="removePhone(i)">Supprimer</button>
          </div>
        }
      </div>
      <button type="button" (click)="addPhone()">Ajouter téléphone</button>

      <button type="submit" [disabled]="userForm.invalid">Submit</button>
    </form>

    <pre>{{ userForm.value | json }}</pre>
  `
})
export class ReactiveFormComponent implements OnInit {
  userForm!: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    // Avec FormBuilder (recommandé)
    this.userForm = this.fb.group({
      username: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(8)]],
      address: this.fb.group({
        street: [''],
        city: [''],
        zipCode: ['']
      }),
      phones: this.fb.array([])
    });

    // Ou sans FormBuilder
    this.userForm = new FormGroup({
      username: new FormControl('', [Validators.required, Validators.minLength(3)]),
      email: new FormControl('', [Validators.required, Validators.email])
    });
  }

  // Getters pour faciliter l'accès
  get username() {
    return this.userForm.get('username')!;
  }

  get email() {
    return this.userForm.get('email')!;
  }

  get phones() {
    return this.userForm.get('phones') as FormArray;
  }

  addPhone() {
    this.phones.push(
      this.fb.group({
        number: ['', Validators.required]
      })
    );
  }

  removePhone(index: number) {
    this.phones.removeAt(index);
  }

  onSubmit() {
    if (this.userForm.valid) {
      console.log(this.userForm.value);
    }
  }

  // Méthodes utiles
  resetForm() {
    this.userForm.reset();
  }

  patchValue() {
    this.userForm.patchValue({
      username: 'John',
      email: 'john@example.com'
    });
  }

  setValue() {
    // Doit spécifier TOUTES les valeurs
    this.userForm.setValue({
      username: 'John',
      email: 'john@example.com',
      password: '',
      address: { street: '', city: '', zipCode: '' },
      phones: []
    });
  }
}
```

### Validators personnalisés

```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';

// Validator simple
export function forbiddenNameValidator(nameRe: RegExp): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const forbidden = nameRe.test(control.value);
    return forbidden ? { forbiddenName: { value: control.value } } : null;
  };
}

// Validator asynchrone
export function uniqueEmailValidator(userService: UserService): AsyncValidatorFn {
  return (control: AbstractControl): Observable<ValidationErrors | null> => {
    return userService.checkEmail(control.value).pipe(
      map(isTaken => (isTaken ? { emailTaken: true } : null)),
      catchError(() => of(null))
    );
  };
}

// Validator de groupe (password confirmation)
export const passwordMatchValidator: ValidatorFn = (
  control: AbstractControl
): ValidationErrors | null => {
  const password = control.get('password');
  const confirmPassword = control.get('confirmPassword');

  return password && confirmPassword && password.value !== confirmPassword.value
    ? { passwordMismatch: true }
    : null;
};

// Utilisation
this.userForm = this.fb.group({
  name: ['', [Validators.required, forbiddenNameValidator(/admin/i)]],
  email: ['', [Validators.required, Validators.email], [uniqueEmailValidator(this.userService)]],
  password: [''],
  confirmPassword: ['']
}, {
  validators: passwordMatchValidator
});
```

### Form Status et Events

```typescript
export class FormComponent implements OnInit {
  userForm!: FormGroup;

  ngOnInit() {
    this.userForm = this.fb.group({
      username: ['']
    });

    // Status du form
    console.log(this.userForm.valid);
    console.log(this.userForm.invalid);
    console.log(this.userForm.pristine);  // Pas modifié
    console.log(this.userForm.dirty);     // Modifié
    console.log(this.userForm.touched);   // Touché (focus)
    console.log(this.userForm.untouched);

    // Écouter les changements
    this.userForm.valueChanges.subscribe(value => {
      console.log('Form value changed:', value);
    });

    this.userForm.statusChanges.subscribe(status => {
      console.log('Form status changed:', status);
    });

    // Changements d'un contrôle spécifique
    this.userForm.get('username')?.valueChanges.subscribe(value => {
      console.log('Username changed:', value);
    });

    // Avec debounce
    this.userForm.get('username')?.valueChanges.pipe(
      debounceTime(300),
      distinctUntilChanged()
    ).subscribe(value => {
      console.log('Search:', value);
    });
  }
}
```

---

## HttpClient

### Configuration

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient, withInterceptors } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withInterceptors([authInterceptor])
    )
  ]
};
```

### Service HTTP de base

```typescript
import { Injectable, inject } from '@angular/core';
import { HttpClient, HttpHeaders, HttpParams } from '@angular/common/http';
import { Observable, catchError, throwError } from 'rxjs';

interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private http = inject(HttpClient);
  private apiUrl = 'https://api.example.com/users';

  // GET
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }

  getUser(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`);
  }

  // GET avec params
  searchUsers(query: string): Observable<User[]> {
    const params = new HttpParams()
      .set('q', query)
      .set('limit', '10');

    return this.http.get<User[]>(`${this.apiUrl}/search`, { params });
  }

  // POST
  createUser(user: Partial<User>): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }

  // PUT
  updateUser(id: number, user: Partial<User>): Observable<User> {
    return this.http.put<User>(`${this.apiUrl}/${id}`, user);
  }

  // PATCH
  patchUser(id: number, changes: Partial<User>): Observable<User> {
    return this.http.patch<User>(`${this.apiUrl}/${id}`, changes);
  }

  // DELETE
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }

  // Avec headers personnalisés
  getUserWithHeaders(id: number): Observable<User> {
    const headers = new HttpHeaders({
      'Content-Type': 'application/json',
      'Authorization': 'Bearer token123'
    });

    return this.http.get<User>(`${this.apiUrl}/${id}`, { headers });
  }

  // Avec gestion d'erreur
  getUserSafe(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`).pipe(
      catchError(error => {
        console.error('Error:', error);
        return throwError(() => new Error('Failed to fetch user'));
      })
    );
  }
}
```

### Utilisation dans un composant

```typescript
@Component({
  selector: 'app-users',
  standalone: true,
  imports: [CommonModule],
  template: `
    @if (loading) {
      <p>Loading...</p>
    }
    @if (error) {
      <p class="error">{{ error }}</p>
    }
    @for (user of users; track user.id) {
      <div>{{ user.name }}</div>
    }
  `
})
export class UsersComponent implements OnInit {
  private userService = inject(UserService);
  users: User[] = [];
  loading = false;
  error: string | null = null;

  ngOnInit() {
    this.loadUsers();
  }

  loadUsers() {
    this.loading = true;
    this.error = null;

    this.userService.getUsers().subscribe({
      next: (users) => {
        this.users = users;
        this.loading = false;
      },
      error: (error) => {
        this.error = 'Failed to load users';
        this.loading = false;
        console.error(error);
      }
    });
  }

  // Ou avec async pipe
  users$ = this.userService.getUsers();
  
  // Template: @for (user of users$ | async; track user.id) { }
}
```

### Intercepteur (Fonction) ⭐ Angular 15+

```typescript
// auth.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { AuthService } from './auth.service';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authService = inject(AuthService);
  const token = authService.getToken();

  // Cloner la requête et ajouter le header
  const authReq = req.clone({
    setHeaders: {
      Authorization: `Bearer ${token}`
    }
  });

  return next(authReq);
};

// logging.interceptor.ts
export const loggingInterceptor: HttpInterceptorFn = (req, next) => {
  console.log('Request:', req.url);
  
  return next(req).pipe(
    tap({
      next: (event) => {
        if (event.type === HttpEventType.Response) {
          console.log('Response:', event.status);
        }
      },
      error: (error) => {
        console.error('Error:', error);
      }
    })
  );
};

// Configuration
export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withInterceptors([authInterceptor, loggingInterceptor])
    )
  ]
};
```

---

## Pipes

### Pipes natifs

```html
<!-- Texte -->
<p>{{ message | uppercase }}</p>
<p>{{ message | lowercase }}</p>
<p>{{ message | titlecase }}</p>

<!-- Nombre -->
<p>{{ price | number }}</p>              <!-- 1,234.57 -->
<p>{{ price | number:'3.2-5' }}</p>      <!-- 001,234.567 -->
<p>{{ price | currency }}</p>            <!-- $1,234.57 -->
<p>{{ price | currency:'EUR' }}</p>      <!-- €1,234.57 -->
<p>{{ ratio | percent }}</p>             <!-- 42% -->

<!-- Date -->
<p>{{ today | date }}</p>                <!-- Feb 2, 2025 -->
<p>{{ today | date:'short' }}</p>        <!-- 2/2/25, 3:04 PM -->
<p>{{ today | date:'fullDate' }}</p>     <!-- Sunday, February 2, 2025 -->
<p>{{ today | date:'dd/MM/yyyy' }}</p>   <!-- 02/02/2025 -->

<!-- JSON -->
<pre>{{ user | json }}</pre>

<!-- Slice -->
<p>{{ text | slice:0:10 }}</p>

<!-- Async -->
<p>{{ observable$ | async }}</p>
@for (item of items$ | async; track item.id) {
  <li>{{ item.name }}</li>
}

<!-- KeyValue (pour objets) -->
@for (item of object | keyvalue; track item.key) {
  <div>{{ item.key }}: {{ item.value }}</div>
}
```

### Pipe personnalisé

```typescript
// truncate.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate',
  standalone: true
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 20, ellipsis: string = '...'): string {
    if (!value) return '';
    
    if (value.length <= limit) {
      return value;
    }
    
    return value.substring(0, limit) + ellipsis;
  }
}

// Utilisation
<p>{{ longText | truncate:50:'...' }}</p>
```

### Pipe avec dépendances

```typescript
import { Pipe, PipeTransform, inject } from '@angular/core';
import { DateService } from './date.service';

@Pipe({
  name: 'relativeTime',
  standalone: true
})
export class RelativeTimePipe implements PipeTransform {
  private dateService = inject(DateService);

  transform(value: Date): string {
    return this.dateService.getRelativeTime(value);
  }
}
```

### Pipe pur vs impur

```typescript
// Pipe pur (par défaut) - ne s'exécute que si l'input change
@Pipe({
  name: 'purePipe',
  standalone: true,
  pure: true  // Par défaut
})
export class PurePipe implements PipeTransform {
  transform(value: any[]): any[] {
    console.log('Pure pipe executed');
    return value.filter(item => item.active);
  }
}

// Pipe impur - s'exécute à chaque détection de changement
@Pipe({
  name: 'impurePipe',
  standalone: true,
  pure: false
})
export class ImpurePipe implements PipeTransform {
  transform(value: any[]): any[] {
    console.log('Impure pipe executed');
    return value.filter(item => item.active);
  }
}
```

---

## Lifecycle Hooks

### Ordre d'exécution

```typescript
import {
  Component,
  OnInit,
  OnChanges,
  DoCheck,
  AfterContentInit,
  AfterContentChecked,
  AfterViewInit,
  AfterViewChecked,
  OnDestroy,
  SimpleChanges
} from '@angular/core';

@Component({
  selector: 'app-lifecycle',
  standalone: true,
  template: `<p>Lifecycle demo</p>`
})
export class LifecycleComponent implements
  OnChanges,
  OnInit,
  DoCheck,
  AfterContentInit,
  AfterContentChecked,
  AfterViewInit,
  AfterViewChecked,
  OnDestroy {

  // 1. Constructor
  constructor() {
    console.log('1. Constructor');
  }

  // 2. OnChanges - Appelé quand @Input() change
  ngOnChanges(changes: SimpleChanges): void {
    console.log('2. ngOnChanges', changes);
  }

  // 3. OnInit - Initialisation (une fois)
  ngOnInit(): void {
    console.log('3. ngOnInit');
    // Idéal pour: fetch data, initialiser des propriétés
  }

  // 4. DoCheck - Détection de changement personnalisée
  ngDoCheck(): void {
    console.log('4. ngDoCheck');
    // Attention: appelé très souvent!
  }

  // 5. AfterContentInit - Après l'initialisation du contenu projeté
  ngAfterContentInit(): void {
    console.log('5. ngAfterContentInit');
  }

  // 6. AfterContentChecked - Après chaque vérification du contenu
  ngAfterContentChecked(): void {
    console.log('6. ngAfterContentChecked');
  }

  // 7. AfterViewInit - Après l'initialisation de la vue
  ngAfterViewInit(): void {
    console.log('7. ngAfterViewInit');
    // Idéal pour: manipulation du DOM, ViewChild disponible
  }

  // 8. AfterViewChecked - Après chaque vérification de la vue
  ngAfterViewChecked(): void {
    console.log('8. ngAfterViewChecked');
  }

  // 9. OnDestroy - Avant la destruction du composant
  ngOnDestroy(): void {
    console.log('9. ngOnDestroy');
    // Idéal pour: cleanup, unsubscribe
  }
}
```

### Hooks les plus utilisés

```typescript
@Component({
  selector: 'app-example',
  standalone: true,
  template: `...`
})
export class ExampleComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit(): void {
    // Initialisation
    this.loadData();
    
    // Subscribe avec cleanup automatique
    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        console.log(data);
      });
  }

  loadData(): void {
    // Fetch initial data
  }

  ngOnDestroy(): void {
    // Cleanup
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

---

## ViewChild & ContentChild

### ViewChild - Accès aux éléments de la vue

```typescript
import { Component, ViewChild, ElementRef, AfterViewInit } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `
    <input #myInput type="text">
    <app-child #childComponent></app-child>
  `
})
export class ExampleComponent implements AfterViewInit {
  // Référence à un élément HTML
  @ViewChild('myInput') inputElement!: ElementRef<HTMLInputElement>;
  
  // Référence à un composant enfant
  @ViewChild('childComponent') childComponent!: ChildComponent;
  
  // Avec options
  @ViewChild('myDiv', { read: ElementRef, static: false }) divElement!: ElementRef;

  ngAfterViewInit(): void {
    // ViewChild disponible ici
    this.inputElement.nativeElement.focus();
    this.childComponent.someMethod();
  }
}
```

### ViewChildren - Multiple éléments

```typescript
import { Component, ViewChildren, QueryList, AfterViewInit } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `
    <input #input1 type="text">
    <input #input2 type="text">
    <input #input3 type="text">
    
    <app-item *ngFor="let item of items"></app-item>
  `
})
export class ExampleComponent implements AfterViewInit {
  @ViewChildren('input1, input2, input3') inputs!: QueryList<ElementRef>;
  @ViewChildren(ItemComponent) itemComponents!: QueryList<ItemComponent>;

  ngAfterViewInit(): void {
    console.log('Total inputs:', this.inputs.length);
    
    // Itérer
    this.inputs.forEach(input => {
      console.log(input.nativeElement.value);
    });

    // Écouter les changements
    this.itemComponents.changes.subscribe((components) => {
      console.log('Components changed:', components.length);
    });
  }
}
```

### ContentChild - Accès au contenu projeté

```typescript
// child.component.ts
import { Component, ContentChild, AfterContentInit } from '@angular/core';

@Component({
  selector: 'app-child',
  standalone: true,
  template: `
    <div>
      <ng-content></ng-content>
    </div>
  `
})
export class ChildComponent implements AfterContentInit {
  @ContentChild('projectedContent') content!: ElementRef;

  ngAfterContentInit(): void {
    console.log('Projected content:', this.content);
  }
}

// parent.component.ts
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [ChildComponent],
  template: `
    <app-child>
      <p #projectedContent>This is projected content</p>
    </app-child>
  `
})
export class ParentComponent { }
```

### viewChild() et contentChild() - Signal queries ⭐ Angular 17.2+

```typescript
import { Component, viewChild, viewChildren, contentChild } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `
    <input #myInput type="text">
    <app-child></app-child>
  `
})
export class ExampleComponent {
  // Signal queries - pas besoin d'AfterViewInit
  myInput = viewChild<ElementRef>('myInput');
  childComponent = viewChild(ChildComponent);
  allInputs = viewChildren<ElementRef>('input');

  // Utilisation
  ngOnInit() {
    // Accès direct via signal
    this.myInput()?.nativeElement.focus();
  }

  handleClick() {
    const input = this.myInput();
    if (input) {
      input.nativeElement.value = 'New value';
    }
  }
}
```

---

## Animations

### Configuration

```typescript
// app.config.ts
import { provideAnimations } from '@angular/platform-browser/animations';

export const appConfig: ApplicationConfig = {
  providers: [
    provideAnimations()
  ]
};
```

### Animations de base

```typescript
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-animated',
  standalone: true,
  template: `
    <button (click)="toggle()">Toggle</button>
    <div [@openClose]="isOpen ? 'open' : 'closed'">
      Content
    </div>
  `,
  animations: [
    trigger('openClose', [
      state('open', style({
        height: '200px',
        opacity: 1,
        backgroundColor: 'yellow'
      })),
      state('closed', style({
        height: '100px',
        opacity: 0.8,
        backgroundColor: 'blue'
      })),
      transition('open => closed', [
        animate('1s')
      ]),
      transition('closed => open', [
        animate('0.5s')
      ])
    ])
  ]
})
export class AnimatedComponent {
  isOpen = true;

  toggle() {
    this.isOpen = !this.isOpen;
  }
}
```

### Animations enter/leave

```typescript
import { trigger, transition, style, animate } from '@angular/animations';

animations: [
  trigger('fadeIn', [
    transition(':enter', [
      style({ opacity: 0 }),
      animate('300ms', style({ opacity: 1 }))
    ]),
    transition(':leave', [
      animate('300ms', style({ opacity: 0 }))
    ])
  ])
]

// Template
<div @fadeIn *ngIf="show">Content</div>
```

---

## Testing

### Test unitaire basique

```typescript
// example.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { ExampleComponent } from './example.component';

describe('ExampleComponent', () => {
  let component: ExampleComponent;
  let fixture: ComponentFixture<ExampleComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [ExampleComponent]  // Standalone component
    }).compileComponents();

    fixture = TestBed.createComponent(ExampleComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should have title', () => {
    expect(component.title).toBe('Expected Title');
  });

  it('should render title in template', () => {
    const compiled = fixture.nativeElement;
    expect(compiled.querySelector('h1')?.textContent).toContain('Expected Title');
  });

  it('should increment count on button click', () => {
    const button = fixture.nativeElement.querySelector('button');
    button.click();
    fixture.detectChanges();
    
    expect(component.count).toBe(1);
  });
});
```

### Test de service

```typescript
// user.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [UserService]
    });

    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should fetch users', () => {
    const mockUsers = [
      { id: 1, name: 'John' },
      { id: 2, name: 'Jane' }
    ];

    service.getUsers().subscribe(users => {
      expect(users.length).toBe(2);
      expect(users).toEqual(mockUsers);
    });

    const req = httpMock.expectOne('https://api.example.com/users');
    expect(req.request.method).toBe('GET');
    req.flush(mockUsers);
  });
});
```

---

## Performance

### OnPush Change Detection ⭐

```typescript
import { Component, ChangeDetectionStrategy, Input } from '@angular/core';

@Component({
  selector: 'app-optimized',
  standalone: true,
  template: `
    <h2>{{ user.name }}</h2>
    <p>{{ user.email }}</p>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush  // ⭐ Performance
})
export class OptimizedComponent {
  @Input() user!: User;
}

// Avec OnPush, le composant ne se met à jour que si:
// - Un @Input() change (référence)
// - Un événement est déclenché dans le template
// - Un Observable avec async pipe émet
// - ChangeDetectorRef.markForCheck() est appelé
```

### TrackBy dans @for

```typescript
@Component({
  selector: 'app-list',
  standalone: true,
  template: `
    <!-- ✅ NOUVEAU avec track -->
    @for (item of items; track item.id) {
      <li>{{ item.name }}</li>
    }

    <!-- ❌ ANCIEN sans trackBy -->
    <li *ngFor="let item of items">{{ item.name }}</li>

    <!-- ✅ ANCIEN avec trackBy -->
    <li *ngFor="let item of items; trackBy: trackByFn">{{ item.name }}</li>
  `
})
export class ListComponent {
  items: Item[] = [];

  trackByFn(index: number, item: Item): number {
    return item.id;
  }
}
```

### Lazy Loading

```typescript
// Lazy load de composants
const routes: Routes = [
  {
    path: 'feature',
    loadComponent: () => import('./feature/feature.component')
      .then(m => m.FeatureComponent)
  },
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.routes')
      .then(m => m.ADMIN_ROUTES)
  }
];
```

### Pure Pipes

```typescript
// Les pipes purs (pure: true par défaut) sont cachés
@Pipe({
  name: 'expensive',
  standalone: true,
  pure: true  // ⭐ Par défaut, meilleure performance
})
export class ExpensivePipe implements PipeTransform {
  transform(value: any): any {
    // Opération coûteuse
    return value;
  }
}
```

---

## Best Practices

### 1. Préférer Standalone Components ⭐

```typescript
// ✅ Standalone (moderne)
@Component({
  selector: 'app-example',
  standalone: true,
  imports: [CommonModule, FormsModule]
})

// ❌ NgModule (ancien)
@NgModule({
  declarations: [ExampleComponent],
  imports: [CommonModule]
})
```

### 2. Utiliser Signals pour l'état ⭐

```typescript
// ✅ Signals
count = signal(0);
doubleCount = computed(() => this.count() * 2);

// ❌ Variables classiques (moins optimisé)
count = 0;
get doubleCount() { return this.count * 2; }
```

### 3. Nouveau Control Flow ⭐

```typescript
// ✅ Nouveau
@if (condition) { }
@for (item of items; track item.id) { }

// ❌ Ancien
<div *ngIf="condition"></div>
<div *ngFor="let item of items"></div>
```

### 4. inject() au lieu de constructor DI

```typescript
// ✅ Moderne et concis
private service = inject(MyService);

// ❌ Plus verbeux
constructor(private service: MyService) { }
```

### 5. OnPush Change Detection

```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### 6. Unsubscribe des Observables

```typescript
// ✅ Async pipe (auto unsubscribe)
data$ = this.service.getData();

// ✅ takeUntil pattern
private destroy$ = new Subject<void>();

ngOnInit() {
  this.service.getData()
    .pipe(takeUntil(this.destroy$))
    .subscribe();
}

ngOnDestroy() {
  this.destroy$.next();
  this.destroy$.complete();
}
```

### 7. Typage avec TypeScript

```typescript
// ✅ Interfaces
interface User {
  id: number;
  name: string;
}

// ✅ Type générique
getData<T>(): Observable<T> { }
```

### 8. Lazy Loading des routes

```typescript
{
  path: 'admin',
  loadComponent: () => import('./admin/admin.component')
    .then(m => m.AdminComponent)
}
```

### 9. @defer pour les composants lourds ⭐

```typescript
@defer (on viewport) {
  <heavy-component />
} @placeholder {
  <skeleton />
}
```

### 10. Code organisation

```
src/app/
├── core/          # Services singletons, guards, interceptors
├── shared/        # Composants, directives, pipes partagés
├── features/      # Modules fonctionnels
└── models/        # Interfaces et types
```

---

## Ressources

- **Documentation officielle**: https://angular.dev/
- **Angular CLI**: https://angular.io/cli
- **Angular Material**: https://material.angular.io/
- **RxJS**: https://rxjs.dev/
- **Angular Update Guide**: https://update.angular.io/
- **Angular Blog**: https://blog.angular.io/
- **Awesome Angular**: https://github.com/PatrickJS/awesome-angular

---

