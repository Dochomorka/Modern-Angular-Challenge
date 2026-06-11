## Day 33 — Local State Patterns

Day 33 is about keeping state inside a component when that state does not need to be shared anywhere else. The main idea is to manage small, UI-focused data locally so your app stays simple and easy to follow.

### Goal

By the end of this day, you should be able to:

- Understand when state should stay local.
- Keep UI state inside a component.
- Use local state for simple interactions.
- Separate component state from shared app state.
- Avoid moving everything into services.
- Make component logic easier to read.

### Why This Matters

Not every piece of state needs a service. If the state only matters to one component, keeping it local is usually the cleanest choice.

This matters because:
- The code stays simpler.
- The component is easier to understand.
- You avoid unnecessary abstraction.
- The UI logic stays close to the template.

### Good Uses for Local State

Local state is a good fit for:
- Toggle buttons.
- Open or closed menus.
- Tabs.
- Selected items in one component.
- Form field values in simple forms.
- Loading flags for a single component.
- Temporary messages or alerts.

### Local State in Practice

A local state component might look like this:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-toggle-panel',
  standalone: true,
  template: `
    <button (click)="toggle()">Toggle Details</button>

    @if (open) {
      <p>These details are only needed in this component.</p>
    }
  `
})
export class TogglePanelComponent {
  open = false;

  toggle() {
    this.open = !this.open;
  }
}
```

The state is simple, self-contained, and does not need to be shared.

### When to Keep State Local

Keep state local when:
- Only one component uses it.
- The state is temporary.
- The state does not affect other parts of the app.
- The logic is small and easy to understand.
- A service would add more complexity than value.

### Common Local State Patterns

#### Boolean toggle
Useful for showing or hiding things.

```ts
isOpen = false;

toggle() {
  this.isOpen = !this.isOpen;
}
```

#### Selected item
Useful for tabs or lists.

```ts
selectedTab = 'home';
```

#### Inline form state
Useful for very small forms or simple input tracking.

```ts
name = '';
```

#### Temporary UI message
Useful for success messages or warnings.

```ts
message = '';
```

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-faq-item',
  standalone: true,
  template: `
    <article>
      <h3>{{ question }}</h3>
      <button (click)="toggleAnswer()">
        {{ showAnswer ? 'Hide' : 'Show' }} Answer
      </button>

      @if (showAnswer) {
        <p>{{ answer }}</p>
      }
    </article>
  `
})
export class FaqItemComponent {
  question = 'What is local state?';
  answer = 'State that only matters inside one component.';
  showAnswer = false;

  toggleAnswer() {
    this.showAnswer = !this.showAnswer;
  }
}
```

### Local State vs Shared State

| Type | Use It For | Best Place |
|---|---|---|
| Local state | One component only | Component class |
| Shared state | Multiple components | Service or shared store |
| Server state | API data | Service with HTTP/RxJS |

### Best Practices

- Keep local state small.
- Put state near the UI that uses it.
- Use clear names like `open`, `selected`, or `loading`.
- Avoid turning simple UI state into a service.
- Move state out only when more than one component needs it.
- Keep update methods small and direct.

### Easy Challenges

- Create a toggle component.
- Add local loading state to one component.
- Build a selected-tab UI with local state.
- Show or hide a message with a boolean.
- Track a single input value inside a component.

### Medium Challenges

- Build a FAQ item with expandable answer state.
- Create a local tab switcher.
- Add a temporary success message after a button click.
- Build a small counter that stays inside one component.
- Create a local search field with display feedback.

### Hard Challenges

- Build a dashboard card with multiple local UI states.
- Add a local modal open/close state.
- Create a small form that keeps all its state local.
- Refactor an overengineered service-based UI into local component state.
- Separate local UI state from shared feature state in one screen.

### Reflection Questions

- When is local state the best choice?
- What kinds of data should not be moved into a service?
- Why is local state easier to understand?
- How do you decide whether state should be shared?
- What problems happen when state is abstracted too early?

### Day Deliverable

Create one component that includes:

- At least one local state value.
- One user action that updates it.
- One conditional UI branch based on that state.
- A clear reason why the state stays local.
- No unnecessary service for the state.

### Suggested Practice Flow

1. Pick one UI-only behavior.
2. Keep the state inside the component.
3. Add a method to update it.
4. Bind it in the template.
5. Test the interaction.
6. Complete the easy, medium, and hard challenges.

### Note for Day 34

Next day will cover shared state patterns, which help when multiple components need the same data.
