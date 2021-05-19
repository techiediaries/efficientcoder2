---
layout: post
title: "Using Comments in JSON with Node.js and JavaScript Examples"
image: "images/content/json.png"
excerpt: "In this quick example, we'll learn how to add single-line and multiple-line comments to JSON"
date: 2020-09-05 
tags: [javascript, python]
---

In this article, we'll learn how to use comments in JSON files. We'll see workarounds and methods used by developers to add single-line and multiple-line comments to their JSON files, the external libraries and packages for stripping comments from your files before feeding them to the regular `JSON.parse()` method in JavaScript and Node.js and we'll also see simple JavaScript code for removing comments without external libraries. Finally, we'll see the alternative formats to JSON that support comments such as JSON5 and JSONC.   

## JSON Doesn't Support Comments!

As you might be aware of, JSON doesn't support comments! But as programmers, we are used to add comments so in this article, we'll see the possible ways that we have to use comments in our JSON files even if they are natively supported by the format.

In fact, comments were not always missing in JSON but were removed later.

This is the reason of removing comments from JSON as stated by 
[Douglas Crockford](https://web.archive.org/web/20120506232618/https://plus.google.com/118095276221607585885/posts/RK8qyGVaGSr).


>I removed comments from JSON because I saw people were using them to hold parsing directives, a practice which would have destroyed interoperability.

JSON can be mostly needed if you use JSON for your configuration files even if JSON in the first place wasn't designed for this purpose but for exchanging data but, we see it nowadays used as a configuration format in many apps.

For example the `tsconfig.json` file which is the configuration file for TypeScript does allow comments, see [microsoft/TypeScript#4987](https://github.com/microsoft/TypeScript/issues/4987).

You also can use comments to comment out values in your data files when testing instead of removing them.

As developers we tend always to find a solution for our problems and in case of JSON comments, we also have solutions, let's explore it.

## Adding Comments As JSON Data Attributes

Since JSON is a text-based format for storing and exchaning data using key-value pairs, we can actually use comments as data pairs. 

For example, this a simple JSON data:


```json
{
   "name": "Product 1",
   "cost": "10USD",
   "count": 100
}
```

We could simply add a designated data element called "__comment":

```json
{
   "__comment": "This is a comment!",
   "name": "Product 1",
   "cost": "10USD",
   "count": 100
}
```

We use the underscore as a convention to differentiate comments from the rest of data, from the first glance, even if the name of key explicitly tells that it's a comment but still it can be a good convention to avoid any collisions since we may vary well have valid data keys with the same name.

Please note that since JSON format doesn't support comments anymore, they will be parsed and processed just like any other JSON data in your applications.

### Parsing JSON with JavaScript and Node.js

For example, if we use Node.js to parse a `products.json` file with the previous JSON data:

```js
const fs = require('fs');
const path = require('path');

let data = fs.readFileSync(path.resolve(__dirname, 'product.json'));
let product = JSON.parse(data);
console.log(product["__comment"]); // Output: This is a comment!
```

The `__comment` key and its value were parsed into the JSON object and can be accessed by the key name just like the other JSON data.

### Adding Multiple-Line Comments in JSON

You can also use the following format for adding multiple-line comments in JSON:

```json
{
  "//": "This is a comment",
  "//": "This is a second comment"
}
```

It's used in the [Google Firebase documentation](https://firebase.google.com/docs/cloud-messaging/js/client#configure_the_browser_to_receive_messages).


Since JSON and strictly-linted JavaScript don't allow duplicate keys of values, you can add a unique letter or number to make it validate:

```json
{
    "//a": "This is the first comment.",
    "//b": "This is the second second." 
}
```

See this [gist](https://gist.github.com/MoOx/5271067) for more details.

You can also add multiple comments as follows:

```json
{
    "Comments": [
        "First comment,",
        "Second comment"
    ]
}
```

## Adding Comments in JSON with External Tools

We can also create tools that pre-process JSON files and remove comments from our files before parsing them with JSON libraries so we don't get parsing errors. 

This allows us the possibility to use comments in any form we want and avoid adding them as data. For example, we can use the popular comments form found in many C-style programming languages. For example: 

```json
// This is a comment
/* This is a comment */
{
   "name": "Product 1",
   "cost": "10USD",
   "count": 100
}
```

Popular tools that helps you remove comments from your JSON files before parsing them include: 

- Google's GYP supports #-style comments, 
- [JSON.minify](http://github.com/getify/JSON.minify) will help you discard C/C++ style comments with JavaScript/Node.js,
- [JSMin](https://www.crockford.com/jsmin.html) is a minification tool for JavaScript that removes comments and unnecessary whitespace from JavaScript files but can be used for JSON files,
- [Hjson](https://hjson.github.io/): a user interface for JSON that allows you to use comments to document your data inline. You can also use them to comment out values when testing,
- [strip-json-comments](https://www.npmjs.com/package/strip-json-comments): It will replace single-line comments `//` and multi-line comments `/**/` with whitespace. This allows JSON error positions to remain as close as possible to the original source. Also available as a [Gulp/Grunt/Broccoli plugin](https://github.com/sindresorhus/grunt-strip-json-comments),
- [comment-json](https://www.npmjs.com/package/comment-json): Parse and stringify JSON with comments in JavaScript/Node.js. It will retain comments even after saved!
-  [nlohmann/json](https://github.com/nlohmann/json#comments-in-json): A JSON parser for modern C++ that provides optional support for ignoring comments on parsing. 
As stated by the creator of JSON, it's okay to add comments to your JSON data as long as you use a tool to strip them before parsing the file:

> Suppose you are using JSON to keep configuration files, which you would like to annotate. Go ahead and insert all the comments you like. Then pipe it through JSMin before handing it to your JSON parser. - [Douglas Crockford, 2012](https://web.archive.org/web/20120507093915/https://plus.google.com/118095276221607585885/posts/RK8qyGVaGSr).



## JSON5 Supports Comments

The [JSON5](https://json5.org/) Data Interchange Format (JSON5) is a superset of JSON that aims to alleviate some of the limitations of JSON by expanding its syntax to include some productions from ECMAScript 5.1.

This is an example of valid JSON5 data:

```json
{
  // comments
  unquoted: 'and you can quote me on that',
  singleQuotes: 'I can use "double quotes" here',
  lineBreaks: "Look, Mom! \
No \\n's!",
  hexadecimal: 0xdecaf,
  leadingDecimalPoint: .8675309, andTrailing: 8675309.,
  positiveSign: +1,
  trailingComma: 'in objects', andIn: ['arrays',],
  "backwardsCompatible": "with JSON",
}
```

This is how can be installed and used:

```js
npm install json5;
const JSON5 = require('json5');
JSON5.parseJSON();
```

### JSON Schema Supports Comments

[JSON Schema](https://json-schema.org/) is a vocabulary that allows you to annotate and validate JSON documents.

> The $comment keyword is strictly intended for adding comments to the JSON schema source. [See reference](https://json-schema.org/understanding-json-schema/reference/generic.html#id3) 

## JSONC: JSON with Comments Proposal

[Jsonc](https://komkom.github.io/) is a simplified json format which allows comments and unquoted values delimited by whitespace. A jsonc formatted file can be transformed to a json file. Comments will be stripped out and quotes added. 

Jsonc is used as an alternative format in many IDEs that use JSON. For example, Visual Studio Code allows you to set JSONC instead of JSON:

```json
"files.associations": {
  "*.master": "html",
  "*.json": "jsonc"
}
```

## Parsing Comments in JSON with JavaScript and Node.js

If you need a simple solution for adding comments to JSON file without using any external libraries, you can pre-parse the file before parsing them with the actual JSON modules. For example, you can use the following JavaScript code:

```js
const stripJSONComments = (data) => {
  var re = new RegExp("\/\/(.*)","g");
  return data.replace(re,'');
}
 
var jsonData = fs.readFileSync(fileName,'utf8');
jsonData = stripJSONComments(jsonData);
var jsonObject = JSON.parse(jsonData);
```

We first first reads the JSON file in a variable. Next, it makes use of a regular expression to remove the single-line comments i.e “//” from the file, and finally parses the JSON syntax using the `JSON.parse()` method. You can modify the regular to support multiple-line comments.

## Conclusion

JSON doesn't support comments by design. But as we can see from practice, in many situations developers tend to use comments following various tricks, and conventions and even using libraries and plugins that pre-parse JSON files and remove any comments before feeding the output to the regular JSON parser. Comments are also natively supported in many super-set formats of JSON such as JSON5, and YAML, etc.  
