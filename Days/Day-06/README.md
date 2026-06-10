## Day 06 — Dependency Injection Basics

Day 06 is about learning how Angular provides and shares code using dependency injection. Angular’s DI guide explains that dependency injection is a design pattern for supplying dependencies to a class instead of creating them manually, and Angular’s `inject()` API can be used in constructors or field initializers [web:92][web:101].

### Goal

By the end of this day, you should be able to:

- Understand what dependency injection is.
- Understand why services exist.
- Create a basic Angular service.
- Inject a service into a component.
- Use `providedIn: 'root'` for shared app-wide services.
- Understand where `inject()` can and cannot be used.

Angular documentation shows that services are reusable pieces of code, and `inject()` is supported in Angular’s injection context such as constructors and field initializers [web:99][web:93][web:101].

### Why Dependency Injection Matters

Dependency injection helps you keep your code clean, modular, and testable. Instead of creating everything directly inside a component, you ask Angular to provide the dependency for you [web:92][web:95].

This matters because Angular apps usually have shared logic such as:
- Data fetching.
- Logging.
- Authentication.
- Caching.
- Utility functions.
- Shared state.

Services are the standard place for this logic [web:99][web:97].

### What Is a Service?

A service is a class that holds reusable logic or shared state outside of components. Angular recommends using services when multiple parts of the app need the same functionality [web:99][web:97].

Example uses for services:
- User data service.
- API service.
- Theme service.
- Auth service.
- Notification service.

### Basic Service Example

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class GreetingService {
  getGreeting(name: string): string {
    return `Hello, ${name}!`;
  }
}
```

The `providedIn: 'root'` setting makes the service available app-wide as a singleton [web:99][web:97].

### Injecting a Service Into a Component

```ts
import { Component } from '@angular/core';
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-home',
  standalone: true,
  template: `
    <h1>{{ message }}</h1>
  `
})
export class HomeComponent {
  message: string;

  constructor(private greetingService: GreetingService) {
    this.message = this.greetingService.getGreeting('Yimesgen');
  }
}
```

This is the classic constructor-based injection style, which Angular fully supports [web:92][web:101].

### Using the `inject()` Function

Angular also supports `inject()` for dependency lookup in a valid injection context [web:92][web:101].

```ts
import { Component, inject } from '@angular/core';
import { GreetingService } from './greeting.service';

@Component({
  selector: 'app-home',
  standalone: true,
  template: `<h1>{{ message }}</h1>`
})
export class HomeComponent {
  private greetingService = inject(GreetingService);
  message = this.greetingService.getGreeting('Yimesgen');
}
```

This style is useful in modern Angular code because it can be more concise and works well with current Angular patterns [web:96][web:101].

### Injection Context

Angular only allows `inject()` in specific places such as:
- Constructors of Angular-managed classes.
- Field initializers.
- Factory functions.

It does **not** work in regular methods like `ngOnInit` after the instance has already been created [web:93][web:101].

### Practical Example

```ts
import { Injectable, Component, inject } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class CounterService {
  private count = 0;

  increment(): number {
    this.count++;
    return this.count;
  }

  reset(): number {
    this.count = 0;
    return this.count;
  }

  getValue(): number {
    return this.count;
  }
}

@Component({
  selector: 'app-counter-demo',
  standalone: true,
  template: `
    <section class="card">
      <h2>Counter Service Demo</h2>
      <p>Value: {{ value }}</p>
      <button (click)="increment()">Increment</button>
      <button (click)="reset()">Reset</button>
    </section>
  `
})
export class CounterDemoComponent {
  private counterService = inject(CounterService);
  value = this.counterService.getValue();

  increment() {
    this.value = this.counterService.increment();
  }

  reset() {
    this.value = this.counterService.reset();
  }
}
```

This example shows how a service can own logic while the component focuses on display and interaction [web:99][web:92].

### Service Scope

A service provided in root is shared across the whole app. This is useful for global state, shared helpers, and API access [web:99][web:97]. If you later need a different scope, Angular also supports more specific provider setups, but `providedIn: 'root'` is the right starting point for this challenge [web:99][web:92].

### Easy Challenges

- Create a service with one method that returns a string.
- Inject the service into a component.
- Display a value returned by the service.
- Create a service method that returns a number.
- Use `providedIn: 'root'` in a service.

### Medium Challenges

- Build a `GreetingService` with different messages for different names.
- Create a `MathService` with `add`, `subtract`, and `multiply`.
- Inject two different services into one component.
- Use `inject()` instead of constructor injection in a component.
- Move logic out of a component and into a service.

### Hard Challenges

- Create a `TaskService` that stores and returns a task list.
- Build a service that keeps internal state and exposes getter methods.
- Create a component that uses service methods for all its actions.
- Refactor a component so it contains no business logic.
- Design a reusable `UserService` with typed methods and data.

### Reflection Questions

- Why are services a better place for shared logic than components?
- What does `providedIn: 'root'` do?
- What is dependency injection solving in Angular?
- When should you use `inject()` instead of constructor injection?
- Why is service-based design easier to test?

### Day Deliverable

Create a simple service and a component that uses it:

- The service should provide at least one method.
- The component should inject the service.
- The component should display data from the service.
- Add at least one action that updates the displayed value.

### Suggested Practice Flow

1. Read the explanation of dependency injection.
2. Create a service with `providedIn: 'root'`.
3. Inject it into a standalone component.
4. Add one or two methods to the service.
5. Use the service methods from buttons in the template.
6. Complete the easy, medium, and hard challenges.

### Note for Day 07

Next day will cover services and reusable logic in more detail, which will help you build cleaner Angular features on top of dependency injection [web:99][web:92].
