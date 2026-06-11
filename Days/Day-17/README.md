## Day 17 — Guards and Resolvers

Day 17 is about controlling route access and preparing data before a route opens. Angular’s router docs show that guards can decide whether navigation is allowed, and resolvers can fetch or prepare data before the route is activated [web:182][web:183][web:184].

### Goal

By the end of this day, you should be able to:

- Understand what route guards do.
- Understand what resolvers do.
- Know the common guard types.
- Protect a route with `canActivate`.
- Prepare data before navigation with a resolver.
- Decide when to use a guard versus a resolver.

Angular’s router supports several guard types, including `CanActivate`, `CanActivateChild`, `CanDeactivate`, and `CanMatch`, and resolvers are used to provide data during navigation [web:182][web:183][web:184].

### Why Guards and Resolvers Matter

Not every route should be open to everyone. Some routes should only be accessible after authentication, and some routes should wait until data is ready before rendering [web:182][web:185][web:183].

This matters because:
- You can protect private pages.
- You can prevent invalid navigation.
- You can reduce loading complexity in components.
- You can improve the user experience by preloading data.

### Guards

A guard is a check that runs before navigation completes. If the guard allows it, the route opens. If not, Angular cancels the navigation [web:184][web:182].

#### `canActivate`

`canActivate` is the most common guard for protecting a route [web:182][web:184].

Example:

```ts
import { CanActivateFn } from '@angular/router';

export const authGuard: CanActivateFn = () => {
  const isLoggedIn = true;
  return isLoggedIn;
};
```

Route config:

```ts
import { Routes } from '@angular/router';
import { DashboardComponent } from './dashboard.component';
import { authGuard } from './auth.guard';

export const routes: Routes = [
  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [authGuard]
  }
];
```

If the guard returns `true`, navigation continues. If it returns `false`, the route is blocked [web:184][web:182].

#### Other Guard Types

Angular also supports:
- `CanActivateChild` for child routes.
- `CanDeactivate` for leaving a page.
- `CanMatch` for deciding whether a route should even match [web:182].

### Resolvers

A resolver prepares data before the route is shown. Angular’s `Resolve` API waits for the data to be resolved before activating the route [web:183][web:180].

Example:

```ts
import { ResolveFn } from '@angular/router';

export const userResolver: ResolveFn<string> = () => {
  return 'Resolved User Data';
};
```

Route config:

```ts
import { Routes } from '@angular/router';
import { ProfileComponent } from './profile.component';
import { userResolver } from './user.resolver';

export const routes: Routes = [
  {
    path: 'profile',
    component: ProfileComponent,
    resolve: {
      userData: userResolver
    }
  }
];
```

Resolvers are useful when a page should wait for data before rendering [web:183][web:180].

### Reading Resolved Data

A component can read resolved route data from `ActivatedRoute` [web:183][web:169].

```ts
import { Component, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-profile',
  standalone: true,
  template: `
    <h2>Profile</h2>
    <p>{{ data }}</p>
  `
})
export class ProfileComponent {
  private route = inject(ActivatedRoute);
  data = this.route.snapshot.data['userData'];
}
```

### Practical Example

```ts
import { Routes, CanActivateFn, ResolveFn } from '@angular/router';
import { Component, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

export const authGuard: CanActivateFn = () => {
  return true;
};

export const taskResolver: ResolveFn<string[]> = () => {
  return ['Learn guards', 'Learn resolvers', 'Build protected routes'];
};

@Component({
  selector: 'app-protected',
  standalone: true,
  template: `
    <section>
      <h2>Protected Tasks</h2>
      <ul>
        @for (task of tasks; track task) {
          <li>{{ task }}</li>
        }
      </ul>
    </section>
  `
})
export class ProtectedComponent {
  private route = inject(ActivatedRoute);
  tasks = this.route.snapshot.data['tasks'];
}

export const routes: Routes = [
  {
    path: 'protected',
    component: ProtectedComponent,
    canActivate: [authGuard],
    resolve: {
      tasks: taskResolver
    }
  }
];
```

This shows a route being protected and also receiving data before activation [web:184][web:183].

### Guards vs Resolvers

| Feature | Purpose | Best Use |
|---|---|---|
| Guard | Allows or blocks navigation | Auth, permissions, route checks |
| Resolver | Fetches data before route activation | Preloading page data |

Guards decide whether the route should open, while resolvers decide what data is ready when it opens [web:182][web:183][web:184].

### Easy Challenges

- Create a simple `canActivate` guard.
- Block one route with a guard.
- Create one resolver that returns static data.
- Read resolved data in a component.
- Add a protected route to your app.

### Medium Challenges

- Create a fake auth guard based on a Boolean flag.
- Add a resolver to a task detail route.
- Display resolver data in the route component.
- Add a fallback route when the guard fails.
- Use both a guard and a resolver on different routes.

### Hard Challenges

- Build a protected dashboard route.
- Create a route that waits for data before rendering.
- Add a guard that simulates role checking.
- Add a `CanDeactivate` style confirmation idea for leaving a form.
- Refactor a route so data loading happens in a resolver instead of the component.

### Reflection Questions

- Why use a guard instead of checking access inside a component?
- When is a resolver better than loading data in `ngOnInit`?
- What happens when a guard returns `false`?
- Why is route data preparation useful?
- Which parts of your app should be protected?

### Day Deliverable

Create one protected route and one resolved route:

- The protected route should use `canActivate`.
- The resolved route should receive data before rendering.
- The component should display the resolved data.
- Add a simple fallback or blocked navigation behavior.

### Suggested Practice Flow

1. Read the explanation of guards and resolvers.
2. Create a simple guard.
3. Add a resolver.
4. Attach them to routes.
5. Read data inside the route component.
6. Complete the easy, medium, and hard challenges.

### Note for Day 18

Next day will cover HttpClient and API requests, which works naturally after learning how routes can prepare or protect data [web:183][web:182].
