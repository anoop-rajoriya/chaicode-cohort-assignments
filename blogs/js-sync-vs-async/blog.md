![Banner Diagram](./banner.png)

# Synchronous vs Asynchronous JavaScript [Live](https://dev.to/anoop-rajoriya/synchronous-vs-asynchronous-javascript-4oh)

To understand javascript you have to understand its **single threaded** nature. Imagin a chef in tiny kitchen with only single burner, that burner is the **javascript engine** and how the chef manage burner heat which determins either restaurant runs smoothly or ends up in a shouting match.

## Content List

- [What synchronous code means](#what-synchronous-code-means)
- [What Asynchronous Code Means](#what-asynchronous-code-means)
- [Why JavaScript Needs Asynchronous Behavior](#why-javascript-needs-asynchronous-behavior)
- [Examples: Timers and API Calls](#examples-timers-and-api-calls)
- [Problems with Blocking Code](#problems-with-blocking-code)

## What synchronous code means
Synchronous or sync is the default behaviour of the javascript, its a top to bottom, line by line execution of the code, in that each operaiton must be finish before next one starts.

- **Call Stack:** a stack data structured by the js we called it call stack, it used to keep track what happening. When operation execution start it pushed into the call stack and after finishing it pop out from it.
- **Sequential Nature:** if line 2 takes 10 seconds to finish the line 3 not run until those 10 seconds are up.

## What Asynchronous Code Means
Asynchronous or async code allow javascript to intiat a task and move to the next one without waiting for completion of first task. When long running task completed it notify js engine to handle the result.

This achieves with the help of **Event Loop** which cordinate between callstack, web apis, and the callback queues.

Non-blocking code which doesn't stop the execution of the script. Javscript hands off the heavy lifting task like file operations, network calling or times to the environment we called it **delegation**.

## Why JavaScript Needs Asynchronous Behavior
If javascript were purly synchronous it will be useless to the moderen browsers in web development.

Since javascript has only one main thread. Any heavy task would freeze the entire browser or programm execution known as **Single Threaded Promblem**.

Asynchronous code allow us to perform any heavy lifting task in the background without intruppting user's experience.

## Examples: Timers and API Calls

**Timer `setTimeout()`**, even if you set timer for 0 milliseconds it becomes asynchronous code.

```js
console.log("Start");

setTimeout(() => {
  console.log("Inside Timeout");
}, 0);

console.log("End");

// Output:
// Start
// End
// Inside Timeout
```

The `setTimeout()` handed callback to the browser timer api and finishing remaing code after event loop push back on to the call stack.

**Api call `fetch()`**, if you make a data request to the server you don't now how much time response take to come back the the client is 10 milliseconds or 10 seconds.

```js
console.log("Fetching data...");

fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log("Data received:", data));

console.log("Doing other things...");
```

The network request handled by the network api which push the result into call stack when it finished, not block the other execution.

## Problems with Blocking Code
Blocking code occure when syncronous code takes too long time which hold the main thread prevent to executre other tasks.

**Frozen UI**, the browser rendering engine usually run on the main thread, if the thread block the browser can not repaint the screen. To user side it looks applicaiton chashed.

**Lagginess**, even minor blocking can cause "junk" - stammering animation make ui cheaper or broken.

**Stack Overflow**, excessive synchronous recursion can fill the call stack and exceed its limits which crashing script entirely.

**Comparison Table:**

| Feature | Synchronous | Asynchronous |
|---------|-------------|--------------|
| Execution | Line-by-line (Sequential) | Concurrent-ish (Non-blocking) |
| Wait time | Blocks until finished | Moves to next task immediately |
| Usage | "Simple logic, math, variable setup" | "Data fetching, Timers, File I/O" |
| Complexity | Easy to read and debug | Requires Promises or Async/Await |

## Summary
Synchronous JavaScript executes code sequentially, blocking the thread until tasks finish. Asynchronous code prevents UI freezes by offloading long-running operations—like API calls or timers—to the browser environment. Managed by the Event Loop, this non-blocking approach ensures applications stay responsive while handling background tasks, crucial for modern web performance.