---
layout: bpost
title: "Using the substring method with React"
categories: [react] 
permalink: "/react-substring/"
author: ahmed
tags: [react, react-18]
excerpt: "The substring method is a method in JavaScript that allows you to get a substring of a string. We can use this method in React either in the class code or in the JSX markup. Let's look at a quick example."
imageSrc: https://www.techiediaries.com/assets/images/icon-512x512.png
---

The `substring()` method is a method in JavaScript that allows you to get a substring of a string. We can use this method in React either in the class code or in the JSX markup. Let's look at a quick example.

To use the `substring()` method, we need to follow these steps:

- Invoke the method with a string.
- Provide the start and end indexes as parameters.
- The method returns a new string containing only the specified part of the original string.

Let's take the following example:

```javascript
const App = () => {
  const str = 'Techiediaries';
  const substr = str.substring(0, 6);
  console.log(substr); // 👉️ "Techie"

  return (
    <div>
      <p>{substr}</p>
    </div>
  );
};

export default App;
```

We need to provide two parameters to the `String.substring` method:

- The start index: or the index of the first character of the string
- The end index - which is the index of the last character but will not be added to the substring.

Please note that the index of the first character is zero.

This is the output in the browser's console:

```javascript
const str = 'Techiediaries';
undefined
const substr = str.substring(0, 6);
undefined
 console.log(substr);
Techie
undefined
```

You can also call the substring method from the JSX markup as follows:

```jsx
const App = () => {
  const str = 'Techiediaries';

  return (
    <div>
      <p>{str.substring(0, 6)}</p>
    </div>
  );
};

export default App;
```



