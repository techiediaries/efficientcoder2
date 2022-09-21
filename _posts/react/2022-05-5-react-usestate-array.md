---
layout: bpost
title: "Handling Arrays with React 18 useState"
date: 2022-04-30 17:54
categories: [react] 
permalink: "/react-18-usestate-array/"
author: ahmed
tags: [react, react-18]
excerpt: "React is a JavaScript library and because JS already has a variety of methods for working with arrays. It makes dealing with arrays simpler although when integrating that with React hooks, you need to use specific methods as state is immutable."
---

React is a JavaScript library and because JS already has a variety of methods for working with arrays. It makes dealing with arrays simpler although when integrating that with React hooks, you need to use specific methods as state is immutable.

We'll be showing simple examples with react 18 but if you need to run the examples, you'll need to create a react project using the create-react-app utility.

React provides the [useState](https://www.techiediaries.com/react-usestate-hook/) method to create state which can be used as follows:

```javascript
const [version, setVersion] = useState('v18');
```

## How to add a new value to React's array state 

Let's get started with the first example of using useState to create an array state and add a new value to it.

Let's presule, we have the following array of products:

```javascript
const productsArray = ['a', 'b', 'c'];
```

We can create a state in react, with the previous array's values as default, using the following syntax:

```javascript
import { useState } from "react";
const [products, setProducts] = useState(productsArray);
```

We can simply just add the array directly as follows:

```javascript
import { useState } from "react";
const [products, setProducts] = useState(['a', 'b', 'c']); 
```

We can also create a state based on an empty array:

```javascript
import { useState } from "react";
const [products, setProducts] = useState([]); 
```

Now, let's see how to add/push a new value to the array state.

Since the state is immutable, we can't simply push a new value value to the array like we do normally in plain JavaScript. Instead, we need to use the `setProducts` method returned by the previous call to useState and the spread operator of JavaScript.

Precisely; we need to return a new state based on the current state and the new value. To do this, the setter function accepts a callback function that takes the current state as argument:

```javascript
setProducts((prevProducts) => [ ...prevProducts, 'd']);
```
We spread the previous state passed as an argument and we add our new value to the new array that we return from the setter function. React will use the returned array as the new state. As a consequence, we could add the new value to the state of our react component.


## Conclusion

As a JavaScript library, React has access to a wide range of array handling capabilities. Array manipulation is simplified, but since state is immutable, specific methods are required when using React hooks.

As a recap, because the state is immutable, we are unable to simply add a new value to the array in the same way that we would ordinarily do with regular JavaScript. Instead, we must make use of the setter function, which is returned by the call to `useState`, as well as the JavaScript spread operator or similar operators.

To be more specific, we must return a new state that is based on the existing state and the new value. In order to do this, the setter method accepts a callback function that takes as an argument the current state.


