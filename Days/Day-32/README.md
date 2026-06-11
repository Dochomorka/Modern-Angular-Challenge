## Day 32 — State Management Basics

Day 32 is about understanding how to manage app state in a clean, predictable way. The goal is to decide what state should stay inside a component, what should move to a service, and when a more structured approach is needed.

### Goal

By the end of this day, you should be able to:

- Understand what app state is.
- Distinguish local state from shared state.
- Know when a service is enough for state.
- Recognize when state becomes too complex for components.
- Keep state updates predictable.
- Build a simple shared state flow.

### Why This Matters

As Angular apps grow, state becomes one of the most important design choices. If state is scattered everywhere, debugging becomes harder and components become tightly coupled.

Good state management helps you:
- Keep UI predictable.
- Share data between components.
- Reduce duplication.
- Make changes easier to trace.

### Types of State

There are a few common kinds of state:

- **Local state**: only one component needs it.
- **Shared state**: multiple components need it.
- **Server state**: comes from an API.
- **UI state**: loading flags, open menus, selected tabs, and similar behavior.

### Local vs Shared

Use local state when the data only matters inside one component.

Use shared state when:
- More than one component needs the same data.
- Different parts of the UI must stay in sync.
- A component tree becomes hard to manage with inputs and outputs alone.

### Service-Based State

A service is often the simplest way to share state across components.

```ts
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class CounterStateService {
  count = signal(0);

  increment() {
    this.count.update(value => value + 1);
  }

  reset() {
    this.count.set(0);
  }
}
```

A component can use this service to read and update shared state.

### Example Usage

```ts
import { Component, inject } from '@angular/core';
import { CounterStateService } from './counter-state.service';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <p>Count: {{ counter.count() }}</p>
    <button (click)="counter.increment()">Increment</button>
    <button (click)="counter.reset()">Reset</button>
  `
})
export class CounterComponent {
  counter = inject(CounterStateService);
}
```

### When a Service Is Enough

A service works well when:
- State is small.
- State is shared by a few components.
- You only need a simple source of truth.
- The update rules are straightforward.

This is often enough for small and medium Angular apps.

### When You Need More Structure

A more formal state approach may be needed when:
- Many components share the same data.
- Updates happen from several places.
- You need derived values and side effects.
- Debugging state changes becomes difficult.
- The app has complex async flows.

### Practical Example

```ts
import { Injectable, signal, computed } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TaskStateService {
  tasks = signal([
    { id: 1, title: 'Learn state basics', done: false },
    { id: 2, title: 'Practice sharing data', done: true }
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

This keeps state, updates, and derived values in one place.

### Best Practices

- Keep state close to where it is used.
- Share state only when needed.
- Use signals or services for simple shared state.
- Keep update methods explicit.
- Derive computed values instead of storing them separately.
- Avoid mutating state directly.

### Easy Challenges

- Create a component with local state.
- Move one piece of shared state into a service.
- Add one update method to a state service.
- Read shared state in two components.
- Add a reset action.

### Medium Challenges

- Build a shared counter service.
- Create a task list service with add and toggle methods.
- Add computed values for totals and completed counts.
- Use one service in two separate components.
- Keep local and shared state separate.

### Hard Challenges

- Build a small shared task manager.
- Add derived state for task progress.
- Sync a list view and a summary view with one service.
- Refactor input/output-based sync into shared state.
- Organize a larger feature into local, shared, and server state.

### Reflection Questions

- What makes state local or shared?
- When is a service enough for state management?
- Why should derived values be computed instead of stored?
- What problems happen when state is scattered?
- When does state become too complex for simple component logic?

### Day Deliverable

Create one shared state service that includes:

- A signal or observable state value.
- At least one update method.
- One computed or derived value.
- Two components that use the shared state.
- A clear separation between state and display.

### Suggested Practice Flow

1. Identify a small piece of state.
2. Decide whether it should stay local or be shared.
3. Move shared state into a service.
4. Add explicit update methods.
5. Read the same state from two components.
6. Complete the easy, medium, and hard challenges.

### Note for Day 33

Next day will cover local state patterns and when to keep state inside a component instead of moving it elsewhere.
