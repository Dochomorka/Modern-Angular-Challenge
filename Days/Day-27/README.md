# Day 27 — Subjects and Multicasting

Day 27 is about Subjects and how they let one stream be shared across multiple subscribers. In RxJS, a Subject is a special kind of observable that multicasts values, meaning one execution can feed many observers [web:278][web:280][web:271].

## Goal

By the end of this day, you should be able to:

- Explain what a Subject is.
- Understand multicasting.
- Use `Subject` for shared events.
- Use `BehaviorSubject` for current state.
- Use `ReplaySubject` for replaying recent values.
- Use `AsyncSubject` for final-value delivery.

## Why This Matters

Plain observables are often unicast, so each subscriber gets its own execution. Subjects solve the shared-stream problem by broadcasting the same values to multiple subscribers [web:274][web:278][web:271].

This matters because:
- Multiple components may need the same data.
- You may want to share UI events.
- State often needs to be read by late subscribers.
- Shared execution avoids duplicated work.

## `Subject`

A `Subject` is both an observable and an observer. It does not store old values, so new subscribers only receive future emissions [web:278][web:272][web:280].

```ts
import { Subject } from 'rxjs';

const subject = new Subject<string>();

subject.subscribe(value => console.log('A:', value));
subject.next('hello');

subject.subscribe(value => console.log('B:', value));
subject.next('world');
```

## `BehaviorSubject`

A `BehaviorSubject` stores the latest value and emits it immediately to new subscribers. It also requires an initial value [web:272][web:278][web:275].

Use it when:
- You need current state.
- Late subscribers should see the latest value.
- You are modeling app state, auth status, or selected items.

```ts
import { BehaviorSubject } from 'rxjs';

const state$ = new BehaviorSubject<string>('idle');

state$.subscribe(value => console.log('A:', value));
state$.next('loading');

state$.subscribe(value => console.log('B:', value));
```

## `ReplaySubject`

A `ReplaySubject` replays a configurable number of previous values to new subscribers [web:272][web:278][web:275].

Use it when:
- New subscribers need past events.
- You want to keep recent history.
- You are modeling chat messages or event logs.

```ts
import { ReplaySubject } from 'rxjs';

const replay$ = new ReplaySubject<string>(2);

replay$.next('one');
replay$.next('two');
replay$.next('three');

replay$.subscribe(value => console.log(value));
```

## `AsyncSubject`

An `AsyncSubject` emits only the last value, and only after completion [web:272][web:278][web:275].

Use it when:
- Only the final result matters.
- You have a one-time process.
- You want a completion-based summary value.

## Practical Example

```ts
import { Component } from '@angular/core';
import { Subject, BehaviorSubject } from 'rxjs';

@Component({
  selector: 'app-subject-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class SubjectDemoComponent {
  event$ = new Subject<string>();
  state$ = new BehaviorSubject<string>('initial');

  constructor() {
    this.event$.subscribe(value => console.log('event A:', value));
    this.state$.subscribe(value => console.log('state A:', value));

    this.event$.next('clicked');
    this.state$.next('ready');

    this.event$.subscribe(value => console.log('event B:', value));
    this.state$.subscribe(value => console.log('state B:', value));

    this.event$.next('submitted');
  }
}
```

## Choosing the Right Type

| Type | Stores previous values? | New subscriber gets current value? | Best use |
|---|---|---|---|
| `Subject` | No | No | Events, notifications |
| `BehaviorSubject` | Latest only | Yes | State, current value |
| `ReplaySubject` | Yes, configurable | Yes | History, late subscribers |
| `AsyncSubject` | Only final value | Only on completion | Final result after completion |

## Best Practices

- Use `Subject` for events, not state.
- Use `BehaviorSubject` for shared state that needs a current value.
- Use `ReplaySubject` only when replay is really needed.
- Use `AsyncSubject` for completion-based outputs.
- Be careful not to expose subjects directly when a read-only observable is enough.

## Easy Challenges

- Create a `Subject` and send three values.
- Add two subscribers to one subject.
- Try a `BehaviorSubject` with an initial value.
- Replay the last two values with `ReplaySubject`.
- Complete an `AsyncSubject` and observe the output.

## Medium Challenges

- Build a shared event bus with `Subject`.
- Build a loading state with `BehaviorSubject`.
- Build a recent notifications stream with `ReplaySubject`.
- Show how late subscribers behave differently.
- Compare all four subject types in one file.

## Hard Challenges

- Create a shared Angular service with `BehaviorSubject`.
- Build a component communication pattern with `Subject`.
- Model a cached message stream with `ReplaySubject`.
- Use `AsyncSubject` for a one-time completion result.
- Refactor a manual state variable into a subject-based stream.

## Reflection Questions

- Why is a Subject multicasting?
- Why does `BehaviorSubject` need an initial value?
- When should you prefer `ReplaySubject` over `BehaviorSubject`?
- Why does `AsyncSubject` wait for completion?
- What problem does multicasting solve in Angular?

## Day Deliverable

Create one example that includes:

- One `Subject`.
- One `BehaviorSubject`.
- One `ReplaySubject` or `AsyncSubject`.
- Two subscribers to the same stream.
- A short note on when each should be used.

## Suggested Practice Flow

1. Create a plain `Subject`.
2. Add multiple subscribers.
3. Compare with `BehaviorSubject`.
4. Test `ReplaySubject`.
5. Observe `AsyncSubject`.
6. Complete the easy, medium, and hard challenges.

## Note for Day 28

Next day covers **Schedulers and Performance**, which is the next step in understanding how RxJS work is scheduled and executed.
