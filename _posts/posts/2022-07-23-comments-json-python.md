---
layout: post
title: "Removing Comments from JSON with Python"
image: "images/content/python.png"
excerpt: "How to remove comments from JSON files with Python"
date: 2022-07-25 15:39
tags : [python]
---

JSON doesn't permit comments by design. As explained by its creator [Douglas Crockford](https://web.archive.org/web/20120506232618/https://plus.google.com/118095276221607585885/posts/RK8qyGVaGSr). 

>I removed comments from JSON because I saw people were using them to hold parsing directives, a practice which would have destroyed interoperability.

But he also stated that you can use external or built-in tools to pre-parse JSON files and remove any comments before the actual parsing takes place.

In this short article we'll see how you can remove comments from JSON files using Python code. 

## How to Read JSON Files with Python

First, we need to be able to read JSON files in our Python code:

```python
import json

with open('example.json') as json_file:
    data = json.load(json_file)
        print(data)

```

## How to Remove Comments from your JSON File

There are various workarounds used by developers to [add comments to JSON](https://www.techiediaries.com/json-comments/) files generally. 

You can use JS-style comments (single-line `//` and multiline `/* .. */`) in your JSON files and pre-parse them with your Python code to remove the comments before reading them in the previous way:


```python
import json

with open('data.json', 'r') as jsonfile:
    jsondata = ''.join(line for line in jsonfile if not line.startswith('//'))
    data = json.loads(jsondata)

print(data)
```

You can also use external packages such as:

- [JSON-minify](https://github.com/getify/JSON.minify/tree/python): A port of the JSON-minify utility to the Python language to minify blocks of JSON-like content into valid JSON by removing all whitespace and JS-style comments (single-line // and multi-line /* .. */). With JSON-minify, you can maintain developer-friendly JSON documents, but minify them before parsing or transmitting them over-the-wire. Can be installed using the `pip install JSON-minify` command,
- [commentjson](https://github.com/vaidik/commentjson): A Python package that helps you create JSON files with Python and JavaScript style inline comments. Its API is very similar to the Python standard library's json module. You can install it using the `pip install commentjson` command.


This is an example using:

```python
import commentjson

with open('data.json', 'r') as file:
    ata = commentjson.load(file)

print(data)
```

This is another example from the docs:

```python
>>> import commentjson
>>>
>>> json_string = """{
...     "name": "Vaidik Kapoor", # Person's name
...     "location": "Delhi, India", // Person's location
...
...     # Section contains info about
...     // person's appearance
...     "appearance": {
...         "hair_color": "black",
...         "eyes_color": "black",
...         "height": "6"
...     }
... }"""
>>>
>>> json_loaded = commentjson.loads(json_string)
>>> print json_loaded
{u'appearance': {u'eyes_color': u'black', u'hair_color': u'black', u'height': u'6'}, u'name': u'Vaidik Kapoor', u'location': u'Delhi, India'}
```
