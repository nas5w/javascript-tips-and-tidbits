<div align="center">
  <img alt="JavaScript Tips & Tidbits" src="https://i.imgur.com/K7MVMOn.png" />
</div>
<p>&nbsp;</p>
<p align="center">
  A continuously-evolving compendium of javascript tips based on common areas of confusion or misunderstanding.
</p>

---

**Want to learn more about JavaScript development? Consider:** 
- Signing up for my [free newsletter](https://buttondown.email/devtuts) where I periodically send out digestible bits of JavaScript knowledge!
- Subscribing to my [free youtube channel](https://www.youtube.com/c/devtutsco) where I teach JavaScript, Typescript, and React tutorials!

---


## Contents

-   [Value vs. Reference Variable Assignment](#value-vs-reference-variable-assignment)
-   [Closures](#closures)
-   [Destructuring](#destructuring)
-   [Spread Syntax](#spread-syntax)
-   [Rest Syntax](#rest-syntax)
-   [Array Methods](#array-methods)
-   [Generators](#generators)
-   [Identity Operator (===) vs. Equality Operator (==)](#identity-operator--vs-equality-operator)
-   [Object Comparison](#object-comparison)
-   [Callback Functions](#callback-functions)
-   [Promises](#promises)
-   [Async Await](#async-await)
-   [DOM Manipulation](#dom-manipulation)
-   [Interview Questions](#interview-questions)
-   [Miscellaneous](#miscellaneous)

## Value vs. Reference Variable Assignment

Understanding how JavaScript assigns to variables is foundational to writing bug-free JavaScript. If you don't understand this, you could easily write code that unintentionally changes values.

When JavaScript assigns one of the seven primitive type (i.e., Boolean, Null, Undefined, String, Number, Symbol, and BigInt.) to a variable, the JavaScript runtime gets to determine whether that primitive is assigned by *reference* or by *value*. It doesn't really matter how it's done because primitives can't be mutated (they're *immutable*). However, when the assigned value is an `Array`, `Function`, or `Object` a reference to the array/function/object in memory is assigned.

Example time! In the following snippet, `var2` is set as equal to `var1`. Since `var1` is a primitive type (`String`), `var2` is set as equal to `var1`'s String value and can be thought of as completely distinct from `var1` at this point. Accordingly, reassigning `var2` has no effect on `var1`.

```javascript
const var1 = 'My string';
let var2 = var1;

var2 = 'My new string';

console.log(var1);
// 'My string'
console.log(var2);
// 'My new string'
```

Let's compare this with object assignment.

```javascript
const var1 = { name: 'Jim' };
const var2 = var1;

var2.name = 'John';

console.log(var1);
// { name: 'John' }
console.log(var2);
// { name: 'John' }
```

How this is working: 
- The object `{ name: 'Jim' }` is created in memory
- The variable `var1` is assigned a *reference* to the created object
- The variable `var2` is set to equal `var1`... which is a reference to that same object in memory!
- `var2` is mutated, which really means *the object var2 is referencing is mutated*
- `var1` is pointing to the same object as `var2`, and therefore we see this mutation when accessing `var1`

One might see how this could cause problems if you expected behavior like primitive assignment! This can get especially ugly if you create a function that unintentionally mutates an object.

For more on variable assignment and primitive/object mutability, see <a href="https://typeofnan.dev/variable-assignment-primitive-object-mutation/" rel="">this article</a>.

## Closures

Closure is an important javascript pattern to give private access to a variable. In this example, `createGreeter` returns an anonymous function that has access to the supplied `greeting`, "Hello." For all future uses, `sayHello` will have access to this greeting!

```javascript
function createGreeter(greeting) {
    return function(name) {
        console.log(greeting + ', ' + name);
    };
}

const sayHello = createGreeter('Hello');

sayHello('Joe');
// Hello, Joe
```

In a more real-world scenario, you could envision an initial function `apiConnect(apiKey)` that returns some methods that would use the API key. In this case, the `apiKey` would just need to be provided once and never again.

```javascript
function apiConnect(apiKey) {
    function get(route) {
        return fetch(`${route}?key=${apiKey}`);
    }

    function post(route, params) {
        return fetch(route, {
            method: 'POST',
            body: JSON.stringify(params),
            headers: {
                Authorization: `Bearer ${apiKey}`
            }
        });
    }

    return { get, post };
}

const api = apiConnect('my-secret-key');

// No need to include the apiKey anymore
api.get('http://www.example.com/get-endpoint');
api.post('http://www.example.com/post-endpoint', { name: 'Joe' });
```

## Destructuring

Don't be thrown off by javascript parameter destructuring! It's a common way to cleanly extract properties from objects.

```javascript
const obj = {
    name: 'Joe',
    food: 'cake'
};

const { name, food } = obj;

console.log(name, food);
// 'Joe' 'cake'
```

If you want to extract properties under a different name, you can specify them using the following format.

```javascript
const obj = {
    name: 'Joe',
    food: 'cake'
};

const { name: myName, food: myFood } = obj;

console.log(myName, myFood);
// 'Joe' 'cake'
```

In the following example, destructuring is used to cleanly pass the `person` object to the `introduce` function. In other words, destructuring can be (and often is) used directly for extracting parameters passed to a function. If you're familiar with React, you probably have seen this before!

```javascript
const person = {
    name: 'Eddie',
    age: 24
};

function introduce({ name, age }) {
    console.log(`I'm ${name} and I'm ${age} years old!`);
}

introduce(person);
// "I'm Eddie and I'm 24 years old!"
```

## Spread Syntax

A javascript concept that can throw people off but is relatively simple is the spread operator! In the following case, `Math.max` can't be applied to the `arr` array because it doesn't take an array as an argument, it takes the individual elements as arguments. The spread operator `...` is used to pull the individual elements out the array.

```javascript
const arr = [4, 6, -1, 3, 10, 4];
const max = Math.max(...arr);
console.log(max);
// 10
```

## Rest Syntax

Let's talk about javascript rest syntax. You can use it to put any number of arguments passed to a function into an array!

```javascript
function myFunc(...args) {
    console.log(args[0] + args[1]);
}

myFunc(1, 2, 3, 4);
// 3
```

## Array Methods

JavaScript array methods can often provide you incredible, elegant ways to perform the data transformation you need. As a contributor to StackOverflow, I frequently see questions regarding how to manipulate an array of objects in one way or another. This tends to be the perfect use case for array methods.

I will cover a number of different array methods here, organized by similar methods that sometimes get conflated. This list is in no way comprehensive: I encourage you to review and practice all of them discussed on MDN (my favorite JavaScript reference).

### map, filter, reduce

There is some confusion around the javascript array methods `map`, `filter`, `reduce`. These are helpful methods for transforming an array or returning an aggregate value.

-   **map:** return array where each element is transformed as specified by the function

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const mapped = arr.map(el => el + 20);
console.log(mapped);
// [21, 22, 23, 24, 25, 26]
```

-   **filter:** return array of elements where the function returns true

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const filtered = arr.filter(el => el === 2 || el === 4);
console.log(filtered);
// [2, 4]
```

-   **reduce:** accumulate values as specified in function

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const reduced = arr.reduce((total, current) => total + current, 0);
console.log(reduced);
// 21
```

_Note:_ It is always advised to specify an _initialValue_ or you could receive an error. For example:

```javascript
const arr = [];
const reduced = arr.reduce((total, current) => total + current);
console.log(reduced);
// Uncaught TypeError: Reduce of empty array with no initial value
```

_Note:_ If there’s no initialValue, then reduce takes the first element of the array as the initialValue and starts the iteration from the 2nd element

You can also read this [tweet](https://twitter.com/sophiebits/status/1099014182261776384?s=20) by Sophie Alpert (@sophiebits), when it is recommended to use <code>reduce</code>

### find, findIndex, indexOf

The array methods `find`, `findIndex`, and `indexOf` can often be conflated. Use them as follows.

-   **find:** return the first instance that matches the specified criteria. Does not progress to find any other matching instances.

```javascript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const found = arr.find(el => el > 5);
console.log(found);
// 6
```

Again, note that while everything after 5 meets the criteria, only the first matching element is returned. This is actually super helpful in situations where you would normally break a `for` loop when you find a match!

-   **findIndex:** This works almost identically to find, but rather than returning the first matching element it returns the index of the first matching element. Take the following example, which uses names instead of numbers for clarity.

```javascript
const arr = ['Nick', 'Frank', 'Joe', 'Frank'];
const foundIndex = arr.findIndex(el => el === 'Frank');
console.log(foundIndex);
// 1
```

-   **indexOf:** Works almost identically to findIndex, but instead of taking a function as an argument it takes a simple value. You can use this when you have simpler logic and don't need to use a function to check whether there is a match.

```javascript
const arr = ['Nick', 'Frank', 'Joe', 'Frank'];
const foundIndex = arr.indexOf('Frank');
console.log(foundIndex);
// 1
```

### push, pop, shift, unshift

There are a lot of great array method to help add or remove elements from arrays in a targeted fashion.

-   **push:** This is a relatively simple method that adds an item to the end of an array. It modifies the array in-place and the function itself returns the length of the new array.

```javascript
const arr = [1, 2, 3, 4];
const pushed = arr.push(5);
console.log(arr);
// [1, 2, 3, 4, 5]
console.log(pushed);
// 5
```

-   **pop:** This removes the last item from an array. Again, it modifies the array in place. The function itself returns the item removed from the array.

```javascript
const arr = [1, 2, 3, 4];
const popped = arr.pop();
console.log(arr);
// [1, 2, 3]
console.log(popped);
// 4
```

-   **shift:** This removes the first item from an array. Again, it modifies the array in place. The function itself returns the item removed from the array.

```javascript
const arr = [1, 2, 3, 4];
const shifted = arr.shift();
console.log(arr);
// [2, 3, 4]
console.log(shifted);
// 1
```

-   **unshift:** This adds one or more elements to the beginning of an array. Again, it modifies the array in place. Unlike a lot of the other methods, the function itself returns the new length of the array.

```javascript
const arr = [1, 2, 3, 4];
const unshifted = arr.unshift(5, 6, 7);
console.log(arr);
// [5, 6, 7, 1, 2, 3, 4]
console.log(unshifted);
// 7
```

### splice, slice

These methods either modify or return subsets of arrays.

-   **splice:** Change the contents of an array by removing or replacing existing elements and/or adding new elements. This method modifies the array in place.

```javascript
The following code sample can be read as: at position 1 of the array, remove 0 elements and insert b.
const arr = ['a', 'c', 'd', 'e'];
arr.splice(1, 0, 'b');
console.log(arr);
// ['a', 'b', 'c', 'd', 'e']
```

-   **slice:** returns a shallow copy of an array from a specified start position and before a specified end position. If no end position is specified, the rest of the array is returned. Importantly, this method does not modify the array in place but rather returns the desired subset.

```javascript
const arr = ['a', 'b', 'c', 'd', 'e'];
const sliced = arr.slice(2, 4);
console.log(sliced);
// ['c', 'd']
console.log(arr);
// ['a', 'b', 'c', 'd', 'e']
```

### sort

-   **sort:** sorts an array based on the provided function which takes a first element and second element argument. Modifies the array in place. If the function returns negative or 0, the order remains unchanged. If positive, the element order is switched.

```javascript
const arr = [1, 7, 3, -1, 5, 7, 2];
const sorter = (firstEl, secondEl) => firstEl - secondEl;
arr.sort(sorter);
console.log(arr);
// [-1, 1, 2, 3, 5, 7, 7]
```

Phew, did you catch all of that? Neither did I. In fact, I had to reference the MDN docs a lot while writing this - and that's okay! Just knowing what kind of methods are out there with get you 95% of the way there.

## Generators

Don't fear the `*`. The generator function specifies what `value` is yielded next time `next()` is called. Can either have a finite number of yields, after which `next()` returns an `undefined` value, or an infinite number of values using a loop.

```javascript
function* greeter() {
    yield 'Hi';
    yield 'How are you?';
    yield 'Bye';
}

const greet = greeter();

console.log(greet.next().value);
// 'Hi'
console.log(greet.next().value);
// 'How are you?'
console.log(greet.next().value);
// 'Bye'
console.log(greet.next().value);
// undefined
```

And using a generator for infinite values:

```javascript
function* idCreator() {
    let i = 0;
    while (true) yield i++;
}

const ids = idCreator();

console.log(ids.next().value);
// 0
console.log(ids.next().value);
// 1
console.log(ids.next().value);
// 2
// etc...
```

## Identity Operator (===) vs. Equality Operator (==)

Be sure to know the difference between the identify operator (`===`) and equality operator (`==`) in javascript! The `==` operator will do type conversion prior to comparing values whereas the `===` operator will not do any type conversion before comparing.

```javascript
console.log(0 == '0');
// true
console.log(0 === '0');
// false
```

## Object Comparison

A mistake I see javascript newcomers make is directly comparing objects. Variables are pointing to references to the objects in memory, not the objects themselves! One method to actually compare them is converting the objects to JSON strings. This has a drawback though: `JSON.stringify` is not able to stringify a lot of object types (e.g., functions and sets)! A safer way to compare objects is to pull in a library that specializes in deep object comparison (e.g., lodash's isEqual).

The following objects appear equal but they are in fact pointing to different references.

```javascript
const joe1 = { name: 'Joe' };
const joe2 = { name: 'Joe' };

console.log(joe1 === joe2);
// false
```

Conversely, the following evaluates as true because one object is set equal to the other object and are therefore pointing to the same reference (there is only one object in memory).

```javascript
const joe1 = { name: 'Joe' };
const joe2 = joe1;

console.log(joe1 === joe2);
// true
```

Make sure to review the Value vs. Reference section above to fully understand the ramifications of setting a variable equal to another variable that's pointing to a reference to an object in memory!

## Callback Functions

Far too many people are intimidated by javascript callback functions! They are simple, take this example. The `console.log` function is being passed as a callback to `myFunc`. It gets executed when `setTimeout` completes. That's all there is to it!

```javascript
function myFunc(text, callback) {
    setTimeout(function() {
        callback(text);
    }, 2000);
}

myFunc('Hello world!', console.log);
// 'Hello world!'
```

## Promises

Once you understand javascript callbacks you'll soon find yourself in nested "callback hell." This is where Promises help! Wrap your async logic in a `Promise` and `resolve` on success or `reject` on fail. Use `then` to handle success and `catch` to handle failure.

```javascript
const myPromise = new Promise(function(res, rej) {
    setTimeout(function() {
        if (Math.random() < 0.9) {
            return res('Hooray!');
        }
        return rej('Oh no!');
    }, 1000);
});

myPromise
    .then(function(data) {
        console.log('Success: ' + data);
    })
    .catch(function(err) {
        console.log('Error: ' + err);
    });

// If Math.random() returns less than 0.9 the following is logged:
// "Success: Hooray!"
// If Math.random() returns 0.9 or greater the following is logged:
// "Error: Oh no!"
```

### Avoid the nesting anti-pattern of promise chaining!

`.then` methods can be chained. I see a lot of new comers end up in some kind of call back hell inside of a promise when it's completely unnecessary.

```javascript
//The wrong way
getSomedata.then(data => {
    getSomeMoreData(data).then(newData => {
        getSomeRelatedData(newData => {
            console.log(newData);
        });
    });
});
```

```javascript
//The right way
getSomeData
    .then(data => {
        return getSomeMoreData(data);
    })
    .then(data => {
        return getSomeRelatedData(data);
    })
    .then(data => {
        console.log(data);
    });
```

You can see how it's much easier to read the second form and with ES6 implicit returns we could even simplify that further:

```javascript
getSomeData
    .then(data => getSomeMoreData(data))
    .then(data => getSomeRelatedData(data))
    .then(data => console.log(data));
```
Because the function supplied to .then will be called with the the result of the resolve method from the promise we can omit the ceremony of creating an anonymous function altogether. This is equivalent to above:

```javascript
getSomeData
    .then(getSomeMoreData)
    .then(getSomeRelatedData)
    .then(console.log);
```

## Async Await

Once you get the hang of javascript promises, you might like `async await`, which is just "syntactic sugar" on top of promises. In the following example we create an `async` function and within that we `await` the `greeter` promise.

```javascript
const greeter = new Promise((res, rej) => {
    setTimeout(() => res('Hello world!'), 2000);
});

async function myFunc() {
    const greeting = await greeter;
    console.log(greeting);
}

myFunc();
// 'Hello world!'
```

### Async functions return a promise

One important thing to note here is that the result of an `async` function is a promise.

```javascript
const greeter = new Promise((res, rej) => {
    setTimeout(() => res('Hello world!'), 2000);
});

async function myFunc() {
    return await greeter;
}

console.log(myFunc()); // => Promise {}

myFunc().then(console.log); // => Hello world!
```

## DOM Manipulation

### Create Your Own Query Selector Shorthand

When working with JS in the browser, instead of writing `document.querySelector()`/`document.querySelectorAll()` multiple times, you could do the following thing:

```javascript
const $ = document.querySelector.bind(document);
const $$ = document.querySelectorAll.bind(document);

// Usage
const demo = $('#demo');
// Select all the `a` tags
[...$$("a[href *='#']")].forEach(console.log);
```

## Interview Questions

### Traversing a Linked List

Here's a javascript solution to a classic software development interview question: traversing a linked list. You can use a while loop to recursively iterate through the linked list until there are no more values!

```javascript
const linkedList = {
    val: 5,
    next: {
        val: 3,
        next: {
            val: 10,
            next: null
        }
    }
};

const arr = [];
let head = linkedList;

while (head !== null) {
    arr.push(head.val);
    head = head.next;
}

console.log(arr);
// [5, 3, 10]
```

## Miscellaneous

### Increment and Decrement

Ever wonder what the difference between `i++` and `++i` was? Did you know both were options? `i++` returns `i` and then increments it whereas `++i` increments `i` and then returns it.

```javascript
let i = 0;
console.log(i++);
// 0
```

```javascript
let i = 0;
console.log(++i);
// 1
```

# Contributing

Contributions welcome! All I ask is that you open an issue and we discuss your proposed changes before you create a pull request.
