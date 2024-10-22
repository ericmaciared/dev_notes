# RxJS Guide

## Table of Contents

- [Chapter 1 - Introduction to RxJS](#chapter-1---introduction-to-rxjs)
- [Chapter 2 - Installing and Setting Up RxJS](#chapter-2---installing-and-setting-up-rxjs)
- [Chapter 3 - Observables and Subscriptions](#chapter-3---observables-and-subscriptions)
- [Chapter 4 - Creating Observables](#chapter-4---creating-observables)
- [Chapter 5 - Operators](#chapter-5---operators)
- [Chapter 6 - Subjects and Multicasting](#chapter-6---subjects-and-multicasting)
- [Chapter 7 - Error Handling in RxJS](#chapter-7---error-handling-in-rxjs)
- [Chapter 8 - Schedulers](#chapter-8---schedulers)
- [Chapter 9 - Higher-Order Observables](#chapter-9---higher-order-observables)
- [Chapter 10 - Testing RxJS Code](#chapter-10---testing-rxjs-code)
- [Chapter 11 - Best Practices with RxJS](#chapter-11---best-practices-with-rxjs)
- [Chapter 12 - Integrating RxJS with Angular](#chapter-12---integrating-rxjs-with-angular)
- [Chapter 13 - Common Patterns and Recipes](#chapter-13---common-patterns-and-recipes)
- [Chapter 14 - Performance Optimization](#chapter-14---performance-optimization)
- [Chapter 15 - Advanced Topics in RxJS](#chapter-15---advanced-topics-in-rxjs)

---

## Chapter 1 - Introduction to RxJS

RxJS (Reactive Extensions for JavaScript) is a library for reactive programming using Observables, to make it easier to compose asynchronous or callback-based code.

**Why Use RxJS?**

- ⚡ **Asynchronous Programming Made Easy:** Handle asynchronous data streams with ease.
- 🔄 **Event Handling and Data Streams:** Work with events, messages, and values over time.
- 🛠️ **Powerful Operators:** Transform, filter, and combine streams with a rich set of operators.
- 🌐 **Cross-Platform Compatibility:** Works with Node.js and browser applications.

---

## Chapter 2 - Installing and Setting Up RxJS

To start using RxJS, you need to install it in your project.

**Installing via npm:**

```bash
npm install rxjs
```

**Importing RxJS:**

For RxJS version 6 and above, import operators and functions individually.

```javascript
import { Observable, of } from 'rxjs';
import { map, filter } from 'rxjs/operators';
```

**Using RxJS in the Browser:**

You can include RxJS via a CDN for quick testing.

```html
<script src="https://unpkg.com/rxjs/bundles/rxjs.umd.min.js"></script>
<script>
  const { rxjs } = window;
  const { of } = rxjs;
</script>
```

---

## Chapter 3 - Observables and Subscriptions

Observables are the core primitive in RxJS, representing a stream of data over time.

**Creating an Observable:**

```javascript
const observable = new Observable(subscriber => {
  subscriber.next('Hello');
  subscriber.next('World');
  subscriber.complete();
});
```

**Subscribing to an Observable:**

```javascript
observable.subscribe({
  next(value) { console.log(value); },
  error(err) { console.error('Error:', err); },
  complete() { console.log('Completed'); }
});
```

**Simplified Subscription:**

```javascript
observable.subscribe(
  value => console.log(value),
  err => console.error('Error:', err),
  () => console.log('Completed')
);
```

**Unsubscribing:**

```javascript
const subscription = observable.subscribe(value => console.log(value));
// Later...
subscription.unsubscribe();
```

---

## Chapter 4 - Creating Observables

RxJS provides several creation functions to make Observables.

**`of` Operator:**

Emits the arguments you provide.

```javascript
import { of } from 'rxjs';

const numbers$ = of(1, 2, 3);
```

**`from` Operator:**

Converts various other objects and data types into Observables.

```javascript
import { from } from 'rxjs';

const array$ = from([10, 20, 30]);
```

**`fromEvent` Operator:**

Creates an Observable from DOM events.

```javascript
import { fromEvent } from 'rxjs';

const clicks$ = fromEvent(document, 'click');
```

**`interval` and `timer` Operators:**

Emit values over time.

```javascript
import { interval, timer } from 'rxjs';

const everySecond$ = interval(1000);
const delayed$ = timer(2000, 1000); // Starts after 2s, then every 1s
```

**Custom Observable:**

```javascript
const custom$ = new Observable(subscriber => {
  subscriber.next('Custom value');
  subscriber.complete();
});
```

---

## Chapter 5 - Operators

Operators are functions that enable complex asynchronous code by composing Observables.

**Pipeable Operators:**

Used to transform Observables.

```javascript
import { of } from 'rxjs';
import { map, filter } from 'rxjs/operators';

const numbers$ = of(1, 2, 3, 4, 5);

numbers$
  .pipe(
    filter(n => n % 2 === 0),
    map(n => n * n)
  )
  .subscribe(value => console.log(value)); // Outputs: 4, 16
```

**Common Operators:**

- **`map`**: Transform each value.

  ```javascript
  const doubled$ = numbers$.pipe(map(n => n * 2));
  ```

- **`filter`**: Emit values that pass a predicate.

  ```javascript
  const even$ = numbers$.pipe(filter(n => n % 2 === 0));
  ```

- **`tap`**: Perform side effects.

  ```javascript
  const tapped$ = numbers$.pipe(tap(n => console.log('Processing:', n)));
  ```

- **`take`**: Take the first N values.

  ```javascript
  const firstTwo$ = numbers$.pipe(take(2));
  ```

- **`switchMap`**, **`mergeMap`**, **`concatMap`**: Flatten higher-order Observables.

  ```javascript
  const apiCall$ = userId$.pipe(switchMap(id => fetchUser(id)));
  ```

---

## Chapter 6 - Subjects and Multicasting

Subjects are both an Observable and an Observer, allowing multicasting to multiple subscribers.

**Types of Subjects:**

- **`Subject`**: Basic subject.

  ```javascript
  import { Subject } from 'rxjs';

  const subject = new Subject();

  subject.subscribe(value => console.log('Observer A:', value));
  subject.subscribe(value => console.log('Observer B:', value));

  subject.next(1);
  subject.next(2);
  ```

- **`BehaviorSubject`**: Stores the latest value.

  ```javascript
  import { BehaviorSubject } from 'rxjs';

  const behaviorSubject = new BehaviorSubject(0); // Initial value

  behaviorSubject.subscribe(value => console.log('Observer A:', value));
  behaviorSubject.next(1);
  behaviorSubject.next(2);

  behaviorSubject.subscribe(value => console.log('Observer B:', value)); // Receives 2 immediately
  ```

- **`ReplaySubject`**: Replays specified number of last values.

  ```javascript
  import { ReplaySubject } from 'rxjs';

  const replaySubject = new ReplaySubject(2); // Buffer size of 2

  replaySubject.next(1);
  replaySubject.next(2);
  replaySubject.next(3);

  replaySubject.subscribe(value => console.log('Observer:', value)); // Receives 2 and 3
  ```

- **`AsyncSubject`**: Emits the last value upon completion.

  ```javascript
  import { AsyncSubject } from 'rxjs';

  const asyncSubject = new AsyncSubject();

  asyncSubject.subscribe(value => console.log('Observer:', value));

  asyncSubject.next(1);
  asyncSubject.next(2);
  asyncSubject.complete(); // Emits 2
  ```

---

## Chapter 7 - Error Handling in RxJS

Handle errors gracefully in Observables.

**`catchError` Operator:**

```javascript
import { of, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

const source$ = throwError('An error occurred');

source$
  .pipe(
    catchError(err => {
      console.error(err);
      return of('Fallback value');
    })
  )
  .subscribe(value => console.log(value));
```

**`retry` Operator:**

Automatically resubscribe to an Observable a specified number of times.

```javascript
import { retry } from 'rxjs/operators';

source$
  .pipe(
    retry(3) // Retry up to 3 times
  )
  .subscribe(/* ... */);
```

---

## Chapter 8 - Schedulers

Schedulers control when execution occurs.

**Types of Schedulers:**

- **`asyncScheduler`**: Schedule tasks asynchronously.

  ```javascript
  import { asyncScheduler } from 'rxjs';

  asyncScheduler.schedule(() => console.log('Async task'), 1000);
  ```

- **`queueScheduler`**: Synchronous queue-based execution.

- **`asapScheduler`**: Schedules tasks to happen as soon as possible.

- **`animationFrameScheduler`**: Schedules task to happen inside the next browser animation frame.

**Specifying a Scheduler:**

Some operators accept a scheduler.

```javascript
import { of } from 'rxjs';
import { asyncScheduler } from 'rxjs';

const asyncNumbers$ = of(1, 2, 3, asyncScheduler);
```

---

## Chapter 9 - Higher-Order Observables

Observables that emit Observables.

**Flattening Operators:**

- **`mergeAll`**: Merges multiple Observables into one.

- **`concatAll`**: Concatenates Observables in sequence.

- **`switchAll`**: Switches to new Observable, unsubscribing from previous.

**Using `switchMap`:**

Combines mapping and flattening.

```javascript
import { fromEvent, interval } from 'rxjs';
import { switchMap } from 'rxjs/operators';

const clicks$ = fromEvent(document, 'click');
const result$ = clicks$.pipe(
  switchMap(() => interval(1000))
);

result$.subscribe(value => console.log(value));
```

---

## Chapter 10 - Testing RxJS Code

Testing Observables requires consideration of asynchronous behavior.

**Marble Testing:**

Represents Observable streams with ASCII diagrams.

**Example Using `TestScheduler`:**

```javascript
import { TestScheduler } from 'rxjs/testing';

const testScheduler = new TestScheduler((actual, expected) => {
  // assertion code
});

testScheduler.run(({ cold, expectObservable }) => {
  const source$ = cold('-a--b--|');
  const expected = '-a--b--|';

  expectObservable(source$).toBe(expected);
});
```

**Jest and Jasmine Integration:**

Use async testing functions to handle Observables.

---

## Chapter 11 - Best Practices with RxJS

- 🧹 **Use Pipeable Operators:** Avoid deprecated operators and use the current syntax.
- 🚫 **Avoid Memory Leaks:** Unsubscribe from Observables when no longer needed.
- 🛑 **Use `takeUntil` for Unsubscription:**

  ```javascript
  import { Subject } from 'rxjs';
  import { takeUntil } from 'rxjs/operators';

  const destroy$ = new Subject();

  someObservable$
    .pipe(takeUntil(destroy$))
    .subscribe();

  // On component destroy
  destroy$.next();
  destroy$.complete();
  ```

- ✅ **Prefer Pure Functions:** Keep operators and Observables pure and free of side effects when possible.
- ⚠️ **Error Handling:** Always handle potential errors in Observables.

---

## Chapter 12 - Integrating RxJS with Angular

RxJS is a core part of Angular, particularly in the `HttpClient` and event handling.

**Using Observables with `HttpClient`:**

```typescript
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export class DataService {
  constructor(private http: HttpClient) {}

  fetchData(): Observable<DataType> {
    return this.http.get<DataType>('/api/data');
  }
}
```

**Reactive Forms:**

Listen to value changes with Observables.

```typescript
this.form.get('name').valueChanges.subscribe(value => {
  console.log(value);
});
```

**Using `async` Pipe in Templates:**

Automatically subscribes and unsubscribes.

```html
<div *ngIf="data$ | async as data">
  {{ data }}
</div>
```

---

## Chapter 13 - Common Patterns and Recipes

**Debouncing User Input:**

```javascript
import { fromEvent } from 'rxjs';
import { debounceTime, map } from 'rxjs/operators';

const searchBox = document.getElementById('search');

const keyup$ = fromEvent(searchBox, 'keyup');

keyup$
  .pipe(
    debounceTime(500),
    map(event => event.target.value)
  )
  .subscribe(value => console.log('Search:', value));
```

**Caching HTTP Requests:**

```javascript
import { shareReplay } from 'rxjs/operators';

const cached$ = this.http.get('/api/data').pipe(
  shareReplay(1) // Cache the latest emitted value
);
```

---

## Chapter 14 - Performance Optimization

- ⚡ **Avoid Unnecessary Subscriptions:** Use `async` pipe when possible.
- ♻️ **Use `share` and `shareReplay` Operators:** Share a single subscription among multiple subscribers.
- ⏱️ **Throttle and Debounce:** Control the rate of emissions.
- 📦 **Batching:** Use operators like `bufferTime` to batch emissions.

---

## Chapter 15 - Advanced Topics in RxJS

**Custom Operators:**

Create reusable operators.

```javascript
import { Observable } from 'rxjs';

function multiplyBy(factor) {
  return source => new Observable(observer => {
    return source.subscribe({
      next(value) { observer.next(value * factor); },
      error(err) { observer.error(err); },
      complete() { observer.complete(); }
    });
  });
}

numbers$
  .pipe(multiplyBy(10))
  .subscribe(value => console.log(value));
```

**Higher-Order Mapping:**

Using operators like `exhaustMap`.

**Creating Custom Subjects:**

Implement complex multicasting behaviors.
