---
layout: bpost
title: "React 18 useEffect runs twice"
date: 2022-05-23 17:54
categories: [react] 
permalink: "/react-18-useeffect/"
author: ahmed
tags: [react, react-18]
excerpt: "If you have just made a new project using Create React App or updated to React version 18, you will notice that the useEffect hook is called twice in development mode. This is the case whether you used Create React App or upgraded to React version 18."
---

If you have just made a new project using Create React App or updated to React version 18, you will notice that the useEffect hook is called twice in development mode. This is the case whether you used Create React App or upgraded to React version 18.

In this article, we'll learn about the **useEffect** hook in React 18's Strict Mode, which has a strange behavior.

The standard behavior of the useEffect hook was modified when React 18 was introduced in March of 2022.

If your application is acting weird after you updated to React 18, this is simply due to the fact that the original behavior of the useEffect hook was changed to execute the effect twice instead of once.

> Although it adds a few enhancements, React 18 introduces some strange behavior to one of the most regularly used hooks -- the useEffect hook.

This only occurs in development mode when strict mode is enabled (the default setting of apps generated with the create-react-app tool).

**The useEffect hook, which should only be called on the first mount, is called two times.**

## React 18 useEffect behavior

A significant change that broke things was introduced in React 18: while Strict Mode is active, all components mount and unmount before being remounted again. 

Because this is done to provide the groundwork for a feature that isn't currently available in React, there is no necessity to do this for the current version of React v18.

This means that each component is mounted, then unmounted, and then remounted and that a useEffect call with no dependencies will be run double-time when it is used in React in version 18.

We can confirm the behavior by using the cleanup function of the useEffect hook.

If we used the useEffect hook as follows:

```javascript
useEffect(() => {
  console.log("First call on mount..");

  return () => console.log("Cleanup..");
}, []);
```

The output to the console should look like this:

```bash
First call on mount..
Cleanup..
First call on mount..
```



Now what if we need to use the `useEffect` hook to fetch data, so that it does not fetch twice?


One easy solution to this behavior is to disable strict mode.

Open the `src/index.js` file and remove the StrictMode higher order component:

```js
import React, { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

import App from './App';

const rootElement = document.getElementById('root');
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

## Conclusion

Beginning with React 18, when in development mode, the components  will be mounted, unmounted, and then mounted once again in StrictMode.

This only occurs in the development mode; it does not occur in the production mode.

This was introduced so that in the future, when React decides to offer a feature that allows it to add or delete an area of the UI while still maintaining the state, this will help it do so. For instance, when moving between tabs, maintaining the state of the preceding tab might assist avoid the execution of effects such as API calls that are not essential.


