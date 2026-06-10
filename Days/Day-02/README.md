## Day 02 — Angular CLI and Project Setup

Day 02 is about getting your Angular development environment ready and understanding how the Angular CLI helps you create, run, and manage projects. The Angular CLI is the official command-line tool for initializing, developing, building, and maintaining Angular applications [web:50][web:52].

### Goal

By the end of this day, you should be able to:

- Understand what the Angular CLI does.
- Install or use the Angular CLI correctly.
- Create a new Angular project.
- Run the development server.
- Explore the project structure.
- Understand the most important CLI commands for daily work.

Angular’s official installation guide starts with setting up a local Angular project, and the CLI reference shows that commands like `new`, `serve`, `build`, and `generate` are part of the normal workflow [web:32][web:52].

### Why the CLI Matters

The Angular CLI saves time by handling repetitive setup tasks for you. Instead of manually creating folders, config files, build steps, and dev-server commands, you can use one tool to scaffold and manage the application lifecycle [web:50][web:52].

The CLI helps you:

- Create new projects quickly.
- Generate components, services, directives, and more.
- Run a local development server.
- Build production-ready output.
- Keep the project structure consistent.
- Reduce setup mistakes.

This is one of the biggest reasons Angular is practical for team development.

### Basic Setup Flow

A common Angular project setup flow looks like this:

```bash
npm install -g @angular/cli
ng new modern-angular-challenge
cd modern-angular-challenge
ng serve
```

The Angular documentation explains that the CLI is used to create and build your first app, and the installation guide walks through setting up a local workspace [web:50][web:32]. The CLI reference also confirms that `new` creates a workspace and `serve` runs the app in development mode [web:52].

### What Happens When You Create a Project

When you run `ng new`, Angular creates a project folder with:

- A `src` directory.
- A main application entry point.
- Configuration files.
- Package metadata.
- TypeScript and Angular defaults.
- Build and test setup.

This gives you a complete working application structure from the start.

### Common CLI Commands

Here are the commands you will use most often:

```bash
ng new app-name
ng serve
ng generate component user-card
ng generate service data
ng build
ng test
```

The CLI reference lists these core commands as part of Angular’s standard workflow [web:52].

### Project Structure Overview

A new Angular project usually includes files and folders such as:

- `src/`
- `app/`
- `assets/`
- `index.html`
- `main.ts`
- `styles.css` or `styles.scss`
- `angular.json`
- `package.json`
- `tsconfig.json`

You do not need to memorize every file today. The goal is to understand what the project contains and where your code will live.

### Simple Example

After creating the app, your root component might look like this:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  standalone: true,
  template: `<h1>Modern Angular Challenge</h1>`
})
export class AppComponent {}
```

This example shows the modern Angular style where components can be standalone by default in current Angular documentation [web:10][web:13].

### Coding Example

Here is a typical first project workflow:

```bash
ng new modern-angular-challenge
cd modern-angular-challenge
ng serve
```

Then open your app component and update it:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <main>
      <h1>Welcome to the Modern Angular Challenge</h1>
      <p>Your Angular project is running successfully.</p>
    </main>
  `
})
export class AppComponent {
  title = 'Modern Angular Challenge';
}
```

### Easy Challenges

- Install the Angular CLI.
- Create a new Angular project.
- Run the app with `ng serve`.
- Open the project structure and identify `src`, `main.ts`, and `angular.json`.
- Change the root component title text.

### Medium Challenges

- Create a new project with a custom name.
- Generate a component using the CLI.
- Generate a service using the CLI.
- Add a small text section to the root component.
- Identify which file controls app-wide styling.

### Hard Challenges

- Create a new Angular project and explain what each generated file is for.
- Generate three features using the CLI: one component, one service, and one directive or pipe.
- Modify the root component and verify that the browser updates automatically.
- Build a small `About` section using a generated component.
- Write down the exact commands you used and what each one did.

### Reflection Questions

- What problem does the Angular CLI solve?
- Why is `ng new` useful?
- What happens when you run `ng serve`?
- Why is project structure important in Angular?
- Which files do you expect to edit most often?

### Day Deliverable

Create a new Angular project and make these changes:

- Update the root component text.
- Add one new generated component.
- Run the app successfully in the browser.
- Save a short note listing the commands you used.

### Suggested Practice Flow

1. Install or confirm Angular CLI is available.
2. Create a new Angular project.
3. Run the project locally.
4. Inspect the file structure.
5. Generate one component.
6. Modify the root UI.
7. Re-run and verify everything works.

### Note for Day 03

Next day will move into components, templates, and styling, so this setup work will be the base for the rest of the challenge [web:10][web:52].
