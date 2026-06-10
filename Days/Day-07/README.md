## Day 07 — Services and Reusable Logic

Day 07 is about using Angular services to keep reusable logic, shared state, and data access outside of components. Angular’s docs describe services as reusable pieces of code for data fetching, business logic, and functionality shared across multiple components, and they are commonly injected through Angular’s dependency injection system [web:99][web:92][web:104].

### Goal

By the end of this day, you should be able to:

- Understand what a service is.
- Know when to move logic into a service.
- Create a service with `@Injectable()`.
- Share a service with `providedIn: 'root'`.
- Inject a service into one or more components.
- Use a service to manage reusable methods and state.

Angular’s service guide and DI documentation both emphasize that services are the place for logic that multiple parts of the app need to access [web:99][web:92][web:97].

### Why Services Matter

Components should mainly focus on UI, while services handle reusable business logic, data access, and shared state. This separation keeps components smaller and easier to maintain [web:99][web:97][web:104].

Services are useful for:
- API calls.
- Shared utility methods.
- Authentication logic.
- Task or product data.
- App-wide state.
- Logging and notifications.

When logic starts being reused or becomes too big for a component, it usually belongs in a service.

### Service Basics

A service is just a TypeScript class with Angular’s injectable metadata. When you mark it with `@Injectable`, Angular knows it can create and provide it through DI [web:99][web:92].

Example:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class MathService {
  add(a: number, b: number): number {
    return a + b;
  }

  subtract(a: number, b: number): number {
    return a - b;
  }
}
```

Providing it in root creates a single shared instance for the app [web:99][web:104].

### Using a Service in a Component

```ts
import { Component, inject } from '@angular/core';
import { MathService } from './math.service';

@Component({
  selector: 'app-calculator',
  standalone: true,
  template: `
    <h2>Result: {{ result }}</h2>
    <button (click)="calculate()">Calculate</button>
  `
})
export class CalculatorComponent {
  private mathService = inject(MathService);
  result = 0;

  calculate() {
    this.result = this.mathService.add(10, 5);
  }
}
```

The component does not need to know how the calculation works internally. It just asks the service for the result [web:101][web:99].

### Reusable Logic Example

A service can also wrap common app behavior:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TextService {
  capitalize(text: string): string {
    return text.trim().charAt(0).toUpperCase() + text.trim().slice(1);
  }

  shorten(text: string, maxLength: number): string {
    return text.length > maxLength ? text.slice(0, maxLength) + '...' : text;
  }
}
```

This logic can now be reused anywhere in the app.

### State in a Service

Services can also store shared state.

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class CounterService {
  private value = 0;

  increment(): number {
    this.value++;
    return this.value;
  }

  reset(): number {
    this.value = 0;
    return this.value;
  }

  getValue(): number {
    return this.value;
  }
}
```

This is useful when multiple components need the same data source [web:99][web:97].

### Practical Example

```ts
import { Injectable, Component, inject } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TaskService {
  private tasks = ['Learn Angular', 'Practice TypeScript', 'Build a project'];

  getTasks(): string[] {
    return [...this.tasks];
  }

  addTask(task: string): void {
    this.tasks.push(task);
  }

  removeLastTask(): string | undefined {
    return this.tasks.pop();
  }
}

@Component({
  selector: 'app-task-list',
  standalone: true,
  template: `
    <section class="card">
      <h2>Tasks</h2>

      <ul>
        @for (task of tasks; track task) {
          <li>{{ task }}</li>
        }
      </ul>

      <button (click)="add()">Add Task</button>
      <button (click)="remove()">Remove Last Task</button>
    </section>
  `
})
export class TaskListComponent {
  private taskService = inject(TaskService);
  tasks = this.taskService.getTasks();

  add() {
    this.taskService.addTask('Review Angular services');
    this.tasks = this.taskService.getTasks();
  }

  remove() {
    this.taskService.removeLastTask();
    this.tasks = this.taskService.getTasks();
  }
}
```

This example shows a service handling the data while the component handles display and user interaction [web:99][web:92].

### When to Use a Service

Use a service when:
- More than one component needs the same logic.
- You want to keep components small.
- You need shared state.
- You need to isolate business logic from UI code.
- You want easier testing and reuse [web:99][web:97].

### Easy Challenges

- Create a service with one method that returns a string.
- Create a service that returns a list of values.
- Inject a service into a standalone component.
- Display the result of a service method in the template.
- Create a simple state value inside a service.

### Medium Challenges

- Build a `GreetingService` with multiple greeting methods.
- Create a `TaskService` that stores a small task list.
- Add methods to insert and remove items from a service array.
- Inject the same service into two different components.
- Move a calculation out of a component and into a service.

### Hard Challenges

- Build a service that stores shared app state and exposes methods for updating it.
- Create a reusable formatting service for text or numbers.
- Refactor a component so it contains no business logic.
- Create a service that manages a list of objects with typed methods.
- Use one service across multiple components and keep the displayed data in sync.

### Reflection Questions

- Why should logic move out of components and into services?
- What is the difference between a reusable method and shared state?
- Why is `providedIn: 'root'` useful?
- How does DI make services easier to use?
- When does a service become the right place for app logic?

### Day Deliverable

Create one service and one component that uses it:

- The service should contain at least two methods.
- The component should inject the service.
- The component should display data from the service.
- Add a button that updates the service state or result.
- Use `@for` or a simple list display if useful.

### Suggested Practice Flow

1. Read the explanation of services.
2. Create a service with `@Injectable({ providedIn: 'root' })`.
3. Add reusable methods to the service.
4. Inject the service into a standalone component.
5. Call the service from a button click or template action.
6. Complete the easy, medium, and hard challenges.

### Note for Day 08

Next day will cover standalone components in more depth, which will help you organize Angular code using the modern component-first style [web:10][web:13].
