# Day 26 — Error Handling: `catchError`, `retry`, `throwError`

Day 26 is about handling failures in RxJS streams so your Angular app stays predictable and resilient. `catchError` lets you replace or transform an error, `retry` lets you try again, and `throwError` creates an observable that emits an error signal [web:262][web:263][web:265].

## Goal

By the end of this day, you should be able to:

- Catch stream errors safely.
- Retry failed observables a limited number of times.
- Create explicit error observables with `throwError`.
- Return fallback values when needed.
- Decide when to retry and when to stop.
- Keep user-facing flows stable during failures.

## Why This Matters

Network calls, validation, and user-driven workflows can fail at any time. RxJS error handling helps you recover gracefully instead of breaking the entire stream [web:262][web:265][web:270].

This matters because:
- Users should see a fallback instead of a crash.
- Retry can recover from temporary failures.
- Explicit errors help with validation and branching.
- Better handling makes debugging easier.

## `catchError`

`catchError` intercepts an error and lets you return a replacement observable or rethrow the error [web:262][web:270].

```ts
import { of, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

of(1).pipe(
  catchError(err => of('fallback'))
).subscribe(console.log);
```

Use it when:
- You want a fallback value.
- You want to show a friendly message.
- You need to recover from a failed request.

## `retry`

`retry` resubscribes to the source observable when an error occurs, up to the number of times you specify [web:264][web:270][web:262].

```ts
import { throwError } from 'rxjs';
import { retry } from 'rxjs/operators';

throwError(() => new Error('fail')).pipe(
  retry(2)
).subscribe({
  error: err => console.log('still failed')
});
```

Use it when:
- Failures may be temporary.
- A second attempt might succeed.
- You want automatic recovery before handling the error.

## `throwError`

`throwError` creates an observable that immediately emits an error notification upon subscription. In modern RxJS, it is recommended to pass a factory function [web:263].

```ts
import { throwError } from 'rxjs';

throwError(() => new Error('Invalid input')).subscribe({
  error: err => console.log(err.message)
});
```

Use it when:
- You need to return an observable that fails.
- You are inside operators like `switchMap` or `mergeMap`.
- You want to signal a validation or business-rule failure [web:263].

## Practical Example

```ts
import { Component } from '@angular/core';
import { of, throwError } from 'rxjs';
import { catchError, retry, switchMap } from 'rxjs/operators';

@Component({
  selector: 'app-error-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class ErrorDemoComponent {
  constructor() {
    of('load').pipe(
      switchMap(() => throwError(() => new Error('Request failed'))),
      retry(2),
      catchError(() => of('fallback data'))
    ).subscribe(console.log);
  }
}
```

## Operator Roles

| Operator | What it does | Best use |
|---|---|---|
| `catchError` | Handles an error and returns a new observable | Fallbacks, friendly recovery |
| `retry` | Resubscribes after failure | Temporary network issues |
| `throwError` | Produces an error observable | Validation, explicit failure |

## Best Practices

- Retry only when recovery is plausible.
- Use `catchError` to keep the stream alive.
- Use `throwError` when you need to fail inside a pipe.
- Return a fallback value when user experience matters.
- Keep error handling close to the failing operation.

## Easy Challenges

- Catch an error and return a fallback string.
- Retry a failing observable twice.
- Emit a custom error with `throwError`.
- Log an error before returning fallback data.
- Compare retry success vs retry failure.

## Medium Challenges

- Wrap an HTTP-like request with `catchError`.
- Add `retry(3)` before `catchError`.
- Create a validation rule using `throwError`.
- Show fallback UI data after failure.
- Simulate a request that succeeds on retry.

## Hard Challenges

- Build a request pipeline with retry and fallback.
- Use `throwError` inside `switchMap`.
- Separate user-facing errors from technical errors.
- Compare retry-first vs catch-first behavior.
- Create a reusable error-handling pattern.

## Reflection Questions

- When should you retry instead of failing immediately?
- What does `catchError` return?
- Why use `throwError` inside a pipe?
- What happens if an observable never recovers?
- How should fallback data affect the UI?

## Day Deliverable

Create one example that includes:

- One `catchError` flow.
- One `retry` flow.
- One `throwError` flow.
- One fallback value.
- One realistic Angular use case.

## Suggested Practice Flow

1. Create a failing observable.
2. Add `retry`.
3. Add `catchError`.
4. Use `throwError` for explicit failure.
5. Test fallback behavior.
6. Complete the easy, medium, and hard challenges.

## Note for Day 27

Next day covers **Subjects and Multicasting**, which is the next step in sharing values across multiple subscribers.
