# Day 23 — Core Operators: `map`, `tap`, `filter`

Day 23 is about the most common RxJS operators you’ll use in Angular. These operators help you transform values, inspect values, and keep only the values you want inside a stream [web:237][web:234][web:239].

## Goal

By the end of this day, you should be able to:

- Use `map` to transform values.
- Use `tap` to inspect values without changing them.
- Use `filter` to remove unwanted values.
- Chain operators with `pipe()`.
- Understand how operators simplify stream logic.
- Apply these operators in Angular components and services.

## Why This Matters

RxJS operators are what make streams powerful. Instead of manually transforming values after subscription, you can shape the data directly inside the pipeline [web:237][web:234].

They help you:
- Keep logic readable.
- Avoid repeated manual code.
- Build cleaner async flows.
- Separate transformation from consumption.

## `map`

`map` transforms each emitted value into something else.

```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(map(value => value * 10))
  .subscribe(value => console.log(value));
```

## `tap`

`tap` lets you observe values for logging or side effects without changing them. It is best used for debugging or other side effects, not for transforming the emitted value [web:234][web:239].

```ts
import { of } from 'rxjs';
import { tap } from 'rxjs/operators';

of('angular')
  .pipe(tap(value => console.log('value:', value)))
  .subscribe();
```

## `filter`

`filter` only lets matching values continue through the stream [web:237].

```ts
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(filter(value => value % 2 === 0))
  .subscribe(value => console.log(value));
```

## Practical Example

```ts
import { Component } from '@angular/core';
import { of } from 'rxjs';
import { map, tap, filter } from 'rxjs/operators';

@Component({
  selector: 'app-operator-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class OperatorDemoComponent {
  constructor() {
    of(1, 2, 3, 4, 5).pipe(
      tap(value => console.log('original:', value)),
      filter(value => value > 2),
      map(value => value * 2)
    ).subscribe(value => console.log('result:', value));
  }
}
```

## Best Practices

- Use `map` for transformation.
- Use `tap` for logging or debugging.
- Use `filter` for value selection.
- Keep operator chains short and clear.
- Prefer pipeline transforms over manual post-processing.

## Easy Challenges

- Multiply each emitted number by 2 with `map`.
- Log values using `tap`.
- Filter even numbers with `filter`.
- Chain two operators with `pipe()`.
- Create a stream that transforms strings.

## Medium Challenges

- Convert task titles to uppercase.
- Filter out short strings from a list.
- Use `tap` to inspect values before transformation.
- Build a number stream with two operators.
- Combine `map` and `filter` in one pipeline.

## Hard Challenges

- Build a search input pipeline with `filter` and `map`.
- Refactor manual data processing into operators.
- Create a task list stream with transformed labels.
- Use `tap` for debugging without changing values.
- Build a stream that cleans and formats user input.

## Reflection Questions

- What does `map` change?
- Why is `tap` useful?
- When should you use `filter`?
- Why are operators better than manual transforms?
- What does `pipe()` do?

## Day Deliverable

Create one observable pipeline that includes:

- `map`
- `tap`
- `filter`
- One subscription
- A clear transformed result

## Suggested Practice Flow

1. Start with a simple observable.
2. Add `tap`.
3. Add `filter`.
4. Add `map`.
5. Observe how the output changes.
6. Complete the easy, medium, and hard challenges.

## Note for Day 24

Next day covers **Higher-order operators: `switchMap`, `mergeMap`, `concatMap`**, which is the next step in building stronger RxJS pipelines.
