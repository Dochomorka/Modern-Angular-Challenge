## Day 09 — Template Syntax and Control Flow

Day 09 is about learning how Angular templates become dynamic using control flow blocks like `@if`, `@for`, and `@switch`. Angular’s current template docs explain that these blocks let you conditionally show, hide, and repeat content directly inside templates, and Angular v17+ also supports `@empty` for list fallback states [web:62][web:113][web:116].

### Goal

By the end of this day, you should be able to:

- Understand Angular template syntax.
- Use `@if` for conditional rendering.
- Use `@for` to render lists.
- Use `@switch` for multiple display cases.
- Use `@empty` for empty list states.
- Build clearer templates with less boilerplate.

Angular’s modern control flow syntax is built into templates and is part of the newer Angular development style [web:62][web:116][web:113].

### Why Control Flow Matters

Templates often need to show different UI depending on data state. For example:
- Show a loading message.
- Show a list when data exists.
- Show an empty state when nothing is available.
- Show different messages for different roles or statuses.

Control flow makes this logic easier to read and maintain directly in the template [web:62][web:63].

### `@if`

Use `@if` to display content only when a condition is true.

```html
@if (isLoggedIn) {
  <p>Welcome back!</p>
} @else {
  <p>Please sign in.</p>
}
```

Angular’s docs show `@if` as the main way to express conditional display in templates [web:62][web:113].

### `@for`

Use `@for` to loop over collections and render repeated UI.

```html
@for (item of items; track item.id) {
  <p>{{ item.name }}</p>
} @empty {
  <p>No items found.</p>
}
```

The `track` expression helps Angular keep list rendering efficient, and `@empty` lets you display fallback content when the collection has no items [web:62][web:116].

### `@switch`

Use `@switch` when one value should map to one of several UI states.

```html
@switch (status) {
  @case ('loading') {
    <p>Loading...</p>
  }
  @case ('success') {
    <p>Data loaded successfully.</p>
  }
  @case ('error') {
    <p>Something went wrong.</p>
  }
  @default {
    <p>Idle</p>
  }
}
```

This is useful when you have a small set of possible states and want to keep the template explicit [web:62][web:85].

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-task-board',
  standalone: true,
  template: `
    <section class="board">
      <h2>Task Board</h2>

      @if (tasks.length > 0) {
        <ul>
          @for (task of tasks; track task.id) {
            <li>
              {{ task.title }} - {{ task.status }}
            </li>
          }
        </ul>
      } @else {
        <p>No tasks available.</p>
      }

      <hr />

      @switch (mode) {
        @case ('view') {
          <p>Viewing tasks.</p>
        }
        @case ('edit') {
          <p>Editing tasks.</p>
        }
        @default {
          <p>Unknown mode.</p>
        }
      }
    </section>
  `
})
export class TaskBoardComponent {
  tasks = [
    { id: 1, title: 'Learn Angular template syntax', status: 'done' },
    { id: 2, title: 'Practice control flow', status: 'in progress' }
  ];

  mode: 'view' | 'edit' | 'unknown' = 'view';
}
```

This example combines conditional rendering, list rendering, and multiple-case rendering in one component [web:62][web:63].

### Template Syntax Tips

- Prefer `@if` when the UI depends on a simple condition.
- Prefer `@for` when you need to render collections.
- Prefer `@switch` when one value chooses between several UI branches.
- Use `@empty` for nicer empty states.
- Use `track` with `@for` to keep rendering efficient [web:62][web:116].

### Easy Challenges

- Create a component that shows a message only when a Boolean is true.
- Render a list of three items using `@for`.
- Add an `@empty` block to a list template.
- Create a status message using `@switch`.
- Show two different messages using `@if` and `@else`.

### Medium Challenges

- Build a task list that shows an empty state when there are no tasks.
- Create a status widget that changes content based on `status`.
- Render a list of user names with `track`.
- Add a loading view and a success view using `@if`.
- Create a small dashboard card using all three control flow blocks.

### Hard Challenges

- Build a mini task board with `@if`, `@for`, `@empty`, and `@switch`.
- Show different UI for loading, success, error, and idle states.
- Add styled list items based on task status.
- Create a reusable component that adapts its template based on input state.
- Refactor a component that uses old template patterns into modern control flow syntax.

### Reflection Questions

- Why is control flow important in Angular templates?
- When should you use `@if` instead of `@switch`?
- What is the purpose of `track` in `@for`?
- Why is `@empty` useful for user experience?
- How does modern template syntax reduce boilerplate?

### Day Deliverable

Create a `TaskBoardComponent` or `StatusPanelComponent` that includes:

- One `@if` block.
- One `@for` block.
- One `@empty` block.
- One `@switch` block.
- At least one tracked list.

### Suggested Practice Flow

1. Read the explanation of control flow.
2. Type the examples manually.
3. Change the data to test each branch.
4. Complete the easy, medium, and hard challenges.
5. Build a small component with multiple UI states.

### Note for Day 10

Next day will move into component communication, which is the next step after learning how templates display different states [web:62][web:63].
