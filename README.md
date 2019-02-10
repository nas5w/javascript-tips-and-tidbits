<div align="center">
  <img alt="JavaScript Tips & Tidbits" src="https://i.imgur.com/K7MVMOn.png" />
</div>
<p>&nbsp;</p>
<p align="center">
  A continuously-evolving compendium of javascript tips based on common areas of confusion or misunderstanding.
</p>

---

## Sections

- Value vs. Reference Variable Assignment
- Closures
- Destructuring
- Spread Syntax
- Rest Syntax
- Array Methods
- Generators
- Identity Operator (===) vs. Equality Operator (==)
- Object Comparison
- Callback Functions
- Promises
- Async Await
- Interview Questions
- Miscellaneous

## Value vs. Reference Variable Assignment

Understanding how JavaScript assigns to variables is foundational to writing bug-free JavaScript. If you don't understand this, you could easily write code that unintentionally changes values.

JavaScript always assigns variables by value. But this part is very important: when the assigned value is one of JavaScript's five primitive type (i.e., `Boolean`, `null`, `undefined`, `String`, and `Number`) the actual value is assigned. However, when the assigned value is an `Array`, `Function`, or `Object` a reference is assigned. 

Example time! In the following snippet, `var2` is set as equal to `var1`. Since `var1` is a primitive type (`String`), `var2` is set as equal to `var1`'s String value and can be thought of as completely distinct from `var1` at this point. Accordingly, reassigning `var2` has not effect on `var1`.

```javascript
let var1 = 'My string';
let var2 = var1;

var2 = 'My string';

console.log(var1);
// 'My string'
console.log(var2);
// 'My new string'
```

Let's compare this with object assignment.

```javascript
let var1 = { name: 'Jim' }
let var2 = var1;

var2.name = 'John';

console.log(var1);
// { name: 'John' }
console.log(var2);
// { name: 'John' }
```

One might see how this could cause problems if you expected behavior like primitive assignment! This can get especially ugly if you create a function that unintentionally mutates an object.

## Closures

Closure is an important javascript pattern to give private access to a variable. In this example, `createGreeter` returns an anonymous function that has access to the supplied `greeting`, "Hello." For all future uses, `sayHello` will have access to this greeting!

```javascript
function createGreeter(greeting) {
  return function(name) {
    console.log(greeting + ', ' + name);
  }
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
          'Authorization': `Bearer ${apiKey}`
        }
      })
  }
  
  return { get, post }
}

const api = apiConnect('my-secret-key');

// No need to include the apiKey anymore
api.get('http://www.example.com/get-endpoint');
api.post('http://www.example.com/post-endpoint', { name: 'Joe' })
```

## Destructuring

Don't be thrown off by javascript parameter destructuring! It's a common way to cleanly extract properties from objects. 

```javascript
const obj = {
  name: 'Joe',
  food: 'cake'
}

const { name, food } = obj;

console.log(name, food);
// 'Joe' 'cake'
```

If you want to extract properties under a different name, you can specify them using the following format.

```javascript
const obj = {
  name: 'Joe',
  food: 'cake'
}

const { name: myName, food: myFood } = obj;

console.log(myName, myFood);
// 'Joe' 'cake'
```

In the following example, destructuring is used to cleanly pass the `person` object to the `introduce` function. In other words, destructuring can be (and often is) used directly for extracting parameters passed to a function. If you're familiar with React, you probably have seen this before!

```javascript
const person = {
  name: 'Eddie',
  age: 24
}

function introduce({ name, age }) {
  console.log(`I'm ${name} and I'm ${age} years old!`);
}

console.log(introduce(person));
// "I'm Eddie and I'm 24 years old!"
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
}

let arr = [];
let head = linkedList;

while(head !== null) {
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
