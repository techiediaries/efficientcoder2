---
layout: bpost
title: "React 18 refs: the useRef hook & createRef"
date: 2022-05-08 16:36
categories: [react] 
author: ahmed
tags: [react, react-18]
excerpt: "In this article, we will explore how to utilize React refs to access DOM elements using both the class-based approach via the createRef method or the functional-based approach via the useRef hook which is the recommended way in React 18."
---

React often utilizes state to change the contents on the view by re-rendering the component. However, there are times when you need to interact with DOM elements explicitly, and this is when references come in handy. In a nutshell, React refs enable developers to directly access DOM elements. 

React ref is a mechanism for obtaining DOM elements in the render function using JSX rather than using the browser's native `document.getElementById` or `document.querySelector()` methods.

In this article, we will explore how to utilize React refs to access DOM elements using both the class-based approach via the `createRef` method or the functional-based approach via the `useRef` hook which is the recommended way in React 18.

A common use case is auto-focusing text fields when a component renders. Because React does not give a straightforward method to do this, we can utilize refs to directly access the corresponding DOM and focus the text field whenever the component renders.

React Refs are a handy feature that allows you to _reference_ a DOM element or a class component from inside a parent component. This gives us the ability to read and change that element.

We can reference elements in our component's render method, by making a ref using the `React.createRef` method and pass it as the value of the `ref` property.

Here is a simple example:

```javascript
class App extends React.Component {
    componentDidMount() {
        this.elmRef = React.createRef();
    }
    render() {
        return (
            <div>
                <p ref={this.elmRef}>Example react ref</p>
            </div>
        )
    }
}
```

In the example above, we used the `React.createRef` method to create a unique reference and passed it to the `ref` attriute of the `<p>` element. 

This wa, we can obtain the DOM element of the `<p>` element in our component's methods.

The DOM node of the referenced `<p>` element will be contained in the `current` property of the `elmRef` reference.

## How react refs work

React refs are often declared inside class component constructors or as variables at the top level of functional components, and then associated to an element in the `render()` method. Here's a simple example of setting a ref and adding it to a `<input>` element:

```javascript
 constructor(props) {  
   super(props)  
   this.exampeRef = React.createRef();  
 } 
 
 // [...]

 render() {  
   return (  
    <>  
      <input         
        name="firstName"  
        onChange={this.onChange}  
        ref={this.exampeRef}  
        type="text"  
    </>  
   )  
 }  
```

Refs are built using `React.createRef()` and applied to class properties. In the above example, the ref is named `exampleRef` and is then connected to the DOM element `<input>`.

Once a ref is associated to an element, the element may be read and updated through the ref.

Next, include a button into our example. Then, we can connect a `onClick` handler that and use the ref to focus the `<input>` element to which it is linked:

```javascript
  onClick = () => {  
   this.exampleRef.current.focus();

  }
  
  render() {  
   return (  
     ...  
       
     <button onClick={this.onClick}>  
        Focus  
     </button>  
    </>  
   )  
}
```

This effectively modifies the state of the input element without updating the React state. This makes sense when focusing a `<input>`; we wouldn't want to re-render elements whenever we focus or blur an input. There are a few additional instances in which references make sense; we will discuss them later in the article.

The `current` attribute of the ref; refers to the element to which the ref is currently related and can be used to access and change the associated element. 

## React refs with functional components and hooks

Previously, while working with class-based components, we used `createRef()` to create a ref. However, because React now encourages functional components and typical practice is to utilize Hooks, we no longer need to use `createRef()`. To obtain references in functional components, we instead use the `useRef()` hook.

Here is an example of using the useRef() hook to access the DOM element in a react component:

```javascript
import React, {useRef, useEffect} from "react";

export default function (props) {
  const aRef = useRef();
  
  useEffect(function () {
    setTimeout(() => {
      titleRef.current.textContent = "Updated via ref"
    }, 1000); // Update the text content of the division after one second 
  }, []);
  
  return <div ref={aRef}> Will be changed</div>
}
```

This is a second example of using the `useRef` hook to focus an input element:

```javascript
import { useRef } from "react";

function ExampleUseRef() {
  const inputNode = useRef(null);
  
  const onClick = () => {
    inputNode.current.focus();
  };
  
  return (
    <>
      <input ref={inputNode} type="text" />
      <button onClick={onClick}>Focus..</button>
    </>
  );
}
```

## Conclusion

A ref is maybe best described as a link that enables a component to access or change an element to which a ref is associated. Using refs allows us to access elements while avoiding state changes and re-renders; this may be beneficial in certain instances, but should not be used in place of props and state.

Refs also give some freedom for referring elements inside a child component from a parent component through _ref forwarding_ — we'll go over how to accomplish this here, as well as how to inject refs from HOCs into their encapsulated components.

Refs are React developers' escape doors, and we should avoid utilizing them whenever possible!

