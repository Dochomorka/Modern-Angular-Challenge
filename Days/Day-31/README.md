# Day 31 — Validation Patterns

Day 31 is about making Angular form validation more user-friendly and structured. Angular’s validation guide shows how to use validators like `required`, `email`, `minLength`, and `pattern`, and how to display validation feedback based on form state [web:305][web:324].

## Goal

By the end of this day, you should be able to:

- Apply built-in validators consistently.
- Show errors only when appropriate.
- Validate based on touch, dirty, or submit state.
- Use pattern validation for structured input.
- Organize validation messages clearly.
- Improve UX without overcomplicating the form.

## Why This Matters

Validation is not just about blocking bad input. It is also about giving people clear feedback at the right time so forms feel helpful instead of annoying [web:305][web:326][web:328].

This matters because:
- Users need clear guidance.
- Errors should appear at the right moment.
- Reusable validation patterns reduce duplicate code.
- Better validation improves completion rates.

## Common Validation Patterns

Angular supports both reactive and template-driven validation, and its docs show that validators can be combined on controls for stronger rules [web:305][web:324].

Common patterns include:
- Required fields.
- Email format checks.
- Minimum and maximum length.
- Pattern-based validation.
- Conditional error display.

## When To Show Errors

A common pattern is to show errors only when a control has been touched, dirtied, or after submit. That avoids showing messages too early and keeps the form calmer for the user [web:326][web:328].

Typical checks:
- `control.touched`
- `control.dirty`
- `form.submitted`
- `control.invalid`

## Pattern Validation

Pattern validation is useful when input must follow a specific format, such as usernames, phone numbers, or codes. Angular supports `Validators.pattern` for this kind of check [web:325][web:324].

```ts
import { FormControl, Validators } from '@angular/forms';

phone = new FormControl('', [
  Validators.required,
  Validators.pattern(/^[0-9]{10}$/)
]);
```

## Error Message Pattern

A clean approach is to create a helper that decides when to show a message. This keeps the template readable and makes validation logic easier to maintain [web:326][web:328].

```ts
isInvalid(controlName: string): boolean {
  const control = this.form.get(controlName);
  return !!control && control.invalid && (control.touched || control.dirty);
}
```

## Practical Example

```ts
import { Component } from '@angular/core';
import { ReactiveFormsModule, FormGroup, FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-validation-patterns',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <div>
        <input formControlName="username" placeholder="Username" />
        <p *ngIf="showError('username', 'required')">Username is required.</p>
        <p *ngIf="showError('username', 'minlength')">Username must be at least 3 characters.</p>
      </div>

      <div>
        <input formControlName="phone" placeholder="Phone" />
        <p *ngIf="showError('phone', 'required')">Phone is required.</p>
        <p *ngIf="showError('phone', 'pattern')">Phone must be 10 digits.</p>
      </div>

      <button type="submit">Save</button>
    </form>
  `
})
export class ValidationPatternsComponent {
  form = new FormGroup({
    username: new FormControl('', [Validators.required, Validators.minLength(3)]),
    phone: new FormControl('', [Validators.required, Validators.pattern(/^[0-9]{10}$/)])
  });

  showError(controlName: string, errorName: string): boolean {
    const control = this.form.get(controlName);
    return !!control && control.hasError(errorName) && (control.touched || control.dirty);
  }

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      return;
    }
    console.log(this.form.value);
  }
}
```

## Best Practices

- Show errors only after touch, dirty, or submit.
- Keep validation messages short and clear.
- Reuse helper methods for error checks.
- Use pattern validators when format matters.
- Mark all controls as touched on submit if needed.

## Easy Challenges

- Add a required validator to one field.
- Show an error only after touch.
- Add a minimum-length rule.
- Use a pattern validator for a phone number.
- Submit and block invalid forms.

## Medium Challenges

- Build reusable error message logic.
- Add validation for three fields.
- Show one message per error type.
- Mark all controls as touched on failed submit.
- Use both `minlength` and `pattern`.

## Hard Challenges

- Build a full form with multiple validation states.
- Support validation after blur and after submit.
- Create a reusable error helper.
- Show different messages for different rules.
- Refactor the validation logic into a pattern.

## Reflection Questions

- When should an error message appear?
- Why is touch or dirty state useful?
- How does `pattern` validation help?
- Why mark all controls as touched on submit?
- What makes a validation pattern reusable?

## Day Deliverable

Create one form that includes:

- At least two validators.
- Conditional error messages.
- One pattern validator.
- One submit-time validation pattern.
- A reusable error helper or equivalent logic.

## Suggested Practice Flow

1. Add validators to controls.
2. Decide when errors should appear.
3. Add conditional template messages.
4. Test submit behavior.
5. Refactor repeated checks.
6. Complete the easy, medium, and hard challenges.

## Note for Day 32

Next day covers **Custom Validators**, which is where you create your own validation rules for app-specific needs.
