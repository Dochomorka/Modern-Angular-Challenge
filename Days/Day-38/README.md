# Day 38 — Accessibility Basics

Day 38 is about making Angular apps usable for everyone, including keyboard and screen-reader users. Angular’s accessibility guidance, W3C labeling guidance, and Google’s Angular accessibility material all point to the same basics: use semantic HTML, label controls properly, and support keyboard interaction [web:391][web:393][web:394].

## Goal

By the end of this day, you should be able to:

- Use semantic HTML correctly.
- Associate labels with form controls.
- Provide accessible names for interactive elements.
- Support keyboard use.
- Add ARIA only when needed.
- Spot common accessibility mistakes.

## Why This Matters

Accessibility is not only for compliance; it improves usability for more people. The W3C emphasizes proper labeling of controls, and Angular’s accessibility guidance highlights using the framework’s built-in support to build more inclusive apps [web:393][web:391][web:394].

This matters because:
- Screen readers need clear labels.
- Keyboard users need focusable controls.
- Semantic HTML improves navigation.
- Accessible apps are easier for everyone to use.

## Semantic HTML

Use the right HTML element for the job. Buttons should be `<button>`, links should be `<a>`, and form sections should use proper form elements so assistive tech can understand the page structure [web:389][web:391].

Good examples:
- Use `<button>` for actions.
- Use `<a>` for navigation.
- Use headings in order.
- Use landmarks like `<main>`, `<nav>`, and `<header>`.

## Labels And Forms

W3C guidance says labels should be associated with form controls either explicitly with `for` and `id`, or implicitly by wrapping the control [web:393]. Angular accessibility articles also emphasize that form inputs need labels to be understandable to screen readers [web:389][web:396].

```html
<label for="email">Email</label>
<input id="email" type="email" />
```

## Keyboard Support

Interactive elements must work with the keyboard, not just the mouse. Angular accessibility resources recommend making controls focusable and handling keyboard interactions for custom widgets when native HTML is not enough [web:389][web:392][web:394].

Rules of thumb:
- Tab should reach interactive controls.
- Enter and Space should activate controls when appropriate.
- Focus should be visible.
- Do not trap the keyboard unexpectedly.

## ARIA Use

ARIA can help, but it should not replace semantic HTML. Angular accessibility guidance and community resources recommend using ARIA only when native elements cannot express the required behavior [web:391][web:389][web:392].

Good uses include:
- `aria-label` for icon-only buttons.
- `aria-labelledby` for complex labeling.
- `role="dialog"` for custom modals.
- `aria-live` for status messages.

## Practical Example

```html
<form>
  <label for="name">Name</label>
  <input id="name" type="text" />

  <button type="submit">Save</button>
</form>

<button aria-label="Close dialog">×</button>
```

This example uses proper labeling and a clear accessible name for the close button, which follows standard accessibility guidance [web:393][web:391][web:389].

## Best Practices

- Prefer semantic HTML first.
- Label every form control.
- Make custom interactive elements keyboard accessible.
- Use ARIA as a supplement, not a replacement.
- Keep focus states visible.
- Test with keyboard and screen readers.

## Easy Challenges

- Add labels to two inputs.
- Replace a clickable div with a button.
- Add accessible text to an icon button.
- Test tab order in one screen.
- Check heading order on one page.

## Medium Challenges

- Audit one form for missing labels.
- Add `aria-label` to an icon-only action.
- Ensure a modal can be closed by keyboard.
- Improve one navigation landmark.
- Fix one focus visibility issue.

## Hard Challenges

- Make a custom widget keyboard accessible.
- Add `aria-live` to a status message.
- Refactor a form for better labeling.
- Review an entire page with a keyboard only.
- Use a screen reader to catch one usability problem.

## Reflection Questions

- Why is semantic HTML the first choice?
- When should you add ARIA?
- What makes a control accessible to keyboard users?
- Why do labels matter so much?
- How can accessibility improve usability for everyone?

## Day Deliverable

Create one accessible screen that includes:

- Proper labels.
- Semantic elements.
- Keyboard-friendly interactions.
- One ARIA attribute used appropriately.
- One accessibility improvement you can explain.

## Suggested Practice Flow

1. Check semantic HTML first.
2. Label every control.
3. Test keyboard navigation.
4. Add ARIA only where needed.
5. Review focus and visibility.
6. Complete the easy, medium, and hard challenges.

## Note for Day 39

Next day covers **Build and Deployment**, which is the next step in preparing the app for production.
