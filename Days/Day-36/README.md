# Day 36 — Performance Optimization

Day 36 is about making Angular apps faster to load and more responsive after they load. Angular’s performance guides highlight techniques like lazy-loaded routes, `@defer`, `OnPush`, image optimization, SSR, hydration, and profiling with Angular DevTools or Chrome DevTools [web:368][web:371][web:374].

## Goal

By the end of this day, you should be able to:

- Recognize loading vs runtime performance issues.
- Use lazy loading to reduce initial bundle size.
- Understand `@defer` for on-demand UI loading.
- Use `OnPush` to reduce unnecessary change detection.
- Spot slow computations in templates or hooks.
- Profile the app before optimizing.

## Why This Matters

Performance affects both user experience and Core Web Vitals such as Largest Contentful Paint and Time to First Byte. Angular’s docs emphasize that you should profile first, then optimize the specific bottleneck rather than guessing [web:368][web:369][web:371].

This matters because:
- Faster apps feel better to use.
- Better loading can improve SEO and conversion.
- Less change detection can improve responsiveness.
- The right optimization depends on the problem.

## Loading Performance

Loading performance is about how quickly the app becomes visible and interactive. Angular recommends tactics like lazy-loaded routes, `@defer`, image optimization, and server-side rendering for improving initial load [web:368][web:374][web:369].

Common loading optimizations:
- Lazy-loaded routes.
- Deferred component loading with `@defer`.
- Image optimization with `NgOptimizedImage`.
- Server-side rendering.
- Hydration and incremental hydration.

## Runtime Performance

Runtime performance is about how responsive the app feels after it loads. Angular’s runtime guidance focuses on change detection, slow computations, and skipping unchanged component subtrees with `OnPush` or zoneless approaches [web:371][web:368][web:369].

Common runtime optimizations:
- Use `OnPush` where appropriate.
- Avoid expensive work in templates.
- Reduce unnecessary change detection.
- Watch for zone pollution.
- Move heavy computations out of hot paths.

## Profiling First

Angular recommends profiling before optimizing so you know where the bottleneck is. Angular DevTools and Chrome DevTools help identify repeated change detection cycles and slow components [web:368][web:371][web:369].

A good workflow is:
1. Measure.
2. Find the bottleneck.
3. Optimize one thing.
4. Measure again.

## Practical Example

```ts
import { Component, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-performance-demo',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <button (click)="load()">Load data</button>
    <p *ngIf="loaded">Data loaded</p>
  `
})
export class PerformanceDemoComponent {
  loaded = false;

  load() {
    this.loaded = true;
  }
}
```

This example uses `OnPush` to reduce unnecessary checks, which matches Angular’s runtime performance guidance [web:371][web:368].

## Best Practices

- Profile before changing code.
- Use lazy loading for non-critical routes.
- Use `@defer` for below-the-fold content.
- Prefer `OnPush` for stable component trees.
- Keep heavy calculations out of templates.
- Optimize images and rendering strategy when loading is the issue.

## Easy Challenges

- Find one slow screen in your app.
- Change one component to `OnPush`.
- Move one expensive calculation out of the template.
- Lazy load one route.
- Test `@defer` on a heavy section.

## Medium Challenges

- Profile a page and note the bottleneck.
- Apply image optimization to one page.
- Use deferred loading for one component.
- Reduce unnecessary re-renders in a list.
- Compare performance before and after a change.

## Hard Challenges

- Refactor a large feature for lazy loading.
- Add `OnPush` to multiple child components.
- Separate loading performance from runtime performance fixes.
- Use profiling data to justify a change.
- Tune a screen with multiple rendering bottlenecks.

## Reflection Questions

- Is your issue loading speed or runtime responsiveness?
- What does `OnPush` skip?
- Why should you profile before optimizing?
- When is `@defer` useful?
- Which optimization gives the biggest benefit in your app?

## Day Deliverable

Create one optimization example that includes:

- One performance problem.
- One profiling step or note.
- One optimization such as lazy loading or `OnPush`.
- One explanation of the improvement.
- One Angular-specific technique.

## Suggested Practice Flow

1. Identify a slow page or component.
2. Measure the issue.
3. Apply one optimization.
4. Re-measure.
5. Compare results.
6. Complete the easy, medium, and hard challenges.

## Note for Day 37

Next day covers **Reusability and Architecture**, which is the next step in keeping Angular apps maintainable.
