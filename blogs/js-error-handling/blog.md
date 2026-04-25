![Banner Diagram](./banner.png)

# Error Handling in JavaScript: Try, Catch, Finally [Live](https://dev.to/anoop-rajoriya/error-handling-in-javascript-try-catch-finally-42j7)

Handling errors is the difference between professional application and one who left user staring at the frozen screen. In javascript things will go wrong - apis will fails, user will provide creative inputs, and variables will be `undfined`

Here is the in depth guide to managing the chaos using `try`, `catch`, `finally`, and custom errors.

## Content List
- [What errors are in JavaScript](#what-errors-are-in-javascript)
- [Using try and catch Blocks](#using-try-and-catch-blocks)
- [The finally Block](#the-finally-block)
- [Throwing Custom Errors](#throwing-custom-errors)
- [Why Error Handling Matters](#why-error-handling-matters)

## What errors are in JavaScript
Error is the object in javascript, when things goes wrong the JS `throws` this object. It contains name (a type or error), message (a human readable description), and a stack tracing (A GPS of code: provide location of error from it generate).

If error did not chaught the js stops the entire exection of programm. There are some common built in errors types:

- **ReferenceError:** using variables that has not been declared.
- **TypeError:** performing operation on wrong data types.
- **SyntaxError:** writing code that js engine can't parse (try/catch can't catch syntax error in same script block because the code will not even run).
- **RangeError:** using a number outside the allowable range.

## Using try and catch Blocks
The `try...catch` statements are you safety net. You wrap the code wich might be fails in the `try` block and defining how you want to handle that failure in `catch` block.

```js
try {
  // Code that might cause an error
  let data = JSON.parse("Invalid JSON String"); 
  console.log("This line will never run.");
} catch (error) {
  // Code to handle the error
  console.error("Oops! Something went wrong:");
  console.error("Message:", error.message); // The error description
  console.error("Name:", error.name);       // The type of error
}
```

The `catch` block only execute if the code in `try` will fails if the code run successfully the `catch` block skipped entirely.

## The `finally` Block
The `finally` block is the "loyal companion" of error handling. It execture not matter what wheter an error was throwen or not, or even you return early form `try` or `catch` blocks.

It primary purpose is cleanup, you placing code which close database connection, stop loading spinners or releasing file handles.

```js
let isLoading = true;

try {
  fetchDataFromAPI();
} catch (err) {
  console.log("Error fetching data:", err);
} finally {
  isLoading = false; 
  console.log("Cleanup complete. Loader hidden.");
}
```

## Throwing Custom Errors
Some times code is technically valid but logically wrong like user entering negative age, you can use `throw` keyword to manually trigger errors.

While you can throw any things like `string`, `number` but it is a good practise to throw `Error Object`

```js
function checkAge(age) {
  if (age < 0) {
    throw new Error("Age cannot be negative!"); // Creating a custom error
  }
  return "Access granted";
}

try {
  checkAge(-5);
} catch (e) {
  console.warn(e.message); // "Age cannot be negative!"
}
```

For advanced use cases you can `extend` Error to create a customized error types like `ValidationError` or `DatabaseError`.

## Why Error Handling Matters
Why not console let to show the error? becuase the error handling is vital for several reasons:

- **Graceful Degradation:** instead of showing white screen, show the user a friendly "Sorry, our servers are napping" message.
- **System Stability:** a single api fails can  should not crash the entire user interface.
- **Easier Debugging:** well-placed `try...catch` blocks can log the data about state of application when it failed, which make it eaier to fix.
- **Security:** by catching errors you can prevent the browers form leaking sensitive stack traces or server-side file paths to attackers.

## Summary

|Block|Execution Rule|Typical Use Case|
|-----|--------------|----------------|
|try|Always runs first.|Code that might fail (API calls, JSON parsing).|
|catch|Runs only if try fails.|Error logging, showing user alerts, retrying logic.|
|finally|Always runs last.|Closing connections, hiding loaders, resetting state.|
|throw|Manual trigger.|Enforcing business logic or custom validation.|
