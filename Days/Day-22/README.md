## Day 22 — Form Validation and Error Messages

Day 22 is about making Angular forms clear, safe, and user-friendly with validation and visible feedback. The main goal is to help users understand what went wrong and how to fix it before they submit the form.

### Goal

By the end of this day, you should be able to:

- Show validation errors for invalid fields.
- Display errors only when appropriate.
- Use built-in validators effectively.
- Mark fields as touched or dirty in a useful way.
- Prevent invalid form submission.
- Build forms that feel easier to use.

### Why This Matters

Validation is what turns a basic form into a reliable one. Without error messages, users may submit bad data, get confused, or abandon the form entirely.

Good validation helps you:
- Reduce mistakes.
- Guide the user step by step.
- Make forms feel polished.
- Keep bad data out of your app.

### Basic Validation Pattern

A good form usually checks:
- Required fields.
- Email format.
- Minimum length.
- Matching rules for related fields.

Error messages should appear when the user has interacted with the field, not immediately on page load.

### Example: Field Errors

```ts
import { Component } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-contact-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="contactForm" (ngSubmit)="submit()">
      <label>
        Name
        <input formControlName="name" />
      </label>
      @if (name.invalid && name.touched) {
        <p>Name is required.</p>
      }

      <label>
        Email
        <input formControlName="email" />
      </label>
      @if (email.invalid && email.touched) {
        <p>Please enter a valid email.</p>
      }

      <button type="submit" [disabled]="contactForm.invalid">Send</button>
    </form>
  `
})
export class ContactFormComponent {
  contactForm = this.fb.group({
    name: ['', Validators.required],
    email: ['', [Validators.required, Validators.email]]
  });

  constructor(private fb: FormBuilder) {}

  get name() {
    return this.contactForm.get('name')!;
  }

  get email() {
    return this.contactForm.get('email')!;
  }

  submit() {
    if (this.contactForm.invalid) {
      this.contactForm.markAllAsTouched();
      return;
    }
  }
}
```

### When to Show Errors

A common rule is:
- Show errors after a field is touched.
- Show errors after submit if the form is invalid.
- Avoid showing errors before the user has had a chance to type.

This keeps the form helpful instead of noisy.

### Common Error Types

- Required.
- Invalid email.
- Too short.
- Pattern mismatch.
- Passwords do not match.

### Practical Example: Password Match

```ts
import { Component } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-register-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="registerForm" (ngSubmit)="submit()">
      <input formControlName="password" type="password" placeholder="Password" />
      @if (password.invalid && password.touched) {
        <p>Password is required.</p>
      }

      <input formControlName="confirmPassword" type="password" placeholder="Confirm password" />
      @if (registerForm.errors?.['mismatch'] && confirmPassword.touched) {
        <p>Passwords do not match.</p>
      }

      <button type="submit" [disabled]="registerForm.invalid">Register</button>
    </form>
  `
})
export class RegisterFormComponent {
  registerForm = this.fb.group(
    {
      password: ['', Validators.required],
      confirmPassword: ['', Validators.required]
    },
    { validators: this.passwordMatchValidator }
  );

  constructor(private fb: FormBuilder) {}

  get password() {
    return this.registerForm.get('password')!;
  }

  get confirmPassword() {
    return this.registerForm.get('confirmPassword')!;
  }

  passwordMatchValidator(group: any) {
    const password = group.get('password')?.value;
    const confirmPassword = group.get('confirmPassword')?.value;
    return password === confirmPassword ? null : { mismatch: true };
  }

  submit() {
    if (this.registerForm.invalid) {
      this.registerForm.markAllAsTouched();
      return;
    }
  }
}
```

### Best Practices

- Keep messages short.
- Place the message near the field.
- Show one clear error at a time if possible.
- Disable submit when the form is invalid.
- Mark all fields as touched on submit.
- Use user-friendly language instead of technical jargon.

### Easy Challenges

- Show a required-field message.
- Show an invalid email message.
- Disable the submit button when the form is invalid.
- Display an error only after a field is touched.
- Mark the form as touched on submit.

### Medium Challenges

- Add error messages to a login form.
- Build a signup form with password validation.
- Show one custom form-level error.
- Add feedback for empty and invalid fields.
- Style errors so they stand out clearly.

### Hard Challenges

- Build a multi-field registration form with custom validation.
- Add a password confirmation check.
- Show errors only after touch or submit.
- Create reusable helper text for validation messages.
- Refactor a form so validation logic is easy to read.

### Reflection Questions

- Why should validation messages not show too early?
- What is the difference between field-level and form-level validation?
- Why is `markAllAsTouched()` useful?
- How do error messages improve UX?
- When should a form-level validator be used?

### Day Deliverable

Create one reactive form that includes:

- At least two validated fields.
- Visible error messages.
- Conditional error display logic.
- Disabled submit when invalid.
- One custom validation rule or mismatch check.

### Suggested Practice Flow

1. Add validators to your form.
2. Create error messages for each field.
3. Show messages only when needed.
4. Add submit-time validation handling.
5. Add one custom validator.
6. Complete the easy, medium, and hard challenges.

### Note for Day 23

Next day will cover custom validators and reusable validation logic, which will help you keep your form rules clean and consistent.
