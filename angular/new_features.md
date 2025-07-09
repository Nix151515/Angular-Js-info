
# Angular 14–19 Feature Overview

## Angular 14
- [1. Leveraging Standalone Components](#1-leveraging-standalone-components)
- [2. Typed Reactive Forms](#2-typed-reactive-forms)
- [3. Route Title Configuration](#3-route-title-configuration)
- [4. Optional Injectors in Component APIs](#4-optional-injectors-in-component-apis)
- [5. Advanced Dependency Injection and Services](#5-advanced-dependency-injection-and-services)

## Angular 15
- [1. Directive Composition API](#1-directive-composition-api)
- [2. Improved Angular Material Components](#2-improved-angular-material-components)

## Angular 16
- [1. Signals API (Developer Preview)](#1-signals-api-developer-preview)
- [2. Hydration for SSR](#2-hydration-for-ssr)
- [3. DestroyRef](#3-destroyref)

## Angular 17
- [1. defer Block](#1-defer-block)
- [2. Control Flow Syntax](#2-control-flow-syntax)
- [3. Final Signals API Release](#3-final-signals-api-release)

## Angular 18
- [1. Module Federation Enhancements](#1-module-federation-enhancements)

## Angular 19
- [1. Advanced Signals Features](#1-advanced-signals-features)
- [2. Performance Optimizations](#2-performance-optimizations)

---
## Angular 14

### 1. Leveraging Standalone Components

**Feature:** Angular 14 introduced standalone components to simplify module dependencies.

**Example:**

```ts
// product.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-product',
  standalone: true, // Marking as standalone
  imports: [CommonModule],
  template: `<h1>Product List</h1>`,
})
export class ProductComponent {}
```

**Usage:**
You can directly import this component in your `AppModule` or `routes` without wrapping it in a separate module.

```ts
// app.module.ts or routes.ts
import { ProductComponent } from './product.component';

@NgModule({
  declarations: [],
  imports: [BrowserModule, ProductComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

---

### 2. Typed Reactive Forms

**Feature:** Angular 14 added strong typing support for Reactive Forms.

**Example:**

```ts
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators, FormControl } from '@angular/forms';

@Component({
  selector: 'app-typed-form',
  template: `
    <form [formGroup]="profileForm">
      <label>
        Name:
        <input formControlName="name" />
      </label>
      <label>
        Age:
        <input formControlName="age" type="number" />
      </label>
    </form>
  `,
})
export class TypedFormComponent {
  profileForm: FormGroup<{
    name: FormControl<string>;
    age: FormControl<number>;
  }>;

  constructor(private fb: FormBuilder) {
    this.profileForm = this.fb.group({
      name: this.fb.control<string>('', [Validators.required]),
      age: this.fb.control<number>(null, [Validators.required, Validators.min(0)]),
    });
  }
}
```

---

### 3. Route Title Configuration

**Feature:** Add route titles directly in the configuration.

**Example:**

```ts
import { Routes } from '@angular/router';
import { ProductComponent } from './product.component';

export const routes: Routes = [
  {
    path: 'products',
    component: ProductComponent,
    title: 'Product List',
  },
];
```
The title will automatically update the browser’s title bar when navigating to this route.

---

### 4. Optional Injectors in Component APIs

**Feature:** Use `injector` with `ViewContainerRef` for dynamic component creation.

**Example:**

```ts
import {
  Component,
  Injector,
  ViewContainerRef,
  createComponent,
} from '@angular/core';

@Component({
  selector: 'app-dynamic',
  template: `<button (click)="loadComponent()">Load Component</button>`,
})
export class DynamicComponent {
  constructor(private viewContainerRef: ViewContainerRef, private injector: Injector) {}

  loadComponent() {
    const componentRef = createComponent(ProductComponent, {
      environmentInjector: this.viewContainerRef.injector,
    });
    this.viewContainerRef.insert(componentRef.hostView);
  }
}
```

---

### 5. Advanced Dependency Injection and Services

**Feature:** Create singleton or scoped services.

**Example:**

```ts
@Injectable({
  providedIn: 'root', // Singleton service
})
export class ApiService {
  fetchData() {
    return 'Data fetched from API';
  }
}

@Injectable({
  providedIn: 'any', // Scoped service
})
export class ScopedService {}
```

---

## Angular 15

### 1. Directive Composition API

**Feature:** Compose behaviors using directives.
The directive composition API lets you apply directives to a component's host element from within the component TypeScript class.

https://angular.dev/guide/directives/directive-composition-api

When the framework renders a component, Angular also creates an instance of each host directive.
The directive's host bindings apply to the component's host element.

- Angular applies host directives statically at compile time.

**Example:**

```ts
@Directive({
  selector: '[appClickTracker]',
})
export class ClickTrackerDirective {
  @HostListener('click', ['$event'])
  trackClicks(event: Event) {
    console.log('Click event tracked!', event);
  }
}

@Component({
  selector: 'app-button',
  template: `<button appClickTracker>Click Me!</button>`,
})
export class ButtonComponent {}
```
- *By default, host directive inputs and outputs are not exposed as part of the component's public API.
 You can explicitly include inputs and outputs in your component's API by expanding the entry in 'hostDirectives':

 ```ts
 @Component({
  selector: 'admin-menu',
  template: 'admin-menu.html',
  hostDirectives: [{
    directive: MenuBehavior,
    inputs: ['menuId'],
    outputs: ['menuClosed'],
  }],
})
export class AdminMenu { }
```
- You can also add 'hostDirectives' to other directives, in addition to components. This enables the transitive aggregation of multiple behaviors.
---

### 2. Improved Angular Material Components

**Feature:** Updated components to align with latest Material Design.

**Example:**

```html
<mat-toolbar color="primary">
  <span>Angular App</span>
</mat-toolbar>

<mat-card>
  <mat-card-header>
    <mat-card-title>Product Card</mat-card-title>
  </mat-card-header>
  <mat-card-content>
    This is an example of an updated Material Design card component.
  </mat-card-content>
</mat-card>
```

---

## Angular 16

### 1. Signals API (Developer Preview)

**Feature:** Reactive state management.

**Example:**

```ts
@Component({
  selector: 'app-counter',
  template: `
    <button (click)="increment()">Increment</button>
    <p>Count: {{ count() }}</p>
  `,
})
export class CounterComponent {
  count = signal(0);

  increment() {
    this.count.set(this.count() + 1);
  }
}
```

---

### 2. Hydration for SSR

**Feature:** Enables hydration for Angular Universal apps.

**Example:**

```ts
import { provideServerRendering } from '@angular/platform-server';
import { provideHydration } from '@angular/platform-browser';

bootstrapApplication(AppComponent, {
  providers: [provideServerRendering(), provideHydration()],
});
```

---

### 3. DestroyRef

**Feature:** Simplified subscription cleanup.

**Example:**

```ts
import { Component, DestroyRef, inject } from '@angular/core';
import { interval } from 'rxjs';

@Component({
  selector: 'app-auto-unsubscribe',
  template: `<p>Check console for interval logs.</p>`,
})
export class AutoUnsubscribeComponent {
  private destroyRef = inject(DestroyRef);

  constructor() {
    const subscription = interval(1000).subscribe((val) =>
      console.log('Interval value:', val)
    );

    this.destroyRef.onDestroy(() => subscription.unsubscribe());
  }
}
```

---

## Angular 17

### 1. `defer` Block

**Feature:** Optimized lazy loading.

**Example:**

```ts
@Component({
  selector: 'app-dashboard',
  template: `<ng-container *defer="loadComponent()"></ng-container>`,
})
export class DashboardComponent {
  loadComponent() {
    return import('./lazy-feature/lazy-feature.component').then(
      (m) => m.LazyFeatureComponent
    );
  }
}
```

---

### 2. Control Flow Syntax

**Feature:** Improved template logic.

**Example:**

```ts
@Component({
  selector: 'app-user-info',
  template: `
    <ng-container *if="isLoggedIn; then loggedIn else loggedOut"></ng-container>
    <ng-template #loggedIn>Welcome back, User!</ng-template>
    <ng-template #loggedOut>Please log in to continue.</ng-template>
  `,
})
export class UserInfoComponent {
  isLoggedIn = false;
}
```

---

### 3. Final Signals API Release

**Feature:** Finalized for production use.

**Example:**

```ts
@Component({
  selector: 'app-login-status',
  template: `<p>Status: {{ status() }}</p>`,
})
export class LoginStatusComponent {
  status = signal('Logged Out');

  logIn() {
    this.status.set('Logged In');
  }

  logOut() {
    this.status.set('Logged Out');
  }
}
```

---

## Angular 18

### 1. Module Federation Enhancements

**Feature:**  Improved support for micro-frontends with Module Federation.Enhanced micro-frontend capabilities to allow seamless sharing of code and features across applications.

**Example:**
Set up Webpack Module Federation to expose or consume remote components:
```js
// webpack.config.js for app1
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'app1',
      exposes: {
        './Button': './src/app/components/button/button.component.ts',
      },
    }),
  ],
};
```

```ts
// Usage in app2
import('app1/Button').then((module) => {
  const Button = module.ButtonComponent;
  // Use Button in your app
});
```
 - Usage: Share components between app1 and app2 without duplication.

---

## Angular 19

### 1. Advanced Signals Features

**Feature:** Derived and computed signals.

**Example 1:**

```ts
@Component({
  selector: 'app-shopping-cart',
  template: `
    <p>Total: {{ total() }}</p>
    <button (click)="addItem()">Add Item</button>
  `,
})
export class ShoppingCartComponent {
  items = signal<number[]>([]);
  total = computed(() => this.items().reduce((sum, item) => sum + item, 0));

  addItem() {
    this.items.set([...this.items(), 10]); // Adding item worth $10
  }
}
```

**Example 2:**

```ts
@Component({
  selector: 'app-calculator',
  template: `
    <p>Base: {{ base() }}</p>
    <p>Square: {{ square() }}</p>
  `,
})
export class CalculatorComponent {
  base = signal(5);
  square = computed(() => this.base() * this.base());
}
```

---

### 2. Performance Optimizations

**Feature:** Continued improvements in build/runtime performance (enabled automatically with `ng build` and optimizations).

---
https://dev.to/renukapatil/angular-updates-explained-features-from-version-14-to-19-3ci5