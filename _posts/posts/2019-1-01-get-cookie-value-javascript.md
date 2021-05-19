---
layout: post
title:  "Get Cookie Value with JavaScript"
date:   2019-01-01
---

We can use JavaScript to get the values of cookies stored on a specific page:


The cookies are stored in your browser for a period of 30 days and they are only accessible from the same domain as the page from which they were set. 


## 1. The getCookie() function

```html
<script type="text/javascript"> // Original JavaScript code by Chirp Internet: chirpinternet.eu // Please acknowledge use of this code by including this header. 
function getCookie(name) { 
var re = new RegExp(name + "=([^;]+)"); 
var value = re.exec(document.cookie); 
return (value != null) ? unescape(value[1]) : null; } 
</script> 
```

To display the value of a cookie called  field1  we simply use the following:

`<script type="text/javascript"> document.write(getCookie("field1")); </script>`

If you view the source of this page you will see that this is how the cookie values are being presented in the list above.


Cookies are stored in the  document.cookie  JavaScript object which in your browser currently holds the following name/value pairs:

Each name/value pair displayed above represents a single cookie. A single cookie can hold up to 4kb of text, and for each domain name your browser will normally permit up to 20 cookies.

You can use js-cookie library to get and set JavaScript cookies.

First, you need to add the folowing line to include the library to your HTML file:

```html
<script src="https://cdn.jsdelivr.net/npm/js-cookie@2/src/js.cookie.min.js"></script>
```

Next; you can set a cookie using the following code:

```js
Cookies.set('name', 'value');
```
You can also get the value of a cookie using the following code:

```js
Cookies.get('name'); 
```
