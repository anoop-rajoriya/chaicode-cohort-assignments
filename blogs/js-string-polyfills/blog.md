![Banner Diagram](./banner.png)

# String Polyfills and Common Interview Methods in JavaScript

Strings are more than just a sequance of characters. It's a backbone concept of the data manipulation. Whether you are cleaning user input to solving leetcode problems, understaing how they work under the hood gives you super power.

## Content List
-[What are String Methods?](#what-are-string-methods)
-[Why Developers Write Polyfills?](#why-developers-write-polyfills)
-[Implementing Simple String Utilities (Polyfills)](#implementing-simple-string-utilities-polyfills)
-[Common Interview String Problems](#common-interview-string-problems)
-[Importance of Understanding Built-in Behavior](#importance-of-understanding-built-in-behavior)

## What are String Methods?

In javascript strings are the primitive data types but js wraped it by `String` object temporarly allow you to access some built-in methods on string. These methods are lived within `String.prototype` property.

When you using those methods remember than strings are immutable so these never change it, they instead return a new string.

**There are some string methods categorized:**

- **Searching:** `indexOf()`, `includes()`, `startsWith()`
- **Transforming:** `toUpperCase()`, `trim()`, `replace()`
- **Extraction:** `slice()`, `substring()`, `split()`

## Why Developers Write Polyfills

A **polyfills** are the piece of code (usually on the brower) which provides a specific morder functionalities to the older systems thos did not have native support of these functionalities.

If the browser already have the method we use it (it's faster), if it does not have this native support then we fill it out that hole with own custom implementation.

**There are some common reasons for polyfills:**

- **Lagacy Support:** supporting older environments like `IE11` that lack of `ES6+` features like `.includes()` and `.padStarts()`.

- **Interview Performance:** some interviewers ask you to write a polyfills to test your understanding of concepts, prototypes, `this` binding, and some edge cases handling.

## Implementing Simple String Utilities (Polyfills)

Lets looking how can create polyfills manually for `String.prototype.includes()`

```js
if(!String.prototype.myIncludes){
    String.prototype.myIncludes = function(searchingStr, position){
        if(typeof position !== "number") position = 0
        // this is the string on that method called
        // it search str from or after the position
        return this.indexOf(searchingStr, position) !== -1
    }
}

const str = "Hello World";
console.log(str.myIncludes("World")); // true
```

**Another example for `repeat`

```js
if(!String.prototype.myRepeat){
    String.prototype.myRepeat = function(count){
        if(count < 0)
            throw new Error("count must be non-negative")

        let result = ""
        for(let i = 0, i < count; i++){
            result += this
        }

        return result
    }
}
```

## Common Interview String Problems

Interviewers love strings because it test you understandin abilites of loops and logic without using other complex data structres.

**There are some common problems interviewers ask:**

- **Palidrome Check:** comparing string to its reverse version or use of concepts two pointer.
- **Anagram Check:** sort both strings and compares, or use frequency counter (Hash map).
- **First Non-repeating Char:** use objects or map to store char coutns and loop again to find the first 1.
- **Reverse Word:** `str.split(" ").reverse().join(" ")`
- **Longest Substring (No-Repeat):** use of sliding window technique with set.

## Importance of Understanding Built-in Behavior

Its easy to revers a string in javascript with the help of `.split("").reverse().join("")` but the experts know the why and how.

- **Performance (Time Complexity):** many people don't understaning these `.split()` and `.join()` takes a O(n) time. Chaining them on short string may be fine but with it will become slow on larger data set.
- **Memory Management:** since you now strings are immutable, every time you concatenate it in a loop like `str+= "0"`, you are creating whole new string in memory which is a O(n^2) behaviour.
- **The 'Prototype' Chain:** understanding these methods are living on `String.Prototype` property is the key of master inheritance in js. If you are modifing prototype you are affacting the every string in you application (which is why you should only do it for polyfills).

## Summary

JavaScript string methods provide immutable data manipulation. Polyfills ensure legacy browser compatibility by manually adding missing functionality to String.prototype. Understanding these tools is vital for solving interview problems like anagrams, optimizing performance, and mastering the prototype chain. It bridges the gap between basic coding and professional software engineering practices.