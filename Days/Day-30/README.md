# Day 30 — Reactive Forms

Day 30 is about building forms with reactive forms in Angular. Angular’s forms guide shows that reactive forms are built around `FormControl`, `FormGroup`, and `ReactiveFormsModule`, with form state managed in the component class instead of the template [web:323][web:304][web:307].

## Goal

By the end of this day, you should be able to:

- Understand what reactive forms are.
- Create a `FormGroup`.
- Create `FormControl` instances.
- Bind a form with `[formGroup]`.
- Use `formControlName` in the template.
- Handle submission from the component.
- Apply validation in code.

## Why This Matters

Reactive forms are a strong choice when you need more control, better testability, and more explicit form logic. Angular’s docs show that this style keeps the form model in the component, which makes complex forms easier to manage [web:323][web:307][web:320].

This matters because:
- You can control form state directly in code.
- Validation becomes easier to organize.
- Complex forms are easier to scale.
- Testing is usually clearer.

## Core Idea

In reactive forms, the component owns the form model. The template just connects to the model using directives like `[formGroup]` and `formControlName` [web:323][web:307][web:320].

## Basic Setup

You need `ReactiveFormsModule` and the form classes from `@angular/forms` [web:304][web:307][web:321].

```ts
import { Component } from '@angular/core';
import { ReactiveFormsModule, FormGroup, FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  templateUrl: './reactive-form.component.html'
})
export class ReactiveFormComponent {
  profileForm = new FormGroup({
    name: new FormControl('', [Validators.required]),
    email: new FormControl('', [Validators.required, Validators.email])
  });

  onSubmit() {
    console.log(this.profileForm.value);
  }
}
```

## Template Example

```html
<form [formGroup]="profileForm" (ngSubmit)="onSubmit()">
  <label>
    Name:
    <input type="text" formControlName="name" />
  </label>

  <label>
    Email:
    <input type="email" formControlName="email" />
  </label>

  <button type="submit" [disabled]="profileForm.invalid">Save</button>
</form>
```

The form is connected to the component through `[formGroup]`, and each input is linked by `formControlName` [web:307][web:319][web:320].

## Validation Basics

Reactive forms support built-in validators like `required` and `email`, and Angular’s validation guide covers displaying useful feedback for both reactive and template-driven forms [web:305][web:323][web:318].

Common validators:
- `Validators.required`
- `Validators.email`
- `Validators.minLength`
- `Validators.maxLength`
- `Validators.pattern`

## Practical Example

```ts
import { Component } from '@angular/core';
import { ReactiveFormsModule, FormGroup, FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-signup-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="signupForm" (ngSubmit)="submit()">
      <input formControlName="username" placeholder="Username" />
      <input formControlName="password" type="password" placeholder="Password" />
      <button type="submit" [disabled]="signupForm.invalid">Create account</button>
    </form>
  `
})
export class SignupFormComponent {
  signupForm = new FormGroup({
    username: new FormControl('', [Validators.required, Validators.minLength(3)]),
    password: new FormControl('', [Validators.required, Validators.minLength(8)])
  });

  submit() {
    console.log(this.signupForm.value);
  }
}
```

## Best Practices

- Use reactive forms for medium to complex forms.
- Keep form structure in the component.
- Use validators in code for clarity.
- Read form state from the `FormGroup`.
- Disable submit when the form is invalid.

## Easy Challenges

- Create one `FormControl`.
- Build a `FormGroup` with two fields.
- Bind the form with `[formGroup]`.
- Add `Validators.required`.
- Submit and log the form value.

## Medium Challenges

- Add email validation.
- Show when the form is invalid.
- Use `Validators.minLength`.
- Reset the form after submit.
- Build a small sign-up form.

## Hard Challenges

- Create a multi-field profile form.
- Add multiple validators per control.
- Display validation messages conditionally.
- Split the form into logical sections.
- Refactor repeated validation patterns.

## Reflection Questions

- Why are reactive forms better for complex forms?
- What does `FormGroup` manage?
- What does `FormControl` represent?
- Why is `ReactiveFormsModule` required?
- When should you choose reactive forms over template-driven forms?

## Day Deliverable

Create one reactive form that includes:

- `ReactiveFormsModule`
- One `FormGroup`
- At least two `FormControl`s
- One built-in validator
- One submit handler

## Suggested Practice Flow

1. Import `ReactiveFormsModule`.
2. Create a `FormGroup`.
3. Add controls and validators.
4. Bind the form in the template.
5. Submit and log values.
6. Complete the easy, medium, and hard challenges.

## Note for Day 31

Next day covers **Validation Patterns**, which builds on reactive forms and template-driven forms with better feedback rules.
