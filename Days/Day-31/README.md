## Day 31 — Higher-Level Reactive Patterns

Day 31 is about using RxJS more strategically in Angular so your components stay simple and your streams do more of the work. The main focus is on building clean reactive flows for forms, route data, search, and async state.

### Goal

By the end of this day, you should be able to:

- Keep reactive logic out of templates.
- Build readable observable pipelines.
- Coordinate multiple async sources.
- Use stream-driven UI state.
- Avoid nested subscriptions.
- Recognize when reactive patterns improve component design.

### Why This Matters

At this stage, observables are no longer just about making requests. They become the way your app handles search, filters, live updates, and coordinated state.

This matters because:
- Components stay smaller.
- Async code becomes easier to reason about.
- State changes can stay predictable.
- You can avoid a lot of manual cleanup and branching.

### Reactive Thinking

A reactive pattern usually means:
- Start with a source stream.
- Transform it with operators.
- Combine it with other streams if needed.
- Expose the final result to the template or another service.

That keeps your code closer to “data flows through the app” instead of “the app constantly pulls and pushes data by hand.”

### Common Patterns

#### Search Flow
Use a form control stream, add `debounceTime`, filter short input, then call the server with `switchMap`.

#### Route + Data Flow
Combine the current route parameter with an API request so the correct item loads automatically.

#### UI State Flow
Represent loading, success, and error as a stream so the template can react cleanly.

### Example Pattern

```ts
import { Component } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { debounceTime, distinctUntilChanged, filter, map } from 'rxjs/operators';

@Component({
  selector: 'app-live-search',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <input [formControl]="search" placeholder="Search tasks" />
    <p>Check the console for the transformed query.</p>
  `
})
export class LiveSearchComponent {
  search = new FormControl('');

  constructor() {
    this.search.valueChanges.pipe(
      filter((value): value is string => !!value && value.length >= 3),
      debounceTime(300),
      distinctUntilChanged(),
      map(value => value.trim().toLowerCase())
    ).subscribe(query => {
      console.log('Query:', query);
    });
  }
}
```

### Better Stream Design

Good reactive design usually means:
- Use one source of truth for each stream.
- Transform data with operators instead of manual loops.
- Expose computed stream results to the template.
- Keep side effects at the edge.
- Avoid using subscriptions just to move data around.

### When to Use Reactive Patterns

Use them when:
- Input should trigger async work.
- Multiple values affect the same result.
- The UI depends on loading or error transitions.
- A value should update automatically over time.
- You want to reduce component-level branching.

### When Not to Overdo It

Reactive code can become hard to read if everything is turned into a stream. For small or static logic, a normal method or a signal may be simpler.

A good rule:
- Use streams for async or changing data.
- Use plain methods for simple synchronous logic.
- Use signals for local UI state when no async composition is needed.

### Practical Example

```ts
import { Component } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { combineLatest, of } from 'rxjs';
import { map, startWith } from 'rxjs/operators';

@Component({
  selector: 'app-filter-summary',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <input [formControl]="query" placeholder="Search" />
    <select [formControl]="status">
      <option value="all">All</option>
      <option value="open">Open</option>
      <option value="done">Done</option>
    </select>

    <p>{{ summary$ | async }}</p>
  `
})
export class FilterSummaryComponent {
  query = new FormControl('');
  status = new FormControl('all');

  items = [
    { title: 'Learn RxJS', status: 'done' },
    { title: 'Build search', status: 'open' }
  ];

  summary$ = combineLatest([
    this.query.valueChanges.pipe(startWith('')),
    this.status.valueChanges.pipe(startWith('all'))
  ]).pipe(
    map(([query, status]) => {
      const q = (query || '').toLowerCase();
      const filtered = this.items.filter(item =>
        (status === 'all' || item.status === status) &&
        item.title.toLowerCase().includes(q)
      );
      return `Matching items: ${filtered.length}`;
    })
  );
}
```

This shows how multiple reactive inputs can produce one clean derived output.

### Best Practices

- Keep observable chains short and readable.
- Use `switchMap` for request-based flows.
- Use `combineLatest` when multiple inputs matter.
- Keep side effects at the edges.
- Prefer streams for async coordination.
- Do not force everything into RxJS if a simpler solution fits.

### Easy Challenges

- Build a search stream from one form control.
- Add a debounce to a text input stream.
- Combine two values into one summary.
- Transform input text before using it.
- Expose a stream result in the template.

### Medium Challenges

- Create a live filter for a list.
- Combine search and category streams.
- Trigger an API call when input changes.
- Add a loading state to a request stream.
- Refactor a component so it uses fewer subscriptions.

### Hard Challenges

- Build a live search page with async data.
- Create a route-driven detail loader.
- Combine several stream sources into one dashboard summary.
- Refactor nested async logic into operator chains.
- Build a reactive form summary that updates automatically.

### Reflection Questions

- Why do reactive patterns help Angular apps scale?
- When is a stream better than a method?
- Why should subscriptions be kept at the edges?
- How do operators improve readability?
- When is a simpler non-RxJS solution better?

### Day Deliverable

Create one component or service that includes:

- At least one source stream.
- One derived stream.
- One combined or request-driven flow.
- Minimal manual subscription logic.
- A template or consumer that uses the final result.

### Suggested Practice Flow

1. Pick one changing input.
2. Turn it into a stream.
3. Add operators to shape the result.
4. Combine it with another source if needed.
5. Bind the final output to the UI.
6. Complete the easy, medium, and hard challenges.

### Note for Day 32

Next day will cover state management basics, which will help you decide when to keep state local and when to centralize it.
