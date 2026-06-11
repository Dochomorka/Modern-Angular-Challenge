## Day 39 — Routing Structure and Feature Navigation

Day 39 is about organizing routes so your app stays easy to navigate and maintain. The main idea is to connect a clean feature structure to a clean URL structure, so users can move through the app naturally and developers can find navigation logic quickly.

### Goal

By the end of this day, you should be able to:

- Organize routes by feature.
- Use parent and child routes cleanly.
- Keep navigation consistent with app structure.
- Use route links in a maintainable way.
- Separate public and protected areas.
- Build navigation that matches the app’s architecture.

### Why This Matters

Routing is the map of your app. If the structure is messy, users get lost and developers have trouble understanding how screens relate to each other.

A good routing setup helps you:
- Keep sections grouped logically.
- Make feature areas easier to navigate.
- Scale the app without confusing URL patterns.
- Reflect the app’s structure in the browser path.

### Common Routing Patterns

A strong routing structure often includes:
- A public area for landing, login, or signup.
- A protected area for app features.
- Feature-based child routes.
- A fallback route for missing paths.

Example structure:

```txt
/
  /login
  /signup
  /app
    /dashboard
    /tasks
    /tasks/:id
    /settings
```

### Feature Navigation

When your app is grouped by features, routing should usually follow the same idea. That means a feature like `tasks` can own its own child routes instead of spreading them across unrelated files.

Example:

```ts
export const routes = [
  {
    path: 'tasks',
    children: [
      { path: '', component: TaskListComponent },
      { path: ':id', component: TaskDetailComponent }
    ]
  }
];
```

This keeps list and detail views under one feature path.

### Navigation Links

Use route links that match your app’s structure clearly.

```html
<a routerLink="/tasks">Tasks</a>
<a routerLink="/tasks/1">Task 1</a>
<a routerLink="/settings">Settings</a>
```

Relative navigation is useful inside feature areas when the parent route already defines the section.

### Public and Protected Areas

Many apps separate routes that anyone can access from routes that require authentication. Public routes often include login and signup, while protected routes include dashboards, settings, and private data.

A structure like this is easier to manage when the app grows.

### Practical Example

```ts
export const routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'tasks',
    children: [
      { path: '', component: TaskListComponent },
      { path: ':id', component: TaskDetailComponent }
    ]
  },
  {
    path: 'settings',
    component: SettingsComponent
  },
  {
    path: '**',
    component: NotFoundComponent
  }
];
```

This gives the app a clear path structure and a fallback for unknown URLs.

### Best Practices

- Keep route names simple and predictable.
- Match route structure to feature structure.
- Group related pages under shared parent paths.
- Use child routes for related screens.
- Add a fallback route.
- Avoid overly deep or confusing URL structures.

### Easy Challenges

- Create a route list for three screens.
- Add a child route under one feature.
- Use route links in a navigation menu.
- Add a fallback route.
- Group one feature under a shared parent path.

### Medium Challenges

- Separate public and protected routes.
- Build a tasks feature with list and detail routes.
- Add a settings section with child pages.
- Refactor flat routes into nested ones.
- Make the route structure match the folder structure.

### Hard Challenges

- Design a route map for a medium-sized app.
- Split navigation into public, app, and admin areas.
- Refactor confusing links into a clear route system.
- Add nested navigation for one feature area.
- Create a route structure that could scale as the app grows.

### Reflection Questions

- Why should route structure match feature structure?
- What makes navigation easier to maintain?
- When should you use child routes?
- Why is a fallback route important?
- How do clean URLs improve user experience?

### Day Deliverable

Create a route structure that includes:

- At least one feature section.
- At least one child route.
- A clear navigation menu.
- A fallback route.
- URLs that reflect the app’s purpose.

### Suggested Practice Flow

1. Map out your app sections.
2. Group related screens under shared paths.
3. Add navigation links.
4. Use child routes where needed.
5. Add a fallback route.
6. Complete the easy, medium, and hard challenges.

### Note for Day 40

Next day will cover component communication patterns, which will help you move data and events cleanly between parent, child, and sibling components.
