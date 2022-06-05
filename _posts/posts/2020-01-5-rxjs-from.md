---
layout: post
title:  "RxJS From Operator"
date:   2020-01-6
tags: [ angular ]
---


The Rx  from  operator is used to transform data that can be iterated over to an observable. It can be useful especially when you want to normalize the types of data that’s being passed and shared in observable sequences or when a function expects to receive and act on an observable. Another use if for when you’d want to use an RxJS operator that wouldn’t normally be available on the original data type.

Example of iterable types that can be transformed into observables using  **from**  are arrays, maps, sets, promises, DOM nodes, and generator functions. Below you’ll find examples for a few of these types:

## Arrays

Most often the  from  operator is used to convert an array to an observable:

```
let myArr = ['🐦', '😺', '🐕', '🐊'];

Rx.Observable
  .from(myArr)
  .filter(x => x !== '🐦')
  .map(x => `Hello ${x}!`)
  .subscribe(console.log);

  // Hello 😺!
  // Hello 🐕!
  // Hello 🐊!

```

Copy

### Synchronous vs Asynchronous

By default the  **from**  operator returns a synchronous observable:

```
let myArr = ['😺', '🐕', '🐊'];

console.log('Before');
Rx.Observable
  .from(myArr)
  .map(x => `Hello ${x}!`)
  .subscribe(console.log);
console.log('After');

// Before
// Hello 😺!
// Hello 🐕!
// Hello 🐊!
// After

```

Copy

If you want however, you can make it asynchronous using an async scheduler:

```
let myArr = ['😺', '🐕', '🐊'];

console.log('Before');
Rx.Observable
  .from(myArr, Rx.Scheduler.async)
  .map(x => `Hello ${x}!`)
  .subscribe(console.log);
console.log('After');

// Before
// After
// Hello 😺!
// Hello 🐕!
// Hello 🐊!

```


## Generator Functions

Generator functions are an iterable type, so they can also be transformed to an observable using the  from  operator. Here’s a simple example:

```
function* generateUnique() {
  let num = 0;
  while (true) {
    yield num++;
  }
}

Rx.Observable.from(generateUnique())
  .take(3)
  .subscribe(console.log);

  // 0
  // 1
  // 2

```

Here we use the  take  operator to complete the observable after a specified number of values. Otherwise we’d create an infinite observable and crash the page when subscribing.

And here’s a slightly more complex example that also uses the  zip  operator to combine the values of multiple observables:

```
function* generateName() {
  yield 'Cat';
  yield 'Dog';
  yield 'Bird';
  return;
}

function* generateEmoji() {
  yield '😺';
  yield '🐕';
  yield '🐦';
  return;
}

function* generateSound() {
  yield 'Meow';
  yield 'Woof';
  yield 'Tweet';
  return;
}

const names = Rx.Observable.from(generateName());
const emojis = Rx.Observable.from(generateEmoji());
const sounds = Rx.Observable.from(generateSound());

const combined = Rx.Observable.zip(names, emojis, sounds, (name, emoji, sound) => {
  return `The ${name} ${emoji} goes ${sound.toUpperCase()}`;
})
.subscribe(console.log);

// The Cat 😺 goes MEOW
// The Dog 🐕 goes WOOF
// The Bird 🐦 goes TWEET

```

Note that the  spawn  operator is also used to combine generators and observables, but the operator is currently not available in RxJS 5+

## Promises

Promises can also easily be transformed into observables, which will be asynchronous and wrap the resolved or rejected value:

```
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello');
  }, 2000);
});

Rx.Observable
  .from(myPromise)
  .subscribe(x => console.log(x, ' World!'));

// Hello World! (after 2 seconds)

```

Copy

## DOM Nodes

Here’s a quick example where a collection of 3 DOM nodes are transformed into an observable and mapped over to extract only the  textContent:

```
<h2>Hey,</h2>
<h2>Hello</h2>


<script>
  const h2s = document.querySelectorAll('h2');

  Rx.Observable.from(h2s)
    .map(h2 => h2.textContent)
    .subscribe(console.log);

    // Hey,
    // Hello
</script>

```


## A Word About Strings

Strings can be iterated over, so the  **from**  operator can be used, but every character in the string will be a separate value:

```
Rx.Observable
  .from('Hi!')
  .subscribe(console.log);

  // H
  // i
  // !

```



Instead, to convert a string as a single value, you’ll want to use the  of  operator:

```
Rx.Observable
  .of('Hi!')
  .subscribe(console.log);

  // Hi!

```

