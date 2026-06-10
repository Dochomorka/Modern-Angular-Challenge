## Day 13 — Computed Values and Effects

Day 13 is about understanding the two most important signal companions: `computed()` and `effect()`. Angular’s signals guide explains that signals notify consumers when values change, `computed()` is used for derived values, and `effect()` is used for side effects that react to signal changes [web:17][web:146][web:145].

### Goal

By the end of this day, you should be able to:

- Understand when to use `computed()`.
- Understand when to use `effect()`.
- Build derived state from signals.
- Keep side effects separate from computed values.
- Avoid common signal misuse.
- Create a component that uses signals intentionally.

Angular’s official signals docs emphasize that computed values should be used for derived data, while effects are for reacting to state changes [web:146][web:145][web:149].

### Why This Matters

Once you start using signals, you need a clean way to separate:
- The source of truth.
- Values derived from that source.
- Side effects triggered by changes.

That separation makes your code easier to understand and much safer to maintain [web:17][web:146][web:145].

### Computed Values

A computed signal derives a new value from one or more existing signals.

```ts
import { Component, computed, signal } from '@angular/core';

@Component({
  selector: 'app-price',
  standalone: true,
  template: `
    <p>Price: {{ price() }}</p>
    <p>Tax: {{ tax() }}</p>
    <p>Total: {{ total() }}</p>
  `
})
export class PriceComponent {
  price = signal(100);
  tax = signal(15);

  total = computed(() => this.price() + this.tax());
}
```

Computed values are read-only and update automatically when dependencies change [web:145][web:148][web:146].

### When to Use Computed

Use `computed()` when:
- You need a value derived from other signals.
- You want memoized reactive calculations.
- You do not want manual recalculation logic.
- You are formatting or summarizing state.

Good examples:
- Full name from first and last name.
- Total price from price and tax.
- Filtered list from search text and items.
- Progress label from completed tasks and total tasks.

### Effects

Effects run whenever the signals they read change. Angular’s docs note that effects always run at least once and then rerun when dependencies change [web:146]. They are meant for side effects, not for deriving values [web:145][web:149].

```ts
import { Component, effect, signal } from '@angular/core';

@Component({
  selector: 'app-log-demo',
  standalone: true,
  template: `
    <p>Count: {{ count() }}</p>
    <button (click)="increment()">Increment</button>
  `
})
export class LogDemoComponent {
  count = signal(0);

  constructor() {
    effect(() => {
      console.log('Count changed:', this.count());
    });
  }

  increment() {
    this.count.update(v => v + 1);
  }
}
```

Use effects for tasks like:
- Logging.
- Syncing external APIs.
- Updating browser storage.
- Triggering non-template side effects.

### What Not to Do

Avoid using computed values for side effects and avoid using effects to calculate values that should be derived state [web:145][web:146][web:149].

Do not:
- Put `console.log` inside a computed just to observe changes.
- Update signals inside effects unless you truly know the implications.
- Use effects as a replacement for derived state.

### Practical Example

```ts
import { Component, computed, effect, signal } from '@angular/core';

@Component({
  selector: 'app-task-summary',
  standalone: true,
  template: `
    <section class="card">
      <h2>Task Summary</h2>

      <p>Total: {{ tasks().length }}</p>
      <p>Done: {{ doneCount() }}</p>
      <p>Remaining: {{ remainingCount() }}</p>
      <p>Status: {{ statusLabel() }}</p>

      <button (click)="addTask()">Add Task</button>
      <button (click)="completeFirst()">Complete First</button>
    </section>
  `
})
export class TaskSummaryComponent {
  tasks = signal([
    { id: 1, title: 'Learn computed', done: false },
    { id: 2, title: 'Learn effect', done: true }
  ]);

  doneCount = computed(() => this.tasks().filter(task => task.done).length);

  remainingCount = computed(() => this.tasks().length - this.doneCount());

  statusLabel = computed(() => {
    const total = this.tasks().length;
    if (total === 0) return 'No tasks';
    return this.doneCount() === total ? 'All complete' : 'In progress';
  });

  constructor() {
    effect(() => {
      localStorage.setItem('task-count', String(this.tasks().length));
    });
  }

  addTask() {
    this.tasks.update(current => [
      ...current,
      { id: Date.now(), title: 'New task', done: false }
    ]);
  }

  completeFirst() {
    this.tasks.update(current =>
      current.map((task, index) =>
        index === 0 ? { ...task, done: true } : task
      )
    );
  }
}
```

This example shows a clean split between source state, derived state, and a side effect [web:146][web:145][web:148].

### Computed vs Effect

| Concept | Purpose | Returns Value? | Best Use |
|---|---|---:|---|
| `computed()` | Derive new state | Yes | Totals, labels, filtered lists |
| `effect()` | Run side effects | No | Logging, storage, external sync |

Angular’s docs and signal guides strongly support this separation [web:146][web:145][web:17].

### Easy Challenges

- Create a computed value from one signal.
- Create a computed value from two signals.
- Use an effect to log signal updates.
- Create a status label from a number signal.
- Build a small component with one computed value.

### Medium Challenges

- Build a full name from first and last name signals.
- Create a computed filtered list from a search term.
- Add an effect that stores data in localStorage.
- Build a summary label for completed items.
- Show a message that changes based on multiple signals.

### Hard Challenges

- Build a task summary component with multiple computed values.
- Create a form-like component that derives validation messages.
- Add an effect that syncs signal state to browser storage.
- Refactor a component so all derived state uses computed values.
- Compare a bad example that uses effect incorrectly with a correct computed version.

### Reflection Questions

- Why should derived values use `computed()`?
- Why are effects not a replacement for computed values?
- What kind of logic belongs in an effect?
- How do computed values improve readability?
- Why is separating these concerns important in Angular?

### Day Deliverable

Create a signal-based component that includes:

- At least two computed values.
- One effect.
- One writable signal array or number.
- One button that changes the source state.
- A template that reads the computed values.

### Suggested Practice Flow

1. Read the explanation of computed values and effects.
2. Create a writable signal.
3. Add one computed value.
4. Add one effect.
5. Build a small reactive component.
6. Complete the easy, medium, and hard challenges.

### Note for Day 14

Next day will cover lifecycle and component structure, which will help you understand where signal-driven logic fits in a real component [web:146][web:17].
