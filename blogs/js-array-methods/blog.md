![Banner Diagram]()

# Array Methods You Must Know [Live](https://dev.to/anoop-rajoriya/array-methods-you-must-know-4kh3)

Arrays are the bread and butter of the javascript. Whether you are building shoping cart or social media feed you are spending lots of time poking and prodding lists of data.

here are the breakwon of the essentials arrays methods with real world examples.

## Content List

- [Adding & Removing from the End](#adding--removing-from-the-end)
- [Adding & Removing from the Front](#adding--removing-from-the-front)
- [Transformation & Selection](#transformation--selection)
- [The "Heavier" Methods](#the-heavier-methods)

## Adding & Removing from the End

`push()` and `pop()` methods are used to adding and removing elements from the end of the arrays, it's like a interact with pringles can.

- `push()`: this method take a items which you want to add in array end, it returns a length of the array after the adding item.

```js
const cart = ["milk", "bread"]
const res = cart.push("eggs")
console.log(cart, res) // ["milk", "bread", "eggs"], 3
```

- `pop()`: remove the last elment form an array and return it. It does not accept any arguments.

```js
const res = cart.pop()
console.log(cart, res) // ["milk", "bread"], "eggs"
```

## Adding & Removing from the Front

`shift()` and `unshift()` methods are used to add or remove items form the front of the array. You can think it like a waiting line at a coffee shop.

- `shift()`: it remove the front element and return the current length after removing element. It does not accept any argument.

```js
const line = ["rahul", "dipali", "jani"]
const res = line.shift()
console.log(line, res) // ["dipali", "jani"], 2
```

- `unshift()`: it add one or more element form beginning of array, elements added in order passed to it. It return new length of the array.

```js
const res = line.unshift("VIP Guest", "Senior cityson")

// ["VIP Guest", "Senior cityson", "dipali", "jani"], 4
console.log(line, res) 
```

## Transformation & Selection

These methods are not changes yor original arrays instead it returns a brand new array based on yor interection.

- `map()`: creates a new array by performaing an action on every item in the original array. Action is provided as callback, processing item and it's index are passed to callback.

```js
const priceInInr = [10, 100, 1000]
const priceInUsd = priceInInr.map((price, index)=>{
    console.log(index) // print prcessing price index
    return (price * 0.011).toFixed(2)
})
console.log(priceInUsd) // ['0.11', '1.10', '11.00']
```

- `filter()`: mostly used to filter items based on certain condition. Accept callback in that item and index passed as arguments. It return a new array containing elements passed a specific condition.

```js
const books = [
    { title: 'Dune', genre: 'Sci-Fi' },
    { title: 'Emma', genre: 'Romance' }
];
const sciFiOnly = books.filter(book => book.genre === 'Sci-Fi');
console.log(sciFiOnly) //[{ title: 'Dune', genre: 'Sci-Fi' }]
```

## The "Heavier" Methods

- `reduce()`: it takes two arguments a callback (reducer) and a accumulator. It runs reducer on each element, accumulator and current items are paased as an argument to reducer. Returned value form reducer is passed as an accumulator in next iteration. If accumulator not passed explicitly it assigned accumulator by it self: first value in array become accumulator and next one is current item.

```js
const bill = [10, 5, 15];
const total = bill.reduce((sum, price) => sum + price, 0);
console.log(total) // 30
```

- `foreach()`: used to traver over an array, it takes two argumetns a callback and thisArg. In callback it passed these three things: 

- **current value:** a items currently being processed.
- **index:** a index place of current item in original array.
- **array:** a original array `foreach` called on.

The thisArg is the value you want to use it as this when executing the callback, its a optional argument.

```js
const cart = [
  { item: "Laptop", price: 1000, inStock: true },
  { item: "Mouse", price: 25, inStock: false },
  { item: "Keyboard", price: 75, inStock: true }
];

let totalBill = 0;

cart.forEach((product) => {
  if (product.inStock) {
    totalBill += product.price;
  } else {
    console.log(`Alert: ${product.item} is currently unavailable.`);
  }
});

console.log(`Your total is: $${totalBill}`);
```

## Summary

`push`/`pop` and `unshift`/`shift` add or remove items from array ends and fronts. `map` transforms elements into new lists, while `filter` extracts specific data. `reduce` condenses arrays into single values, and `forEach` runs logic for every item. These methods are essential for clean, efficient JavaScript data handling.