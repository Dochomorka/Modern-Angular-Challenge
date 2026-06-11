# Day 28 — Schedulers and Performance

Day 28 is about how RxJS controls when work runs and how notifications are delivered. A scheduler manages subscription timing and notification delivery, and RxJS documents schedulers as tools for controlling execution context and time [web:287][web:281][web:282].

## Goal

By the end of this day, you should be able to:

- Explain what a scheduler does.
- Understand why timing matters in RxJS.
- Recognize `queueScheduler`, `asapScheduler`, `asyncScheduler`, and `animationFrameScheduler`.
- Use schedulers to improve responsiveness.
- Distinguish synchronous from scheduled execution.
- Know when scheduler choice affects performance.

## Why This Matters

Schedulers help you control performance and execution order in Angular and RxJS apps. They matter when you want to defer work, smooth rendering, or avoid blocking the UI thread [web:282][web:285][web:287].

This matters because:
- Large synchronous work can hurt responsiveness.
- Some updates should happen before paint.
- Animation work should align with browser frames.
- Timing differences can change app behavior.

## What A Scheduler Does

A scheduler controls when subscription starts and when notifications are delivered. In practice, it decides whether work happens immediately, later in the task queue, in a microtask, or before the next repaint [web:287][web:282][web:286].

## Main Schedulers

| Scheduler | Timing behavior | Common use |
|---|---|---|
| `queueScheduler` | Runs in a queued synchronous order | Recursive or ordered work |
| `asapScheduler` | Runs after current sync code, before macrotasks | High-priority deferred work |
| `asyncScheduler` | Uses timed async execution | Delayed tasks, timers, throttling |
| `animationFrameScheduler` | Runs before browser repaint | Smooth animations and visual updates |

These descriptions are consistent with RxJS scheduler references and performance guides [web:282][web:287][web:286].

## `queueScheduler`

`queueScheduler` queues work and executes it synchronously in order. It is useful when you want predictable sequencing without jumping to a later event loop turn [web:282][web:286].

```ts
import { of, queueScheduler } from 'rxjs';

of(1, 2, 3, queueScheduler).subscribe(console.log);
```

## `asapScheduler`

`asapScheduler` schedules work as soon as possible after the current synchronous work finishes. It is often described as microtask-like behavior [web:282][web:286].

Use it when:
- You want to defer slightly.
- The update should happen very soon.
- You want to avoid blocking the current call stack.

## `asyncScheduler`

`asyncScheduler` schedules work asynchronously, similar to timer-based execution. It is useful for delayed work, debouncing patterns, and tasks that do not need to happen immediately [web:282][web:285].

Use it when:
- You need a real async delay.
- You are working with timers.
- You want to move work off the current synchronous flow.

## `animationFrameScheduler`

`animationFrameScheduler` aligns work with the browser’s repaint cycle. That makes it a strong choice for UI updates and smooth animations [web:282][web:285][web:286].

Use it when:
- You are animating elements.
- You want frame-aligned rendering.
- You need smoother visual updates.

## Practical Example

```ts
import { Component } from '@angular/core';
import { of, asyncScheduler, animationFrameScheduler } from 'rxjs';

@Component({
  selector: 'app-scheduler-demo',
  standalone: true,
  template: `<p>Check the console</p>`
})
export class SchedulerDemoComponent {
  constructor() {
    of('sync').subscribe(console.log);
    of('async', asyncScheduler).subscribe(console.log);
    of('frame', animationFrameScheduler).subscribe(console.log);
  }
}
```

## Performance Tips

- Use scheduling to avoid blocking the UI.
- Prefer `animationFrameScheduler` for visual updates.
- Use `asyncScheduler` for deferred background work.
- Keep synchronous chains short when data volume is large.
- Choose the scheduler that matches the timing you actually need.

## Easy Challenges

- Compare synchronous and scheduled emissions.
- Use `asyncScheduler` in a small example.
- Test the order of `queueScheduler` and `asyncScheduler`.
- Emit one value on `animationFrameScheduler`.
- Observe how timing changes output.

## Medium Challenges

- Create a delayed stream with `asyncScheduler`.
- Build a frame-based visual update example.
- Compare `asapScheduler` and `asyncScheduler`.
- Move heavy work out of the immediate call stack.
- Explore how scheduler choice changes operator behavior.

## Hard Challenges

- Build a UI update example with `animationFrameScheduler`.
- Compare multiple schedulers in one demo.
- Refactor a synchronous stream to reduce blocking.
- Measure how scheduling affects perceived responsiveness.
- Design a stream that balances timing and performance.

## Reflection Questions

- What does a scheduler control?
- Why can synchronous execution hurt performance?
- When is `animationFrameScheduler` the best fit?
- How is `asyncScheduler` different from `queueScheduler`?
- Why does scheduler choice matter in Angular UIs?

## Day Deliverable

Create one example that includes:

- One synchronous emission.
- One scheduled emission.
- One animation-aligned emission.
- A note on performance impact.
- One Angular-style use case.

## Suggested Practice Flow

1. Start with a synchronous observable.
2. Add `queueScheduler`.
3. Add `asyncScheduler`.
4. Add `animationFrameScheduler`.
5. Compare timing and behavior.
6. Complete the easy, medium, and hard challenges.

## Note for Day 29

Next day covers **Custom Operators**, which is the next step in packaging reusable RxJS behavior.
