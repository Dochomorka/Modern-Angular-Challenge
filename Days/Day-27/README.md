## Day 27 — API Abstractions and Service Organization

Day 27 is about keeping your API code clean as the app grows. The main idea is to move request logic out of components and into well-structured services so your UI stays simple and your data layer stays reusable.

### Goal

By the end of this day, you should be able to:

- Keep API calls out of components.
- Organize requests inside services.
- Reuse methods for multiple endpoints.
- Separate API logic from UI logic.
- Return typed data from service methods.
- Keep your app easier to test and maintain.

### Why This Matters

Once an app starts talking to several endpoints, component code can get messy fast. If every screen handles its own request logic, you end up with duplication, inconsistent error handling, and harder maintenance.

Good service organization helps you:
- Reuse request logic.
- Centralize error handling.
- Keep components focused on display.
- Make future changes easier.

### Basic Service Structure

A service should usually handle:
- HTTP requests.
- Request URLs.
- Response typing.
- Optional transformation of data.
- Shared helpers for related endpoints.

### Example: Posts Service

```ts
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

interface Post {
  id: number;
  title: string;
  body: string;
}

@Injectable({
  providedIn: 'root'
})
export class PostsService {
  private http = inject(HttpClient);
  private baseUrl = 'https://jsonplaceholder.typicode.com/posts';

  getAll(): Observable<Post[]> {
    return this.http.get<Post[]>(this.baseUrl);
  }

  getById(id: number): Observable<Post> {
    return this.http.get<Post>(`${this.baseUrl}/${id}`);
  }

  create(post: Pick<Post, 'title' | 'body'>): Observable<Post> {
    return this.http.post<Post>(this.baseUrl, post);
  }
}
```

### What to Notice

- The service owns the endpoint details.
- The methods are typed.
- The component does not need to know how the HTTP call works.
- Related request methods live together.

### Keeping Components Thin

A component should mostly:
- Call the service.
- Store UI state.
- Show loading or errors.
- Render the result.

It should not be responsible for request URLs, request formatting, or response transformation unless the logic is very small.

### Example: Component Using the Service

```ts
import { Component, inject } from '@angular/core';
import { AsyncPipe } from '@angular/common';
import { PostsService } from './posts.service';

@Component({
  selector: 'app-post-list',
  standalone: true,
  imports: [AsyncPipe],
  template: `
    <section>
      <h2>Posts</h2>

      @if (posts$ | async; as posts) {
        <ul>
          @for (post of posts; track post.id) {
            <li>{{ post.title }}</li>
          }
        </ul>
      }
    </section>
  `
})
export class PostListComponent {
  private postsService = inject(PostsService);
  posts$ = this.postsService.getAll();
}
```

### Service Organization Tips

- Group related endpoints into one service.
- Use one service per feature or domain.
- Keep names clear and consistent.
- Use helper methods when multiple requests share setup.
- Avoid putting unrelated endpoints in the same file.

Good examples:
- `AuthService` for login and session actions.
- `UsersService` for user profile endpoints.
- `PostsService` for posts and comments.
- `TasksService` for task-related requests.

### Practical Example

```ts
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

interface Task {
  id: number;
  title: string;
  done: boolean;
}

@Injectable({
  providedIn: 'root'
})
export class TasksService {
  private http = inject(HttpClient);
  private baseUrl = '/api/tasks';

  getTasks(): Observable<Task[]> {
    return this.http.get<Task[]>(this.baseUrl);
  }

  addTask(title: string): Observable<Task> {
    return this.http.post<Task>(this.baseUrl, { title, done: false });
  }

  toggleTask(task: Task): Observable<Task> {
    return this.http.put<Task>(`${this.baseUrl}/${task.id}`, {
      ...task,
      done: !task.done
    });
  }

  deleteTask(id: number): Observable<void> {
    return this.http.delete<void>(`${this.baseUrl}/${id}`);
  }
}
```

This keeps all task-related request logic in one place.

### Best Practices

- Put API code in services, not components.
- Type request and response data.
- Keep each service focused on one domain.
- Use small methods with clear names.
- Handle common errors in a shared way when possible.
- Return observables from service methods.

### Easy Challenges

- Move one HTTP call from a component into a service.
- Create one typed `getAll()` method.
- Add one `getById()` method.
- Rename endpoints clearly.
- Use the service from a component template flow.

### Medium Challenges

- Build a `TasksService` with list, create, and delete methods.
- Split auth and task API logic into separate services.
- Add typed interfaces for API responses.
- Keep loading state in the component and requests in the service.
- Create a reusable base URL constant.

### Hard Challenges

- Organize an app into feature-based services.
- Add request transformation inside a service.
- Centralize error mapping for multiple endpoints.
- Create a service that wraps a full CRUD resource.
- Refactor a component-heavy API screen into service-driven design.

### Reflection Questions

- Why should API calls live in services?
- How does service organization reduce duplication?
- When should a service be split into smaller services?
- Why are typed methods helpful?
- What belongs in the component versus the service?

### Day Deliverable

Create one feature service that includes:

- At least two request methods.
- Typed request or response data.
- A clear base URL or endpoint structure.
- One component that consumes the service.
- Minimal API logic inside the component.

### Suggested Practice Flow

1. Identify one feature domain.
2. Move its request logic into a service.
3. Add typed methods.
4. Use the service from a component.
5. Keep the component focused on UI state.
6. Complete the easy, medium, and hard challenges.

### Note for Day 28

Next day will cover interceptors, which help you apply shared request behavior like headers, auth tokens, and error handling across all HTTP calls.
