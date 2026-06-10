## Day 04 — Data Binding and Event Handling

Day 04 is about learning how Angular connects component data to the template and how the template sends user actions back to the component. Angular’s official docs describe binding as the dynamic connection between template and component data, and event binding as the way templates react to user actions [web:69][web:76].

### Goal

By the end of this day, you should be able to:

- Understand interpolation.
- Use property binding.
- Use event binding.
- Read values from template interactions.
- Combine data display with user interaction.
- Build a small interactive Angular component.

Angular supports binding text, properties, and attributes directly in templates, and it also supports event binding with parentheses syntax [web:69][web:71][web:76].

### Why Data Binding Matters

Data binding is what makes Angular feel interactive. Without it, your UI would be static HTML. With it, the component can control what appears on the screen, and user actions can update component state in response [web:69][web:78].

This is important because Angular applications are built around data flow:
- Component state goes into the template.
- User actions come back through events.
- The UI updates automatically when state changes.

### Interpolation

Interpolation displays component data inside HTML using double curly braces.

```ts
title = 'Modern Angular Challenge';
```

```html
<h1>{{ title }}</h1>
```

Angular uses interpolation to render dynamic text from the component into the template [web:69][web:78].

### Property Binding

Property binding uses square brackets to bind data to an element property.

```ts
imageUrl = 'assets/angular-logo.png';
```

```html
<img [src]="imageUrl" alt="Angular logo" />
```

Property binding is useful when the value should come from the component and be applied directly to the DOM or another Angular element [web:71][web:77].

### Event Binding

Event binding uses parentheses to listen for user events.

```ts
message = 'Click the button';
```

```html
<button (click)="changeMessage()">Click me</button>
<p>{{ message }}</p>
```

```ts
changeMessage() {
  this.message = 'Button clicked!';
}
```

Angular uses event binding so templates can respond to clicks, typing, mouse actions, and other DOM events [web:76][web:73].

### Two-way Thinking

Even before using full two-way binding, it helps to understand the pattern:
- The template shows data from the component.
- The template sends changes back through events.
- The component updates its state.
- The UI re-renders automatically.

This is the core idea behind interactive Angular apps [web:69][web:75].

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <section class="counter-card">
      <h2>{{ title }}</h2>
      <p>Count: {{ count }}</p>

      <button (click)="increment()">+1</button>
      <button (click)="reset()">Reset</button>

      <p [class.active]="count > 0">
        {{ statusMessage }}
      </p>

      @if (count === 0) {
        <p>The counter is at zero.</p>
      }
    </section>
  `,
  styles: [`
    .counter-card {
      padding: 1rem;
      border: 1px solid #ddd;
      border-radius: 12px;
      max-width: 320px;
    }

    .active {
      color: green;
      font-weight: bold;
    }

    button {
      margin-right: 0.5rem;
    }
  `]
})
export class CounterComponent {
  title = 'Counter Demo';
  count = 0;

  get statusMessage(): string {
    return this.count > 0 ? 'Counter is active' : 'Counter is idle';
  }

  increment() {
    this.count++;
  }

  reset() {
    this.count = 0;
  }
}
```

This example combines interpolation, event binding, property-style class binding, and modern control flow [web:69][web:62][web:76].

### Reading Input Values

You can also react to user input with event binding.

```html
<input (input)="onSearch($event)" placeholder="Search..." />
<p>You typed: {{ searchText }}</p>
```

```ts
searchText = '';

onSearch(event: Event) {
  const input = event.target as HTMLInputElement;
  this.searchText = input.value;
}
```

This pattern is very common in Angular when building search boxes, filters, and forms [web:73][web:76].

### Easy Challenges

- Show a title using interpolation.
- Display a number stored in the component.
- Add a button that changes a message when clicked.
- Bind an image source with property binding.
- Handle a simple click event.

### Medium Challenges

- Build a counter with increment and reset buttons.
- Create a text input that updates a property when the user types.
- Show different messages based on a number value.
- Add a boolean flag that changes the text of a button.
- Display a search term typed by the user.

### Hard Challenges

- Build an interactive profile card with edit and reset actions.
- Create a mini dashboard widget that updates multiple values from events.
- Combine input handling, click handling, and conditional display in one component.
- Use property binding for class or style changes.
- Build a small component that reacts to both mouse and keyboard events.

### Reflection Questions

- What is the difference between interpolation and property binding?
- Why is event binding important in Angular?
- How does the template communicate back to the component?
- Why are bindings central to Angular’s design?
- When would you use input events instead of button clicks?

### Day Deliverable

Create a `CounterComponent` or `ProfileCardComponent` that includes:

- At least one interpolation example.
- At least one property binding example.
- At least one event binding example.
- A button that changes component state.
- A conditional message using `@if`.

### Suggested Practice Flow

1. Read the explanation of interpolation, property binding, and event binding.
2. Type the examples manually.
3. Modify the component state and event handlers.
4. Complete the easy, medium, and hard challenges.
5. Build a small interactive component from scratch.

### Note for Day 05

Next day will cover directives and pipes, which help you control display logic and transform data inside templates [web:62][web:68].
