![Banner Diagram](./banner.png)

# Blocking vs Non-Blocking Code in Node.js [Live](https://chaicode-cohort-blogs-with-anoop.hashnode.dev/blocking-vs-non-blocking-code-in-node-js)

In Node.js, the distinction betweeen blocking and non-blocking code is the differerence between smooth running engine and a massive trafic jam. Because the node.js is a single-threaded, how you handled the time taking tasks determins wheter you code scale or crashes under the pressure.

---

## Content List

- [What blocking code means](#what-is-blocking-code-the-wait-for-me-approach)
- [What non-blocking code means](#what-is-non-blocking-code-the-call-me-maybe-approach)
- [Why blocking slows servers](#why-blocking-slows-servers)
- [Async operations in Node.js](#async-operations-in-nodejs)
- [Real-world examples (file read, DB calls)](#real-world-scenarios-file-handling--db-calls)

---

## What is Blocking Code? (The "Wait for Me" Approach)

Blocking occurs when a execution of additional javascript in the node.js process must wait untile a non-javascript operation completes. This happen because **Event Loop** unable to continue running javascript while a blocking operations is occuring.

In synchronous programming, the execution stay on one line of code until it finishes. If this line takes 10 seconds to finish read a massive data then you whole server freez for those 10 seconds.

**For batter understanding:** imagic a restaurent with only on waiter:

- A customer orders a "Sahi Paneer".
- Waiter goes to the kitchen and stands their staring at the chef untile the "Sahi Paneer" is cooked.
- Only when "Sahi Paneer" cooked and served does the waiter acknowledge the next customer in line.
- Results: line out the door gets very longer, and everyone is hungry and angry.

## What is Non-Blocking Code? (The "Call Me Maybe" Approach)

Non-blocking code allow the execution continue immediatly, instead watiing for a task like reading a file or database query to finish. Node.js initate a task and sets a remainder (a callback, promise, or async/await) and moves on the next line of the code.

**Lets look again** on our restaurent:

- Waiter takes the "Sahi Paneer" order and gives to the kitchen.
- Instead waitng, waiter goes to the next table to take another order.
- When the "Sahi Paneer" ready, the kitchen rings the bell (a callback), and the waiter delivers it between taking the orders.
- Results: many customers servered simultaneously, even though there is still one waiter.

## Why Blocking Slows Servers

Node.js use single thread to handle all requests, this sounds counter-intiutive how can one thread handle thousands of users requests? It does it with the help of \*_Event Loop_ so it don't have to wait.

If you write blocking code like `fs.readFileSync()` you are effectively hijacking that main thread.

- **Bottleneck:** while the thread is busy reading a 2gb file from the disk during it, it cannot responds the new HTTP request form a different user.

- **Performance:** Every other users requests queued, if the one user request takes 500ms, then every subsequents users experiance this 500ms delay. If multiple users trigger blocking tasks, the server latancy compound until it becomes unresponsive.

## Async Operations in Node.js

Node.js provide a asynchronous version of its almost all it's standard library functions. These use underlying system kernal or a thread pool (Libuv) to handle heavy lifting in the background, keeping the main thread free.

**The big Three of async:**

1. Callbacks: the original way, you pass a function that runs when a task is done.
2. Promises: a more robust way to handle successs or failure `.then()` or `.catch()`.
3. Async/Await: Syntatic sugar over promises that makes async code look and behave like a sync code without the breaking penalty.

## Real-World Scenarios: File Handling & DB Calls

### Scenerio 1: File Handling

**Blocking (Synchronous) Simple CLI Tool :**

Application need a config file this cli tool reads and load this file so application run. During the file reading the entire code stops and waits for file completely read and load successfully before moving to the next line.

```js
const fs = require("fs");

console.log("1. Checking configuration...");

// The execution STOPS here until the file is read
const config = fs.readFileSync("config.json", "utf8");

console.log("2. Config loaded:", config);
console.log("3. App starting...");
```

The problem is if there was a web server and file size is large like 5gb then reading and loading file takes much times and the entire server is freez for that duration, no other users can connect untile that file finished loading.

**Non-Blocking (Asynchronous) Profile Uploading :**

The code tell the system "Hey bro, go grab this file and let me inform when you'r done" meanwhile, it keeps running the rest of the script.

In profile uploading the user one uploade high definition profile pic this tasks wouldn't affact the user two who right now registering the account.

```js
const fs = require("fs");

console.log("1. Starting file upload...");

// This runs in the background
fs.readFile("large-image.png", (err, data) => {
  if (err) throw err;
  console.log("3. File upload complete!");
});

console.log("2. Doing other work while we wait...");
```

### Scenerio 2: DB Calls

Imagin a node.js server api serving a product page, there are some actions it need to do for custructing a page with products:

1. Fetching products details in DB (takes 100ms)
2. Fetch related products in DB (takes 100ms)
3. Update users visite counts (takes 50ms)

**Blocking (Synchronous) One Another :**

If the server handles these calles one after another on the main thread, other users waiting for request completion for first user request taking `100ms + 100ms + 50ms` during the all operations.

```js
// Synchronous - Bad Practice
const product = db.querySync("SELECT * FROM products WHERE id = 1"); // 100ms
const related = db.querySync("SELECT * FROM related WHERE id = 1"); // 100ms
db.executeSync("UPDATE visits..."); // 50ms
res.send(product);
// Total time blocked: 250ms+
// Result: Other users trying to visit the site are forced to wait.
```

**Non-Blocking (Asynchronous) One Another :**

If the server starts all operations immediatly and continue to handling other users, and responds when data arrives to first user.

```js
// Asynchronous - Best Practice
app.get("/product/:id", async (req, res) => {
  // Start all requests concurrently
  const productPromise = db.query("SELECT * FROM products..."); // Non-blocking
  const relatedPromise = db.query("SELECT * FROM related..."); // Non-blocking
  db.execute("UPDATE visits..."); // Fire and forget

  // Await them simultaneously
  const [product, related] = await Promise.all([
    productPromise,
    relatedPromise,
  ]);

  res.send({ product, related });
  // Total time blocked: ~100ms (fastest total)
  // Result: The thread is free during the 100ms wait, allowing thousands of other users to browse.
});
```

## Summary

In Node.js, **blocking code** is synchronous, halting the single-threaded Event Loop until a task (like file I/O or DB calls) completes. This "wait" freezes the server, preventing it from handling other requests and destroying performance.

Conversely, **non-blocking code** is asynchronous; it initiates tasks and immediately continues execution, using callbacks or promises to handle results later. Like an efficient waiter serving multiple tables rather than standing in the kitchen, non-blocking operations allow Node.js to manage thousands of concurrent connections simultaneously, ensuring high scalability and responsive applications.
