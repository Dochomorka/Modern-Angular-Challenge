## Day 03 — Components, Templates, and Styling

Day 03 is about understanding how Angular builds the user interface with components, templates, and styles. Angular components are the building blocks of Angular apps, and each component combines a TypeScript class, an HTML template, and styles into one reusable UI unit [web:16][web:60][web:61].

### Goal

By the end of this day, you should be able to:

- Understand what an Angular component is.
- Know how templates work.
- Use interpolation and event binding.
- Understand basic property binding.
- Add component styles.
- Build a simple reusable UI block.

Angular documentation explains that every component has a TypeScript class, a template, and styles, while the template controls what the user sees [web:16][web:60][web:63].

### Why Components Matter

Components are the main way Angular organizes an application. Instead of writing one huge page, you break the UI into small pieces that are easier to build, test, reuse, and maintain [web:60][web:61][web:64].

A component usually contains:

- A selector.
- A template.
- Styles.
- A TypeScript class with behavior.

This structure makes Angular apps modular and easier to scale.

### What Is a Template?

A template is the HTML that Angular renders for a component. It can include normal HTML plus Angular syntax for displaying data and reacting to user actions [web:63][web:68].

Common template features include:

- Interpolation: `{{ value }}`.
- Property binding: `[value]="expression"`.
- Event binding: `(click)="handler()"`.
- New control flow: `@if`, `@for`, `@switch` [web:62][web:65].

### Basic Component Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-welcome',
  standalone: true,
  template: `
    <section class="card">
      <h1>{{ title }}</h1>
      <p>{{ message }}</p>
      <button (click)="changeMessage()">Change Message</button>
    </section>
  `,
  styles: [`
    .card {
      padding: 1rem;
      border-radius: 8px;
      background: #f5f5f5;
    }

    button {
      margin-top: 1rem;
      padding: 0.5rem 1rem;
    }
  `]
})
export class WelcomeComponent {
  title = 'Modern Angular Challenge';
  message = 'Learning components is the first step toward building real Angular apps.';

  changeMessage() {
    this.message = 'You just changed component state with a click.';
  }
}
```

This example shows the three key pieces working together: class, template, and styling [web:16][web:60][web:61].

### Template Syntax You Should Know

#### 1. Interpolation

```html
<h1>{{ title }}</h1>
```

Interpolation displays a value from the component class in the template.

#### 2. Property Binding

```html
<img [src]="imageUrl" />
```

Property binding passes values from the component into an element property.

#### 3. Event Binding

```html
<button (click)="save()">Save</button>
```

Event binding lets the template respond to user actions.

#### 4. New Control Flow

Angular’s modern template syntax includes control flow blocks like `@if`, `@for`, and `@switch` [web:62][web:65].

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

### Styling Basics

You can style a component directly inside the component definition or with a separate CSS/SCSS file. Component styles are useful because they keep UI styling close to the component they belong to [web:60][web:61].

Example:

```ts
styles: [`
  .title {
    color: #2563eb;
    font-size: 2rem;
  }
`]
```

### Practical Example

Here is a simple card component using data display and a click handler:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-profile-card',
  standalone: true,
  template: `
    <article class="profile-card">
      <h2>{{ name }}</h2>
      <p>{{ role }}</p>
      <button (click)="toggleStatus()">
        {{ isActive ? 'Set Inactive' : 'Set Active' }}
      </button>
      <p>Status: {{ isActive ? 'Active' : 'Inactive' }}</p>
    </article>
  `,
  styles: [`
    .profile-card {
      border: 1px solid #ddd;
      padding: 1rem;
      border-radius: 12px;
      max-width: 320px;
    }
  `]
})
export class ProfileCardComponent {
  name = 'Yimesgen Morka';
  role = 'Software Engineer';
  isActive = true;

  toggleStatus() {
    this.isActive = !this.isActive;
  }
}
```

### Easy Challenges

- Create a component with a title and subtitle.
- Display a property using interpolation.
- Add a button with a click handler.
- Add a CSS class and style it.
- Show a Boolean value in the template.

### Medium Challenges

- Build a profile card component.
- Add a button that changes text when clicked.
- Use property binding for an image or input.
- Show a list of items using `@for`.
- Add an `@if` block that shows different messages.

### Hard Challenges

- Build a reusable `UserCardComponent`.
- Create a component that displays a list of data with empty-state handling.
- Combine interpolation, property binding, event binding, and `@if` in one component.
- Style the component to look like a real UI card.
- Refactor a plain HTML page into a component-based Angular view.

### Reflection Questions

- Why are components the building blocks of Angular?
- What is the difference between a template and a class?
- When should you use interpolation versus property binding?
- Why is component styling useful?
- How does `@for` improve list rendering?

### Day Deliverable

Create one standalone component called `ProfileCardComponent` that includes:

- A title.
- A short description.
- A button.
- A click event that changes text or state.
- Basic styling.
- At least one `@if` or `@for` block.

### Suggested Practice Flow

1. Read the concept overview.
2. Study the component example.
3. Type the code yourself.
4. Change the text and styling.
5. Complete the easy, medium, and hard challenges.
6. Build a small reusable card component.

### Note for Day 04

Next day will move into data binding and event handling in more depth, so this component work will become the foundation for interaction [web:63][web:66].
