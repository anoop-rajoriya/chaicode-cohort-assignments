![Banner Diagram](./banner.png)

# JavaScript Promises Explained for Beginners [Live](https://dev.to/anoop-rajoriya/javascript-promises-explained-for-beginners-1big)

Think Javascript Promises as promise you makes in real life. Like you promise your friend to buy a coffee, they don't have coffee now but they have guarantee you will apear with a fresh coffee or a come back with excuse to why cafe was closed.

In code promise help use to manage tasks which takes time like fetching data from server or leading heavy image wihout freezing the entire code while we waiting.

## Content List 
- [What Problem Promises Solve](#what-problem-promises-solve)
- [Promise States (Pending, Fulfilled, Rejected)](#promise-states-pending-fulfilled-rejected)
- [Basic Promise Lifecycle](#basic-promise-lifecycle)
- [Handling Success and Failure](#handling-success-and-failure)
- [Promise Chaining Concept](#promise-chaining-concept)

## What Problem Promises Solve
Before promises javascript used callbacks to perform tasks. If you need to do three taks depends on each other like logic user, fetch user profile, fetch user posts then you would end up nesting function inside funciton inside function.

This creates infamous **callback hell** also known as **pyramid of doom**. The code become nearly impossible to read, incredibly fregile and nightmare to debugg because error handling had to reapeted at every level.

Promise solve this by **flattening** instead of nesting. Which makes code more linear and readable way that looks much more like standard top to bottom code.

## Promise States (Pending, Fulfilled, Rejected)
At any given movment the promise exist in one of the three state. Think it like ordering pizza.

- **Pending:** its a initial process, you had ordered a pizza but it has not arrive yet, the process still in progress.
- **Fulfilled (resolved):** success, the pizza arived. The process completed succefully, you had the result.
- **Rejected:** the bad news state, may be the shope ran out pepperoni or the delivery driver got lost. The operation failed and you have the reason (the error).

Once a promise either be resolved or rejected it considred settled, it can not change its state again. you can't un-eat the pizza or have it fail after it already succeeded.

## Basic Promise Lifecycle
The lifecycle starts with its creations and ends with its consuptions.

1. **Creation:** you create a promise using `new Promise()` constructor, it takes an callback called executor, which runs immeditaly. This function have 2 tools `resolve` and `reject`.
2. **Execution:** inside that executor you performd you heavy lifting like api calls etc.
3. **Resolution:** if the everything goes correctly you call `reslve(value)` and if someting goes wrong you call `reject(error)`

```js
const myPromise = new Promise((resolve, reject) => {
    const success = true; 
    if (success) {
        resolve("The operation worked!");
    } else {
        reject("Something went wrong.");
    }
});
```

## Handling Success and Failure
Once a promise created you need a way to say "okay, now what", here the consumer comes in, you use a specific methods to listen for the promise settle.

- `.then(data)`: it runs when the promise **fulfilled**, it recieves data you paased into `resolve(data)`
- `.catch()`: it runs when the promise **rejected**, it catchs the error so your whole program does not crash.
- `.finally()`: This runs no matter what happens (sucess or failure), it's great for cleanup tasks like removing loading spinner.

```js
myPromise
    .then((data) => {
        console.log(data); // "The operation worked!"
    })
    .catch((err) => {
        console.error(err); // "Something went wrong."
    })
    .finally(() => {
        console.log("All done!");
    });
```

## Promise Chaining Concept
This is the superpower of promises, When you return a value or a another promise from `then()`. It automatically wrap that value in new promise. This allow you to link multiple asynchronous steps together in a vertical chain.

instead of nesting, you simply keep adding `.then()` blocks, if any step in chain will fail it skip all remaining `.then()` blocks and jump to the nearest `.catch()` block.

```js
fetchUser(1)
    .then(user => fetchPosts(user.id)) // Returns a new promise
    .then(posts => console.log(posts))  // Handles the result of fetchPosts
    .catch(err => console.log(err));    // Catches errors from ANY of the above steps
```

By using chaining you code stays clean, and you logic stay linear.

## Summary
Promises manage asynchronous tasks, replacing messy "callback hell" with clean, readable code. They exist in three states: **pending**, **fulfilled**, or **rejected**. Once settled, they use `.then()` for success and `.catch()` for errors, allowing developers to "chain" multiple operations together linearly for better control and error handling.