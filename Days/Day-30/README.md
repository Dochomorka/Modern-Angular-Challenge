## Day 30 — RxJS Operators and Stream Composition

Day 30 is about going beyond the basics and learning how to combine, filter, and coordinate observables. The focus is on the operators you’ll use most often in real Angular apps: transforming streams, combining values, and controlling async flows.

### Goal

By the end of this day, you should be able to:

- Use common RxJS operators.
- Filter and transform observable values.
- Combine multiple streams.
- Understand `switchMap` at a basic level.
- Build cleaner reactive data flows.
- Recognize when operators simplify your code.

### Why This Matters

Basic observables are useful, but real apps usually need more. You may want to search as the user types, combine route params with API calls, or react to several streams together.

Operators help you:
- Reduce manual state handling.
- Keep async logic readable.
- Work with multiple sources of data.
- Avoid nested subscriptions.

### Common Operators

Some of the most useful operators are:

- `map` — transform each value.
- `filter` — keep only matching values.
- `tap` — inspect values without changing them.
- `debounceTime` — wait before emitting.
- `switchMap` — switch to a new inner observable.
- `combineLatest` — merge the latest values from multiple streams.

### `filter`

Use `filter` when only certain values should continue through the stream.

```ts
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(filter(value => value % 2 === 0))
  .subscribe(value => console.log(value));
```

### `tap`

Use `tap` for debugging or side effects without changing the value.

```ts
import { of } from 'rxjs';
import { tap } from 'rxjs/operators';

of('angular')
  .pipe(tap(value => console.log('value:', value)))
  .subscribe();
```

### `switchMap`

Use `switchMap` when one stream should trigger another stream, such as search input triggering API calls.

```ts
import { of } from 'rxjs';
import { switchMap } from 'rxjs/operators';

of('search term')
  .pipe(
    switchMap(term => of(`Results for ${term}`))
  )
  .subscribe(result => console.log(result));
```

### Combining Streams

Sometimes you need values from multiple observables at once. `combineLatest` helps when the latest value from each source matters.

```ts
import { combineLatest, of } from 'rxjs';
import { map } from 'rxjs/operators';

const a$ = of(10);
const b$ = of(20);

combineLatest([a$, b$])
  .pipe(map(([a, b]) => a + b))
  .subscribe(total => console.log(total));
```

### Practical Example

```ts
import { Component } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { debounceTime, distinctUntilChanged, filter, map } from 'rxjs/operators';

@Component({
  selector: 'app-search-demo',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <input [formControl]="search" placeholder="Search tasks" />
    <p>Type at least 3 characters.</p>
  `
})
export class SearchDemoComponent {
  search = new FormControl('');

  constructor() {
    this.search.valueChanges.pipe(
      filter((value): value is string => !!value && value.length >= 3),
      debounceTime(300),
      distinctUntilChanged(),
      map(value => value.toUpperCase())
    ).subscribe(value => console.log('Search:', value));
  }
}
```

This example shows how operators help clean up user input before using it.

### Best Practices

- Use operators to keep logic readable.
- Avoid nested subscriptions when possible.
- Prefer `switchMap` for request-driven streams.
- Use `tap` for logging or debugging.
- Add `debounceTime` to input-driven streams.
- Keep stream transformations close to the source.

### Easy Challenges

- Filter an observable to keep only even numbers.
- Transform strings to uppercase with `map`.
- Log values with `tap`.
- Create a stream that waits before emitting.
- Subscribe to a filtered stream.

### Medium Challenges

- Build a search input with `debounceTime`.
- Combine two streams and compute a result.
- Use `switchMap` to chain one stream into another.
- Remove duplicate values with `distinctUntilChanged`.
- Create a form value stream and transform it.

### Hard Challenges

- Build a live search flow.
- Combine route data and API data.
- Refactor nested subscriptions into operator chains.
- Add loading behavior to a stream-driven request.
- Build a multi-stream summary component.

### Reflection Questions

- Why are operators important in RxJS?
- When should you use `switchMap`?
- Why is `tap` useful?
- How does `combineLatest` help with multiple sources?
- Why should nested subscriptions be avoided?

### Day Deliverable

Create one RxJS example that includes:

- At least three operators.
- One filtered stream.
- One transformed stream.
- One stream combination or switch.
- A simple subscription or observable output.

### Suggested Practice Flow

1. Start with a simple stream.
2. Add one operator at a time.
3. Test filtering and transforming.
4. Try combining streams.
5. Build one input-driven example.
6. Complete the easy, medium, and hard challenges.

### Note for Day 31

Next day will cover higher-level reactive patterns and how to use streams more cleanly in Angular components and services.
