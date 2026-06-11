# Day 37 — Reusability and Architecture

Day 37 is about structuring Angular code so it stays clean, scalable, and easy to reuse. Angular’s docs describe components as part of a component tree, and the Angular style guide plus architecture resources emphasize keeping responsibilities small and boundaries clear [web:386][web:384][web:379][web:378].

## Goal

By the end of this day, you should be able to:

- Apply single-responsibility thinking to components and services.
- Split code into reusable pieces.
- Organize features with clear boundaries.
- Use shared components or utilities wisely.
- Recognize when abstraction helps and when it hurts.
- Keep Angular architecture maintainable as the app grows.

## Why This Matters

Good architecture reduces duplication and makes refactoring safer. Angular’s platform and style guidance support building applications that scale with both team size and codebase size, which depends heavily on reusable design and clear structure [web:379][web:384][web:386].

This matters because:
- Reused code is easier to maintain.
- Small components are easier to test.
- Clear boundaries reduce accidental coupling.
- A consistent structure helps teams move faster.

## Core Principles

A practical Angular architecture usually separates presentation, state, and business logic. Component docs show that Angular apps are built as trees of components, which makes composition a natural way to reuse pieces [web:386][web:378].

Useful principles:
- One component, one job.
- Keep UI and logic separated when possible.
- Put shared logic into services or helpers.
- Reuse through composition, not copy-paste.
- Make dependencies explicit.

## Reuse Patterns

Common reuse patterns in Angular include shared components, shared services, utility functions, and feature folders. Architecture resources often recommend clear layers or feature-based grouping so code stays easy to discover [web:378][web:381][web:382].

Examples:
- A reusable button component.
- A date formatting utility.
- A shared API service.
- A feature folder for related screens.
- A base service for common logic.

## Simple Architecture Example

```ts
// shared/button/button.component.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-button',
  standalone: true,
  template: `<button [type]="type"><ng-content /></button>`
})
export class ButtonComponent {
  @Input() type: 'button' | 'submit' = 'button';
}
```

This kind of component is reusable because it focuses on one UI responsibility and can be composed into larger features [web:386][web:384].

## Practical Example

```ts
import { Component } from '@angular/core';
import { ButtonComponent } from './shared/button/button.component';

@Component({
  selector: 'app-profile-card',
  standalone: true,
  imports: [ButtonComponent],
  template: `
    <section>
      <h2>{{ name }}</h2>
      <app-button type="button">Edit</app-button>
    </section>
  `
})
export class ProfileCardComponent {
  name = 'Alice';
}
```

This shows composition in action: the profile card uses a reusable button component rather than duplicating markup [web:386][web:378].

## Best Practices

- Keep components small and focused.
- Prefer composition over inheritance.
- Group code by feature when it makes sense.
- Extract repeated logic into services or utilities.
- Avoid building abstractions too early.
- Make shared code easy to find.

## Easy Challenges

- Split one large component into two smaller ones.
- Move repeated formatting into a helper.
- Create one shared UI component.
- Group related files into a feature folder.
- Replace duplicated template code with a reusable component.

## Medium Challenges

- Build a shared card component.
- Extract API logic into one service.
- Create a reusable form field component.
- Organize a feature using a clear folder structure.
- Refactor repeated logic out of two components.

## Hard Challenges

- Design a small feature with presentation and service layers.
- Create a reusable component with inputs and content projection.
- Build a shared utility used across two features.
- Refactor a messy module into a cleaner structure.
- Explain why each extracted piece exists.

## Reflection Questions

- What should stay in a component?
- When should logic move to a service?
- Why is composition better than duplication?
- When does abstraction become too heavy?
- How does a clear folder structure help teams?

## Day Deliverable

Create one reusable architecture example that includes:

- One shared component or utility.
- One feature component that uses it.
- One clear responsibility split.
- One reason for the chosen structure.
- One reusable design decision.

## Suggested Practice Flow

1. Find repeated code.
2. Extract the repeated part.
3. Decide whether it belongs in a component, service, or utility.
4. Use it in another place.
5. Review the folder structure.
6. Complete the easy, medium, and hard challenges.

## Note for Day 38

Next day covers **Accessibility Basics**, which is the next step in making Angular apps usable for everyone.
