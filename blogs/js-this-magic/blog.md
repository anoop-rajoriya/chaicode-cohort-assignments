![Banner Diagram](./banner.png)

# The Magic of this, call(), apply(), and bind() in JavaScript [Live](https://dev.to/anoop-rajoriya/the-magic-of-this-call-apply-and-bind-in-javascript-3d86)

The `this` is the final boss of the javascript developers. It's slippery, contaxtual, and changes its meaning based on where it called. Understanding it along with `call`, `apply` and `bind` is the moment where you stop guessing what your code do and stat knowing.

## Content List

- [What this means in JavaScript (simple explanation)](#what-this-means-in-javascript-simple-explanation)
- [this inside normal functions](#this-inside-normal-functions)
- [this inside objects](#this-inside-object)
- [What call() does](#what-call-does)
- [What apply() does](#what-apply-does)
- [What bind() does](#what-bind-does)
- [Difference between call, apply, and bind](#difference-between-call-apply-and-bind)

## What `this` means in JavaScript (simple explanation)

Think of `this` as dynamic noune like "i" or "me"

if i say "i am eating" the word "i" refer to me, if you say same sentance then "i" refer to you. The sentace is same but subject is change depending on who is speaking.

In javascript `this` refer to the object which currently executing the code. It provide a way to access the properies of the object without knowing the name of object beforehand.

## `this` inside normal functions

When you use `this` inside a standard, standalone function its value depending on whether you using **Strict Mode** or not.

- **Non Strict Mode**: `this` default to the window object inside the browser and global inside the node environment.

- **Strict Mode `use strict`**: `this` remains `undefined` because javascript in strict mode prevents you from accidently modifying global object.

```js
function checkThis() {
  console.log(this);
}

checkThis();
// In a browser: Window object
// In 'use strict': undefined
```

## `this` inside object

When function is defined inside object as a method, `this` point to the object that owns the method. This is where the magic starts to become useful.

```js
const smartphone = {
  brand: "Apple",
  model: "iPhone 15",
  describe() {
    // 'this' refers to the smartphone object
    console.log(`This is an ${this.brand} ${this.model}.`);
  },
};

smartphone.describe(); // Output: This is an Apple iPhone 15.
```

If you were extract the function and run it elsewhere, `this` would lose its connection to the `smartphone`. This losing context problem is exactly why the `call`, `apply` and `bind` where invented.

## What call() does

The `call()` allow you to borrow a function from one object and use it for another. It invokes the funciton immediatly and allow you to manually specify what the `this` should point to.

**Syntax:** functionName.call(thisArg, arg1, arg2, ...)

```js
function setPrice(tax, shipping) {
  console.log(`${this.item} costs $${this.price + tax + shipping}`);
}

const laptop = { item: "MacBook", price: 1200 };
const phone = { item: "Pixel 8", price: 800 };

// We call setPrice but tell it to use 'laptop' as 'this'
setPrice.call(laptop, 100, 20); // MacBook costs $1320
setPrice.call(phone, 50, 10); // Pixel 8 costs $860
```

## What apply() does

`apply()` is the twin siblin to the `call()` it does the same thing: invokes the function immedialty with a custom this context, but it handle arguments differently, instead of listing arguments one by one, you pass them as an array.

**Mnemonics:** **A**pply == **A**rray

```js
const numbers = [5, 10, 15];

function sumThree(a, b, c) {
  console.log(this.label + (a + b + c));
}

const data = { label: "Total: " };

// Using apply to pass the array as individual arguments
sumThree.apply(data, numbers); // Total: 30
```

## What bind() does

`bind()` is the different it does not invoke funciton immediatly, instead it create a new function that have a specific `this` value locken in forever. This is incrediblly useful in react components, event listeners, or anytime you want to preset context for later use.

```js
const user = {
  name: "Alex",
  greet: function () {
    console.log(`Hello, ${this.name}`);
  },
};

// Imagine passing this to a button click handler
const clickHandler = user.greet;
// clickHandler(); // Error! 'this' is lost.

// We "bind" the context to user and save it as a new function
const boundGreet = user.greet.bind(user);

setTimeout(boundGreet, 1000); // Hello, Alex (after 1 second)
```

## Difference between call, apply, and bind

While they all manipulate `this`, their usage patterns differ:

| Feature       | `call()`           | `apply()`                           | `bind()`                      |
| ------------- | ------------------ | ----------------------------------- | ----------------------------- |
| Execution     | execute immediatly | also execute immediatly             | return a new function         |
| Arguments     | comma separated    | Array                               | Comma separeted (for preset)  |
| Main use case | method borrowing   | when arguments are already in array | Event listeners and callbacks |

## Summary

`this` is JavaScript’s dynamic "subject," referring to the current object context. `call()` and `apply()` invoke functions immediately—`call` takes individual arguments, while `apply` uses an array. `bind()` is the patient one; it returns a new function with a permanently locked context, perfect for preserving the `this` keyword in future event callbacks.
