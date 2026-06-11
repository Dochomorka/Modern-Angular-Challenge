## Day 23 — Custom Validators

Day 23 is about creating your own validation rules when Angular’s built-in validators are not enough. Custom validators help you enforce app-specific rules, like password strength, forbidden names, matching fields, or business-specific input formats.

### Goal

By the end of this day, you should be able to:

- Understand when a custom validator is needed.
- Create a reusable validator function.
- Attach a custom validator to a control or form group.
- Return validation errors in the correct format.
- Show custom error messages in the template.
- Keep validation logic separate from UI logic.

### Why This Matters

Built-in validators cover common cases, but real apps usually need custom rules. A booking form might need a date check, a signup form might need a password rule, and a profile form might need forbidden words or unique naming rules.

Custom validators help you:
- Encode business rules.
- Reuse validation logic.
- Keep forms consistent.
- Make error handling more specific.

### Basic Validator Shape

A validator usually checks a value and returns either `null` if it is valid or an error object if it is not.

```ts
export function forbiddenNameValidator(control: any) {
  const value = control.value || '';
  return value.toLowerCase().includes('admin')
    ? { forbiddenName: true }
    : null;
}
```

If the value contains the forbidden text, the validator returns an error.

### Using a Custom Validator

```ts
import { Component } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

export function forbiddenNameValidator(control: any) {
  const value = control.value || '';
  return value.toLowerCase().includes('admin')
    ? { forbiddenName: true }
    : null;
}

@Component({
  selector: 'app-profile-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="profileForm" (ngSubmit)="submit()">
      <input formControlName="username" placeholder="Username" />

      @if (username.touched && username.errors?.['required']) {
        <p>Username is required.</p>
      }
      @if (username.touched && username.errors?.['forbiddenName']) {
        <p>Username cannot contain admin.</p>
      }

      <button type="submit" [disabled]="profileForm.invalid">Save</button>
    </form>
  `
})
export class ProfileFormComponent {
  profileForm = this.fb.group({
    username: ['', [Validators.required, forbiddenNameValidator]]
  });

  constructor(private fb: FormBuilder) {}

  get username() {
    return this.profileForm.get('username')!;
  }

  submit() {
    if (this.profileForm.invalid) {
      this.profileForm.markAllAsTouched();
      return;
    }
  }
}
```

### Control-Level vs Form-Level Validators

Custom validators can be applied to:
- A single field.
- A whole form group.

Use a control-level validator when one field has a custom rule. Use a form-level validator when several fields must work together, like password confirmation or date ranges.

### Example: Form-Level Validator

```ts
export function passwordsMatch(group: any) {
  const password = group.get('password')?.value;
  const confirmPassword = group.get('confirmPassword')?.value;
  return password === confirmPassword ? null : { mismatch: true };
}
```

This is useful when the rule depends on multiple values.

### Practical Example

```ts
import { Component } from '@angular/core';
import { FormBuilder, ReactiveFormsModule, Validators } from '@angular/forms';

function noSpacesValidator(control: any) {
  const value = control.value || '';
  return value.includes(' ') ? { noSpaces: true } : null;
}

@Component({
  selector: 'app-signup-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <label>
        Username
        <input formControlName="username" />
      </label>

      @if (username.touched && username.errors?.['required']) {
        <p>Username is required.</p>
      }
      @if (username.touched && username.errors?.['noSpaces']) {
        <p>Username cannot contain spaces.</p>
      }

      <label>
        Password
        <input type="password" formControlName="password" />
      </label>

      <button type="submit" [disabled]="form.invalid">Create account</button>
    </form>
  `
})
export class SignupFormComponent {
  form = this.fb.group({
    username: ['', [Validators.required, noSpacesValidator]],
    password: ['', [Validators.required, Validators.minLength(6)]]
  });

  constructor(private fb: FormBuilder) {}

  get username() {
    return this.form.get('username')!;
  }

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      return;
    }
  }
}
```

### Best Practices

- Keep validators small and focused.
- Return `null` when valid.
- Return a descriptive error key when invalid.
- Reuse validator functions when possible.
- Keep validation separate from template display logic.
- Use form-level validators for cross-field rules.

### Easy Challenges

- Create a validator that blocks a specific word.
- Add a custom validator to one input.
- Show a custom error message in the template.
- Combine a custom validator with `required`.
- Create a validator that rejects spaces.

### Medium Challenges

- Build a username validator with forbidden names.
- Create a password-strength validator.
- Add a form-level validator for matching passwords.
- Reuse one validator across two forms.
- Show different messages for different custom errors.

### Hard Challenges

- Build a signup form with multiple custom validators.
- Add a date-range validator.
- Create a reusable validator file for your app.
- Validate one field based on another field’s value.
- Refactor custom rules out of the component into shared functions.

### Reflection Questions

- When should you create a custom validator?
- What should a validator return when the value is valid?
- Why are form-level validators useful?
- How do reusable validators improve code quality?
- What is the benefit of keeping validator logic outside the template?

### Day Deliverable

Create one reactive form with:

- At least one custom validator.
- At least one built-in validator.
- A visible custom error message.
- A submit button that respects form validity.
- Optional: one form-level validator.

### Suggested Practice Flow

1. Decide on a custom rule.
2. Write the validator function.
3. Attach it to a control or form group.
4. Show the error message in the template.
5. Test valid and invalid cases.
6. Complete the easy, medium, and hard challenges.

### Note for Day 24

Next day will cover form arrays and dynamic forms, which let you build inputs that can grow or shrink at runtime.
