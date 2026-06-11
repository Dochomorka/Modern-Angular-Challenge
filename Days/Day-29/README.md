# Day 29 — Template-Driven Forms

Day 29 is about building forms the Angular way using template-driven forms. Angular’s forms guide explains that there are two main form styles, and template-driven forms use template directives like `FormsModule`, `ngForm`, and `ngModel` to handle binding and validation in the template [web:309][web:315][web:306].

## Goal

By the end of this day, you should be able to:

- Understand what template-driven forms are.
- Use `FormsModule` in a component.
- Bind form inputs with `ngModel`.
- Read form state with `ngForm`.
- Handle submit events.
- Apply basic validation in the template.

## Why This Matters

Template-driven forms are a simple way to build forms with less component code. Angular’s form docs show that this approach is useful when you want quick setup and template-centered logic, especially for straightforward forms [web:315][web:306].

This matters because:
- It is easy to start with.
- It keeps form markup readable.
- It is good for simple forms.
- It introduces Angular form concepts naturally.

## Core Ideas

Template-driven forms rely on the template to define the form structure and most of the behavior. Inputs are connected with `ngModel`, and the form itself is managed with `ngForm` [web:315][web:300][web:306].

## Basic Setup

You need `FormsModule` imported before using template-driven features like `ngModel` [web:315][web:300].

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-template-form',
  standalone: true,
  imports: [FormsModule],
  templateUrl: './template-form.component.html'
})
export class TemplateFormComponent {
  user = {
    name: '',
    email: ''
  };

  onSubmit(form: any) {
    console.log(form.value);
  }
}
```

## Template Example

```html
<form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
  <label>
    Name:
    <input name="name" [(ngModel)]="user.name" required />
  </label>

  <label>
    Email:
    <input name="email" [(ngModel)]="user.email" email required />
  </label>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
```

`ngModel` creates two-way binding between the input and your component data, while `ngForm` gives you access to the form object and its validation state [web:315][web:300].

## Validation Basics

Template-driven validation is usually added directly in the template with standard validators like `required` and `email`. Angular’s form guidance shows that template-driven forms support validation through template attributes and directives [web:305][web:306][web:314].

Common checks include:
- `required`
- `email`
- `minlength`
- `maxlength`

## Practical Example

```ts
import { Component } from '@angular/core';
import { FormsModule, NgForm } from '@angular/forms';

@Component({
  selector: 'app-contact-form',
  standalone: true,
  imports: [FormsModule],
  template: `
    <form #f="ngForm" (ngSubmit)="submit(f)">
      <input name="fullName" [(ngModel)]="model.fullName" required minlength="3" />
      <input name="message" [(ngModel)]="model.message" required maxlength="100" />
      <button type="submit" [disabled]="f.invalid">Send</button>
    </form>
  `
})
export class ContactFormComponent {
  model = {
    fullName: '',
    message: ''
  };

  submit(form: NgForm) {
    console.log(form.value);
  }
}
```

## Best Practices

- Use template-driven forms for simple to medium forms.
- Import `FormsModule` before using `ngModel`.
- Always give inputs a `name` attribute.
- Use `ngForm` to read form state.
- Disable submit until the form is valid.

## Easy Challenges

- Create a form with one input and one button.
- Bind one field with `ngModel`.
- Read form values on submit.
- Add a `required` validator.
- Disable the submit button until valid.

## Medium Challenges

- Build a contact form with three fields.
- Show form values in the console.
- Add `minlength` and `email` validation.
- Display whether the form is valid.
- Reset the form after submit.

## Hard Challenges

- Build a registration form with several fields.
- Show error messages only after touch or submit.
- Disable submit until all fields are valid.
- Add conditional validation hints.
- Refactor the form for readability.

## Reflection Questions

- What makes a form template-driven?
- Why do you need `FormsModule`?
- How does `ngModel` work?
- What does `ngForm` give you?
- When is template-driven better than reactive forms?

## Day Deliverable

Create one template-driven form that includes:

- `FormsModule`
- At least two inputs
- `ngModel` binding
- One validation rule
- A submit handler

## Suggested Practice Flow

1. Import `FormsModule`.
2. Create a simple form.
3. Add `ngModel` bindings.
4. Add validation.
5. Handle submit.
6. Complete the easy, medium, and hard challenges.

## Note for Day 30

Next day covers **Reactive Forms**, which moves form logic into the component and gives more control over validation and state.
