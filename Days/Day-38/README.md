## Day 38 — App Structure and Architecture Patterns

Day 38 is about organizing an Angular app so it stays easy to grow, debug, and maintain. The main idea is to keep features separated, responsibilities clear, and shared logic in the right place.

### Goal

By the end of this day, you should be able to:

- Organize an Angular app by feature.
- Separate components, services, and models cleanly.
- Understand where shared code should live.
- Keep a predictable folder structure.
- Avoid putting too much logic in one file.
- Make the app easier for other developers to understand.

### Why This Matters

Small apps can survive with loose structure, but larger apps become difficult to manage if everything is mixed together. A good architecture makes it easier to find code, reuse logic, and scale without confusion.

This matters because:
- Features stay isolated.
- Shared code is easier to reuse.
- New work is easier to add.
- Refactoring becomes less risky.

### Common Structure Ideas

A clean Angular app often groups code by feature instead of by file type alone. For example, one feature folder may contain its component, service, routes, and related models.

A simple structure could look like this:

```txt
src/app/
  features/
    tasks/
      tasks.component.ts
      tasks.service.ts
      task.model.ts
    auth/
      login.component.ts
      auth.service.ts
  shared/
    components/
    pipes/
    directives/
  core/
    interceptors/
    guards/
    services/
```

### Feature-Based Thinking

Feature-based organization means each domain owns its own code. For example:
- `tasks` feature keeps task screens and task logic together.
- `auth` feature keeps authentication code together.
- `shared` holds reusable UI pieces.
- `core` holds app-wide infrastructure.

### Example Pattern

```ts
// task.model.ts
export interface Task {
  id: number;
  title: string;
  done: boolean;
}
```

```ts
// tasks.service.ts
import { Injectable, signal } from '@angular/core';
import { Task } from './task.model';

@Injectable({
  providedIn: 'root'
})
export class TasksService {
  tasks = signal<Task[]>([]);
}
```

```ts
// tasks.component.ts
import { Component, inject } from '@angular/core';
import { TasksService } from './tasks.service';

@Component({
  selector: 'app-tasks',
  standalone: true,
  template: `
    <h2>Tasks</h2>
    <p>Total: {{ service.tasks().length }}</p>
  `
})
export class TasksComponent {
  service = inject(TasksService);
}
```

This keeps the model, state, and UI in clear places.

### Core vs Shared

A simple rule:
- **Core**: app-wide services and infrastructure.
- **Shared**: reusable UI pieces and helpers.
- **Feature**: code specific to one business area.

Avoid placing every utility in one giant folder. Put code where its purpose is most obvious.

### Practical Example

```ts
// auth.service.ts
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  user = signal<{ name: string } | null>(null);

  login(name: string) {
    this.user.set({ name });
  }

  logout() {
    this.user.set(null);
  }
}
```

A login component can use this service, while the header can read the same user state. That keeps the authentication concern in one place.

### Best Practices

- Organize by feature when possible.
- Keep models close to the feature that uses them.
- Put reusable UI in shared folders.
- Keep app-wide infrastructure in core.
- Avoid circular dependencies.
- Keep each folder responsible for one clear purpose.

### Easy Challenges

- Create one feature folder.
- Move a component and service into the same feature.
- Add a model file next to the feature.
- Create a shared component.
- Separate app-wide code from feature code.

### Medium Challenges

- Reorganize a small app into feature folders.
- Add a `shared` folder for reusable UI.
- Move a global service into `core`.
- Create a `tasks` feature with its own files.
- Keep routes and logic together inside a feature.

### Hard Challenges

- Refactor a mixed folder structure into a feature-based one.
- Split shared and core responsibilities clearly.
- Organize a multi-feature app with clear boundaries.
- Create a structure that scales to several screens.
- Reduce cross-feature imports by improving layout.

### Reflection Questions

- Why is feature-based structure helpful?
- What belongs in shared versus core?
- Why should models live near the features that use them?
- How does structure affect maintainability?
- What happens when everything is placed in one folder?

### Day Deliverable

Create or refactor one Angular feature so it includes:

- A component.
- A service.
- A model or type.
- A clear folder location.
- A structure that would make sense to another developer.

### Suggested Practice Flow

1. Pick one feature area.
2. Group its files together.
3. Separate shared and app-wide code.
4. Move models near the feature.
5. Check whether the layout is easy to follow.
6. Complete the easy, medium, and hard challenges.

### Note for Day 39

Next day will cover routing structure and feature navigation patterns, which will help you connect a clean architecture to real app screens.
