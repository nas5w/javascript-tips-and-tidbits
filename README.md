[Imgur](https://i.imgur.com/mrN2Qmx.png)
<div align="center">
  <img alt="JavaScript" src="https://i.imgur.com/mrN2Qmx.png" width="100px" height="100px" />&nbsp;&nbsp;&nbsp;
  <img alt="Tips & Tidbits" src="https://i.imgur.com/EHbPbzj.png" height="100px" /> 
</div>
<p>&nbsp;</p>
<p align="center">
  A continuously-evolving compendium of javascript tips based on common areas of confusion or misunderstanding.
</p>

---

## Sections

- [General Concepts](#general-concepts)
- [Comparison](#comparison)
- [Miscellaneous](#miscellaneous)

## General Concepts

### Closures

Closure is an important javascript pattern to give private access to a variable. In this example, createGreeter returns an anonymous function that has access to the supplied greeting, "Hello." For all future uses, sayHello will have access to this greeting! 

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

### Generators

Don’t fear the \*. The generator function specifies what `value` is yielded next time `next()` is called. Can either have a finite number of yields, after which `next()` returns an undefined `value`, or an infinite number of values using a loop.

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

```javascript
function* idCreator() {
  let i = 0;
  while (true)
    yield i++;
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

## Comparison

### Comparing Objects

A mistake I see javascript newcomers make is directly comparing objects. Variables are pointing to references to the objects in memory, not the objects themselves! One method to actually compare them is converting the objects to JSON strings. This has a drawback though: object property order is not guaranteed! A safer way to compare objects is to pull in a library that specializes in deep object comparison (e.g., [lodash's isEqual](https://lodash.com/docs#isEqual)).

```javascript
const joe1 = { name: 'Joe' };
const joe2 = { name: 'Joe' };

console.log(joe1 === joe2);
// False
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

## Section TBD

Content

---

# Contributing

Contributing note
