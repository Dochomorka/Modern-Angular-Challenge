## Day 25 — File Uploads and Media Inputs

Day 25 is about handling files in Angular forms, such as profile images, documents, attachments, and media. The main idea is to let users select files, preview them when needed, and prepare them for upload to a server.

### Goal

By the end of this day, you should be able to:

- Understand how file inputs work.
- Read selected files from an input element.
- Store file data in component state.
- Show a basic file preview.
- Prepare files for upload.
- Validate file type or size at a basic level.

### Why This Matters

Many real apps need file handling: avatars, resumes, receipts, screenshots, and product images. File inputs are slightly different from text inputs because you usually work with the selected file object instead of a simple string.

This matters because:
- Users can attach media easily.
- Forms become more practical.
- You can preview uploads before sending them.
- You can validate file size and type early.

### Basic File Input

A file input lets the user choose one or more files from their device.

```html
<input type="file" (change)="onFileSelected($event)" />
```

In Angular, you usually capture the change event and read the selected file from `event.target.files`.

### Example: Single File Selection

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-file-upload',
  standalone: true,
  template: `
    <input type="file" (change)="onFileSelected($event)" />

    @if (fileName) {
      <p>Selected file: {{ fileName }}</p>
    }
  `
})
export class FileUploadComponent {
  fileName = '';

  onFileSelected(event: Event) {
    const input = event.target as HTMLInputElement;
    const file = input.files?.;

    if (file) {
      this.fileName = file.name;
    }
  }
}
```

### File Preview

For image uploads, you can create a simple preview using `FileReader`.

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-image-upload',
  standalone: true,
  template: `
    <input type="file" accept="image/*" (change)="onImageSelected($event)" />

    @if (previewUrl) {
      <img [src]="previewUrl" alt="Preview" width="200" />
    }
  `
})
export class ImageUploadComponent {
  previewUrl = '';

  onImageSelected(event: Event) {
    const input = event.target as HTMLInputElement;
    const file = input.files?.;

    if (!file) return;

    const reader = new FileReader();
    reader.onload = () => {
      this.previewUrl = String(reader.result);
    };
    reader.readAsDataURL(file);
  }
}
```

### Validation Basics

You can check:
- File type.
- File size.
- Number of files selected.

For example, you might reject files larger than a certain size or allow only images.

### Practical Example

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-profile-photo',
  standalone: true,
  template: `
    <form>
      <label>
        Upload profile photo
        <input type="file" accept="image/*" (change)="onFileSelected($event)" />
      </label>

      @if (error) {
        <p>{{ error }}</p>
      }

      @if (previewUrl) {
        <img [src]="previewUrl" alt="Profile preview" width="180" />
      }

      @if (fileName) {
        <p>{{ fileName }}</p>
      }
    </form>
  `
})
export class ProfilePhotoComponent {
  fileName = '';
  previewUrl = '';
  error = '';

  onFileSelected(event: Event) {
    this.error = '';
    this.previewUrl = '';

    const input = event.target as HTMLInputElement;
    const file = input.files?.;

    if (!file) return;

    if (!file.type.startsWith('image/')) {
      this.error = 'Please select an image file.';
      return;
    }

    if (file.size > 2 * 1024 * 1024) {
      this.error = 'File must be smaller than 2 MB.';
      return;
    }

    this.fileName = file.name;

    const reader = new FileReader();
    reader.onload = () => {
      this.previewUrl = String(reader.result);
    };
    reader.readAsDataURL(file);
  }
}
```

### Upload Preparation

When you're ready to send a file to the server, you usually build a `FormData` object and append the file to it. That object can then be sent with an HTTP request later.

### Best Practices

- Use `accept` to guide file selection.
- Validate file type and size early.
- Show a preview for images when useful.
- Reset errors when a new file is selected.
- Keep file handling logic in the component or a service.
- Be careful with memory if you generate object URLs.

### Easy Challenges

- Add a file input to a component.
- Display the selected file name.
- Restrict the file input to images.
- Show an error for invalid file types.
- Add a preview for one image.

### Medium Challenges

- Build an avatar upload component.
- Validate file size before previewing.
- Support both image preview and file name display.
- Allow replacing a selected file.
- Create a simple attachment picker.

### Hard Challenges

- Build a profile photo upload flow with validation and preview.
- Allow multiple file selection.
- Show a list of selected files.
- Create a form with text fields plus file input.
- Prepare a `FormData` payload for future API upload.

### Reflection Questions

- Why is file input handling different from text input handling?
- When should you validate file type and size?
- Why is previewing an image useful?
- What is the role of `FormData`?
- When would you move file handling into a service?

### Day Deliverable

Create one file upload component that includes:

- A file input.
- File type or size validation.
- A file name display.
- Optional image preview.
- Clear error handling.

### Suggested Practice Flow

1. Add a file input.
2. Read the selected file.
3. Display the file name.
4. Add validation.
5. Add preview support for images.
6. Complete the easy, medium, and hard challenges.

### Note for Day 26

Next day will cover submitting forms and sending data to APIs, which is the natural next step after handling file uploads and inputs.
