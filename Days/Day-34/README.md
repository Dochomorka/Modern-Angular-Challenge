# Day 34 — Component Testing

Day 34 is about testing Angular components so you can verify what they render and how they behave. Angular’s testing guides explain that component tests commonly use `TestBed`, component fixtures, and DOM queries to exercise a component in isolation [web:356][web:359][web:352].

## Goal

By the end of this day, you should be able to:

- Create a component test with `TestBed`.
- Render a component in a test.
- Query DOM elements in the fixture.
- Trigger user-like interactions.
- Assert component output and state.
- Stub dependencies when needed.

## Why This Matters

Component tests help you catch UI regressions early. Angular’s docs show that testing components is a core part of Angular testing and is usually done with a fixture that lets you interact with the rendered component [web:356][web:359].

This matters because:
- UI behavior can be tested directly.
- You can verify component output.
- Tests help prevent accidental breakage.
- Components are easier to refactor safely.

## Core Idea

A component test creates the component, runs change detection, and checks what the user would see in the DOM. Angular’s testing references and standalone-component guidance show that `TestBed.createComponent` is the main entry point for a basic component fixture [web:356][web:352].

## Basic Test Setup

```ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { MyComponent } from './my.component';

describe('MyComponent', () => {
  let fixture: ComponentFixture<MyComponent>;

  beforeEach(() => {
    fixture = TestBed.createComponent(MyComponent);
    fixture.detectChanges();
  });

  it('renders text', () => {
    const el = fixture.debugElement.query(By.css('h1')).nativeElement;
    expect(el.textContent).toContain('Hello');
  });
});
```

## Testing Interactions

You can trigger button clicks, input changes, and other user actions by interacting with the DOM and then running change detection again [web:356][web:353].

```ts
it('increments the counter', () => {
  const button = fixture.debugElement.query(By.css('button')).nativeElement;
  button.click();
  fixture.detectChanges();

  expect(fixture.componentInstance.count).toBe(1);
});
```

## Stubbing Dependencies

If your component depends on child components or services, Angular’s testing docs show that you can replace those dependencies using `TestBed.overrideComponent` or other test doubles [web:352].

This is useful when:
- The child component is not relevant to the test.
- You want a focused test.
- You need to isolate the unit under test.

## Practical Example

```ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { CounterComponent } from './counter.component';

describe('CounterComponent', () => {
  let fixture: ComponentFixture<CounterComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [CounterComponent]
    });

    fixture = TestBed.createComponent(CounterComponent);
    fixture.detectChanges();
  });

  it('shows the initial count', () => {
    const countEl = fixture.debugElement.query(By.css('[data-testid="count"]')).nativeElement;
    expect(countEl.textContent).toContain('0');
  });

  it('increments when clicked', () => {
    const button = fixture.debugElement.query(By.css('button')).nativeElement;
    button.click();
    fixture.detectChanges();

    expect(fixture.componentInstance.count).toBe(1);
  });
});
```

## Best Practices

- Test what the user can see or do.
- Keep tests focused on one behavior.
- Use `data-testid` when selectors need stability.
- Stub only what is necessary.
- Prefer readable assertions over internal details.

## Easy Challenges

- Render a component in a test.
- Check that text appears in the DOM.
- Click a button and verify a value changes.
- Test a simple input binding.
- Assert that a list renders items.

## Medium Challenges

- Test a component with one child dependency.
- Stub a service in a component test.
- Verify conditional rendering.
- Test a click handler.
- Check class or attribute changes.

## Hard Challenges

- Test a component with multiple DOM states.
- Stub a child component with `overrideComponent`.
- Test form-related rendering behavior.
- Verify interaction plus output together.
- Refactor repeated test setup into helpers.

## Reflection Questions

- What does `TestBed.createComponent` do?
- Why test the DOM instead of only the class?
- When should you stub dependencies?
- What makes a component test stable?
- What should a component test avoid?

## Day Deliverable

Create one component test suite that includes:

- One rendering assertion.
- One interaction assertion.
- One DOM query.
- One fixture setup.
- One isolated component behavior.

## Suggested Practice Flow

1. Create a component test file.
2. Render the component with `TestBed`.
3. Query the DOM.
4. Trigger an interaction.
5. Assert the result.
6. Complete the easy, medium, and hard challenges.

## Note for Day 35

Next day covers **Service and RxJS Testing**, which is the next step in testing shared logic and streams.
