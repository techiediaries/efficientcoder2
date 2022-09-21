---
layout: post
title: "How to get current time zone in JavaScript"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this article we will learn how to get current time zone in JavaScript" 
categories: [javascript]
permalink: /howto-get-current-time-zone-javascript/
tags : [javascript] 
---

You can get your time zone using [JavaScript](https://30daysofjavascript.com/) by using the `Intl.DateTimeFormat` object which is available in all modern browsers and returns the language-specific date and time formatting methods.

Here is how it works. Simply open the developer console in your web browser and run the following code: 

```js
const timezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
console.log(timezone); 
// Africa/Casablanca
```

This is the exact time zone but you can also get the offset in minutes of your timzeone using `getTimezoneOffset()` method.

## Get timezone offset in minutes

The `getTimezoneOffset()` method of the JavaScript Date object can be used to obtain the time zone of the web browser.

The `getTimezoneOffset()` function gives you the offset, in minutes, from Coordinated Universal Time to your current time zone. 

Positive is returned if the local time zone is later than UTC, and negative if it is earlier. The `getTimezoneOffset()` is available in all modern browsers.

Let's take the following example:

```js
const date = new Date();
console.log(date.getTimezoneOffset());    // -60
```

The difference between my time zone and UTC is 60 minutes which means one hour.

Please note that Daylight Saving Time (DST) causes the returned value to fluctuate throughout the year.

The result of a call to `getTimezoneOffset()` for a time zone that transitions into and out of Daylight Saving Time (DST) each year can vary by up to 60 minutes.
