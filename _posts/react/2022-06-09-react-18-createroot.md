---
layout: bpost
title: "Understanding React 18 root API: ReactDOM.createRoot"
categories: [react] 
permalink: "/react-18-createroot/"
author: ahmed
tags: [react, react-18]
excerpt: "The introduction of the new root API, ReactDOM.createRoot, is one of the most significant improvements in React 18."
imageSrc: https://www.techiediaries.com/assets/images/icon-512x512.png
---

The introduction of the new root API, `ReactDOM.createRoot`, is one of the most significant improvements in React 18. 

The new root API overcomes the problem of passing the container when performing the updates. The container is no longer required to be sent into the render.

Let's look at the following example:

```javascript
import ReactDOM from "react-dom";
import App from './App';

const container = document.getElementById('root');

// Create a root.
const root = ReactDOM.createRoot(container);

root.render(<App name="Techiediaries" />);

// Second update
root.render(<App name="Techiediaries website" />);
```

In the second render, we don't no need to pass the container.

The new root API serves as the entry point for new React 18 features and delivers out-of-the-box enhancements.

Check out this [WG discussion](https://github.com/reactwg/react-18/discussions/5) for a more in-depth look at the new root API.

## How to solve `ReactDOM.render` is no longer supported in React 18

The error "ReactDOM.render is no longer supported in React 18. Use createRoot instead" occurs because the ReactDOM.render method has been deprecated. To solve the error, create a root element and use the ReactDOMClient.render method instead.

This occurs since the `render()` method of the react-dom package is considered legacy starting react-dom version 18.

The method has been substituted with the `createRoot()` method exported from react-dom/client.

You can solve the error, by creating a root element and use the `ReactDOMClient.render` method as follows 👇️:

```javascript
import {StrictMode} from 'react';
import {createRoot} from 'react-dom/client';

import App from './App';
const root = document.getElementById('root');
const root = createRoot(root);


root.render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

## Conclusion

The `createRoot()` method constructs a React root by passing the root element as a parameter.

