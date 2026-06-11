# Day 21 — Observables and Streams

Day 21 is about understanding observables, the foundation of RxJS in Angular. Angular uses observables to handle common asynchronous operations like HTTP requests, events, and other values that arrive over time [web:208][web:222][web:211].

## Goal

By the end of this day, you should be able to:

- Understand what an observable is.
- Recognize a stream of values over time.
- Subscribe to an observable.
- Understand why Angular uses observables.
- Create a simple observable.
- Use observables for async values.

## Why This Matters

Observables let Angular work naturally with data that changes or arrives later. That makes them a strong fit for HTTP, events, route changes, and other reactive UI behavior [web:208][web:222][web:211].

They help you:
- Work with async data.
- Represent changing values.
- Build reactive flows.
- Keep code composable.

## Basic Idea

An observable is a stream that can emit zero, one, or many values over time. A subscription is what starts listening to that stream [web:211][web:219].

## Example

```ts
import { Observable } from 'rxjs';

const names$ = new Observable<string>(subscriber => {
  subscriber.next('Angular');
  subscriber.next('RxJS');
  subscriber.complete();
});

names$.subscribe(value => console.log(value));
```

## Why Angular Uses Them

Angular uses observables because many common app behaviors are asynchronous:
- HTTP responses.
- Route changes.
- Input changes.
- Timer events.
- Shared app state [web:208][web:222][web:211].

## Practical Example

```ts
import { Component } from '@angular/core';
import { of } from 'rxjs';

@Component({
  selector: 'app-stream-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class StreamDemoComponent {
  constructor() {
    of('one', 'two', 'three').subscribe(value => console.log(value));
  }
}
```

## Best Practices

- Use observables for async or changing values.
- Subscribe when you need the emitted values.
- Keep streams small and readable.
- Prefer declarative stream handling where possible.
- Avoid treating observables like plain values.

## Easy Challenges

- Create an observable with three values.
- Subscribe and log each value.
- Use `of()` to make a simple stream.
- Observe a timer or interval.
- Create a stream from a basic string list.

## Medium Challenges

- Build a stream that emits task titles.
- Turn a number stream into uppercase text.
- Subscribe inside a component method.
- Compare observable output to a plain array.
- Create a stream from a changing value source.

## Hard Challenges

- Build a stream that models user typing.
- Use an observable for a small async workflow.
- Create a reactive value pipeline.
- Refactor manual state updates into a stream.
- Represent a changing list as an observable.

## Reflection Questions

- What is a stream of values?
- Why does Angular use observables so often?
- What starts an observable?
- How is a stream different from a plain variable?
- When is an observable the right choice?

## Day Deliverable

Create one observable example that includes:

- One stream.
- One subscription.
- A clear emitted output.
- A small real use case.
- An understanding of async flow.

## Suggested Practice Flow

1. Create one observable.
2. Subscribe to it.
3. Test its output.
4. Make a second stream.
5. Compare direct values and streams.
6. Complete the easy, medium, and hard challenges.

## Note for Day 22

Next day covers **Subscriptions and cleanup**, which is about managing active streams safely.
