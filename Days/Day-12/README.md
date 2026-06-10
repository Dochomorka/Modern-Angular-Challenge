## Day 12 — Signals Fundamentals

Day 12 is about learning Angular signals, which are the modern way to manage local reactive state. Angular’s signals docs describe a signal as a lightweight wrapper around a value that notifies consumers when the value changes, and the essentials guide shows `signal` and `computed` as the core APIs for managing dynamic data [web:140][web:137].

### Goal

By the end of this day, you should be able to:

- Understand what a signal is.
- Create writable signals.
- Read signal values correctly.
- Use `computed` for derived values.
- Use `effect` for side effects.
- Build a small reactive component with signals.

Angular’s current guidance emphasizes signals as a core reactivity model for managing component state [web:137][web:140].

### Why Signals Matter

Signals make local state easier to reason about because the value is explicit and reactive. Instead of manually pushing changes through multiple pieces of UI logic, you create a state value and let Angular update the template when it changes [web:140][web:137].

This matters because:
- UI state becomes easier to read.
- Derived values are simple to express.
- Side effects can be isolated.
- Components become more predictable.

### Writable Signals

A writable signal is a piece of state you can change directly.

```ts
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <p>Count: {{ count() }}</p>
    <button (click)="increment()">Increment</button>
  `
})
export class CounterComponent {
  count = signal(0);

  increment() {
    this.count.update(value => value + 1);
  }
}
```

You read a signal by calling it like a function, and you update it with methods like `set` or `update` [web:137][web:140].

### Computed Signals

Computed signals are derived from other signals.

```ts
import { Component, computed, signal } from '@angular/core';

@Component({
  selector: 'app-price-summary',
  standalone: true,
  template: `
    <p>Price: {{ price() }}</p>
    <p>Tax: {{ tax() }}</p>
    <p>Total: {{ total() }}</p>
  `
})
export class PriceSummaryComponent {
  price = signal(100);
  tax = signal(15);

  total = computed(() => this.price() + this.tax());
}
```

Computed values update automatically when the signals they depend on change [web:137][web:138][web:140].

### Effects

Effects run side-effect logic when signals change. Angular’s signals guidance describes signals as reactive state and effects as a way to react to that state [web:140][web:135].

```ts
import { Component, effect, signal } from '@angular/core';

@Component({
  selector: 'app-name-editor',
  standalone: true,
  template: `
    <input [value]="name()" (input)="onInput($event)" />
    <p>Hello, {{ name() }}</p>
  `
})
export class NameEditorComponent {
  name = signal('Yimesgen');

  constructor() {
    effect(() => {
      console.log('Name changed:', this.name());
    });
  }

  onInput(event: Event) {
    const input = event.target as HTMLInputElement;
    this.name.set(input.value);
  }
}
```

Effects are useful for logging, syncing, and other side effects, not for computing display values [web:135][web:140].

### Practical Example

```ts
import { Component, computed, effect, signal } from '@angular/core';

@Component({
  selector: 'app-task-counter',
  standalone: true,
  template: `
    <section>
      <h2>Task Counter</h2>
      <p>Total: {{ tasks().length }}</p>
      <p>Completed: {{ completedCount() }}</p>
      <p>Progress: {{ progressLabel() }}</p>

      <button (click)="addTask()">Add Task</button>
      <button (click)="completeTask()">Complete Task</button>
    </section>
  `
})
export class TaskCounterComponent {
  tasks = signal([
    { id: 1, title: 'Learn signals', done: false },
    { id: 2, title: 'Practice computed', done: true }
  ]);

  completedCount = computed(() =>
    this.tasks().filter(task => task.done).length
  );

  progressLabel = computed(() => {
    const total = this.tasks().length;
    const done = this.completedCount();
    return total === 0 ? 'No tasks yet' : `${done}/${total} completed`;
  });

  constructor() {
    effect(() => {
      console.log('Task count:', this.tasks().length);
    });
  }

  addTask() {
    this.tasks.update(current => [
      ...current,
      { id: Date.now(), title: 'New task', done: false }
    ]);
  }

  completeTask() {
    this.tasks.update(current =>
      current.map((task, index) =>
        index === 0 ? { ...task, done: true } : task
      )
    );
  }
}
```

This example combines writable signals, computed signals, and an effect in one component [web:137][web:140][web:135].

### Signals Best Practices

- Use `signal` for local state.
- Use `computed` for derived values.
- Use `effect` for side effects only.
- Call signals like functions in templates and class code.
- Keep signal state small and focused [web:140][web:137][web:135].

### Easy Challenges

- Create a signal that holds a number.
- Display a signal value in a template.
- Update a signal using `set`.
- Create a computed signal for a doubled number.
- Add a button that updates a signal.

### Medium Challenges

- Build a counter with increment, decrement, and reset.
- Create a signal array of tasks and a computed completed count.
- Use an effect to log signal changes.
- Create a computed label from two signals.
- Build a small component with one writable signal and one computed signal.

### Hard Challenges

- Build a task list using signals only for local state.
- Create a profile editor with editable signal fields.
- Use computed values to summarize a collection.
- Add an effect that reacts to changes in multiple signals.
- Refactor a simple stateful component from plain properties to signals.

### Reflection Questions

- Why are signals useful for component state?
- What is the difference between `signal` and `computed`?
- Why should effects be used carefully?
- How do signals make Angular state easier to reason about?
- When would you still use RxJS instead of signals?

### Day Deliverable

Create a component that includes:

- One writable signal.
- One computed signal.
- One effect.
- At least one button that updates signal state.
- A template that reads signals correctly using `()`.

### Suggested Practice Flow

1. Read the explanation of signals.
2. Create a simple writable signal.
3. Add a computed signal.
4. Add an effect.
5. Build a small interactive component.
6. Complete the easy, medium, and hard challenges.

### Note for Day 13

Next day will cover computed values and effects in more depth, which will help you use signals more intentionally in real Angular features [web:137][web:135].
