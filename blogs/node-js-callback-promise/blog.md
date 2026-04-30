![Banner Diagram](./banner.png)

# Async Code in Node.js: Callbacks and Promises [Live](https://chaicode-cohort-blogs-with-anoop.hashnode.dev/async-code-in-node-js-callbacks-and-promises)

In the world of node.js, things don't always happens one after another in straight line. If it did you application were incredibly slow. Lets break down how node.js handle mutiple tasks at once without getting overwhelmed.

---

## Content List

- [Why async code exists in Node.js](#why-async-code-exists-in-nodejs)
- [Callback-based async execution](#callback-based-async-execution)
- [Problems with nested callbacks](#problems-with-nested-callbacks)
- [Promise-based async handling](#promise-based-async-handling)
- [Benefits of promises](#benefits-of-promises)

---

## Why async code exists in Node.js

Imagin you are at a busy restorant:

- **Synchronouse (Blocking):** waiter takes you order, go in to kitchen, and stand there staring at the chef untile you food is ready. Only then he bring you order to you table and move to the next customer. The entire restorant grids to halt because the waiter is blocked.

- **Asynchronouse (Non-Blocking):** waiter takes you order, and hands the order slip to the kitchen, and immedialty goes to help to the another talbe. When your food is ready the chef rings the bell, and waiter comes back to serve you.

Node.js is like that efficiant waiter. It is single threaded meaning it can to one thing at a time. If it spend a 5 seconds waiting for a large file to load form a hard drive, it couldn't handle any other users request during that time. Async code allow node.js to "start" a task and move on, coming back when the taks is finished.

## Callback-based async execution

For a long times callback is the only way to handle async tasks in node.js. A callback is simply a function passed as an argument to antoher function, which gets executed once a task completed.

**The File Reading Schenerio:**

Lets assume we need a file to read from hard drive named `config.txt`, using the builtin `fs` file system module. The code looks like that:

```js
const fs = require("fs");

console.log("1. Starting to read file...");

fs.readFile("config.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error found:", err);
    return;
  }
  console.log("3. File content:", data);
});

console.log("2. Moving on to other work!");
```

**Code Flow step-by-step:**

- Node import `fs` module in the current module, print `1. Starting to read file...` on console.
- Node.js call function `fs.readFile`, it tells operating system "Hy go grab this file. Here is a function (a callback) to run when you are done."
- Instead of waiting for the file read, node.js moved to next line of the code and print `2. Moving on to other work!` on console.
- While node.js doing other things, the os finishes file reading it puts the callback funciton into the todo list called **Task Queue**.
- Once Node.js finished with its current scripts, The Event Loop checks the Task Queue, it sees file is ready and finally callback executes and print `3. File content...`.

## Problems with nested callbacks

Callbacks work fine with simple tasks but in real applications several tasks is requried in a row where each task depends on other result like get user profile -> fetch its posts -> fetch comments for each posts.

This lead to a nightmare known as **Callback Hell** or **Pyramid of Doom**.

```js
getData(function (a) {
  getMoreData(a, function (b) {
    getEvenMoreData(b, function (c) {
      getFinalData(c, function (d) {
        console.log(d);
      });
    });
  });
});
```

**The main issue are:**

1. **Readability:** the code moves horizontally rahter than vertically which makes hard to follow the logic.
2. **Error Handling:** you have to check for error at every single level, which makes code bulky and prone to bug.
3. **Debugging:** tracking where error occure in a deep nesting is a headache.

## Promise-based async handling

A promise is a modern javascript object that represent the eventual completion or failure of an asynchronous operation. Think it as literal promise or IOU.

When you call a async function and return a promise. its like ordering a pizza. They give you a pager. The pager (the promise) is currently in a pending state.

1. **Fulfilled:** the pizza is ready (success)
2. **Rejected:** they ran out of dough (failure)

**There how its looks in code:**

```js
const fs = require("fs").promises;

fs.readFile("config.txt", "utf8")
  .then((data) => {
    console.log("File content:", data);
  })
  .catch((err) => {
    console.error("Something went wrong:", err);
  });
```

## Benefits of promises

Promises were introduce to cleanup the mess left out by the callbacks, here is why they are generally prefered:

| **Feature**        | **Callbacks**                       | **Promises**                                            |
| ------------------ | ----------------------------------- | ------------------------------------------------------- |
| **Structure**      | Deeply nested **Pyramid of Doom**   | Linear and flate chaining                               |
| **Error Handling** | must handle error in every callback | only single `.catch()` handle errors in the whole chain |
| **Readability**    | hard to see the main logic path     | its look more like standard top to bottom code          |
| **Controll**       | hard to cordinates multiple tasks   | easy to use `Promise.all()` for parallal tasks          |

step-by-step readability comparison for the same logic with callbacks and promises. **Fetching user, getting their posts, and then fetching comments for the first post.**

**1. Callback Way (Nested):** notice how code moveing to the right side with every step. This makes error handling nightmare because you have to check for error in every single level.

```js
// Mocking an async task with callbacks
function getData(id, callback) {
  setTimeout(() => {
    callback(null, { id: id, name: "User" + id });
  }, 500);
}

// The "Pyramid of Doom"
getData(1, (err, user) => {
  if (err) return console.error(err);

  console.log("User:", user.name);
  getData(user.id, (err, posts) => {
    if (err) return console.error(err);

    console.log("Posts retrieved");
    getData(posts[0], (err, comments) => {
      if (err) return console.error(err);

      console.log("Comments:", comments);
      // It keeps going...
    });
  });
});
```

**2. Promise Way (Chained):** promise allow you to flatten the structure. You return a a promise in the `.then()` and next `.then()` waits for it. Plus one `.catch()` handle error for the entire chain.

```js
// Mocking an async task with Promises
const getDataPromise = (id) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve({ id: id, name: "User" + id }), 500);
  });
};

getDataPromise(1)
  .then((user) => {
    console.log("User:", user.name);
    return getDataPromise(user.id); // Return the next promise
  })
  .then((posts) => {
    console.log("Posts retrieved");
    return getDataPromise(posts[0]);
  })
  .then((comments) => {
    console.log("Comments:", comments);
  })
  .catch((err) => {
    console.error("Something went wrong:", err);
  });
```

## Summary

Node.js uses asynchronous code to prevent its single thread from "blocking" during slow tasks like file reading. Initially, **callbacks** handled this by triggering functions after a task finished. However, complex tasks created "Callback Hell"—messy, nested code that was hard to debug.

**Promises** modernized this by acting as placeholders for future results. They replace deep nesting with **linear chaining** (`.then()`) and simplify error handling with a single `.catch()`. Essentially, promises turn a horizontal "pyramid of doom" into readable, vertical logic, making Node.js apps faster to write and significantly easier to maintain.
