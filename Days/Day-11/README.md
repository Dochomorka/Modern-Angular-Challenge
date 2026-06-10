## Day 11 — Inputs, Outputs, and Model Patterns

Day 11 is about taking component communication one step further by learning the modern Angular patterns for inputs, outputs, and model-based state. Angular’s current docs explain that inputs let a parent pass data to a child, while model inputs can also propagate new values back to the parent [web:120][web:129][web:125].

### Goal

By the end of this day, you should be able to:

- Use input properties to receive data.
- Use output events to emit changes.
- Understand the difference between one-way and two-way style patterns.
- Use signal-based input style where appropriate.
- Know when to use `input()` and `model()` patterns.
- Build components that expose clean and predictable APIs.

Angular docs note that model inputs are a special type of input that can send new values back to the parent, and signal-based inputs expose values through signals [web:120][web:129][web:130].

### Why This Matters

Inputs and outputs are how Angular components create clear communication contracts. A component should say, “Here is what I accept” and “Here is what I emit,” which keeps it reusable and easy to reason about [web:120][web:125][web:122].

This matters because:
- Parents control child data.
- Children notify parents about changes.
- Components stay reusable.
- State flow is easier to trace.

### Standard Inputs

Traditional inputs are used when a parent sends data into a child.

```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user-card',
  standalone: true,
  template: `
    <article>
      <h2>{{ name }}</h2>
      <p>{{ role }}</p>
    </article>
  `
})
export class UserCardComponent {
  @Input() name = 'Guest';
  @Input() role = 'Learner';
}
```

Angular’s docs explain that `@Input` marks a property as bindable from the parent template [web:125][web:120].

### Standard Outputs

Outputs are used when a child sends an event to the parent.

```ts
import { Component, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-action-button',
  standalone: true,
  template: `<button (click)="clicked.emit()">Click</button>`
})
export class ActionButtonComponent {
  @Output() clicked = new EventEmitter<void>();
}
```

The parent listens with `(clicked)="handleClick()"` [web:122][web:127].

### Signal-Based Inputs

Angular also supports signal-based input styles. The docs show that input values can be exposed as signals, which fits nicely with modern reactive code [web:120][web:130].

```ts
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-profile-badge',
  standalone: true,
  template: `
    <p>{{ name() }}</p>
    <p>{{ role() }}</p>
  `
})
export class ProfileBadgeComponent {
  name = input<string>('Guest');
  role = input<string>('Learner');
}
```

Because `name` and `role` are signals, you call them like functions in the template and class logic [web:120][web:130].

### Model Inputs

Model inputs are useful when the child component needs to modify a value and propagate it back to the parent. Angular describes model inputs as a special input type for this kind of two-way propagation [web:129][web:120].

A simple model-style idea:

```ts
import { Component, model } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <button (click)="count.update(v => v + 1)">Increment</button>
    <p>{{ count() }}</p>
  `
})
export class CounterComponent {
  count = model(0);
}
```

This is useful for custom controls and components that intentionally manage a value with parent synchronization [web:129][web:120].

### Practical Example

```ts
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-todo-item',
  standalone: true,
  template: `
    <article class="todo">
      <h3>{{ title() }}</h3>
      <p>{{ done() ? 'Completed' : 'Pending' }}</p>
      <button (click)="toggle.emit()">Toggle</button>
      <button (click)="removed.emit()">Remove</button>
    </article>
  `
})
export class TodoItemComponent {
  title = input.required<string>();
  done = input<boolean>(false);

  toggle = output<void>();
  removed = output<void>();
}
```

Parent usage:

```ts
import { Component } from '@angular/core';
import { TodoItemComponent } from './todo-item.component';

@Component({
  selector: 'app-todo-list',
  standalone: true,
  imports: [TodoItemComponent],
  template: `
    @for (todo of todos; track todo.id) {
      <app-todo-item
        [title]="todo.title"
        [done]="todo.done"
        (toggle)="toggle(todo.id)"
        (removed)="remove(todo.id)"
      />
    }
  `
})
export class TodoListComponent {
  todos = [
    { id: 1, title: 'Learn inputs and outputs', done: false },
    { id: 2, title: 'Practice model inputs', done: true }
  ];

  toggle(id: number) {
    this.todos = this.todos.map(todo =>
      todo.id === id ? { ...todo, done: !todo.done } : todo
    );
  }

  remove(id: number) {
    this.todos = this.todos.filter(todo => todo.id !== id);
  }
}
```

This example shows a clean contract: parent passes values in, child emits events out [web:120][web:122][web:127].

### When to Use Which Pattern

- Use **input properties** when parent-to-child data flow is enough.
- Use **output events** when the child needs to notify the parent.
- Use **signal-based inputs** when you want modern reactive style.
- Use **model inputs** when the child must update a bound value back to the parent [web:120][web:129][web:130].

### Easy Challenges

- Create a component with one required input.
- Pass a string from parent to child.
- Add one output event from a child component.
- Display an input value in the template.
- Emit a click event to the parent.

### Medium Challenges

- Build a `ProfileBadgeComponent` using signal-based inputs.
- Create a todo item with two inputs and one output.
- Make a child component emit a selected value.
- Use `input.required()` for required data.
- Refactor a traditional `@Input()` component into signal-style inputs.

### Hard Challenges

- Build a reusable editable card component with inputs and outputs.
- Create a custom counter using a model input.
- Sync parent and child state through a controlled value.
- Build a mini list manager with add, toggle, and remove events.
- Compare a classic input/output component to a signal-based version.

### Reflection Questions

- Why are inputs and outputs central to Angular component design?
- When is a model input better than separate input and output pairs?
- What benefits do signal-based inputs provide?
- How do you keep components predictable and reusable?
- When should a service be used instead of component communication?

### Day Deliverable

Create a parent-child feature using at least one of these patterns:

- One required input.
- One output event.
- One signal-based input.
- Optional: one model input for a value that flows both ways.

### Suggested Practice Flow

1. Read the explanation of input and output patterns.
2. Create a child component with typed inputs.
3. Add an output event or model input.
4. Connect it to a parent component.
5. Test state flow in both directions.
6. Complete the easy, medium, and hard challenges.

### Note for Day 12

Next day will cover signals fundamentals, which will make the reactive input style and modern state management much easier to understand [web:120][web:130].
