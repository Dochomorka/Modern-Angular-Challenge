## Day 24 — Form Arrays and Dynamic Forms

Day 24 is about building forms that can grow and shrink at runtime. Form arrays let you manage repeated fields like multiple phone numbers, skills, tags, or task items without hardcoding every input in advance.

### Goal

By the end of this day, you should be able to:

- Understand what a form array is.
- Add items to a dynamic form.
- Remove items from a dynamic form.
- Render repeated controls in the template.
- Build forms for lists of values.
- Use dynamic forms for real-world data entry.

### Why This Matters

Not every form has a fixed number of fields. Sometimes users need to add multiple addresses, several email contacts, or an arbitrary list of skills. Form arrays make this possible without duplicating form code.

They help you:
- Handle flexible input lists.
- Keep the form structure organized.
- Add and remove controls easily.
- Model real-world data better.

### Core Idea

A form array is a list of form controls or form groups. Instead of one static field, you have a collection that can change during user interaction.

### Example: Skills Form

```ts
import { Component } from '@angular/core';
import { FormArray, FormBuilder, ReactiveFormsModule, Validators } from '@angular/forms';

@Component({
  selector: 'app-skills-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <div formArrayName="skills">
        @for (skill of skills.controls; track $index) {
          <div>
            <input [formControlName]="$index" placeholder="Skill" />
            <button type="button" (click)="removeSkill($index)">Remove</button>
          </div>
        }
      </div>

      <button type="button" (click)="addSkill()">Add Skill</button>
      <button type="submit" [disabled]="form.invalid">Save</button>
    </form>
  `
})
export class SkillsFormComponent {
  form = this.fb.group({
    skills: this.fb.array([
      this.fb.control('', Validators.required)
    ])
  });

  constructor(private fb: FormBuilder) {}

  get skills(): FormArray {
    return this.form.get('skills') as FormArray;
  }

  addSkill() {
    this.skills.push(this.fb.control('', Validators.required));
  }

  removeSkill(index: number) {
    this.skills.removeAt(index);
  }

  submit() {
    if (this.form.invalid) {
      this.form.markAllAsTouched();
      return;
    }
  }
}
```

### What to Notice

- `FormArray` holds a dynamic list of controls.
- `addSkill()` appends a new control.
- `removeSkill()` deletes one control.
- The template loops over the array and binds each control by index.

### When to Use Form Arrays

Use form arrays when the number of inputs is not fixed:
- Skills.
- Phone numbers.
- Email addresses.
- Tasks.
- Tags.
- Addresses.

### Practical Example

```ts
import { Component } from '@angular/core';
import { FormArray, FormBuilder, ReactiveFormsModule, Validators } from '@angular/forms';

@Component({
  selector: 'app-task-list-form',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="form" (ngSubmit)="submit()">
      <div formArrayName="tasks">
        @for (task of tasks.controls; track $index) {
          <div>
            <input [formControlName]="$index" placeholder="Task title" />
            <button type="button" (click)="removeTask($index)">Remove</button>
          </div>
        }
      </div>

      <button type="button" (click)="addTask()">Add Task</button>
      <button type="submit">Submit</button>
    </form>
  `
})
export class TaskListFormComponent {
  form = this.fb.group({
    tasks: this.fb.array([
      this.fb.control('Learn form arrays', Validators.required)
    ])
  });

  constructor(private fb: FormBuilder) {}

  get tasks(): FormArray {
    return this.form.get('tasks') as FormArray;
  }

  addTask() {
    this.tasks.push(this.fb.control('', Validators.required));
  }

  removeTask(index: number) {
    this.tasks.removeAt(index);
  }

  submit() {
    console.log(this.form.value);
  }
}
```

### Best Practices

- Keep each dynamic item simple.
- Use `track $index` when looping over form array controls.
- Add validation to each item.
- Prevent the array from becoming empty if your app needs at least one item.
- Use form arrays for repeating values only; use form groups for structured objects.

### Easy Challenges

- Create a form array with two items.
- Add a button to append a new control.
- Add a remove button for each item.
- Display form values after submit.
- Use validation on each array item.

### Medium Challenges

- Build a skills form with add/remove controls.
- Create a phone-number list form.
- Prevent empty values from being submitted.
- Add a minimum of one item.
- Render the array using `@for`.

### Hard Challenges

- Build a dynamic contact form with multiple phone numbers and emails.
- Create a nested form array with groups of objects.
- Add per-item validation messages.
- Refactor a static form into a dynamic one.
- Build a tag editor with add/remove actions.

### Reflection Questions

- Why are form arrays useful?
- When should you use a form array instead of a form group?
- How do add and remove actions change the form structure?
- Why is validation still important in dynamic lists?
- What real app data might benefit from a dynamic form?

### Day Deliverable

Create one dynamic form that includes:

- A form array.
- At least one add action.
- At least one remove action.
- A loop in the template.
- Validation for each repeated item.

### Suggested Practice Flow

1. Choose a repeating field type.
2. Create a form array.
3. Add one control to start.
4. Add and remove controls from the UI.
5. Test validation and submit behavior.
6. Complete the easy, medium, and hard challenges.

### Note for Day 25

Next day will cover file upload and media input handling, which often works alongside dynamic and validated forms.
