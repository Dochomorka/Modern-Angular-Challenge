## Day 36 — Performance Basics

Day 36 is about making Angular apps feel faster and more responsive. The main idea is to avoid unnecessary work in templates, reduce repeated rendering, and keep change detection-friendly patterns in place.

### Goal

By the end of this day, you should be able to:

- Understand what usually slows Angular apps down.
- Avoid unnecessary template work.
- Keep list rendering efficient.
- Use good component boundaries.
- Reduce heavy logic in templates.
- Recognize common performance pitfalls.

### Why This Matters

As apps grow, performance issues often come from small repeated costs: too much template logic, large lists rendering often, or components updating more than they need to. Fixing these early keeps the app smooth and easier to scale.

This matters because:
- The UI feels more responsive.
- Lists render more efficiently.
- Components do less unnecessary work.
- The app remains easier to maintain.

### Common Performance Habits

A few habits make a big difference:
- Keep templates simple.
- Avoid expensive function calls in templates.
- Break large UIs into smaller components.
- Use efficient list tracking.
- Only compute data when needed.

### Template Work

If a template does too much work, Angular may repeat that work during updates. A better approach is to compute values in the component or derive them once instead of recalculating them constantly in the template.

Bad pattern:
- Calling heavy methods directly from the template.

Better pattern:
- Store the computed result in a property or derived value.

### Efficient Lists

Large lists can become expensive if every item is re-rendered unnecessarily. When you render arrays, use stable item identity so Angular can keep existing DOM elements where possible.

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-task-list',
  standalone: true,
  template: `
    <ul>
      @for (task of tasks; track task.id) {
        <li>{{ task.title }}</li>
      }
    </ul>
  `
})
export class TaskListComponent {
  tasks = [
    { id: 1, title: 'Learn performance basics' },
    { id: 2, title: 'Track list items properly' }
  ];
}
```

Tracking by `id` helps Angular reuse items more effectively.

### Avoid Heavy Template Logic

Templates should display data, not do a lot of computation. If you need to filter, sort, or summarize data, it is usually better to do that in the component or in a derived stream/value.

Good rule:
- Use templates for display.
- Use component logic for data shaping.

### Smaller Components

Breaking large views into smaller parts can help performance and readability. Smaller components are easier to reason about, and they often re-render in a more targeted way.

### Best Practices

- Keep templates simple.
- Use stable tracking for lists.
- Avoid unnecessary function calls in markup.
- Split big views into smaller components.
- Compute derived values outside the template when possible.
- Measure before optimizing deeply.

### Easy Challenges

- Replace a template function with a property.
- Add tracking to a repeated list.
- Split one large section into two components.
- Remove one expensive calculation from a template.
- Keep display logic simple.

### Medium Challenges

- Optimize a task list with proper tracking.
- Refactor a filter method out of the template.
- Break a dashboard into smaller components.
- Move summary calculations into component code.
- Reduce repeated rendering in one list view.

### Hard Challenges

- Refactor a complex page into smaller performance-friendly parts.
- Improve a large list’s rendering behavior.
- Remove multiple template computations.
- Reorganize a page so only necessary sections update.
- Compare an unoptimized and optimized version of the same screen.

### Reflection Questions

- What kinds of template work can slow an app down?
- Why is list tracking important?
- When should logic move out of the template?
- How do smaller components help performance?
- Why is it better to optimize only after identifying a problem?

### Day Deliverable

Create one UI screen that includes:

- A repeated list.
- Proper item tracking.
- No heavy template computation.
- At least one component split or simplification.
- A clear improvement in clarity or efficiency.

### Suggested Practice Flow

1. Find a component with repeated work.
2. Simplify the template.
3. Add stable tracking to lists.
4. Move heavy logic into the component.
5. Split large sections if needed.
6. Complete the easy, medium, and hard challenges.

### Note for Day 37

Next day will cover change detection basics, which will help you understand when Angular updates the UI and how to keep those updates efficient.
