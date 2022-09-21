---
layout: post
title: "How to add days to dates in JavaScript"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this article we will learn how to add days to dates in JavaScript." 
categories: [javascript]
permalink: /howto-add-days-dates-javascript/
tags : [javascript] 
---

We can add days to dates in JavaScript as follows:

- Call the JavaScript' `getDate()` method to obtain the day of the month for the date.
- Call the JavaScript' `setDate()` method, and passing as a parameter the result of calling `getDate()` plus the number of days you need to add.

The `setDate()` method takes care of adding the wanted number of days to the `Date` object.

Let's take a simple example of adding 1 day to a date:

```js
const date = new Date();
console.log(date);
// Tue Sep 20 2022 19:46:50 GMT+0100 (GMT+01:00)
date.setDate(date.getDate() + 1);
console.log(date);
// Wed Sep 21 2022 19:45:27 GMT+0100 (GMT+01:00)
```

When we first create an instance of the Date object, the date contains the date of today. We use the console.log() method to display it. Then we add a day which will give to date of tomorrow.

## Add days to any given date in JavaScript

If you need to add days to any day, simpy create a data object of the desired date as follows:

```js
const tomorrow = new Date('2022-09-21');
console.log(tomorrow);
// Wed Sep 21 2022 01:00:00 GMT+0100 (GMT+01:00)
```

We specify the desired as a string to the Date constructor.

Next, simply add the number of the days as before.




