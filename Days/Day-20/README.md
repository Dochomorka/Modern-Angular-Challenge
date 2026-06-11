# Day 20 — Interceptors and Request Patterns

Day 20 is about using Angular HTTP interceptors to apply shared behavior to all requests in one place. Interceptors can inspect or transform outgoing requests and incoming responses, which makes them ideal for auth headers, logging, error handling, and request-wide patterns [web:199][web:215][web:192].

## Goal

By the end of this day, you should be able to:

- Understand what an interceptor does.
- Add a header to outgoing requests.
- Attach an auth token globally.
- Log requests and responses.
- Handle common errors centrally.
- Recognize when request patterns belong in an interceptor instead of a service.

## Why This Matters

Without interceptors, the same request setup often gets repeated in many services. That usually means duplicated headers, duplicated auth logic, and inconsistent error handling across the app [web:199][web:201].

Interceptors help you:
- Keep services smaller.
- Centralize cross-cutting HTTP behavior.
- Make auth and logging consistent.
- Simplify API request code.

## Basic Idea

An interceptor sits in the HTTP pipeline. It receives a request, can clone and modify it, and then passes it on to the next handler. Angular’s interceptor docs explain that most interceptors transform the outgoing request before forwarding it [web:199][web:215].

## Example: Add an Auth Header

```ts
import { HttpInterceptorFn } from '@angular/common/http';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = 'demo-token';

  const modifiedReq = req.clone({
    setHeaders: {
      Authorization: `Bearer ${token}`
    }
  });

  return next(modifiedReq);
};
```

This adds the same authorization header to every request handled by the interceptor [web:199][web:201].

## Example: Log Requests

```ts
import { HttpInterceptorFn } from '@angular/common/http';
import { tap } from 'rxjs';

export const loggingInterceptor: HttpInterceptorFn = (req, next) => {
  console.log('Request:', req.method, req.url);

  return next(req).pipe(
    tap({
      next: () => console.log('Response received'),
      error: () => console.log('Request failed')
    })
  );
};
```

This is useful when you want to trace HTTP activity in one place [web:199][web:201].

## Example: Handle Errors

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

This keeps shared HTTP error behavior out of individual services [web:199][web:215].

## Registering Interceptors

Modern Angular allows interceptors to be registered through `provideHttpClient(...)` using `withInterceptors(...)` [web:192][web:189].

```ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { authInterceptor } from './auth.interceptor';
import { loggingInterceptor } from './logging.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withInterceptors([authInterceptor, loggingInterceptor])
    )
  ]
};
```

## Request Pattern Ideas

Interceptors are a good place for patterns that apply to many requests:
- Attach auth tokens.
- Add common headers.
- Log traffic.
- Normalize errors.
- Retry failed requests in special cases.

## Best Practices

- Clone requests before modifying them.
- Keep each interceptor focused on one concern.
- Use interceptors for shared HTTP behavior, not feature-specific logic.
- Keep services responsible for feature requests.
- Centralize auth, logging, retries, and error handling where appropriate.

## Easy Challenges

- Add one custom header to all requests.
- Log request URLs in the console.
- Print a message when a response arrives.
- Create a simple auth token interceptor.
- Make one interceptor and test it with a GET request.

## Medium Challenges

- Add an auth header globally.
- Create separate logging and error interceptors.
- Show a different message for failed requests.
- Apply a custom app header to all HTTP calls.
- Move repeated request setup out of a service.

## Hard Challenges

- Build an interceptor chain with auth, logging, and error handling.
- Add retry behavior for failed requests.
- Centralize unauthorized handling.
- Refactor a service so it no longer repeats headers.
- Design a request layer that can support future auth refresh logic.

## Reflection Questions

- Why are interceptors better than repeating logic in services?
- What should be modified in an interceptor?
- Why do you need to clone a request?
- What kinds of behavior belong in interceptors?
- How do interceptors improve maintainability?

## Day Deliverable

Create one interceptor that:

- Modifies outgoing requests.
- Adds a shared header or token.
- Logs or handles errors.
- Applies globally.
- Keeps service code cleaner.

## Suggested Practice Flow

1. Pick one shared HTTP concern.
2. Write one interceptor for it.
3. Register it in the app.
4. Test it with a real request.
5. Add logging or error behavior if useful.
6. Complete the easy, medium, and hard challenges.

## Note for Day 21

Next day covers **Observables and streams**, which is the RxJS foundation for Angular’s async workflows.
