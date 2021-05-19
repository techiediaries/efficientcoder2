---
layout: post
title:  "The Map JavaScript Data Structure"
date:   2019-06-24
---

In this tutorial, we'll learn about the Map data structure introduced in ES6 to associate data with keys. Before its introduction, people generally used objects as maps, by associating some object or value to a specific key value

## What is a Map

A Map data structure allows to associate data to a key.

## Before ES6

[ECMAScript](https://flaviocopes.com/ecmascript/)  6 (also called ES2015) introduced the Map data structure to the  [JavaScript](https://flaviocopes.com/javascript/)  world, along with  [Set](https://flaviocopes.com/javascript-data-structures-set/)

Before its introduction, people generally used objects as maps, by associating some object or value to a specific key value:

```js
const car = {}
car['color'] = 'red'
car.owner = 'Flavio'
console.log(car['color']) //red
console.log(car.color) //red
console.log(car.owner) //Flavio
console.log(car['owner']) //Flavio

```

## Enter Map

ES6 introduced the Map data structure, providing us a proper tool to handle this kind of data organization.

A Map is initialized by calling:

```js
const m = new Map()

```

### Add items to a Map

You can add items to the map by using the  `set`  method:

```js
m.set('color', 'red')
m.set('age', 2)

```

### Get an item from a map by key

And you can get items out of a map by using  `get`:

```js
const color = m.get('color')
const age = m.get('age')

```

### Delete an item from a map by key

Use the  `delete()`  method:

```js
m.delete('color')

```

### Delete all items from a map

Use the  `clear()`  method:

```js
m.clear()

```

### Check if a map contains an item by key

Use the  `has()`  method:

```js
const hasColor = m.has('color')

```

### Find the number of items in a map

Use the  `size`  property:

```js
const size = m.size

```

## Initialize a map with values

You can initialize a map with a set of values:

```js
const m = new Map([['color', 'red'], ['owner', 'Flavio'], ['age', 2]])

```

## Map keys

Just like any value (object, array, string, number) can be used as the value of the key-value entry of a map item,  **any value can be used as the key**, even objects.

If you try to get a non-existing key using  `get()`  out of a map, it will return  `undefined`.

## Weird situations you’ll almost never find in real life

```js
const m = new Map()
m.set(NaN, 'test')
m.get(NaN) //test

```

```js
const m = new Map()
m.set(+0, 'test')
m.get(-0) //test

```

## Iterating over a map

### Iterate over map keys

Map offers the  `keys()`  method we can use to iterate on all the keys:

```js
for (const k of m.keys()) {
  console.log(k)
}

```

### Iterate over map values

The Map object offers the  `values()`  method we can use to iterate on all the values:

```js
for (const v of m.values()) {
  console.log(v)
}

```

### Iterate over map key, value pairs

The Map object offers the  `entries()`  method we can use to iterate on all the values:

```js
for (const [k, v] of m.entries()) {
  console.log(k, v)
}

```

which can be simplified to

```js
for (const [k, v] of m) {
  console.log(k, v)
}

```

## Convert to array

### Convert the map keys into an array

```js
const a = [...m.keys()]

```

### Convert the map values into an array

```js
const a = [...m.values()]

```

## WeakMap

A WeakMap is a special kind of map.

In a map object, items are never garbage collected. A WeakMap instead lets all its items be freely garbage collected. Every key of a WeakMap is an object. When the reference to this object is lost, the value can be garbage collected.

Here are the main differences:

1.  you cannot iterate over the keys or values (or key-values) of a WeakMap
2.  you cannot clear all items from a WeakMap
3.  you cannot check its size

A WeakMap exposes those methods, which are equivalent to the Map ones:

-   `get(k)`
-   `set(k, v)`
-   `has(k)`
-   `delete(k)`

The use cases of a WeakMap are less evident than the ones of a Map, and you might never find the need for them, but essentially it can be used to build a memory-sensitive cache that is not going to interfere with garbage collection, or for careful encapsualtion and information hiding.
