# Day 35 — Service and RxJS Testing

Day 35 is about testing Angular services and RxJS logic so you can verify data flow, side effects, and stream behavior. Angular’s testing guide and HTTP testing guide show that services are commonly tested with `TestBed`, while `HttpClientTestingController` can mock HTTP requests and let you assert expected calls [web:359][web:351][web:366].

## Goal

By the end of this day, you should be able to:

- Test a service with `TestBed`.
- Mock service dependencies.
- Test HTTP-based service methods.
- Test observable output from a service.
- Verify RxJS stream behavior.
- Use marble-style thinking for stream tests when needed.

## Why This Matters

Services often hold business logic, data fetching, and shared state, so they deserve direct tests. Angular’s testing docs show that services and HTTP interactions can be tested without real network calls, and RxJS testing tools help validate stream behavior more precisely [web:351][web:359][web:363].

This matters because:
- Service logic is often reused.
- Bugs in services affect many components.
- HTTP tests should not depend on real APIs.
- RxJS streams can be tested deterministically.

## Service Testing Basics

A service test usually creates the service with `TestBed`, injects dependencies, and checks the result of a method call. If the service depends on another service, you can replace that dependency with a mock or spy object [web:366][web:361][web:359].

## HTTP Service Testing

Angular’s HTTP testing utilities let you capture requests, inspect them, and provide mock responses [web:351].

Typical flow:
- Set up `HttpClientTestingModule` or the modern testing provider.
- Inject the service.
- Call the method.
- Expect a request.
- Flush a fake response.

## Observable Testing Basics

If a service returns an observable, you can subscribe in the test and assert the emitted values. For more complex timing behavior, RxJS `TestScheduler` and marble diagrams help you test streams in a controlled way [web:363][web:360].

## Example Service

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { map, Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {}

  getUserName(): Observable<string> {
    return this.http.get<{ name: string }>('/api/user').pipe(
      map(user => user.name)
    );
  }
}
```

## Example Service Test

```ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [UserService]
    });

    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('returns the user name', () => {
    service.getUserName().subscribe(name => {
      expect(name).toBe('Alice');
    });

    const req = httpMock.expectOne('/api/user');
    expect(req.request.method).toBe('GET');
    req.flush({ name: 'Alice' });
  });
});
```

This test verifies both the request and the transformed observable result, which is the main value of service testing [web:351][web:366].

## RxJS Stream Test Example

```ts
import { TestScheduler } from 'rxjs/testing';
import { map } from 'rxjs/operators';
import { of } from 'rxjs';

describe('RxJS stream', () => {
  it('maps values correctly', () => {
    const scheduler = new TestScheduler((actual, expected) => {
      expect(actual).toEqual(expected);
    });

    scheduler.run(({ expectObservable }) => {
      const source$ = of(1, 2, 3).pipe(map(x => x * 2));
      expectObservable(source$).toBe('(abc|)', {
        a: 2,
        b: 4,
        c: 6
      });
    });
  });
});
```

## Best Practices

- Test services in isolation when possible.
- Mock HTTP instead of calling real endpoints.
- Assert both emitted values and side effects.
- Keep RxJS tests deterministic.
- Use marble tests for tricky timing or stream logic.

## Easy Challenges

- Test a service method that returns a value.
- Mock one dependency with a spy.
- Test one HTTP GET request.
- Subscribe to one observable in a test.
- Verify a simple mapping step.

## Medium Challenges

- Test a service with two methods.
- Mock an HTTP response.
- Check that a service transforms API data.
- Test a loading flag emitted by a stream.
- Write one `TestScheduler` example.

## Hard Challenges

- Test a service with multiple dependencies.
- Verify error handling in a service.
- Test a stream with timing behavior.
- Use marble diagrams for an RxJS pipeline.
- Refactor repeated test setup into helpers.

## Reflection Questions

- Why test services separately from components?
- Why mock HTTP requests?
- When should you use `TestScheduler`?
- What does `HttpTestingController` help verify?
- Which service logic is best tested directly?

## Day Deliverable

Create one test file that includes:

- One service test.
- One mocked HTTP request.
- One observable assertion.
- One RxJS stream test or equivalent.
- One dependency mock.

## Suggested Practice Flow

1. Create a service test with `TestBed`.
2. Mock HTTP if the service uses it.
3. Assert one emitted value.
4. Add a dependency mock.
5. Test one RxJS pipeline.
6. Complete the easy, medium, and hard challenges.

## Note for Day 36

Next day covers **Performance Optimization**, which is the next step in preparing Angular apps for production.
