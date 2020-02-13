# Exception Handling in JavaScript

> **Reference:** [Exception handling in JavaScript
](https://blog.logrocket.com/exception-handling-in-javascript/) &mdash; Blog post on _LogRocket_ by Deepak Gupta
>
> "When done properly throughout, exception handling can help you improve the maintainability, extensibility, and readability of your code."

- code generates error? -> try to recover as gracefully as possible
- on error, JS interpreter looks for exception handling code
  - if none found, the error causing function gets returned and continues to do so for the remainder of the call stack
  - keeps doing this until EH is found, else program terminates with an error

## Two Ways to Handle Exceptions

### Throwing an Exception

Throw one if a problem can't be handled meaningfully during runtime:

```javascript
function openFile(fileName) {
    if (!exists(fileName)) {
        throw new Error(`Could not find file ${fileName}`);
    }
    ...
}
```

- Throw via statement: `throw "You messed up!"`
  - **Not recommended!** Throw `Error` types rather than strings for more detailed messaging
- `Error` type represents generic exceptions
  - `message` property is the argument we pass to the constructor
    - `throw new Error("My Error message")`
  - `stack` property returns the call stack causing the error

### Catch an Exception

- Thrown exceptions are caught and handled at the place they make the most sense at runtime
- catch exceptions to stop them from crashing the program
- `try-catch-finally` is the simplest way to handle exceptions
  - use when can't check correctness beforehand
  - not best used with asynchronous code (see next section)

```javascript
try {
    openFile('../test.js');
} catch(e) {
    // gracefully handled the thrown expection
}
```

```javascript
try {
    // Code to run
} catch (e) {
    // Code to run if an exception occurs
} finally {
    // (optional)
    // Code that is always executed regardless of an exception occurring
}
```

- `try`: code than can potentially generate exceptions
- `catch`: code that runs when an exception occurs
- `finally`: code that runs whether or not an exception occured
  - **NOTE**: will execute even if `try` or `catch` executes a `return` statement

## How to Handle Exceptions in an Asynchronous Code Block

### Callback Functions **(Not Recommended)**

```javascript\
asyncfunction(code, (err, result) => {
    if(err) return console.error(err);
    console.log(result);
});
```

- With callback functions (not recommended) ,  we usually receive two parameters (such as `err` and `result`)
  - If there is an error, the `err` parameter will be equal to that error, and we can throw the error to do exception handling.
  - Must return `err` to avoid more errors (such as `result` being undefined)
    - can use `if(err) ...` like example OR
    - in an `else` block

### Promises

```javascript
promise.then(onFulfilled, onRejected)
// or just `promise.catch(onRejected)`
```

- With promises — `then` or `catch` — we can process errors by passing an error handler to the `then` method or using a `catch` clause.

### `async` and `await` with `try-catch`

This is a great way to handle exceptions

```javascript
async function() {
    try {
        await someFuncThatThrowsAnError()
    } catch (err) {
        console.error(err)
    }
});
```

## How to Handle Uncaught Exceptions

### In the Browser

When an error occurs at runtime, `window.onerror()` forces the error event to be fired on the window object.

`onerror` can also be used like the below example to do something when an image fails to load.

```html
<img src="testt.jpg" onerror="alert('An error occurred loading yor photo.')" />
```

### On a Node.js Server

- `EventEmitter` module can be subscribed to the `uncaughtException` event.
  - Only works on synchronous code
  - Can pass callback for handling
  - Catching it wouldn't terminate the process
    - hHve to do it manually
- Asynchronous code would use the event `unhandledRejection`

```javascript
process.on("uncaughtException", () => {})
// OR
process.on("unhandledRejection", () => {})
```

**TIP:** it is bad practice to make a catch-all for the generic `Error` type as it would make your code harder to maintain or extend. Also it would be harder to figure out what happened.

## Key Takeaways

- Generate user-defined exceptions with `throw`
  - during runtime, it will look for a `catch` block, otherwise it will terminate
- JS `Error` object is a built-in exception type
- Use `try` to contain code with possibilities of generating exceptions
  - the `catch` will run when the exception is encountered
- For asynchronous code, best to use `async-await` with `try-catch-finally`
- Unhandled exceptions may be caught to avoid the app from crashing.
