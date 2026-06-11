## Day 18 — HttpClient and API Requests

Day 18 is about learning how Angular communicates with backend APIs using `HttpClient`. Angular’s HTTP docs explain that `HttpClient` is the built-in client API for downloading and uploading data, and in modern Angular it is configured with `provideHttpClient()` in the application providers [web:195][web:189][web:192].

### Goal

By the end of this day, you should be able to:

- Understand what `HttpClient` does.
- Configure HTTP support in a modern Angular app.
- Make GET requests.
- Understand typed responses.
- Subscribe to HTTP results.
- Handle basic success and error states.
- Use API data inside a component.

Angular’s HTTP client is designed for typed responses, error handling, request interception, and testing support [web:195][web:191][web:192].

### Why HttpClient Matters

Most Angular apps need to load data from a server. That could be users, tasks, products, posts, or authentication data. `HttpClient` gives you a clean way to make these requests and work with the response in your components and services [web:195][web:191].

This matters because:
- You can connect your app to real data.
- You can keep API logic inside services.
- You can handle loading and errors more cleanly.
- You can type your backend data.

### Setup

In modern Angular, `HttpClient` is enabled by providing it in your application config [web:189][web:192].

```ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()]
};
```

Once this is added, `HttpClient` can be injected into services or components [web:189][web:192].

### Basic GET Request

A service is the best place for API calls.

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
export class PostService {
  private http = inject(HttpClient);
  private url = 'https://jsonplaceholder.typicode.com/posts';

  getPosts(): Observable<Post[]> {
    return this.http.get<Post[]>(this.url);
  }
}
```

The HTTP docs emphasize typed response values and streamlined error handling [web:195][web:191].

### Using Data in a Component

```ts
import { Component, inject } from '@angular/core';
import { AsyncPipe } from '@angular/common';
import { PostService } from './post.service';

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
            <li>
              <h3>{{ post.title }}</h3>
              <p>{{ post.body }}</p>
            </li>
          }
        </ul>
      }
    </section>
  `
})
export class PostListComponent {
  private postService = inject(PostService);
  posts$ = this.postService.getPosts();
}
```

This approach keeps the component focused on displaying data while the service handles the request [web:195][web:189].

### Subscribing Manually

You can also subscribe in a component if needed.

```ts
import { Component, inject, OnInit } from '@angular/core';
import { PostService } from './post.service';

@Component({
  selector: 'app-post-list',
  standalone: true,
  template: `<p>{{ count }} posts loaded</p>`
})
export class PostListComponent implements OnInit {
  private postService = inject(PostService);
  count = 0;

  ngOnInit() {
    this.postService.getPosts().subscribe(posts => {
      this.count = posts.length;
    });
  }
}
```

For simple display cases, an `Observable` plus `AsyncPipe` is often cleaner, but manual subscription is still useful when you need custom behavior [web:195][web:191].

### Error Handling

HTTP requests can fail, so your code should be ready for errors.

```ts
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { catchError, of } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class SafePostService {
  private http = inject(HttpClient);

  getPosts() {
    return this.http.get<any[]>('https://jsonplaceholder.typicode.com/posts').pipe(
      catchError(error => {
        console.error('Request failed', error);
        return of([]);
      })
    );
  }
}
```

Angular’s HTTP guide highlights streamlined error handling as one of the client’s major features [web:195][web:191].

### Practical Example

```ts
import { Component, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AsyncPipe } from '@angular/common';
import { Observable } from 'rxjs';

interface User {
  id: number;
  name: string;
  email: string;
}

@Component({
  selector: 'app-users',
  standalone: true,
  imports: [AsyncPipe],
  template: `
    <section>
      <h2>Users</h2>

      @if (users$ | async; as users) {
        <ul>
          @for (user of users; track user.id) {
            <li>
              <strong>{{ user.name }}</strong> — {{ user.email }}
            </li>
          }
        </ul>
      }
    </section>
  `
})
export class UsersComponent {
  private http = inject(HttpClient);
  users$: Observable<User[]> =
    this.http.get<User[]>('https://jsonplaceholder.typicode.com/users');
}
```

This example shows a fully typed request and template display flow [web:195][web:191].

### HttpClient Best Practices

- Put API calls in services.
- Use typed interfaces for response data.
- Prefer `Observable`-based flows.
- Handle errors with `catchError`.
- Keep loading and display logic in the component.
- Use `provideHttpClient()` in app configuration [web:189][web:192][web:195].

### Easy Challenges

- Configure `provideHttpClient()` in the app.
- Make one GET request to a public API.
- Define an interface for a response object.
- Display the number of items loaded.
- Use `AsyncPipe` to render API results.

### Medium Challenges

- Create a service that fetches posts.
- Show a loading message before data arrives.
- Display API data in a list.
- Add basic error handling with `catchError`.
- Create a second endpoint request for another resource.

### Hard Challenges

- Build a mini posts or users dashboard with live API data.
- Create a reusable API service with typed methods.
- Add loading, success, and error states to a component.
- Fetch detail data based on a route parameter.
- Refactor manual subscriptions into observable-driven templates.

### Reflection Questions

- Why should HTTP calls usually live in a service?
- Why are typed responses useful?
- When should you use `AsyncPipe` instead of manual subscription?
- What belongs in error handling?
- Why is `provideHttpClient()` important in modern Angular?

### Day Deliverable

Create one service and one component that:

- Makes a GET request.
- Uses a typed interface.
- Displays the response in the template.
- Handles loading or error states.
- Uses `provideHttpClient()` in app setup.

### Suggested Practice Flow

1. Configure HTTP support in the app.
2. Create a service for API calls.
3. Define response interfaces.
4. Load and display data in a component.
5. Add error handling.
6. Complete the easy, medium, and hard challenges.

### Note for Day 19

Next day will cover loading states and error handling in more depth, which will make your API screens feel more complete and user-friendly [web:195][web:189].
