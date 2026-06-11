# Day 22 — Subscriptions and Cleanup

Day 22 is about managing RxJS subscriptions safely in Angular. Some observables complete on their own, but long-lived streams often need explicit cleanup to avoid memory leaks; Angular also offers `takeUntilDestroyed` as a concise way to auto-unsubscribe when a component or directive is destroyed [web:225][web:205][web:232].

## Goal

By the end of this day, you should be able to:

- Understand what a subscription is.
- Know when cleanup is needed.
- Unsubscribe from long-lived streams.
- Use `ngOnDestroy` for manual cleanup.
- Recognize when cleanup is not necessary.
- Use modern Angular cleanup helpers when appropriate.

## Why This Matters

A subscription keeps listening to a stream. If that stream keeps running after a component is gone, you can waste resources or create memory leaks [web:205][web:226][web:232].

This matters because:
- Components are created and destroyed often.
- Timers and event streams may never stop by themselves.
- Cleanup keeps apps healthy.
- It prevents hidden bugs over time.

## Basic Idea

Subscribing starts listening to the observable. Cleanup stops that listening when the component no longer needs the stream [web:205][web:225].

## Manual Cleanup Example

```ts
import { Component, OnDestroy } from '@angular/core';
import { interval, Subscription } from 'rxjs';

@Component({
  selector: 'app-sub-demo',
  standalone: true,
  template: `<p>Open the console</p>`
})
export class SubDemoComponent implements OnDestroy {
  sub: Subscription;

  constructor() {
    this.sub = interval(1000).subscribe(value => console.log(value));
  }

  ngOnDestroy() {
    this.sub.unsubscribe();
  }
}
```

## Using `takeUntilDestroyed`

Angular provides `takeUntilDestroyed` in `@angular/core/rxjs-interop` to automatically unsubscribe when a component or directive is destroyed [web:225].

```ts
import { Component, inject } from '@angular/core';
import { interval } from 'rxjs';
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';

@Component({
  selector: 'app-auto-cleanup',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class AutoCleanupComponent {
  constructor() {
    interval(1000)
      .pipe(takeUntilDestroyed())
      .subscribe(value => console.log(value));
  }
}
```

## When Cleanup Matters

Cleanup is important when:
- The observable does not complete automatically.
- The stream is long-lived, like a timer or event stream.
- The component may be destroyed and recreated.
- You are manually subscribing in component code [web:225][web:205][web:232].

## When Cleanup May Not Be Needed

Some observables complete naturally, such as many `HttpClient` requests. In those cases, explicit manual unsubscribe is usually not required [web:232][web:205].

## Best Practices

- Unsubscribe from long-lived streams.
- Use `ngOnDestroy` when manual cleanup is appropriate.
- Use `takeUntilDestroyed` when you want a simpler cleanup pattern.
- Keep subscriptions obvious and organized.
- Avoid leaving event listeners or timers active after the component is destroyed.

## Easy Challenges

- Subscribe to an interval and unsubscribe.
- Track one subscription in a variable.
- Clean up a click stream.
- Identify a stream that completes automatically.
- Add destroy logic to a demo component.

## Medium Challenges

- Manage two subscriptions in one component.
- Separate one-time and long-lived streams.
- Clean up a form event stream.
- Use a timer and stop it safely.
- Refactor subscription logic for clarity.

## Hard Challenges

- Build a component with multiple active streams.
- Avoid leaks in a long-lived event listener.
- Compare manual cleanup with automatic completion.
- Create a pattern for tracking several subscriptions.
- Refactor cleanup logic into a reusable approach.

## Reflection Questions

- What is a subscription?
- Why do some streams need cleanup?
- Which kinds of streams usually complete on their own?
- What happens if you forget to unsubscribe?
- How does `takeUntilDestroyed` help?

## Day Deliverable

Create one component that includes:

- At least one live subscription.
- A cleanup step.
- A long-lived stream.
- A clear reason for unsubscribing.
- Safe component teardown.

## Suggested Practice Flow

1. Create one long-lived observable.
2. Subscribe to it.
3. Add cleanup logic.
4. Test the behavior.
5. Compare manual cleanup with `takeUntilDestroyed`.
6. Complete the easy, medium, and hard challenges.

## Note for Day 23

Next day covers **Core operators: `map`, `tap`, `filter`**, which is the next step in building readable RxJS pipelines.
