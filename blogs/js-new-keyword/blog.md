![Banner Diagram](./banner.png)

# The new Keyword in JavaScript [Live](https://dev.to/anoop-rajoriya/the-new-keyword-in-javascript-17f9)

To understand the `new` keyword in deep you have to imagine it like a factory supervioser which takes a generic function and force it to behave like a object-manufacturing plant.

Here are the deep dive to understand how these concepts intertwine.

## Content List
- [What is constructor function](#the-constructure-function-blueprint)
- [What is new Keyword](#the-new-keyword-the-factory-supervisor)
- [Waht is Object Creation Process](#the-object-creation-process-step-by-step)
- [How `new` Links Prototypes](#how-new-links-prototypes-the-secret-tunnel)
- [Instances Created from Constructors](#instances-created-from-constructors)

## The constructure function: Blueprint

A constructor funciton is like a any normal function. There is nothing syntatically special about it unitle you invoked it with `new` keyword. By convention we named it with capital letter to tell other developers "Hey brother this is constructor funciton never ever call it without new, Ok".

```js
function Plane(brand, model){
    this.brand = brand
    this.model = model
}

Plane.prototype.fly = function (){
    console.log(`${this.brand} is taking off!`);
}
```

If you calling `Plane()` without `new` keyword. `this` is pointing to global object (window) or undefined in strict mode and you will end up like a error or global variable which you didn't need.

## The new Keyword: The Factory Supervisor

The `new` keyword is the operator which trigger the "Object Creation" process, it's a four steps internal ritual. Without it the funciton executed normally line-by-line and return undefined or retured valus, but with it the js engine shift into "Instentiation Mode".

## The Object Creation Process (Step-by-Step)

When you call function like it, the js engine perform bellow four steps:

```js
const myPlane = new Plane("Boeing" 747)
```

1. **Creation:** it creates a brand new object `{}` in memory.

2. **Linking:** it takes this created object and sets its internal `[[Prototype]]` (the hidden `__prototype__` property) to match the `Plane.prototype` object. This create a "inheritance" link between them.

3. **Binding:** it calls the `Plane` function, but it hands function newly created empty object to use it as `this`. Every function say `this.brand` it is actually, writing data onto that new object.

4. **Returning:** if the function returns an non-primitives like `arrya` or `objects` (`return {a:1}`). Javascript throws away work it just did and return that object instead. But if the non-primitives or nothing at all returns like (`return "Done"`) then javascript returned that newly creaetd object form step 1.

## How `new` Links Prototypes: The "Secret Tunnel"

This is the most misunderstood part of the `new` keyword. It established a delegation chain:

- The **Constructor** has a `.prototype` property where you put all you methods or values you want to share it with all its instances.
- The **Instances** have a `[[Prototype]]` link which points back to that same object.

Because of the `new` keyword the second steps is implemented, when you call `myPlane.fly()` it finds into the `myPlane` object, it doesn't find `fly()` there, then it follow its "secret tunnel" (the prototype link) to `Plane.prototype` and it finds `fly()` there, executes it.

## Instances Created from Constructors

The final result of these process is **Instace**

The **Instace** is a unique object belongs to a type. You can create multiple instances form one type like `myPlane` or `otherPlane`, both are separate objects in memory of `Plane` type (changin the brand of one will not change other), they are siblings because they share the same prototype.

> The `new` keyword is what transform javascript form simple functions into a system capable of Object-Orianted Programming (OOP). it automate a boilerplate of creating an objects, linking its lineage, and populating its data.

## Summary

The new keyword automates object creation. It creates an empty object, links its internal prototype to the constructor's prototype, and binds this to the new instance. After the constructor populates the object, it returns the instance (unless an object is explicitly returned), enabling shared methods and powerful prototype inheritance patterns.