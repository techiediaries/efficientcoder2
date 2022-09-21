---
layout: bpost
title: "React useState by example"
image: "images/content/react.png"
excerpt: "useState() is an example built-in React hook that lets you use states in your functional components. " 
tags : [react]
categories: [react]
---

We'll learn about React's useState hook in this example. It is a [React hook](https://www.techiediaries.com/react-hooks/) that allows you to utilize states in your functional components. This was not feasible prior to the release of **React 16.7**.
 
We'll show how to leverage the built-in hook `useState()` to enable component functions to have local state.

In the previous tutorial, we've cloned our React/Axios/Django project and installed the project's dependencies including React 16.7 which provides support for React Hooks.

Our front-end application has two React components:

- `CustomerCreateUpdate`: for creating and updating customers,
- `CustomersList`:  for listing customers

The API requests sent with Axios are encapsulated in the [`CustomersService.js`](https://github.com/techiediaries/django-react/blob/master/frontend/src/CustomersService.js) class.

Classes are used by both components. Before we get started, let's go over some theory regarding the `useState()` hook.

## Why `useState`

Prior to React 16.7, if you were using functional components in your app and suddenly needed to add some state to your function, you had just one solution: transform your function to a class that extends `React.Component`, then use `this.state` to add initial state and `setState()` to update it.

With the most recent versions of React (for example React 18), you may still utilize functional components while taking use of all capabilities like state and life-cycle functions through hooks.

In this article, we'll focus on the `useState()` hook.


## What Does `useState` Exactly Do
 
The  `useState`  function is a built in hook that can be imported from the `react` package. It allows you to add state to your functional components. Using the  `useState` hook  inside a function component, you can create a piece of state without switching to class components.

There some differences between handling state in functions vs classes:

- In class components, the state is an object accessed using `this.state`; You simply add properties on this object to add an initial state and then use `setState()` to change it later.
- In function components and using `useState`; the state is not necessarily an object. It can be an array, a number, a boolean or a string, etc. You can make multiple calls to  `useState`  in order to create a single piece of state with an initial value but also the function that's used to change that state later.

## Understanding `useState` by Example

Now, let's see an example of `useState` to create a stateful  function component.

This a truncated example of the [`CustomerCreateUpdate`](https://github.com/techiediaries/django-react/blob/master/frontend/src/CustomerCreateUpdate.js) component:

```js
import React, { Component } from 'react';
import CustomersService from './CustomersService';
const customersService = new CustomersService();

class CustomerCreateUpdate extends Component {
    constructor(props) {
        super(props);
    }
    componentDidMount(){}
    handleCreate(){
        customersService.createCustomer(
          {
            "first_name": this.refs.firstName.value,
            "last_name": this.refs.lastName.value,
            "email": this.refs.email.value,
            "phone": this.refs.phone.value,
            "address": this.refs.address.value,
            "description": this.refs.description.value
        }          
        )
    }
    handleUpdate(pk){}
    handleSubmit(event) {}
    render() {}
}
export default CustomerCreateUpdate;
```

This component extends `React.Component`. This allows the component to use the `componentDidMount` life-cycle event and `this.refs` for accessing the form fields.  We'll see in the next part, how to access these features from a function component using hooks like `useEffect()` and `useRef()`. 

This is an a truncated example of the `CustomersList` component which is available from this file:

```js
import React, { Component } from 'react';
import CustomersService from './CustomersService';

const customersService = new CustomersService();

class CustomersList extends Component {
  
  constructor(props) {
    super(props);

    this.state = {
      customers: [],
      nextPageURL: ''

    };
    /*...*/
    
  }  
  componentDidMount() {
    var self = this;
    customersService.getCustomers().then(function (result){

      self.setState({ customers: result.data, nextPageURL: result.nextlink})
    });
  }

  handleDelete(e,pk){    
  }
  
  nextPage(){    
  }

  render() {
    return (
      <div className="customers--list">
          <table className="table">
          <-- Truncated -->

            <tbody>
            {this.state.customers.map( c =>

              <tr key={c.pk}>
              <!-- Truncated -->
              </tr>)}
              </tbody>
          </table>

      </div>
    );
  }
}

export default CustomersList;
```

This component extends `React.Component` and use the `this.state` object to maintain the state which contains the *customers* array and the `nextPageURL` string which stores the link to the next page of data to fetch from the Django API. We also use the `setState()` method to update the state once we get data from the server using the `customersService.getCustomers()` method.

In the `render()` method we loop through the `this.state.customers` array and display our table rows.

Now we want to convert this class component to a function component and be able to maintain state in our function just like the example on top.

```js
import React, { useState } from 'react';

import CustomersService from './CustomersService';

const customersService = new CustomersService();

function CustomersList(props){

  const [customers, setCustomers] = useState([]);
  const [nextPageURL, setNextPageURL] = useState('');

    function handleDelete(e, pk){}
    function nextPage(){}

    return (
      <div className="customers--list">
          <table className="table">
          <thead key="thead">
          <tr>
            <!-- Truncated --> 
          </tr>
          </thead>

            <tbody>
            {customers.map( c =>

              <tr key={c.pk}>
                <!-- Truncated -->
              </tr>)}
              </tbody>
          </table>  
          <button className="btn btn-primary" onClick= { nextPage }>Next</button>

      </div>
    );
}
```

The `useState` hook returns an array with 2 elements. We use using array destructuring to assign names to the two elements. The first element is the initial value of the state, and the second element is a function to set the state; which you can call with a new value to set the related state.

Next, on the `componentDidMount` life-cycle method which is fired when the component is mounted on the DOM we sent a request to the server,  we subscribe to the Observable and then we call the `setState` method  

```js
this.setState({ customers: result.data, nextPageURL: result.nextlink})
```

We can achieve this same behavior using the `useEffect()` hook, `setCustomers()` and `setNextPageURL()` functions that we get from the `useState()` hook.

`useEffect()` is a built in React hook that's used to run side effects and it's equivalent to three life-cycle methods `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`. We'll see more about this hook in a separate tutorial.

Just below the `useState()` hook in your component function, add the following code:

```js
useEffect(() => {
    customersService.getCustomers().then(function (result) {
        setCustomers(result.data);
        setNextPageURL(result.nextlink);  
    });
});
``` 
Finally, just add the implementations for `handleDelete()`  and `nextPage()` in your `CustomersList` function component:

```js
    function handleDelete(e, pk){
       customersService.deleteCustomer({pk : pk}).then(()=>{
         var newArr = customers.filter(function(obj) {
           return obj.pk !== pk;
         });
         setCustomers(newArr);
       });
    }
    function nextPage(){

      customersService.getCustomersByURL(this.state.nextPageURL).then((result) => {
         setCustomers(result.data); 
         setNextPageURL(result.nextlink);
      });      
    }
```

Again, we simply use the `setCustomers()` and `setNextPageURL()` functions returned from destructuring the array returned by the `useState()` hook.

## Conclusion

In this article, we've learned how to utilize the `useState` hook to maintain state inside React 18 functional components.

In the next tutorial, we'll look at another useful built-in hook called `useEffect` which can be used to perform side effects in your function components.
