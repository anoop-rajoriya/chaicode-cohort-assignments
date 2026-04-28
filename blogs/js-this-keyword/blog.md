![Banner Diagram](./banner.png)

# Understanding the this Keyword in JavaScript [Live](https://dev.to/anoop-rajoriya/understanding-the-this-keyword-in-javascript-530e)

The `this` keyword is one of the confusing concept of javascript, it not behave like it do in other languages like java or c++ in javascript. The value of `this` determined not by where function definition is define but how it is called.

Thinks it like a shapeshifter: it change its identity based on it's surroundings.

## Content List

- [What this represents](#what-this-represents)
- [`this` in global context](#this-in-global-context)
- [`this` inside objects](#this-inside-objects)
- [`this` inside functions](#this-inside-functions)
- [How calling context changes `this`](#how-calling-context-changes-this)

## What this represents

At its core `this` is a keyword that refers to a object. Which object? The one which currently execution the code.

You can think `this` as pronoun, in the sentance "Sarah is reading a book because she is bored." the word 'she' refer to a 'Sarah'. In javascript the `this` keyword act as that 'she' or 'he' allowing you to reuse function across different objects without hardcodding object's name.

## `this` in global context

When you are using `this` outside of any function or object, you are in **Global Execution Context**.

In a browsers `this` refer to the `window` object and in node.js `this` refer to a `global` object or empty object if you are using module.

```js
console.log(this); // In a browser, this outputs the Window object
this.color = "Red";
console.log(window.color); // "Red"
```

In this scenerio `this` is essentially the "king of hill" its represent a entire environment.

## `this` inside objects

When a funciton is defined as method of object, here `this` refer to the **object that owns the method**. This is the most common and intutive use of `this` keyword.

```js
const person = {
  name: "Leo",
  greet: function () {
    console.log(`Hi, I am ${this.name}`);
  },
};

person.greet(); // Output: "Hi, I am Leo"
```

In the example obove, because `greet` was called on the `person` object (`person.greet()`) so the `this` directly point to that `person` object.

## `this` inside functions

This is where thing becom spicy, the vlaue of `this` is depends on how the funciton is invoked.

- **Simple Function Call**: in regular function `this` point to global object inside function if you are using **Strict Mode** like `use strict` the `this` will be `undefined`.

- **Arrow Function**: arrow function are special they do not have thier own `this` instead they inherit from surrounding (lexical) scope. This makes them perfect for callbacks but terrible for object methods.

| Function Type    | this Value                                      |
| ---------------- | ----------------------------------------------- |
| Regular Function | The object that called it (or global/undefined) |
| Arrow Function   | Inherited from the parent scope                 |

## How calling context changes `this`

Javascript have explicit way to force `this` to be whatever you want, this is called explicit binding. There are three primary methods to do that:

### 1. `call()` and `bind()`

These methods call function immediatly while manually setting the value of `this`.

`call()` method takes argumetns separatly and `apply()` takes arguments as an array.

```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

const user1 = { name: "Alice" };
const user2 = { name: "Bob" };

// .call() takes arguments individually
greet.call(user1, "Hello", "!");
// Output: "Hello, I'm Alice!"

// .apply() takes arguments as an array
greet.apply(user2, ["Hi", "."]);
// Output: "Hi, I'm Bob."
```

### 2. `bind()`

Unlike `call()`, `bind()` doen't run function immediatly it return a new function with `this` value permanantly locked in. This is incrediblly usefull for event listeners.

```js
const user = {
  name: "Alex",
  greet: function () {
    console.log("Hello, " + this.name);
  },
};

// This loses context because 'this' becomes the global object or undefined
setTimeout(user.greet, 1000); // Output: "Hello, undefined"

// bind() creates a new function with 'this' locked to 'user'
const boundGreet = user.greet.bind(user);
setTimeout(boundGreet, 1000); // Output: "Hello, Alex"
```

### 3. `new` keyword

When you use `new` keyword to create a instance from constructor function the js creates a breand new object and sets instance `this` point to that object.

```js
function Robot(model) {
  this.model = model;
}

const myBot = new Robot("T-800");
console.log(myBot.model); // "T-800"
```

## Summary

**`this`** is a dynamic "shapeshifter" representing the execution context. Its value depends on how a function is called: pointing to the global object, an owner object, or a new instance. Regular functions bind at runtime; arrow functions inherit context lexically. Use `call`, `apply`, or `bind` to manually control its identity.
