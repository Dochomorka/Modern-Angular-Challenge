## Day 21 — Reactive Forms

Day 21 is about learning Angular reactive forms, the more structured and scalable way to handle form state. Reactive forms keep the form model in the component class, which makes validation, updates, and testing easier to manage.

### Goal

By the end of this day, you should be able to:

- Understand what reactive forms are.
- Create a `FormGroup`.
- Create `FormControl` fields.
- Set initial form values.
- Read form values in the component.
- Add validators.
- Handle submit with a structured form model.

### Why This Matters

Reactive forms are a strong fit for medium and large forms because the form structure lives in TypeScript instead of being spread across the template. That makes the logic easier to test and easier to extend as the app grows.

You get:
- More control.
- Better validation structure.
- Clear form state.
- Easier scaling for complex forms.

### Core Pieces

Reactive forms usually use these pieces:

- `FormControl` for a single field.
- `FormGroup` for a set of controls.
- `FormBuilder` to create forms more easily.
- Validators to enforce rules.

### Example: Basic Reactive Form

```ts
import { Component } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-login-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="loginForm" (ngSubmit)="submit()">
      <label>
        Email
        <input type="email" formControlName="email" />
      </label>

      <label>
        Password
        <input type="password" formControlName="password" />
      </label>

      <button type="submit" [disabled]="loginForm.invalid">Login</button>
    </form>

    @if (submitted) {
      <p>Submitted: {{ loginForm.value | json }}</p>
    }
  `
})
export class LoginFormComponent {
  submitted = false;

  loginForm = this.fb.group({
    email: ['', [Validators.required, Validators.email]],
    password: ['', [Validators.required, Validators.minLength(6)]]
  });

  constructor(private fb: FormBuilder) {}

  submit() {
    this.submitted = true;
    console.log(this.loginForm.value);
  }
}
```

### What to Notice

- The form structure is in the component class.
- Each field is explicitly declared.
- Validators are attached in TypeScript.
- The template binds to the form model with `formGroup` and `formControlName`.

### Validation Basics

Reactive forms make validation very clear.

Common validators:
- Required.
- Email.
- Min length.
- Pattern.

You can also inspect control state like:
- `valid`
- `invalid`
- `touched`
- `dirty`
- `errors`

### Practical Example

```ts
import { Component } from '@angular/core';
import { FormBuilder, Validators, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-profile-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="profileForm" (ngSubmit)="save()">
      <label>
        Name
        <input formControlName="name" />
      </label>

      <label>
        Role
        <input formControlName="role" />
      </label>

      <button type="submit" [disabled]="profileForm.invalid">Save</button>
    </form>

    @if (saved) {
      <p>Profile saved for {{ profileForm.value.name }}</p>
    }
  `
})
export class ProfileFormComponent {
  saved = false;

  profileForm = this.fb.group({
    name: ['', Validators.required],
    role: ['', Validators.required]
  });

  constructor(private fb: FormBuilder) {}

  save() {
    this.saved = true;
  }
}
```

### Easy Challenges

- Create a reactive form with one field.
- Add a submit button.
- Use `ReactiveFormsModule`.
- Add one validator.
- Display form values after submit.

### Medium Challenges

- Build a login form with email and password.
- Add required and min-length validation.
- Disable submit when the form is invalid.
- Show a success message after submit.
- Create a form with three fields.

### Hard Challenges

- Build a profile form with validation.
- Add custom error messages.
- Create a form group with multiple related controls.
- Reset the form after submit.
- Prepare the form to submit data to an API.

### Reflection Questions

- Why are reactive forms more scalable?
- Why keep the form model in TypeScript?
- How do validators improve form quality?
- When would a simple form still be enough?
- What makes reactive forms easier to test?

### Day Deliverable

Create one reactive form that includes:

- A `FormGroup`.
- At least two `FormControl` fields.
- At least one validator.
- A submit action.
- A success or feedback message.

### Suggested Practice Flow

1. Create a reactive form.
2. Add controls and validators.
3. Bind the form in the template.
4. Handle submit.
5. Add validation feedback.
6. Complete the easy, medium, and hard challenges.

### Note for Day 22

Next day will cover form validation and error messages in more depth, which will help you make your forms user-friendly and reliable.
