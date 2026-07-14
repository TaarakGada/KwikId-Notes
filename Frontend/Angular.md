# Angular

## What is it?
- Angular is a **full-featured frontend framework** by Google
- Unlike React (a library), Angular is an opinionated framework — it has built-in solutions for routing, forms, HTTP, etc.
- Uses **TypeScript** by default
- Heavily structured with components, modules, services

## Core Concepts

### Component
- The basic building block — has HTML template, CSS styles, and TypeScript logic
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-button',        // Used as <app-button> in HTML
  template: `<button (click)="handleClick()">{{ label }}</button>`,
  styles: [`button { color: red; }`]
})
export class ButtonComponent {
  label = 'Click Me';

  handleClick() {
    console.log('Clicked!');
  }
}
```

### Module
- Groups related components, services, and imports together
- Every Angular app has a root module `AppModule`

### Service
- A class that handles business logic, API calls, shared data
- Injected into components via **Dependency Injection**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {}

  getUser(id: string) {
    return this.http.get(`/api/users/${id}`);
  }
}
```

### Data Binding
```html
<!-- Property Binding: JS → HTML -->
<img [src]="imageUrl">

<!-- Event Binding: HTML → JS -->
<button (click)="doSomething()">Click</button>

<!-- Two-way Binding: both directions -->
<input [(ngModel)]="username">

<!-- Interpolation -->
<p>Hello, {{ username }}!</p>
```

### Directives
```html
<!-- *ngIf: Conditional rendering -->
<p *ngIf="isLoggedIn">Welcome back!</p>

<!-- *ngFor: List rendering -->
<li *ngFor="let item of items">{{ item.name }}</li>
```

### Routing
```typescript
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users/:id', component: UserDetailComponent },
  { path: '**', component: NotFoundComponent }  // Wildcard
];
```

## Good to Know
- Angular CLI: `ng new`, `ng generate component`, `ng serve`, `ng build`
- Angular uses **RxJS** heavily (Observables for async data)
- Built-in **HttpClient** for API calls
- **Angular Material** for pre-built UI components
- Learning curve is steeper than React due to more concepts
- Great for large enterprise applications
