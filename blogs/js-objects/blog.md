![Banner Diagram](./banner.png)

# Understanding Objects in JavaScript [Live](https://dev.to/anoop-rajoriya/understanding-objects-in-javascript-4ca5)

Objects are the backbone of the javascript eco-system. Understanding it like a understanding language DNA's. Here are the deep dive into how they works and why they are the MVP or your codebase.

## Content List

- [What objects are and why they are needed](#what-objects-are-and-why-they-are-needed)
- [Creating objects](#creating-objects)
- [Accessing properties](#accessing-properties)
- [Updating object properties](#updating-object-properties)
- [Adding and deleting properties](#adding-and-deleting-properties)
- [Looping through object keys](#looping-through-object-keys)

## What objects are and why they are needed
In programming we ofter need to represent complex entitis. A simple variables like string or number can only hold one piece of data, however real world item have a multiple characterstics.

A objects are a collection or properties & methods, they store values in key-values pairs.

**You might thinking is really we need object there is why:**

without object keeping track of related data is messy, imagin you have to store a user profile data, keeping each data in single variable like `username`, `userEmail`, and `userAge`. If there are 100 user then you have to manage 300 disconnected variable. Object allow you to group that data into one single package like `const user = {username: "siv", email: "siv2testing.com", age: 21}`.

## Creating objects
The most common way to create object is useing **Object Literals** syntax (the curly braces)

```js
// A Smartphone Example
const smartphone = {
  brand: "Apple",
  model: "iPhone 15",
  is5G: true,
  batteryLevel: 85,
  // Objects can even contain functions (methods)
  ring: function() {
    console.log("Beep! Beep!");
  }
};
```

## Accessing properties
One the object created you need a way to get the data back out. There are two primary ways to do this.

**1. Dot Notation:** this is the most common and readable way, you use it when you now the name of the property beforehand.

```js
console.log(smartphone.brand); // "Apple"
```

**2. Bracket Notation:** this is more powerful way you will be using it, if you key names have spaces, special characters or using variables to access keys.

```js
const keyToLookup = "model";

console.log(smartphone["model"]);    // "iPhone 15"
console.log(smartphone[keyToLookup]); // "iPhone 15" (Dynamic access)
```

## Updating object properties
Objects in js are mutables means you can change the values inside the object even if you declare it with `const`

```js
const car = {
  make: "Tesla",
  status: "Parked"
};

// Changing the value
car.status = "Driving"; 
car["make"] = "Tesla Motors";

console.log(car.status); // "Driving"
```

## Adding and deleting properties
Objects are flexibles mean they can grow or shrink during the execution of you programm.

### Adding Properties
You adding a property by simply assigning a vlaue to a key that dosn't exist yet.

```js
const employee = { name: "Sarah" };

employee.role = "Developer"; // Property added
employee["salary"] = 90000;  // Property added
```

### Deleting Properties
You using a `delete` operator to remvoe the key-value paires entirely.

```js
delete employee.salary; 
console.log(employee.salary); // undefined
```

## Looping through object keys
Unlike array, you can not use standard `for` loop to iterate over objects because they don't have indexes, instead we use `for...in` loop or `Object` built-in methods.

### The `for...in` Loop
This loop iterate over every enumarable property of the objects.

```js
const scores = {
  math: 90,
  science: 85,
  history: 88
};

for (let subject in scores) {
  console.log(`${subject}: ${scores[subject]}`);
}
// Output: 
// math: 90
// science: 85
// history: 88
```

### Using `Object.keys()`
If you need a array of keys to use moderen array methods like `map()`, or `filter()`. Use this to create a array contains all keys of the intended object.

```js
const keys = Object.keys(scores); 
// ["math", "science", "history"]

keys.forEach(key => {
  console.log(scores[key]);
});
```

## Summary
JavaScript objects group related data using **key-value pairs**, making complex data manageable. You create them via literals `{}`, access properties using **dot** or **bracket** notation, and freely add, update, or delete entries. To iterate through data, use `for...in` loops or `Object.keys()` to handle keys dynamically and efficiently.