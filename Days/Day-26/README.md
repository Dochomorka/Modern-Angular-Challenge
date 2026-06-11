## Day 26 — Submitting Forms to APIs

Day 26 is about connecting your forms to a backend so user input can actually be saved. The goal is to take validated form data, send it with an HTTP request, and handle the response cleanly in the UI.

### Goal

By the end of this day, you should be able to:

- Collect form data from a reactive form.
- Send that data to an API.
- Handle success and error responses.
- Show loading while submission is in progress.
- Reset or update the form after success.
- Keep form submission logic organized.

### Why This Matters

A form is only useful when it does something after submission. In real apps, users create accounts, send messages, place orders, update profiles, and upload records through forms.

This matters because:
- Form data reaches your backend.
- The UI can respond to success or failure.
- Users get feedback on what happened.
- Submission logic stays predictable.

### Basic Submission Flow

A simple form submission usually follows this pattern:

1. Validate the form.
2. Mark fields as touched if invalid.
3. Send the data to the API.
4. Show loading while waiting.
5. Handle success or failure.
6. Reset or keep the form based on the result.

### Example: Sending Form Data

```ts
import { Component, inject } from '@angular/core';
import { ReactiveFormsModule, FormBuilder, Validators } from '@angular/forms';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-contact-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <input formControlName="name" placeholder="Name" />
      <input formControlName="email" placeholder="Email" />
      <textarea formControlName="message" placeholder="Message"></textarea>

      <button type="submit" [disabled]="form.invalid || loading">
        {{ loading ? 'Sending...' : 'Send' }}
      </button>
    </form>

    @if (success) {
      <p>Message sent successfully.</p>
    }

    @if (error) {
      <p>Something went wrong.</p>
    }
  `
})
export class ContactFormComponent {
  private fb = inject(FormBuilder);
  private http = inject(HttpClient);

  loading = false;
  success = false;
  error = false;

  form = this.fb.group({
    name: ['', Validators.required],
    email: ['', [Validators.required, Validators.email]],
    message: ['', Validators.required]
  });

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      return;
    }

    this.loading = true;
    this.success = false;
    this.error = false;

    this.http.post('https://example.com/api/contact', this.form.value).subscribe({
      next: () => {
        this.loading = false;
        this.success = true;
        this.form.reset();
      },
      error: () => {
        this.loading = false;
        this.error = true;
      }
    });
  }
}
```

### What to Notice

- The form is validated before submission.
- The submit button is disabled while loading.
- Success and error states are shown separately.
- The form resets after a successful request.

### Submission Best Practices

- Never submit invalid forms.
- Show loading during the request.
- Handle errors gracefully.
- Clear success or error messages before retrying.
- Reset the form only after success if that fits the use case.
- Keep API calls in a service when the app grows.

### Common Submission Cases

- Contact form.
- Login form.
- Signup form.
- Profile update form.
- Checkout form.
- Comment form.

### Practical Example

```ts
import { Component, inject } from '@angular/core';
import { ReactiveFormsModule, FormBuilder, Validators } from '@angular/forms';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-feedback-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <input formControlName="title" placeholder="Title" />
      <textarea formControlName="comment" placeholder="Comment"></textarea>

      <button type="submit" [disabled]="form.invalid || loading">
        Submit Feedback
      </button>
    </form>

    @if (statusMessage) {
      <p>{{ statusMessage }}</p>
    }
  `
})
export class FeedbackFormComponent {
  private fb = inject(FormBuilder);
  private http = inject(HttpClient);

  loading = false;
  statusMessage = '';

  form = this.fb.group({
    title: ['', Validators.required],
    comment: ['', Validators.required]
  });

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      this.statusMessage = 'Please fill in all required fields.';
      return;
    }

    this.loading = true;
    this.statusMessage = '';

    this.http.post('https://example.com/api/feedback', this.form.value).subscribe({
      next: () => {
        this.loading = false;
        this.statusMessage = 'Feedback submitted successfully.';
        this.form.reset();
      },
      error: () => {
        this.loading = false;
        this.statusMessage = 'Submission failed. Please try again.';
      }
    });
  }
}
```

### Easy Challenges

- Submit a valid form to a fake API.
- Disable the submit button while loading.
- Show a success message after submit.
- Show an error message when the request fails.
- Reset the form after success.

### Medium Challenges

- Build a contact form that posts to an endpoint.
- Add loading, success, and error states.
- Move submission logic into a service.
- Show validation errors before sending.
- Keep the form usable after a failed request.

### Hard Challenges

- Build a profile update form with API submission.
- Add form submission with file upload preparation.
- Handle different server error responses.
- Create a reusable submit helper in a service.
- Refactor the form so the component only handles UI state.

### Reflection Questions

- Why should invalid forms not be submitted?
- What should the UI show while a request is in progress?
- When should a form reset after submission?
- Why is it useful to move submission logic into a service?
- How do success and error messages improve UX?

### Day Deliverable

Create one form that includes:

- Validation before submit.
- An HTTP submission request.
- A loading state.
- A success state.
- An error state.

### Suggested Practice Flow

1. Build or reuse a reactive form.
2. Add submit validation.
3. Send form data to an API.
4. Show loading and status messages.
5. Reset the form after success if needed.
6. Complete the easy, medium, and hard challenges.

### Note for Day 27

Next day will cover API abstractions and service organization, which will help you keep request logic clean as your app grows.
