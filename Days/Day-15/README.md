## Day 15 — Routing Basics

Day 15 is about learning Angular routing, which lets users move between different views in a single-page application. Angular’s routing guide explains that the Angular Router is the official library for managing navigation, and it is configured with route definitions, `RouterOutlet`, and navigation helpers like `routerLink` [web:169][web:161][web:166].

### Goal

By the end of this day, you should be able to:

- Understand what routing does.
- Define a basic routes array.
- Use `RouterOutlet` to display routed views.
- Navigate with `routerLink`.
- Navigate programmatically with the Router service.
- Understand route parameters at a basic level.

Angular’s router is designed to connect URLs to components, making the app feel like multiple pages while still behaving as a single-page application [web:169][web:161][web:166].

### Why Routing Matters

Routing gives structure to your app. Instead of keeping everything on one screen, you can split the UI into pages or feature views like Home, About, Tasks, and Details [web:169][web:161].

This matters because:
- Users expect navigation.
- Apps become easier to organize.
- Features can be separated cleanly.
- URLs can represent app state.

### Router Basics

Angular routing starts with a route definition array that maps a path to a component [web:161][web:163][web:169].

Example:

```ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home.component';
import { AboutComponent } from './about.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent }
];
```

The router matches the current URL to the route config and displays the associated component [web:163][web:169].

### RouterOutlet

`RouterOutlet` is the placeholder where the active routed component appears [web:161][web:169].

```ts
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, RouterLink],
  template: `
    <nav>
      <a routerLink="/">Home</a>
      <a routerLink="/about">About</a>
    </nav>

    <router-outlet></router-outlet>
  `
})
export class AppComponent {}
```

Without `RouterOutlet`, the router has nowhere to render the active view [web:161][web:169].

### RouterLink

`routerLink` lets users navigate without reloading the page [web:161][web:169].

```html
<a routerLink="/">Home</a>
<a routerLink="/about">About</a>
```

This is the standard way to create links in Angular navigation.

### Programmatic Navigation

Sometimes navigation should happen from code, not from a link. Angular’s router guide shows that `router.navigate()` and `router.navigateByUrl()` are used for programmatic navigation [web:162][web:166].

```ts
import { Component, inject } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-login',
  standalone: true,
  template: `<button (click)="goHome()">Go Home</button>`
})
export class LoginComponent {
  private router = inject(Router);

  goHome() {
    this.router.navigate(['/']);
  }
}
```

This is useful after login, form submission, or button actions [web:162][web:166].

### Practical Example

```ts
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, RouterLink],
  template: `
    <header>
      <h1>Angular Routing Demo</h1>
      <nav>
        <a routerLink="/">Home</a>
        <a routerLink="/tasks">Tasks</a>
        <a routerLink="/about">About</a>
      </nav>
    </header>

    <main>
      <router-outlet></router-outlet>
    </main>
  `
})
export class AppComponent {}
```

Routes:

```ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home.component';
import { TasksComponent } from './tasks.component';
import { AboutComponent } from './about.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'tasks', component: TasksComponent },
  { path: 'about', component: AboutComponent }
];
```

This gives you a basic multi-view Angular app [web:169][web:161].

### Route Parameters

Route parameters let you pass values in the URL, such as `/tasks/1`. Even though this day is focused on basics, knowing that routes can include parameters is important for later lessons [web:163][web:161].

Example route:

```ts
{ path: 'tasks/:id', component: TaskDetailComponent }
```

### Easy Challenges

- Create two routes in your app.
- Add a `RouterOutlet` to the root component.
- Add two links with `routerLink`.
- Navigate between two simple components.
- Use `router.navigate()` from a button click.

### Medium Challenges

- Build a small app with Home, About, and Tasks pages.
- Highlight the active route with `routerLinkActive`.
- Add a button that navigates to another page.
- Create a detail route using a path parameter.
- Add a not-found route for unknown URLs.

### Hard Challenges

- Build a feature area with nested navigation.
- Create a task list page and a task detail page.
- Navigate to a detail route from a clicked list item.
- Use programmatic navigation after a simulated save action.
- Refactor a multi-view app so each page has its own route.

### Reflection Questions

- Why is routing important in a single-page Angular app?
- What is the role of `RouterOutlet`?
- When should you use `routerLink` versus `router.navigate()`?
- Why are route parameters useful?
- How does routing improve app structure?

### Day Deliverable

Create a small routed app with:

- At least three routes.
- One `RouterOutlet`.
- Navigation links.
- One button that navigates programmatically.
- At least one route with a parameter or a planned detail page.

### Suggested Practice Flow

1. Read the explanation of routing.
2. Define a few routes.
3. Add `RouterOutlet` to your root layout.
4. Add `routerLink` navigation.
5. Add one programmatic navigation action.
6. Complete the easy, medium, and hard challenges.

### Note for Day 16

Next day will cover nested routes and route parameters in more depth, which will help you build detail pages and feature sections [web:163][web:162].
