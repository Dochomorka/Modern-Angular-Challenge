## Day 14 — Lifecycle and Component Structure

Day 14 is about understanding the Angular component lifecycle and how component code should be organized across creation, updates, and cleanup. Angular’s lifecycle guide explains that components go through defined stages, and hooks like `ngOnInit` and `ngOnDestroy` are used to run initialization and cleanup logic at the right time [web:159][web:155][web:153].

### Goal

By the end of this day, you should be able to:

- Understand what a component lifecycle is.
- Know the purpose of `constructor`.
- Understand `ngOnInit`.
- Understand `ngOnDestroy`.
- Know when `ngOnChanges` is useful.
- Organize component logic more cleanly.

Angular lifecycle hooks help you react to a component being created, updated, and destroyed, which is essential for real apps that manage data and subscriptions [web:159][web:156][web:153].

### Why Lifecycle Matters

A component is not just a static class. It is created, rendered, updated, and eventually removed. Lifecycle hooks let you attach logic to those moments so your code runs at the correct time [web:155][web:156][web:159].

This matters because:
- Initialization should happen after inputs are ready.
- Cleanup should happen before a component is destroyed.
- Side effects should not be placed randomly.
- Component logic becomes easier to maintain.

### `constructor`

The constructor is for basic class setup and dependency injection. It is not the best place for heavy initialization logic because Angular may not have finished setting up inputs and the view yet [web:155][web:156].

```ts
constructor(private logger: LoggerService) {}
```

### `ngOnInit`

`ngOnInit` runs after Angular has initialized the component’s data-bound properties. It is a common place to fetch data, initialize state, or start subscriptions [web:158][web:153][web:159].

```ts
ngOnInit() {
  this.loadTasks();
}
```

### `ngOnChanges`

`ngOnChanges` is useful when input values change and the component needs to react to those changes. It is especially important for components that receive data from a parent [web:156][web:155].

```ts
ngOnChanges() {
  this.updateView();
}
```

### `ngOnDestroy`

`ngOnDestroy` is the cleanup hook. Use it to unsubscribe from observables, clear timers, or remove event handlers before the component is destroyed [web:153][web:154][web:155].

```ts
ngOnDestroy() {
  clearInterval(this.timerId);
}
```

### Practical Example

```ts
import { Component, Input, OnChanges, OnDestroy, OnInit, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-user-status',
  standalone: true,
  template: `
    <section>
      <h2>{{ name }}</h2>
      <p>Status: {{ status }}</p>
    </section>
  `
})
export class UserStatusComponent implements OnInit, OnChanges, OnDestroy {
  @Input() name = 'Guest';
  @Input() status = 'offline';

  timerId: number | undefined;

  constructor() {
    console.log('Constructor: component created');
  }

  ngOnInit() {
    console.log('ngOnInit: initialization logic');
    this.timerId = window.setInterval(() => {
      console.log('Component still alive');
    }, 5000);
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges:', changes);
  }

  ngOnDestroy() {
    console.log('ngOnDestroy: cleanup logic');
    if (this.timerId) {
      clearInterval(this.timerId);
    }
  }
}
```

This example shows how lifecycle hooks coordinate setup, updates, and cleanup [web:159][web:156][web:153].

### Lifecycle Mental Model

A simple way to think about component lifecycle is:
1. Angular creates the component.
2. Inputs are assigned.
3. Initialization runs.
4. The component updates as data changes.
5. The component is destroyed.
6. Cleanup runs.

That sequence is what lifecycle hooks help you manage [web:155][web:156].

### Component Structure Tips

Keep component code organized by responsibility:
- Constructor: dependencies only.
- `ngOnInit`: initial setup.
- `ngOnChanges`: react to input changes.
- `ngOnDestroy`: cleanup.
- Template: rendering and user interaction.
- Helper methods: reusable component behavior.

This organization helps your Angular code stay readable as the app grows [web:159][web:155].

### Easy Challenges

- Create a component that logs a message in the constructor.
- Add `ngOnInit` and log initialization.
- Add `ngOnDestroy` and log cleanup.
- Create a component with one `@Input`.
- Use `ngOnChanges` to react to input updates.

### Medium Challenges

- Build a component that starts a timer in `ngOnInit` and clears it in `ngOnDestroy`.
- Create a component that logs every input change.
- Move initialization logic out of the constructor and into `ngOnInit`.
- Add a cleanup method for event listeners or subscriptions.
- Build a simple status component that reacts to changes in its inputs.

### Hard Challenges

- Create a component that fetches or simulates data loading in `ngOnInit`.
- Create a component that subscribes to a stream and unsubscribes in `ngOnDestroy`.
- Build a parent-child setup where the child reacts to changing inputs.
- Refactor a messy component so each lifecycle hook has one clear job.
- Create a component that demonstrates all three: init, update, and cleanup.

### Reflection Questions

- Why is `constructor` not ideal for initialization logic?
- When should you use `ngOnInit`?
- Why is `ngOnChanges` important for input-driven components?
- What belongs in `ngOnDestroy`?
- How does lifecycle knowledge help prevent bugs and leaks?

### Day Deliverable

Create one component that includes:

- A constructor with dependency injection or simple setup.
- An `ngOnInit` method.
- An `ngOnChanges` method if the component uses inputs.
- An `ngOnDestroy` method.
- A small template showing the component’s state.

### Suggested Practice Flow

1. Read the explanation of the lifecycle.
2. Create a component with one or two hooks.
3. Add logging to see the order of execution.
4. Add an input and react to changes.
5. Add cleanup logic.
6. Complete the easy, medium, and hard challenges.

### Note for Day 15

Next day will cover Angular routing basics, which builds on understanding how components are created and destroyed as users navigate the app [web:159][web:155].
