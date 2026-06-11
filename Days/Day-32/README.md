# Day 32 — Custom Validators

Day 32 is about creating your own validation rules in Angular. Angular’s forms validation guide and custom validator references show that validators are functions that return `null` when valid and an error object when invalid [web:305][web:322][web:336].

## Goal

By the end of this day, you should be able to:

- Create a custom validator function.
- Attach a validator to a `FormControl`.
- Attach a validator to a `FormGroup`.
- Return meaningful validation errors.
- Show custom validation messages in the template.
- Reuse validators across forms.

## Why This Matters

Built-in validators cover common cases, but many apps need rules specific to the business domain. Custom validators let you encode those rules directly into your form logic [web:322][web:335][web:337].

This matters because:
- You can validate app-specific rules.
- You reduce duplicated logic.
- You keep invalid data out earlier.
- You can validate single fields or whole forms.

## What A Validator Does

A validator is a function that checks a control’s value and returns either `null` or an error object. Angular’s documentation and validator references describe validators this way for both built-in and custom rules [web:336][web:322][web:334].

## Field-Level Validator

Use a field-level validator when the rule applies to one input only.

```ts
import { AbstractControl, ValidationErrors } from '@angular/forms';

export function noSpacesValidator(control: AbstractControl): ValidationErrors | null {
  const value = control.value as string;
  return value && value.includes(' ') ? { noSpaces: true } : null;
}
```

## Form-Level Validator

Use a form-level validator when the rule depends on more than one field, such as a start date and end date comparison [web:332][web:337][web:322].

```ts
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';

export function startBeforeEndValidator(): ValidatorFn {
  return (group: AbstractControl): ValidationErrors | null => {
    const start = group.get('startDate')?.value;
    const end = group.get('endDate')?.value;

    if (start && end && new Date(start) > new Date(end)) {
      return { startAfterEnd: true };
    }

    return null;
  };
}
```

## Reactive Form Example

```ts
import { Component } from '@angular/core';
import { ReactiveFormsModule, FormControl, FormGroup, Validators } from '@angular/forms';
import { noSpacesValidator } from './no-spaces.validator';

@Component({
  selector: 'app-custom-validator-demo',
  standalone: true,
  imports: [ReactiveFormsModule],
  templateUrl: './custom-validator-demo.component.html'
})
export class CustomValidatorDemoComponent {
  form = new FormGroup({
    username: new FormControl('', [Validators.required, noSpacesValidator]),
    email: new FormControl('', [Validators.required, Validators.email])
  });

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      return;
    }
    console.log(this.form.value);
  }
}
```

## Template Example

```html
<form [formGroup]="form" (ngSubmit)="submit()">
  <input formControlName="username" placeholder="Username" />
  <p *ngIf="form.get('username')?.hasError('required')">Username is required.</p>
  <p *ngIf="form.get('username')?.hasError('noSpaces')">Username cannot contain spaces.</p>

  <input formControlName="email" placeholder="Email" />
  <p *ngIf="form.get('email')?.hasError('required')">Email is required.</p>
  <p *ngIf="form.get('email')?.hasError('email')">Enter a valid email.</p>

  <button type="submit">Save</button>
</form>
```

## Good Validator Design

A good custom validator is:
- Small.
- Easy to read.
- Reusable.
- Focused on one rule.
- Return-value based, not side-effect based [web:322][web:337][web:334].

## Best Practices

- Use a field validator for single-control rules.
- Use a group validator for cross-field rules.
- Return a clear error key like `noSpaces` or `startAfterEnd`.
- Keep validators pure.
- Reuse validators from shared files when possible.

## Easy Challenges

- Create a validator that rejects empty strings with spaces.
- Add a custom validator to one input.
- Show the custom error in the template.
- Create a validator for a minimum numeric range.
- Return a custom error key.

## Medium Challenges

- Build a username validator.
- Build a password strength validator.
- Create a cross-field validator for two dates.
- Reuse one validator in two forms.
- Mark all controls as touched on invalid submit.

## Hard Challenges

- Build a reusable validator library file.
- Combine built-in and custom validators.
- Add a group-level validator with a custom error message.
- Support multiple custom error states.
- Refactor validation logic into clean helper functions.

## Reflection Questions

- When should you use a field validator?
- When should you use a group validator?
- Why should validators return `null` when valid?
- What makes an error key useful?
- How can you keep validators reusable?

## Day Deliverable

Create one form that includes:

- One custom field validator.
- One custom group validator.
- At least one built-in validator.
- A custom error message.
- One reusable validator function.

## Suggested Practice Flow

1. Pick one rule that Angular does not provide.
2. Write a validator function.
3. Attach it to a control or group.
4. Add error messages.
5. Test invalid and valid input.
6. Complete the easy, medium, and hard challenges.

## Note for Day 33

Next day covers **State Management Patterns**, which builds on forms and shared data handling.
