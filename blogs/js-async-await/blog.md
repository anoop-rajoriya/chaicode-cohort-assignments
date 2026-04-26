![Banener Diagram](./banner.png)

# Async/Await in JavaScript: Writing Cleaner Asynchronous Code [Live](https://dev.to/anoop-rajoriya/asyncawait-in-javascript-writing-cleaner-asynchronous-code-3e8a)

If you ever lost youself forest of `.then()` or trapped in the **callback hell** then `async/await` is about to become you'r best friend. It introduced in ES2017, it's ofthen known as **syntatic sugar** because it does not change how javascript work under the hood, its only make it easier for us humans to read.

## Content List
- [Why async/await was introduced](#why-asyncawait-was-introduced)
- [How Async Functions Work](#how-async-functions-work)
- [The await Keyword Concept](#the-await-keyword-concept)
- [Error Handling with Async Code](#error-handling-with-async-code)
- [Comparison: Promises vs. Async/Await](#comparison-promises-vs-asyncawait)

## Why async/await was introduced
Before the `asyn/await`, we handle asynchronous operation by callbacks and then promises were introduced. While promises were a massive upgrade but it have some issues:

- **Chaining complexisty:** long chaining of `.then()` become difficult to follow, especially when passing data between them.
- **Verbosity:** you had write a lot of boiler plate just to write a simple data fetch.
- **Debugging:** error handling with `.catch()` was something tricky and stack traces inside promises chain were not alway helpful.

`async/await` was created to make asynchronous code look and behave like a synchronous code, reduceing cognitive load and making logic flow linearly.

## How Async Functions Work
To use this featuer, you start with `async` keyword by placing it before the function declaration, arrow function and methods.

The most important thing is `async` function always return a `pormise`. Even you return a simple string or number it authomatically wrap it by resolved promise and if you throw any error then it return a rejected promise.

```js
async function greet() {
  return "Hello, World!"; 
}

// This is equivalent to:
// return Promise.resolve("Hello, World!");

greet().then(val => console.log(val)); // "Hello, World!"
```

## The `await` Keyword Concept
The `await` keyword is the magical ingrediant of the `async` function. It tell the javascript to paused the specific awaited code inside async function untile this promise settles.

While the function freez but you rest of applicaiton dose not freez, js engine free go to do other things because pause is the non-blocking to main thread.

`await` is only used inside the `async` function or at top level of a module in modern environments.

```js
async function fetchData() {
  console.log("Fetching...");
  
  // Execution pauses here until the fetch promise resolves
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  
  console.log("Data received:", data);
}
```

## Error Handling with Async Code
The one of the biggest wins is it allow us to use standard `try...catch` blocks. This is the same pattern used in synchronous code, making our error handling logic consistance across entire codebase.

instead of attaching a `.catch()` block you wrape you awated calls inside the `try` block and handle error in `catch` block.

```js
async function getUserData() {
  try {
    const response = await fetch('https://api.example.com/user');
    if (!response.ok) throw new Error("Network response was not ok");
    
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Oops! Something went wrong:", error.message);
  }
}
```

## Comparison: Promises vs. Async/Await
To see the clear difference lets look at the same task handled in both ways:

**1. Using Promises:**

```js
function getRecipe() {
  fetch('/api/ingredients')
    .then(res => res.json())
    .then(ingredients => {
      return fetch(`/api/cook/${ingredients[0]}`);
    })
    .then(dish => console.log(dish))
    .catch(err => console.error(err));
}
```
**2. Using Async/Await:**

```js
async function getRecipe() {
  try {
    const res = await fetch('/api/ingredients');
    const ingredients = await res.json();
    const dish = await fetch(`/api/cook/${ingredients[0]}`);
    console.log(dish);
  } catch (err) {
    console.error(err);
  }
}
```

| Feature | Promise (`.then`) | Async/Await |
|---------|-------------------|-------------|
| Readability | become nested or overly long | more lenear, looks like synchronous code|
| Error Handling | Uses `.catch` | Uses `try...catch` |
| Conditionals | hard to handle logic inside chains | stanard `if...else` work naturally |
| Debugging | harder to set breakpoints in chains | easy to set breakpoints on each line |

## Summary
Async/await makes asynchronous JavaScript look synchronous, replacing messy Promise chains. The `async` keyword ensures functions return Promises, while `await` pauses execution non-blockingly until tasks finish. This enables standard `try...catch` error handling, making code cleaner, easier to debug, and more readable than traditional `.then()` syntax.