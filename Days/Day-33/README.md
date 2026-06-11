# Day 33 — State Management Patterns

Day 33 is about organizing shared and local state in Angular so your app stays predictable as it grows. Angular state guidance and RxJS references show that state can live in components, services, or dedicated stores, and should be exposed in a way that keeps updates controlled and readable [web:340][web:347][web:341].

## Goal

By the end of this day, you should be able to:

- Understand what application state is.
- Distinguish local state from shared state.
- Use a service as a simple state store.
- Expose state reactively.
- Update state through methods instead of direct mutation.
- Choose a simple pattern before reaching for a heavier solution.

## Why This Matters

State management is about how data is stored, updated, and shared across your app. In Angular, you often do not need a large global store at first; a service with reactive state is often enough for feature-level sharing [web:342][web:340][web:341].

This matters because:
- Shared state can otherwise become scattered.
- Components stay simpler when state lives elsewhere.
- A single source of truth reduces bugs.
- The right pattern depends on app size and complexity.

## Common Patterns

A practical Angular state pattern is to keep local UI state in the component and move shared feature state into a service. State can be exposed with observables, subjects, or signals depending on the app style, and updates should happen through methods rather than direct writes [web:340][web:341][web:346].

Common patterns include:
- Local component state.
- Shared service state.
- Read-only exposed streams.
- Update methods on a service.
- Derived state from existing state.

## Service State Example

```ts
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class CartStateService {
  private readonly itemsSubject = new BehaviorSubject<string[]>([]);
  readonly items$ = this.itemsSubject.asObservable();

  addItem(item: string) {
    this.itemsSubject.next([...this.itemsSubject.value, item]);
  }

  removeItem(item: string) {
    this.itemsSubject.next(this.itemsSubject.value.filter(x => x !== item));
  }
}
```

## Component Usage

```ts
import { Component } from '@angular/core';
import { AsyncPipe, NgFor } from '@angular/common';
import { CartStateService } from './cart-state.service';

@Component({
  selector: 'app-cart',
  standalone: true,
  imports: [AsyncPipe, NgFor],
  template: `
    <button (click)="add()">Add Item</button>
    <ul>
      <li *ngFor="let item of items$ | async">{{ item }}</li>
    </ul>
  `
})
export class CartComponent {
  items$ = this.cartState.items$;

  constructor(private cartState: CartStateService) {}

  add() {
    this.cartState.addItem('Book');
  }
}
```

This pattern keeps state in one place while letting multiple components observe it reactively [web:341][web:340][web:347].

## Best Practices

- Keep local UI state inside the component.
- Promote state into a service only when sharing is needed.
- Expose state as read-only whenever possible.
- Update state through methods, not direct mutation.
- Separate UI state from server or cache state.

## When To Use Heavier Tools

More advanced libraries are useful when the app becomes large, the rules become more complex, or many features need coordinated state. For smaller apps and feature-level sharing, Angular guidance and community practice both emphasize keeping state simple first [web:340][web:342][web:346].

## Easy Challenges

- Keep a loading flag in a component.
- Share one value between two components using a service.
- Use `BehaviorSubject` for current state.
- Expose state through `asObservable()`.
- Update state with one method.

## Medium Challenges

- Build a shopping cart state service.
- Add remove and clear methods.
- Show the state in two components.
- Keep update logic inside the service.
- Add a derived count of items.

## Hard Challenges

- Split UI state and data state.
- Add a reset method for the state service.
- Create a read-only state API.
- Refactor duplicated state into one store-like service.
- Compare a service-based pattern with a library-based approach.

## Reflection Questions

- What state should stay local?
- When should state move into a service?
- Why expose read-only streams?
- Why avoid direct mutation?
- When is a heavier state library worth it?

## Day Deliverable

Create one state pattern example that includes:

- One shared service.
- One reactive state source.
- One update method.
- One consumer component.
- One clear use case.

## Suggested Practice Flow

1. Identify local vs shared state.
2. Move shared state into a service.
3. Expose it reactively.
4. Add update methods.
5. Consume it in a component.
6. Complete the easy, medium, and hard challenges.

## Note for Day 34

Next day covers **Component Testing**, which is the next step in verifying Angular behavior.
