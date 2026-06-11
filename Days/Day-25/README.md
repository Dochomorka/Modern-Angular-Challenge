# Day 25 — Combination Operators: `combineLatest`, `forkJoin`, `zip`

Day 25 is about combining multiple observables into one result. RxJS describes combination operators as a way to join information from multiple observables, and the main difference between these three is how they handle timing, order, and completion [web:256][web:252][web:237].

## Goal

By the end of this day, you should be able to:

- Combine multiple streams into one.
- Understand when each stream emits.
- Use `combineLatest` for live combined updates.
- Use `forkJoin` for one-time completion results.
- Use `zip` for paired, ordered emissions.
- Choose the right combination operator for a task.

## Why This Matters

Angular apps often need data from more than one source at the same time. You might combine form inputs, multiple API calls, or several UI streams into one response [web:256][web:252].

These operators help you:
- Coordinate multiple async values.
- Avoid manual synchronization logic.
- Keep code declarative.
- Match behavior to the business requirement.

## `combineLatest`

`combineLatest` waits until each input has emitted at least once, then emits a new combined value whenever any input changes, using the latest value from each stream [web:252][web:255].

Use it when:
- You want live updates.
- Any change should refresh the result.
- You are combining form controls or reactive UI state.

```ts
import { combineLatest, of } from 'rxjs';
import { map } from 'rxjs/operators';

combineLatest([of('A'), of('B')])
  .pipe(map(([a, b]) => `${a}-${b}`))
  .subscribe(console.log);
```

## `forkJoin`

`forkJoin` waits for all input observables to complete, then emits the last value from each one. It will not emit if one observable never completes [web:252][web:255][web:259].

Use it when:
- You need final results only.
- You are combining one-time HTTP requests.
- Completion matters more than intermediate values.

```ts
import { forkJoin, of } from 'rxjs';

forkJoin([of('user'), of('settings')])
  .subscribe(console.log);
```

## `zip`

`zip` combines values by index, pairing the first with the first, the second with the second, and so on. It waits until each source has the matching value for the next pair [web:252][web:260].

Use it when:
- You want strict pairing.
- Value order matters.
- You need synchronized emissions.

```ts
import { zip, of } from 'rxjs';

zip([of(1, 2), of('a', 'b')])
  .subscribe(console.log);
```

## Practical Example

```ts
import { Component } from '@angular/core';
import { combineLatest, forkJoin, zip, of } from 'rxjs';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-combine-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class CombineDemoComponent {
  constructor() {
    combineLatest([of('John'), of('Doe')])
      .pipe(map(([first, last]) => `${first} ${last}`))
      .subscribe(console.log);

    forkJoin([of('profile'), of('preferences')])
      .subscribe(console.log);

    zip([of(1, 2), of('A', 'B')])
      .subscribe(console.log);
  }
}
```

## Choosing the Right Operator

| Operator | When it emits | Best use |
|---|---|---|
| `combineLatest` | After each source has emitted once, then on any new emission | Live UI state, forms, reactive dashboards |
| `forkJoin` | After all sources complete | HTTP calls, final combined results |
| `zip` | When each source has the next matching value | Paired records, synchronized sequences |

## Best Practices

- Use `combineLatest` for continuously updating views.
- Use `forkJoin` for one-shot completion-based work.
- Use `zip` when matching by position matters.
- Remember that `forkJoin` needs completion.
- Prefer the operator whose timing behavior matches your task.

## Easy Challenges

- Combine two strings with `combineLatest`.
- Join two HTTP-like observables with `forkJoin`.
- Pair two number streams with `zip`.
- Observe when each operator emits.
- Compare live vs final-only behavior.

## Medium Challenges

- Build a full name stream from first and last name controls.
- Combine two API requests and show the final result.
- Pair numbers and letters in order with `zip`.
- Refactor manual merging into one operator.
- Use `combineLatest` with three observables.

## Hard Challenges

- Create a form preview with `combineLatest`.
- Simulate a dashboard with several live streams.
- Build a completion-based data load with `forkJoin`.
- Compare `zip` and `combineLatest` side by side.
- Combine multiple streams and explain why each operator fits.

## Reflection Questions

- Why does `combineLatest` wait for an initial value from each stream?
- Why does `forkJoin` need completion?
- How is `zip` different from `combineLatest`?
- Which operator fits live UI updates best?
- Which one is safest for one-time API calls?

## Day Deliverable

Create one example that includes:

- One `combineLatest` flow.
- One `forkJoin` flow.
- One `zip` flow.
- A clear explanation of when each is used.
- One Angular-style scenario.

## Suggested Practice Flow

1. Start with two simple observables.
2. Test `combineLatest`.
3. Test `forkJoin`.
4. Test `zip`.
5. Compare timing and output.
6. Complete the easy, medium, and hard challenges.

## Note for Day 26

Next day covers **Error Handling: `catchError`, `retry`, `throwError`**, which is the next step in making RxJS streams resilient.
