## Day 19 — Loading and Error States

Day 19 is about making API-driven Angular screens feel complete by handling loading, success, empty, and error states clearly. A good data flow does not just fetch data; it also tells the user what is happening while the request is in progress and what went wrong if it fails.

### Goal

By the end of this day, you should be able to:

- Show a loading message or spinner while data is being fetched.
- Display API data after it arrives.
- Show an empty state when no data exists.
- Show an error state when a request fails.
- Keep the UI clear and predictable.
- Avoid confusing blank screens.

### Why This Matters

Real users need feedback. If a page is loading, failing, or empty, the interface should say so instead of leaving a blank area. This makes the app feel more trustworthy and easier to use.

### Core States

Most API screens should handle these states:

- Loading.
- Success.
- Empty.
- Error.

A simple pattern is to keep a state value in the component and switch the template based on that state.

### Example Pattern

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-post-page',
  standalone: true,
  template: `
    @if (state === 'loading') {
      <p>Loading posts...</p>
    } @else if (state === 'error') {
      <p>Something went wrong.</p>
    } @else if (posts.length === 0) {
      <p>No posts available.</p>
    } @else {
      <ul>
        @for (post of posts; track post.id) {
          <li>{{ post.title }}</li>
        }
      </ul>
    }
  `
})
export class PostPageComponent {
  state: 'loading' | 'success' | 'error' = 'loading';

  posts = [
    { id: 1, title: 'Learn Angular' },
    { id: 2, title: 'Practice API states' }
  ];
}
```

This kind of structure keeps the UI logic easy to follow.

### Loading State

Loading state tells the user data is on the way. It is usually shown before the request finishes. You can use a simple message, a skeleton screen, or a spinner.

Common tips:
- Show loading immediately before the request starts.
- Hide loading when the request completes.
- Do not leave loading on forever.
- Keep the loading UI simple.

### Error State

Error state appears when the request fails or the server is unavailable. It should explain what happened in plain language and, when possible, offer a retry button.

Useful error patterns:
- “Could not load data.”
- “Please try again.”
- “Retry” button.
- Optional technical details in development only.

### Empty State

Empty state is not the same as error. It means the request succeeded, but there was nothing to show. This is common in new accounts, empty search results, or filtered lists.

Examples:
- “No tasks yet.”
- “No results found.”
- “Your inbox is empty.”

### Practical Example

```ts
import { Component } from '@angular/core';

type ViewState = 'loading' | 'success' | 'error';

@Component({
  selector: 'app-task-screen',
  standalone: true,
  template: `
    @if (state === 'loading') {
      <p>Loading tasks...</p>
    } @else if (state === 'error') {
      <p>Could not load tasks.</p>
      <button (click)="retry()">Retry</button>
    } @else if (tasks.length === 0) {
      <p>No tasks found.</p>
    } @else {
      <ul>
        @for (task of tasks; track task.id) {
          <li>{{ task.title }}</li>
        }
      </ul>
    }
  `
})
export class TaskScreenComponent {
  state: ViewState = 'loading';

  tasks: { id: number; title: string }[] = [];

  retry() {
    this.state = 'loading';
  }
}
```

### Best Practices

- Keep each state visually distinct.
- Use short, clear messages.
- Do not confuse empty with error.
- Reset errors when retrying.
- Make loading temporary and obvious.
- Keep state logic close to the template or in a small state variable.

### Easy Challenges

- Show a loading message before data appears.
- Show an error message for a failed request.
- Show an empty message when a list is empty.
- Add a retry button.
- Use `@if` to switch between states.

### Medium Challenges

- Build a component with loading, success, empty, and error states.
- Add a fake delay to simulate API loading.
- Keep state in one variable and data in another.
- Show a different message when the result list is empty.
- Add a second action button for retrying a request.

### Hard Challenges

- Build a full API list screen with all states.
- Add loading and error handling to a routed detail page.
- Create reusable state UI blocks for empty and error cases.
- Refactor a blank screen into a fully stateful one.
- Show different messages for initial load and refresh load.

### Reflection Questions

- Why is a loading state important?
- How is an empty state different from an error state?
- What should a retry button do?
- Why should the UI never look blank when data is loading or missing?
- How do stateful screens improve user experience?

### Day Deliverable

Create one screen that includes:

- A loading state.
- A success state.
- An empty state.
- An error state.
- A retry action.

### Suggested Practice Flow

1. Decide which states your screen needs.
2. Add a state variable.
3. Build template branches for each state.
4. Add a retry action.
5. Style each state clearly.
6. Complete the easy, medium, and hard challenges.

### Note for Day 20

Next day will cover forms basics, which is the next major step after data display and API handling.
