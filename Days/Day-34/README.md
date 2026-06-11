## Day 34 — Shared State Patterns

Day 34 is about managing data that needs to be used by more than one component. The goal is to keep one source of truth so different parts of the UI stay in sync without passing data through many component layers.

### Goal

By the end of this day, you should be able to:

- Understand what shared state is.
- Know when multiple components need the same data.
- Store shared state in a service.
- Expose state through signals or observables.
- Update shared data from different components.
- Keep a single source of truth.

### Why This Matters

Shared state is useful when a component tree gets too deep for simple input and output chains. If a header, sidebar, and content area all need the same data, a shared state pattern keeps them aligned.

This matters because:
- Components stay in sync.
- You avoid prop drilling.
- State changes are easier to trace.
- The app becomes easier to scale.

### Shared State Basics

A shared state pattern usually has:
- A service that owns the state.
- Methods that update the state.
- Components that read the state.
- Components that call update methods.

### Example: Shared Counter

```ts
import { Injectable, signal, computed } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class SharedCounterService {
  count = signal(0);
  doubled = computed(() => this.count() * 2);

  increment() {
    this.count.update(value => value + 1);
  }

  reset() {
    this.count.set(0);
  }
}
```

A component can read and update that shared state.

```ts
import { Component, inject } from '@angular/core';
import { SharedCounterService } from './shared-counter.service';

@Component({
  selector: 'app-counter-panel',
  standalone: true,
  template: `
    <p>Count: {{ state.count() }}</p>
    <p>Doubled: {{ state.doubled() }}</p>
    <button (click)="state.increment()">Increment</button>
    <button (click)="state.reset()">Reset</button>
  `
})
export class CounterPanelComponent {
  state = inject(SharedCounterService);
}
```

### When to Use Shared State

Use shared state when:
- Two or more components need the same data.
- Different parts of the app must update together.
- Passing data through many layers would be messy.
- The data should have one source of truth.

### Common Shared State Examples

- Authentication state.
- Cart items.
- Selected user.
- Theme settings.
- Filter state.
- Task lists.
- Notification counts.

### Practical Example

```ts
import { Injectable, signal, computed } from '@angular/core';

interface Task {
  id: number;
  title: string;
  done: boolean;
}

@Injectable({
  providedIn: 'root'
})
export class TaskStateService {
  tasks = signal<Task[]>([
    { id: 1, title: 'Learn shared state', done: false },
    { id: 2, title: 'Practice service state', done: true }
  ]);

  total = computed(() => this.tasks().length);
  completed = computed(() => this.tasks().filter(task => task.done).length);

  addTask(title: string) {
    this.tasks.update(current => [
      ...current,
      { id: Date.now(), title, done: false }
    ]);
  }

  toggleTask(id: number) {
    this.tasks.update(current =>
      current.map(task =>
        task.id === id ? { ...task, done: !task.done } : task
      )
    );
  }
}
```

One component might show the list, and another might show a summary. Both read from the same service.

### Best Practices

- Keep one owner for shared state.
- Expose update methods instead of direct mutation.
- Use computed values for derived data.
- Keep shared state small when possible.
- Use local state for UI-only behavior.
- Avoid duplicating the same value in multiple places.

### Easy Challenges

- Create a service with one shared signal.
- Read the same state in two components.
- Add one update method.
- Add one computed value.
- Reset shared state from a component.

### Medium Challenges

- Build a shared task list state.
- Add a summary component that reads the same data.
- Update the shared state from more than one component.
- Add derived totals and completion counts.
- Keep the UI in sync across two views.

### Hard Challenges

- Build a mini cart with shared item count.
- Create a theme state service.
- Share a selected item across components.
- Refactor input/output chains into shared state.
- Build a dashboard summary from one shared source of truth.

### Reflection Questions

- Why is shared state useful in Angular apps?
- What is a single source of truth?
- When should data stay local instead?
- Why are update methods better than direct mutation?
- How do computed values help shared state?

### Day Deliverable

Create one shared state service that includes:

- A shared state value.
- At least one update method.
- One derived value.
- Two components that read from it.
- One component that updates it.

### Suggested Practice Flow

1. Identify data used by multiple components.
2. Move it into a service.
3. Add update methods.
4. Read it from more than one component.
5. Add derived values.
6. Complete the easy, medium, and hard challenges.

### Note for Day 35

Next day will cover syncing UI state across views, which is useful when multiple screens need to reflect the same selection or filter.
