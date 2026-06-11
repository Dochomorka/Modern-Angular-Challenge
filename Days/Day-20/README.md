## Day 20 — Forms Basics

Day 20 is about learning how Angular forms work and how to capture user input cleanly. The main idea is to connect input fields to component state so you can read, validate, and submit user-entered data.

### Goal

By the end of this day, you should be able to:

- Understand what Angular forms are for.
- Use basic form controls like inputs, textareas, and selects.
- Bind form values to component state.
- Handle form submission.
- Understand the difference between template-driven and reactive forms at a basic level.
- Build a simple form with validation.

### Why This Matters

Forms are one of the most common parts of any app. Login screens, search fields, checkout forms, profile editors, and comment boxes all depend on good form handling.

A solid form setup helps you:
- Capture user input.
- Validate data before sending it.
- Give clear feedback.
- Prevent bad submissions.

### Basic Form Ideas

At a minimum, a form has:
- Inputs.
- A submit action.
- A model for the values.
- Validation or checks.

A simple Angular form usually connects the template to component state and then reacts when the user submits it.

### Example: Simple Contact Form

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-contact-form',
  standalone: true,
  template: `
    <form (ngSubmit)="submit()">
      <label>
        Name
        <input type="text" [(ngModel)]="name" name="name" />
      </label>

      <label>
        Message
        <textarea [(ngModel)]="message" name="message"></textarea>
      </label>

      <button type="submit">Send</button>
    </form>

    @if (submitted) {
      <p>Thanks, {{ name }}. Your message was sent.</p>
    }
  `
})
export class ContactFormComponent {
  name = '';
  message = '';
  submitted = false;

  submit() {
    this.submitted = true;
  }
}
```

### What to Notice

- The form uses `ngSubmit` for submission.
- The fields are connected to component properties.
- The component stores the submitted state.
- The template shows feedback after submit.

### Template-Driven vs Reactive

There are two common form styles in Angular:

- **Template-driven forms**: simpler, more HTML-focused, good for small forms.
- **Reactive forms**: more explicit and scalable, good for complex forms.

For now, the key idea is to understand that Angular gives you a structured way to manage user input instead of reading values manually from the DOM.

### Validation Basics

Forms should usually check for required values before submission.

Common validation ideas:
- Required fields.
- Minimum length.
- Email format.
- Disabled submit button until valid.

Example:

```html
<input type="text" name="name" [(ngModel)]="name" required />
```

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-login-form',
  standalone: true,
  template: `
    <form (ngSubmit)="login()">
      <label>
        Email
        <input type="email" [(ngModel)]="email" name="email" />
      </label>

      <label>
        Password
        <input type="password" [(ngModel)]="password" name="password" />
      </label>

      <button type="submit">Login</button>
    </form>

    @if (submitted) {
      <p>Login submitted for {{ email }}</p>
    }
  `
})
export class LoginFormComponent {
  email = '';
  password = '';
  submitted = false;

  login() {
    this.submitted = true;
  }
}
```

### Easy Challenges

- Create a form with one input.
- Add a submit button.
- Store the input value in a component property.
- Show a message after submit.
- Add a second field to the form.

### Medium Challenges

- Build a contact form with name, email, and message.
- Add a required field check.
- Show submitted data on the page.
- Disable submit until fields are filled.
- Create a select dropdown in the form.

### Hard Challenges

- Build a small login form with validation.
- Create a profile form with multiple fields.
- Show error messages when inputs are empty.
- Refactor a form to keep all state in one component.
- Prepare the form to send data to an API later.

### Reflection Questions

- Why are forms important in Angular apps?
- What is the purpose of `ngSubmit`?
- Why is validation important before submit?
- When should you use template-driven forms?
- What kinds of forms does your app need most often?

### Day Deliverable

Create one form that includes:

- At least two inputs.
- A submit button.
- Component state for the values.
- A submitted or success message.
- At least one validation check.

### Suggested Practice Flow

1. Build a simple form.
2. Connect fields to component state.
3. Add submit handling.
4. Add a validation check.
5. Show feedback after submit.
6. Complete the easy, medium, and hard challenges.

### Note for Day 21

Next day will cover reactive forms in more depth, which is the better fit for larger and more structured form logic.
