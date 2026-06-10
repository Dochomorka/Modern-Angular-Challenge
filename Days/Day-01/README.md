## Day 01 — TypeScript Essentials for Angular

Day 01 is about building the TypeScript foundation you need before Angular starts feeling natural. Angular is built in TypeScript, and its CLI-based workflow starts with setting up a workspace and app, so TypeScript is the right place to begin [web:31][web:48][web:50].

### Goal

By the end of this day, you should understand the TypeScript features you will use constantly in Angular:

- Types.
- Interfaces.
- Classes.
- Optional properties.
- Union types.
- Functions with typed parameters and return values.
- Basic generics.

These concepts show up everywhere in Angular code, from components and services to route data and API models [web:31][web:47][web:45].

### Why TypeScript Matters

TypeScript adds static typing on top of JavaScript, which helps catch mistakes earlier and makes code easier to read and maintain. In Angular, you will use TypeScript for components, services, forms, routing, and RxJS-based logic, so understanding it well will save you time later [web:31][web:47][web:45].

A strong TypeScript base helps you:

- Understand Angular syntax faster.
- Avoid runtime bugs.
- Work comfortably with IDE autocomplete.
- Organize app data more clearly.
- Refactor code with confidence.

### Core Concepts

#### 1. Types

Types describe what kind of value a variable can hold.

```ts
let appName: string = "Modern Angular Challenge";
let days: number = 40;
let isActive: boolean = true;
```

Types make your code safer because TypeScript can warn you when you assign the wrong kind of value.

#### 2. Interfaces

Interfaces define the shape of an object.

```ts
interface User {
  id: number;
  name: string;
  email?: string;
}

const user: User = {
  id: 1,
  name: "Yimesgen"
};
```

The `email?` means the property is optional. Interfaces are very common in Angular for API responses, form models, and component inputs [web:47].

#### 3. Functions

You can type function parameters and return values.

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

This becomes useful when building Angular services and helper functions.

#### 4. Union Types

Union types let a variable hold more than one allowed type.

```ts
let status: "loading" | "success" | "error";
status = "loading";
```

This is useful in Angular when managing UI state.

#### 5. Classes

Classes are a key part of Angular because components and services are class-based.

```ts
class Product {
  constructor(
    public id: number,
    public name: string
  ) {}
}

const p = new Product(1, "Laptop");
```

Angular uses classes heavily in its architecture, so this concept matters a lot.

#### 6. Generics

Generics let you write reusable typed code.

```ts
function wrapValue<T>(value: T): T {
  return value;
}

const result = wrapValue<string>("Angular");
```

Generics will matter later when you work with RxJS, services, and reusable utilities [web:45][web:49][web:51].

### Angular Connection

Angular’s official setup flow starts with creating a workspace using the CLI, and Angular’s modern component model supports standalone components that do not require an `NgModule` [web:48][web:50][web:10][web:13]. When you create a component or service, Angular generates TypeScript files, so the language is part of daily Angular work from the first day onward [web:31][web:48].

A simple Angular component looks like this:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  standalone: true,
  template: `<h1>{{ title }}</h1>`
})
export class AppComponent {
  title: string = 'Modern Angular Challenge';
}
```

This is why TypeScript is not optional in Angular — it is the language you use to define application behavior.

### Coding Example

Here is a small TypeScript model and helper function you might later use in Angular:

```ts
interface Course {
  id: number;
  title: string;
  completed: boolean;
}

const courses: Course[] = [
  { id: 1, title: "TypeScript Basics", completed: false },
  { id: 2, title: "Angular Components", completed: true }
];

function getIncompleteCourses(items: Course[]): Course[] {
  return items.filter(course => !course.completed);
}

console.log(getIncompleteCourses(courses));
```

This example shows typed data, arrays of objects, and a reusable function — all common patterns in Angular apps.

### Easy Challenges

- Declare 5 variables using different TypeScript types.
- Create an interface for a `Student` object.
- Write a function that accepts a `string` and returns a greeting.
- Make a union type for UI status values.
- Create a class with a constructor and 2 public properties.

### Medium Challenges

- Define an interface for a blog post with optional fields.
- Write a generic function that returns the first item in an array.
- Create a typed array of products and filter by price.
- Build a function that accepts a union type and returns a different message for each value.
- Convert a plain JavaScript object example into a strongly typed TypeScript version.

### Hard Challenges

- Design interfaces for a small Angular-style app domain such as users, tasks, or courses.
- Write a generic utility function for sorting arrays.
- Create a typed state object with loading, success, and error states.
- Model API response types for a list and a single item.
- Refactor untyped code into fully typed TypeScript.

### Reflection Questions

- Why does Angular prefer TypeScript over plain JavaScript?
- When should you use an interface instead of a class?
- Why are union types useful in UI state?
- How do generics improve reusability?
- What problems does typing help you catch early?

### Day Deliverable

Create a single `day-01-typescript-basics.ts` file that includes:

- At least 3 typed variables.
- 2 interfaces.
- 2 functions.
- 1 class.
- 1 generic helper.
- 1 small array-based example.

### Suggested Practice Flow

1. Read the concept overview.
2. Type the code examples yourself.
3. Modify the examples.
4. Complete all challenge levels.
5. Review your own code and check where TypeScript helped.

### Note for Day 02

Next day will move into Angular CLI and project setup, so the TypeScript foundation you build here will be used immediately [web:48][web:50].
