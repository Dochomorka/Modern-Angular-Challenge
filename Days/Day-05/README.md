## Day 05 — Directives and Pipes

Day 05 is about learning how Angular changes the behavior and presentation of your templates with directives and pipes. Angular’s directive docs describe directives as classes that add behavior to elements, while the template control flow docs show how `@if`, `@for`, and `@switch` are used to conditionally render content [web:89][web:62][web:86].

### Goal

By the end of this day, you should be able to:

- Understand what directives are.
- Distinguish structural and attribute directives.
- Use built-in Angular control flow blocks.
- Understand what pipes do.
- Use common built-in pipes.
- Create a simple custom pipe.
- Build a template that transforms and formats data cleanly.

Angular’s modern template system supports direct control flow blocks, and directives remain a key way to attach behavior to DOM elements [web:62][web:89][web:86].

### Why Directives and Pipes Matter

Directives let you change how elements behave or whether they appear at all. Pipes let you transform values before they are displayed. Together, they help keep templates readable and keep formatting logic out of component classes [web:89][web:62][web:84].

This is important because Angular apps often need to:
- Show or hide UI based on conditions.
- Repeat lists of data.
- Change styling dynamically.
- Format dates, numbers, and text.
- Display values in a cleaner way.

### Directives

Directives are classes that add behavior to elements in an Angular app [web:86][web:89].

#### Structural Directives
Structural directives change the DOM by adding or removing elements. In modern Angular, the preferred syntax is control flow blocks like `@if` and `@for` [web:62].

```html
@if (isLoggedIn) {
  <p>Welcome back!</p>
} @else {
  <p>Please sign in.</p>
}
```

```html
@for (item of items; track item.id) {
  <p>{{ item.name }}</p>
} @empty {
  <p>No items found.</p>
}
```

These blocks are a cleaner and more modern alternative to legacy structural syntax in new code [web:62][web:85].

#### Attribute Directives
Attribute directives change the appearance or behavior of an existing element without removing it from the DOM [web:89][web:86].

Example:

```html
<p [class.highlight]="isImportant">Important note</p>
```

You can also create your own directive later when you want reusable behavior such as highlighting, focusing, or status styling.

### Pipes

Pipes transform data in templates before it is displayed. Angular provides built-in pipes for common formatting tasks, and custom pipes let you build your own reusable formatting logic [web:84][web:90].

Common built-in pipes include:
- `date`
- `uppercase`
- `lowercase`
- `currency`
- `percent`
- `json`

Example:

```html
<p>{{ username | uppercase }}</p>
<p>{{ price | currency }}</p>
<p>{{ today | date:'mediumDate' }}</p>
```

Pipes help keep formatting logic out of your component class and make templates easier to read [web:84][web:90].

### Custom Pipe Example

Here is a simple custom pipe that trims and adds a label:

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'labelText',
  standalone: true
})
export class LabelTextPipe implements PipeTransform {
  transform(value: string): string {
    return `Label: ${value.trim()}`;
  }
}
```

Usage:

```html
<p>{{ title | labelText }}</p>
```

This is useful when you want repeated formatting behavior across your app [web:84][web:87].

### Practical Example

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { LabelTextPipe } from './label-text.pipe';

@Component({
  selector: 'app-dashboard',
  standalone: true,
  imports: [CommonModule, LabelTextPipe],
  template: `
    <section class="dashboard">
      <h2>{{ heading | uppercase }}</h2>

      @if (isLoading) {
        <p>Loading data...</p>
      } @else {
        <ul>
          @for (item of items; track item.id) {
            <li [class.done]="item.done">
              {{ item.name | labelText }}
            </li>
          } @empty {
            <li>No items available.</li>
          }
        </ul>
      }
    </section>
  `,
  styles: [`
    .dashboard {
      padding: 1rem;
      border: 1px solid #ddd;
      border-radius: 12px;
    }

    .done {
      text-decoration: line-through;
      color: green;
    }
  `]
})
export class DashboardComponent {
  heading = 'task overview';
  isLoading = false;

  items = [
    { id: 1, name: 'Learn directives', done: true },
    { id: 2, name: 'Learn pipes', done: false },
    { id: 3, name: 'Practice templates', done: false }
  ];
}
```

This example combines control flow, list rendering, class binding, and a custom pipe in one component [web:62][web:84][web:89].

### Easy Challenges

- Use `@if` to show a message only when a condition is true.
- Use `@for` to render a list of names.
- Apply `uppercase` to a title.
- Apply `date` to a date value.
- Add a class binding to highlight a message.

### Medium Challenges

- Build a task list with `@for` and an `@empty` block.
- Create a component that uses `currency` for product prices.
- Make a custom pipe that adds a prefix to a string.
- Use `@if` to show loading, success, and empty states.
- Style a list item conditionally based on its data.

### Hard Challenges

- Build a mini dashboard using `@if`, `@for`, and multiple pipes.
- Create a custom pipe for truncating long text.
- Combine a pipe with conditional class styling.
- Refactor template formatting logic into a pipe.
- Create a reusable directive-like behavior using class bindings and component logic.

### Reflection Questions

- What is the difference between a directive and a pipe?
- When should you use `@if` and `@for`?
- Why are pipes useful in templates?
- Why should formatting logic often stay out of component code?
- How do directives help keep Angular templates expressive?

### Day Deliverable

Create a `DashboardComponent` or `TaskListComponent` that includes:

- One `@if` block.
- One `@for` block.
- One built-in pipe.
- One custom pipe.
- One conditional style or class binding.

### Suggested Practice Flow

1. Read the explanation of directives and pipes.
2. Study the example component and custom pipe.
3. Type the code manually.
4. Add your own items and formatting.
5. Complete the easy, medium, and hard challenges.
6. Build a small UI that displays and formats data clearly.

### Note for Day 06

Next day will move into dependency injection basics, which will show how Angular provides services and shared functionality to your components [web:89][web:86].
