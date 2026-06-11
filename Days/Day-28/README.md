## Day 28 — Interceptors

Day 28 is about interceptors, which let you apply shared behavior to every HTTP request and response in one place. In Angular, interceptors are a standard way to modify requests, attach headers, handle auth tokens, or centralize error handling before the request reaches the server or before the response reaches your app.

### Goal

By the end of this day, you should be able to:

- Understand what an interceptor does.
- Attach headers to outgoing requests.
- Add an auth token to requests.
- Handle errors in one place.
- Log or inspect requests globally.
- Keep HTTP behavior consistent across the app.

### Why This Matters

As your app grows, many requests need the same setup. You might need to send a token on every request, add a content type header, or handle expired sessions. Interceptors prevent you from repeating that code in every service.

They help you:
- Reduce duplication.
- Keep services smaller.
- Make auth behavior consistent.
- Centralize cross-cutting HTTP concerns.

### Basic Idea

An interceptor sits between your app and the server. It can:

- Read the outgoing request.
- Clone and modify it.
- Pass it along.
- Inspect the response or error.

### Example: Adding a Header

```ts
import { HttpInterceptorFn } from '@angular/common/http';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = 'demo-token';

  const authReq = req.clone({
    setHeaders: {
      Authorization: `Bearer ${token}`
    }
  });

  return next(authReq);
};
```

This example adds an authorization header to every request that passes through it.

### Registering the Interceptor

Interceptors are registered when you configure HTTP support for the app. Once registered, they apply automatically to matching requests.

### Error Handling Idea

You can also use an interceptor to watch for request failures and handle common cases, like showing a message or redirecting after unauthorized access.

```ts
import { HttpInterceptorFn, HttpErrorResponse } from '@angular/common/http';
import { catchError, throwError } from 'rxjs';

export const errorInterceptor: HttpInterceptorFn = (req, next) => {
  return next(req).pipe(
    catchError((error: HttpErrorResponse) => {
      console.error('HTTP error:', error.message);
      return throwError(() => error);
    })
  );
};
```

### Logging Requests

Interceptors are also useful for debugging.

```ts
import { HttpInterceptorFn } from '@angular/common/http';
import { tap } from 'rxjs';

export const logInterceptor: HttpInterceptorFn = (req, next) => {
  console.log('Request:', req.method, req.url);
  return next(req).pipe(
    tap({
      next: () => console.log('Response received'),
      error: () => console.log('Request failed')
    })
  );
};
```

### Practical Example

```ts
import { HttpInterceptorFn } from '@angular/common/http';

export const simpleInterceptor: HttpInterceptorFn = (req, next) => {
  const modified = req.clone({
    setHeaders: {
      'X-App-Source': 'AngularApp'
    }
  });

  return next(modified);
};
```

This kind of shared request behavior is ideal for app-wide setup.

### Best Practices

- Use interceptors for shared HTTP behavior.
- Keep them small and focused.
- Clone requests before modifying them.
- Avoid putting feature-specific logic in interceptors.
- Use them for auth, logging, headers, and global error handling.
- Do not overload a single interceptor with too many responsibilities.

### Easy Challenges

- Add one header to every request.
- Create a logging interceptor.
- Print each request URL in the console.
- Add a token header to outgoing requests.
- Use one interceptor in the app setup.

### Medium Challenges

- Create an auth interceptor.
- Add a custom app header globally.
- Handle unauthorized errors in one place.
- Build a logging and error interceptor pair.
- Apply one interceptor only to specific request behavior.

### Hard Challenges

- Combine auth, logging, and error handling across the app.
- Add a refresh-token flow idea to your interceptor design.
- Refactor repeated header logic out of services.
- Centralize request tracing for debugging.
- Build an interceptor strategy for protected API routes.

### Reflection Questions

- Why are interceptors useful in Angular?
- What should be handled in an interceptor instead of a service?
- Why must requests be cloned before modification?
- What kinds of cross-cutting concerns belong here?
- How do interceptors reduce repetitive code?

### Day Deliverable

Create one interceptor that:

- Modifies outgoing requests.
- Adds a shared header or token.
- Logs or handles an error.
- Applies globally to HTTP calls.
- Keeps service code cleaner.

### Suggested Practice Flow

1. Decide what shared behavior you want.
2. Write a small interceptor.
3. Register it in the app.
4. Test it with one API request.
5. Add logging or error handling if needed.
6. Complete the easy, medium, and hard challenges.

### Note for Day 29

Next day will cover RxJS fundamentals, which will help you understand the observable streams used by HTTP requests, forms, and many Angular patterns.
