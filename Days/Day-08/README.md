## Day 08 — Standalone Components

Day 08 is about learning the modern Angular component model, where components can be standalone and directly import what they need. Angular’s current documentation says components are standalone by default, and standalone components provide a simplified way to build Angular applications without relying on `NgModule` declarations [web:16][web:13][web:107].

### Goal

By the end of this day, you should be able to:

- Understand what a standalone component is.
- Know why Angular moved toward standalone-first development.
- Create a standalone component.
- Import other standalone components, directives, or pipes.
- Understand how standalone components reduce boilerplate.
- Build small component-based features without `NgModule`.

Angular’s docs and migration guide both describe standalone components as a simpler way to organize Angular apps and reduce unnecessary module wiring [web:13][web:16][web:111].

### Why Standalone Components Matter

Standalone components make Angular easier to read and compose. Instead of declaring everything in an `NgModule`, each component can directly state its dependencies, which makes the code more explicit and often easier to maintain [web:16][web:13][web:18].

This matters because it helps you:
- Reduce boilerplate.
- Create smaller and clearer app structures.
- Import only what a component actually needs.
- Learn Angular with a more direct mental model.

### What Makes a Component Standalone

A standalone component is a component that can be used without being declared in an `NgModule`. Instead, it is marked as `standalone: true` and can import dependencies directly in its own metadata [web:16][web:107][web:13].

Basic example:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hello',
  standalone: true,
  template: `<h1>Hello from standalone Angular</h1>`
})
export class HelloComponent {}
```

This kind of component is self-contained and can be used more directly than older module-based patterns [web:107][web:18].

### Using Imports in Standalone Components

Standalone components can import other standalone components, directives, and pipes directly. This is one of the biggest differences from the older `NgModule` approach [web:16][web:111][web:13].

Example:

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-list',
  standalone: true,
  imports: [CommonModule],
  template: `
    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>
  `
})
export class ListComponent {
  items = ['Angular', 'TypeScript', 'RxJS'];
}
```

In newer Angular templates, you will often use `@for` instead of `*ngFor`, but the key idea is the same: the component directly declares what it needs [web:62][web:111].

### Standalone and Modern Angular

Angular’s migration docs show that standalone is part of a broader move toward modern Angular APIs, including built-in control flow, improved injection, and newer template patterns [web:111]. The standalone model fits well with that direction because it keeps components more self-contained and easier to compose [web:13][web:16].

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-user-badge',
  standalone: true,
  template: `
    <article class="badge">
      <h2>{{ name }}</h2>
      <p>{{ role }}</p>
    </article>
  `,
  styles: [`
    .badge {
      padding: 1rem;
      border: 1px solid #ddd;
      border-radius: 12px;
      max-width: 280px;
    }
  `]
})
export class UserBadgeComponent {
  name = 'Yimesgen Morka';
  role = 'Software Engineer';
}
```

This component is fully standalone and can be reused without module setup [web:16][web:107].

### Composing Components

One of the best benefits of standalone components is that you can compose features by importing smaller pieces directly into a page component [web:16][web:13].

Example:

```ts
import { Component } from '@angular/core';
import { UserBadgeComponent } from './user-badge.component';

@Component({
  selector: 'app-dashboard',
  standalone: true,
  imports: [UserBadgeComponent],
  template: `
    <h1>Dashboard</h1>
    <app-user-badge />
  `
})
export class DashboardComponent {}
```

This style makes component composition straightforward and explicit [web:16][web:111].

### Easy Challenges

- Create a standalone component with one title.
- Add `standalone: true` to a component.
- Import one standalone component into another.
- Display a small card with a name and role.
- Use one standalone component inside a page component.

### Medium Challenges

- Build a reusable `UserBadgeComponent`.
- Create a parent component that imports and displays two child components.
- Make a standalone list component.
- Refactor a component to remove module-based assumptions.
- Add styling directly in the component metadata.

### Hard Challenges

- Build a small dashboard using multiple standalone components.
- Create a parent page that composes header, card, and list components.
- Convert an older module-style component into standalone style.
- Design a feature where each part imports only what it needs.
- Build a clean component tree without any unnecessary `NgModule` wiring.

### Reflection Questions

- Why are standalone components simpler than module-based components?
- What problem does `standalone: true` solve?
- Why is direct import management useful?
- How does standalone fit modern Angular better?
- When would you import a component directly into another component?

### Day Deliverable

Create one standalone component and one parent component that uses it:

- The child component must be standalone.
- The parent component must import the child component directly.
- The parent component should display at least one child instance.
- Add styling so the components look visually separated.

### Suggested Practice Flow

1. Read the explanation of standalone components.
2. Create a standalone child component.
3. Import it into a parent component.
4. Add styling and content.
5. Build a small UI that uses multiple standalone pieces.
6. Complete the easy, medium, and hard challenges.

### Note for Day 09

Next day will cover template syntax and control flow in more depth, which builds directly on the standalone component approach [web:62][web:111].
