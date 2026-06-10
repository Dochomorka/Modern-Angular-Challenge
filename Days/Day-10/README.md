## Day 10 — Component Communication

Day 10 is about how Angular components talk to each other. The official Angular docs describe common parent-child communication patterns using `@Input()` and `@Output()`, and newer Angular docs also cover input properties and model-based patterns for passing data between components [web:127][web:120][web:125].

### Goal

By the end of this day, you should be able to:

- Understand parent-to-child communication.
- Understand child-to-parent communication.
- Use `@Input()` to pass data down.
- Use `@Output()` to send events up.
- Build components that work together cleanly.
- Know when to use inputs, outputs, or services.

Angular’s component communication model is a core part of building reusable UI pieces, and the official docs highlight `@Input()` and `@Output()` as the standard pattern for sharing data between parent and child components [web:127][web:120][web:125].

### Why Component Communication Matters

Most Angular apps are made of many small components. Those components often need to share data or notify each other about user actions. Communication patterns help you keep components modular while still making them work together [web:16][web:121][web:123].

This matters because:
- Parents often need to control child display.
- Children often need to notify parents about clicks or form changes.
- Shared state should be predictable.
- Reusable components become easier to build.

### Parent to Child with `@Input()`

`@Input()` lets a parent pass data into a child component [web:127][web:125].

Child component:

```ts
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-user-card',
  standalone: true,
  template: `
    <article class="card">
      <h2>{{ name() }}</h2>
      <p>{{ role() }}</p>
    </article>
  `
})
export class UserCardComponent {
  name = input<string>('Guest');
  role = input<string>('Learner');
}
```

Parent component:

```ts
import { Component } from '@angular/core';
import { UserCardComponent } from './user-card.component';

@Component({
  selector: 'app-dashboard',
  standalone: true,
  imports: [UserCardComponent],
  template: `
    <app-user-card name="Yimesgen" role="Software Engineer" />
  `
})
export class DashboardComponent {}
```

This is the most common way to send data from parent to child [web:127][web:120].

### Child to Parent with `@Output()`

`@Output()` lets a child emit an event to its parent [web:127][web:122][web:121].

Child component:

```ts
import { Component, output } from '@angular/core';

@Component({
  selector: 'app-action-button',
  standalone: true,
  template: `
    <button (click)="clicked.emit()">Send Event</button>
  `
})
export class ActionButtonComponent {
  clicked = output<void>();
}
```

Parent component:

```ts
import { Component } from '@angular/core';
import { ActionButtonComponent } from './action-button.component';

@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [ActionButtonComponent],
  template: `
    <app-action-button (clicked)="handleClick()" />
    <p>{{ message }}</p>
  `
})
export class ParentComponent {
  message = 'Waiting for child event';

  handleClick() {
    this.message = 'Child clicked!';
  }
}
```

This is the standard pattern for child-to-parent communication [web:127][web:122].

### Communication Pattern Summary

- **Input**: parent sends data to child.
- **Output**: child sends event to parent.
- **Service**: shared logic or shared state for unrelated components.
- **Template reference / ViewChild**: use only when you need direct access to a child instance.

For most everyday Angular communication, inputs and outputs are the best choice [web:127][web:16].

### Practical Example

```ts
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-todo-item',
  standalone: true,
  template: `
    <article class="todo">
      <h3>{{ title() }}</h3>
      <button (click)="removed.emit()">Remove</button>
    </article>
  `
})
export class TodoItemComponent {
  title = input.required<string>();
  removed = output<void>();
}
```

Parent:

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
        (removed)="remove(todo.id)"
      />
    }
  `
})
export class TodoListComponent {
  todos = [
    { id: 1, title: 'Learn Angular communication' },
    { id: 2, title: 'Practice inputs and outputs' }
  ];

  remove(id: number) {
    this.todos = this.todos.filter(todo => todo.id !== id);
  }
}
```

This example shows a child receiving data and sending an event back to the parent [web:120][web:127].

### Easy Challenges

- Create a child component that accepts a title from a parent.
- Pass one string value from parent to child.
- Emit one click event from child to parent.
- Display a message in the parent when the child emits.
- Use one input and one output in separate small components.

### Medium Challenges

- Build a todo item component with title input and remove output.
- Build a profile card that receives name and role from parent.
- Create a parent list component that controls child cards.
- Send a selected value from child to parent.
- Use the modern `input()` and `output()` style in a standalone component.

### Hard Challenges

- Build a mini task manager with input and output communication.
- Create a reusable item component that works inside a list.
- Add multiple inputs and one output to a child component.
- Let the parent update its state when a child emits a value.
- Refactor a component tree to use inputs and outputs instead of shared local logic.

### Reflection Questions

- Why is `@Input()` useful for parent-to-child communication?
- Why is `@Output()` useful for child-to-parent communication?
- When should you use a service instead of inputs and outputs?
- Why are reusable components easier to build with these patterns?
- What is the benefit of keeping communication one-directional?

### Day Deliverable

Create a parent-child component pair where:

- The parent passes data into the child.
- The child emits one event back to the parent.
- The parent updates a displayed message when the child emits.
- The child displays at least one input value.

### Suggested Practice Flow

1. Read the explanation of inputs and outputs.
2. Create a child component that accepts input.
3. Add an output event to the child.
4. Connect the child to a parent component.
5. Test the communication by clicking buttons or changing values.
6. Complete the easy, medium, and hard challenges.

### Note for Day 11

Next day will cover inputs, outputs, and the newer signal-based patterns in more depth, which builds naturally on this communication foundation [web:120][web:127].
