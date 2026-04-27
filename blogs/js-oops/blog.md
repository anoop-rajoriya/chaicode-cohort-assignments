![Banner Diagram](./banner.png)

# Understanding Object-Oriented Programming in JavaScript [Live](https://dev.to/anoop-rajoriya/understanding-object-oriented-programming-in-javascript-2lke)

OOPs is the programming styel which organize code into **Objects** rather than lists of instructions. It helps developers to model complex systems by grouping related data and behaviours together.

## Content List
- [What Object-Oriented Programming (OOP) means](#what-object-oriented-programming-oop-means)
- [Real-world analogy (blueprint → objects)](#real-world-analogy-blueprint--objects)
- [What is a class in JavaScript](#what-is-a-class-in-javascript)
- [Creating objects using classes](#creating-objects-using-classes)
- [Constructor method](#constructor-method)
- [Methods inside a class](#methods-inside-a-class)
- [Basic idea of encapsulation](#basic-idea-of-encapsulation)

## What Object-Oriented Programming (OOP) means
A object oriented programming is a paradigm (a way of thinking) where you view progarm as collection of objects that interact with each other.

Instead of writing a script with lots of functions, you organize them as specific data (properties) and actions (methods) into a single unit called **Object**. It make you program to easy to reuse, organize, and scale as project get bigger.

## Real-world analogy (blueprint → objects)
The easiest way to understand OOPs is to think about architect and house:

### 1. The Blueprint (class)
Architect design a detailed plan for a house, its not a house it self. It's just a detailed set of instructions of house which define overall structure of house like how walls constructs, how many window there are, where the doors go. 

### 2. The House (object)
Using this plan builder can buid ten different houses, each house is a real physical thing made from that plan. While they all follow the same blueprint, one house might be printed blue or another is red.

## What is a class in JavaScript
In javascript, class is used to create a blueprint before class was introduced in ES6 **Prototypes** are used. Prototypes were a bit more complex, class provides a cleaner, easy readable way to define how object should looks and behave.

In a class you define two things **Properties & Methods**:

- **Propeties:** what the object is like color, name, size etc.
- **Methods:** what the object does like run, stop, burk etc.

## Creating objects using classes
To create a new object from a class we use `new` keyword. This process called **instantiation**,

```js
class Laptop {
  // Class definition goes here
}

// Creating an object (instance) of the Laptop class
const myMacbook = new Laptop();
const myThinkpad = new Laptop();
```

In this `myMackbook` and `myThinkpad` are the two distinct object created from same `Laptop` class.

## Constructor method
The `constructor` is the special method which run authomatically the moment you create new object. It job is to set-up or initialize object's properties.

```js
class Car {
  constructor(brand, color) {
    this.brand = brand; // 'this' refers to the object being created
    this.color = color;
  }
}

const myCar = new Car("Toyota", "Red");
console.log(myCar.brand); // Output: Toyota
```

The `this` keyword is crucial, it tell javascript to assign a values to the unique object you are currently creating.

## Methods inside a class
Methods are the function belongs to class, its used to perform tasks that object can do. Unlike standard function we don't use `function` keyword, we declare methods without it in class.

```js
class Robot {
  constructor(name) {
    this.name = name;
  }

  // This is a method
  greet() {
    console.log(`Hello, my name is ${this.name}!`);
  }
}

const wallE = new Robot("WALL-E");
wallE.greet(); // Output: Hello, my name is WALL-E!
```

## Basic idea of encapsulation
The encapsulation is the practise of bundling data and mehods into a single unit (a class) and **restricting access** to some of the other objects components.

Think it like a capsul you dont need to see the chamical powder inside to take madicine, you just need to interact with outer shell.

In programming encapsulation: protect data from external code to accientally changing the internal variabals. Hide complexity: you only interact with simple method `startEngine()` wihtout needing to know complex logic happening inside.

In OOPs, its a good practise to use `#` or `_` symbols before the any private variable to make it private, meaning it cannot be directly accessed or changed from outside the class.

## Symmary
Object-Oriented Programming (OOP) organizes code into reusable **objects** containing data and behaviors. **Classes** act as blueprints, while objects are the physical instances. **Constructors** initialize properties, and **methods** define actions. **Encapsulation** protects internal data, hiding complexity and improving code structure, making systems easier to manage, scale, and maintain effectively over time.