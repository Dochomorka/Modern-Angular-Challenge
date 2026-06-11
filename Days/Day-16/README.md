## Day 16 — Nested Routes and Route Parameters

Day 16 is about learning how Angular handles child routes and dynamic route values. Angular’s routing docs explain that nested routes, also called child routes, let you render part of the application inside a parent route, and route parameters let you pass dynamic values through the URL [web:173][web:169][web:163].

### Goal

By the end of this day, you should be able to:

- Understand nested routes.
- Use child routes with a parent layout.
- Add a second `RouterOutlet` inside a parent view.
- Define and use route parameters.
- Read route parameters inside a component.
- Build a master-detail navigation flow.

Angular’s routing documentation highlights nested routes, route params, and programmatic navigation as part of the router’s core features [web:169][web:173][web:162].

### Why Nested Routes Matter

Nested routes are useful when part of the page should stay visible while only a section changes. This is common in dashboards, account pages, settings pages, and master-detail layouts [web:173][web:176][web:172].

This matters because:
- You can keep a parent layout visible.
- The URL can represent the currently selected child view.
- Feature sections become easier to organize.
- Detail pages feel natural and scalable.

### What Are Child Routes

A child route is a route defined inside the `children` array of another route [web:173][web:169][web:178].

Example:

```ts
import { Routes } from '@angular/router';
import { SettingsComponent } from './settings.component';
import { ProfileComponent } from './profile.component';
import { SecurityComponent } from './security.component';

export const routes: Routes = [
  {
    path: 'settings',
    component: SettingsComponent,
    children: [
      { path: 'profile', component: ProfileComponent },
      { path: 'security', component: SecurityComponent }
    ]
  }
];
```

The parent component renders first, and then the child component renders inside the child `RouterOutlet` [web:173][web:176].

### Parent Component with Child Outlet

The parent route needs its own `RouterOutlet` where child routes will render [web:173][web:169].

```ts
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink } from '@angular/router';

@Component({
  selector: 'app-settings',
  standalone: true,
  imports: [RouterOutlet, RouterLink],
  template: `
    <section>
      <h2>Settings</h2>

      <nav>
        <a routerLink="profile">Profile</a>
        <a routerLink="security">Security</a>
      </nav>

      <router-outlet></router-outlet>
    </section>
  `
})
export class SettingsComponent {}
```

Notice that the child links are relative to the parent path [web:173][web:176].

### Route Parameters

Route parameters let you put dynamic values into the URL, such as `/tasks/5` or `/users/123` [web:169][web:163][web:177].

Example route:

```ts
export const routes: Routes = [
  { path: 'tasks/:id', component: TaskDetailComponent }
];
```

The `:id` part means the route accepts a dynamic value [web:169][web:177].

### Reading Route Parameters

A component can read route parameters using `ActivatedRoute` [web:177][web:174][web:169].

```ts
import { Component, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-task-detail',
  standalone: true,
  template: `
    <h2>Task Detail</h2>
    <p>Task ID: {{ taskId }}</p>
  `
})
export class TaskDetailComponent {
  private route = inject(ActivatedRoute);
  taskId = this.route.snapshot.paramMap.get('id');
}
```

The route snapshot is useful when you only need the parameter once on load [web:174][web:177].

### Parameter Navigation

You can also link to a parameterized route with `routerLink` [web:177][web:169].

```html
<a [routerLink]="['/tasks', task.id]">{{ task.title }}</a>
```

This is a common pattern in master-detail applications [web:174][web:177].

### Practical Example

```ts
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink } from '@angular/router';

@Component({
  selector: 'app-tasks',
  standalone: true,
  imports: [RouterOutlet, RouterLink],
  template: `
    <section class="layout">
      <aside>
        <h3>Tasks</h3>
        <nav>
          @for (task of tasks; track task.id) {
            <a [routerLink]="[task.id]">{{ task.title }}</a>
          }
        </nav>
      </aside>

      <main>
        <router-outlet></router-outlet>
      </main>
    </section>
  `
})
export class TasksComponent {
  tasks = [
    { id: 1, title: 'Learn nested routes' },
    { id: 2, title: 'Practice parameters' },
    { id: 3, title: 'Build a detail page' }
  ];
}
```

Routes:

```ts
import { Routes } from '@angular/router';
import { TasksComponent } from './tasks.component';
import { TaskListComponent } from './task-list.component';
import { TaskDetailComponent } from './task-detail.component';

export const routes: Routes = [
  {
    path: 'tasks',
    component: TasksComponent,
    children: [
      { path: '', component: TaskListComponent },
      { path: ':id', component: TaskDetailComponent }
    ]
  }
];
```

This pattern is ideal for list-and-detail screens [web:173][web:176].

### Easy Challenges

- Create one parent route with one child route.
- Add a second `RouterOutlet` inside a parent component.
- Create a route with `:id`.
- Display the `id` in the component.
- Create a list of links to parameterized routes.

### Medium Challenges

- Build a settings page with child routes.
- Create a tasks page with a list view and detail view.
- Use `routerLink` for relative child navigation.
- Read a route parameter with `ActivatedRoute`.
- Show a fallback view when no child route is selected.

### Hard Challenges

- Build a master-detail task manager with nested routes.
- Create a dashboard area with multiple child pages.
- Add programmatic navigation to a detail route.
- Use parameters and child routes together in one feature.
- Refactor a flat route structure into nested routing.

### Reflection Questions

- Why are child routes useful in larger apps?
- Why does a parent route need its own `RouterOutlet`?
- When should you use route parameters?
- What is the difference between a list route and a detail route?
- Why do nested routes improve app structure?

### Day Deliverable

Create a routed feature with:

- One parent route.
- At least one child route.
- One route parameter.
- One child `RouterOutlet`.
- A list view that navigates to a detail view.

### Suggested Practice Flow

1. Read the explanation of nested routes.
2. Define a parent route with children.
3. Add a second `RouterOutlet`.
4. Create a detail route with `:id`.
5. Link list items to the detail page.
6. Complete the easy, medium, and hard challenges.

### Note for Day 17

Next day will cover guards and resolvers, which help control access to routes and prepare route data before display [web:169][web:173].
