# Day 24 — Higher-order Operators: `switchMap`, `mergeMap`, `concatMap`

Day 24 is about higher-order mapping operators, which are used when one observable produces another observable. RxJS documents these as flattening operators for higher-order observables, and Angular apps use them frequently for API calls, search, and reactive workflows [web:246][web:248][web:243].

## Goal

By the end of this day, you should be able to:

- Understand what a higher-order observable is.
- Know why `switchMap`, `mergeMap`, and `concatMap` exist.
- Use `switchMap` for “latest value wins” flows.
- Use `mergeMap` for parallel flows.
- Use `concatMap` for ordered sequential flows.
- Choose the right operator for a task.

## Why This Matters

Real Angular apps often need to turn one stream into another stream. A search box might trigger API calls, a save queue might need sequential requests, and a dashboard might need multiple independent requests at once [web:246][web:245][web:238].

These operators help you:
- Avoid nested subscriptions.
- Control concurrency.
- Preserve order when needed.
- Cancel stale requests when the latest one matters.

## Higher-order Idea

A higher-order observable is an observable that emits other observables. The mapping operators then “flatten” those inner observables into one output stream [web:246][web:248].

## `switchMap`

`switchMap` keeps only the latest inner observable. When a new source value arrives, it unsubscribes from the previous inner stream and switches to the new one [web:246][web:245].

Use it when:
- Only the latest result matters.
- You are building search/autocomplete.
- You want to cancel stale requests.

```ts
import { of } from 'rxjs';
import { switchMap } from 'rxjs/operators';

of('a', 'ab', 'abc').pipe(
  switchMap(value => of(`Result for ${value}`))
).subscribe(console.log);
```

## `mergeMap`

`mergeMap` subscribes to all inner observables and merges their results as they arrive. It does not guarantee order, so it is useful when tasks are independent and can run in parallel [web:246][web:245].

Use it when:
- Requests can run independently.
- Parallel behavior is fine.
- You want all results, not just the latest.

```ts
import { of } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

of(1, 2, 3).pipe(
  mergeMap(value => of(`Item ${value}`))
).subscribe(console.log);
```

## `concatMap`

`concatMap` processes inner observables sequentially, waiting for one to complete before starting the next. RxJS docs note that it behaves like `mergeMap` with concurrency set to 1 [web:248][web:246].

Use it when:
- Order matters.
- Requests must run one after another.
- You are building a queue or save chain.

```ts
import { of } from 'rxjs';
import { concatMap } from 'rxjs/operators';

of(1, 2, 3).pipe(
  concatMap(value => of(`Saved ${value}`))
).subscribe(console.log);
```

## Practical Example

```ts
import { Component } from '@angular/core';
import { of } from 'rxjs';
import { switchMap, mergeMap, concatMap } from 'rxjs/operators';

@Component({
  selector: 'app-higher-order-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class HigherOrderDemoComponent {
  constructor() {
    of('search').pipe(
      switchMap(term => of(`Latest only: ${term}`))
    ).subscribe(console.log);

    of(1, 2, 3).pipe(
      mergeMap(value => of(`Parallel: ${value}`))
    ).subscribe(console.log);

    of(1, 2, 3).pipe(
      concatMap(value => of(`Sequential: ${value}`))
    ).subscribe(console.log);
  }
}
```

## Choosing the Right Operator

| Operator | Behavior | Best Use |
|---|---|---|
| `switchMap` | Cancels the previous inner stream | Search, autocomplete, latest-only results |
| `mergeMap` | Runs inner streams in parallel | Independent requests, parallel actions |
| `concatMap` | Runs inner streams in order | Queues, ordered saves, sequential tasks |

## Best Practices

- Use `switchMap` for requests driven by fast-changing input.
- Use `mergeMap` when work can happen in parallel.
- Use `concatMap` when order matters.
- Avoid nested subscriptions where a higher-order operator fits.
- Match the operator to the behavior you want, not just the syntax.

## Easy Challenges

- Map one value to one inner observable with `switchMap`.
- Run two independent inner streams with `mergeMap`.
- Queue three values with `concatMap`.
- Compare the output order of each operator.
- Use each operator once in a small script.

## Medium Challenges

- Build a search flow with `switchMap`.
- Simulate parallel saves with `mergeMap`.
- Simulate ordered saves with `concatMap`.
- Refactor nested subscriptions into one operator chain.
- Explain when each operator should be used.

## Hard Challenges

- Build a live search example with cancelable requests.
- Build a parallel request demo with merged results.
- Build a sequential task queue with `concatMap`.
- Compare operator behavior in one component.
- Combine higher-order mapping with other operators from Day 23.

## Reflection Questions

- What is a higher-order observable?
- Why does `switchMap` cancel previous streams?
- When is `mergeMap` the right choice?
- Why does `concatMap` preserve order?
- How do these operators help avoid nested subscriptions?

## Day Deliverable

Create one example that includes:

- One `switchMap` flow.
- One `mergeMap` flow.
- One `concatMap` flow.
- A clear explanation of when each is used.
- One real Angular-style use case.

## Suggested Practice Flow

1. Start with a simple outer observable.
2. Map it to inner observables.
3. Test `switchMap`.
4. Test `mergeMap`.
5. Test `concatMap`.
6. Compare their output and order.

## Note for Day 25

Next day covers **Combination Operators: `combineLatest`, `forkJoin`, `zip`**, which are used when you need to work with multiple streams together.
