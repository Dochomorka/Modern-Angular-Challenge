## Day 35 — Syncing UI State Across Views

Day 35 is about keeping the same UI state in sync across multiple components or screens. The goal is to make selections, filters, tabs, or other shared UI choices behave consistently no matter where they are changed.

### Goal

By the end of this day, you should be able to:

- Understand what synced UI state is.
- Keep selected UI values consistent across views.
- Share filters, tabs, or theme state.
- Update state from more than one component.
- Keep different parts of the UI aligned.
- Avoid duplicated UI state.

### Why This Matters

In many apps, one change affects more than one part of the screen. A selected filter might change a list and a summary card. A theme switch might affect the whole app. A selected tab might need to stay active as you move through related views.

This matters because:
- The app feels consistent.
- Users do not see conflicting UI.
- You avoid storing the same value in multiple places.
- Shared UI behavior becomes easier to manage.

### Common Synced UI Cases

- Selected tabs.
- Active filters.
- Theme mode.
- Sort order.
- Search query.
- Selected item or category.

### Basic Sync Pattern

The usual pattern is:
- Store the state in one shared place.
- Read it from every component that needs it.
- Update it from whichever component changes it.
- Let the UI react automatically.

### Example Idea

A filter bar and a result list can share the same search query. When the user types in the filter bar, the results update. If another component also shows the query, it stays in sync because both read from the same source of truth.

### Practical Example

```ts
import { Injectable, signal } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UiStateService {
  selectedTab = signal('overview');
  theme = signal('light');

  setTab(tab: string) {
    this.selectedTab.set(tab);
  }

  toggleTheme() {
    this.theme.set(this.theme() === 'light' ? 'dark' : 'light');
  }
}
```

One component can change the tab, and another can display the same current tab.

```ts
import { Component, inject } from '@angular/core';
import { UiStateService } from './ui-state.service';

@Component({
  selector: 'app-tab-bar',
  standalone: true,
  template: `
    <button (click)="ui.setTab('overview')">Overview</button>
    <button (click)="ui.setTab('details')">Details</button>
    <button (click)="ui.setTab('settings')">Settings</button>
  `
})
export class TabBarComponent {
  ui = inject(UiStateService);
}
```

```ts
import { Component, inject } from '@angular/core';
import { UiStateService } from './ui-state.service';

@Component({
  selector: 'app-tab-view',
  standalone: true,
  template: `
    <p>Current tab: {{ ui.selectedTab() }}</p>
    <p>Theme: {{ ui.theme() }}</p>
  `
})
export class TabViewComponent {
  ui = inject(UiStateService);
}
```

### Good Sync Use Cases

Use synced UI state when:
- Multiple parts of the app should reflect the same choice.
- The state is small and simple.
- The state is part of the user experience.
- You want one source of truth.

### When Not to Sync Everything

Not every value should be shared. If a value is only meaningful inside one component, keep it local. Syncing too much state can make the app harder to reason about.

### Best Practices

- Keep the synced state small.
- Use one shared owner for the data.
- Update state through methods.
- Avoid duplicate local copies of the same value.
- Derive display data instead of storing it separately.
- Use clear names like `selectedTab`, `theme`, or `query`.

### Easy Challenges

- Share one selected tab across two components.
- Sync a light/dark theme value.
- Keep one search query visible in two places.
- Update shared UI state from a button.
- Display the same selected item in multiple components.

### Medium Challenges

- Build a filter bar and results list with shared query state.
- Create a tab system that stays in sync across views.
- Add a theme toggle that updates the whole layout.
- Share a sort order across two components.
- Keep a selected category synced across a header and content area.

### Hard Challenges

- Build a dashboard with shared filter and sort state.
- Sync selected tabs across multiple nested views.
- Create a theme and layout preference state.
- Refactor duplicated local state into one shared UI service.
- Keep summary cards and list views aligned through one shared selection.

### Reflection Questions

- What kinds of UI state are worth syncing?
- Why should synced values have one owner?
- When is local state still better?
- How do shared methods help keep UI in sync?
- What happens when the same UI value is stored in two places?

### Day Deliverable

Create one shared UI state service and two components that:

- Read the same UI value.
- Update that value from at least one component.
- Stay visually in sync.
- Use one source of truth.
- Keep the logic simple.

### Suggested Practice Flow

1. Pick one shared UI choice.
2. Put it in a shared service.
3. Read it from two components.
4. Update it from one or both components.
5. Verify that both views stay aligned.
6. Complete the easy, medium, and hard challenges.

### Note for Day 36

Next day will cover performance basics, which will help you keep Angular apps fast as shared state and component interactions grow.
