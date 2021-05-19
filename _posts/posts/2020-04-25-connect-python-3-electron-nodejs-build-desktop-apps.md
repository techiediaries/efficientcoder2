---
layout: post
title:  "Connecting Python 3 and Electron/Node.JS: Building Modern Desktop Apps"
date:   2020-04-25
tags: [ python ]
---

In this post, you’ll learn about the possible ways that you can use to connect or integrate Python with Node.js and Electron with simple examples.

We'll introduce Electron for Python developers, a great tool if you want to build GUIs for your Python apps with modern web technologies based on HTML, CSS and JavaScript. We'll also see different ways to connect Python and Electron such as `child_process`, `python-shell`  and an HTTP (Flask) server.


## Prerequisites

This tutorial is designed for Python developers that want to build desktop applications and GUIs with modern web technologies, HTML, CSS, and JS and related frameworks.

To be able to follow this tutorial comfortably, you will need to have a few prerequisites:

- Knowledge of Python is a must. 
- You should be comfortable with working with web technologies like JavaScript, HTML, and CSS.
- You should also be familiar with installing and using Node.js packages from npm.  
  

## Step 1 - Setting up a Development Environment

In this section, we'll set up a development environment for running our examples. We need to have Node.js together with NPM and Python 3 installed on our machine.
 
### Installing Node.js and NPM

There are various ways that you can follow to install Node.js and NPM on your development machine, such as using:

- the official binaries for your target operating system, 
- the official package manager for your system, 
-  [NVM](https://github.com/nvm-sh/nvm) (Node Version Manager) for installing and managing multiple versions of Node.js in the same machine.

Let's keep it simple, simply go to the [official website](https://nodejs.org/en/download/) and download the binaries for your target operating system then follow the instructions to install Node.js and NPM on your system.
 
### Setting up a Python Virtual Environment  

There is a big chance that you already have Python 3 installed on your development machine. In case it's not installed, the simplest way is to go to the  [official website](https://www.python.org/downloads/)  and grab the binaries for your target system.


You can make sure you have Python 3 installed on your system by opening a command-line interface and running the following command:

```shell
$ python --version
Python 3.7.0
```
Now, let's set up a virtual environment.

### Creating a Virtual Environment

In this section, you’ll use  `venv`  to create an isolated virtual environment for  running your example and install the required packages.

A virtual environment allows you to create an environment for isolating the dependencies of your current project. This will allow you to avoid conflicts between the same packages that have different versions.

In Python 3, you can make use of the  `venv`  module to create a virtual environment.


Now, head over to your terminal and run the following command to create a virtual environment:

```shell
$ python -m venv env
```

Next, you need to activate the environment using following command:

```shell
$ source env/bin/activate
```

On Windows, you can activate the virtual environment using the `Scripts\activate.bat` file as follows:

```shell
$ env\Scripts\activate.bat
```

That’s it. You now have your virtual environment activated, and you can install the packages for your example.

## Step 4 - Creating a Node.js Application 

Now that we have setup our development environment for Python and Electron development by installing Node.js together with npm and creating a Python virtual environment, let's proceed to create our Electron application.

Let's start by creating a folder for our project and create a `package.json` file inside it using the following commands:

````shell
$ mkdir python-nodejs-example
$ cd python-nodejs-example
$ npm init -y
```

The `init` command of npm will generate a  `package.json`  file with the following default values:

```json
{
  "name": "python-nodejs-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

You can customize the values in this file as you see fit in your project and you can also simply go with the deafult values for this simple example.

Next, we need to create two  `index.html`  and  `index.js`  files inside the project's folder:

```shell
$ touch index.js 
$ touch index.html
```
  
## How to Communicate Between Electron and Python 

In this section, we'll see the various available ways that you can use to achieve communications between Electron and Python processes.
  
### What is IPC?
 
According to [Wikipedia](https://en.wikipedia.org/wiki/Inter-process_communication):

>In computer science, inter-process communication or interprocess communication (IPC) refers specifically to the mechanisms an operating system provides to allow the processes to manage shared data. Typically, applications can use IPC, categorized as clients and servers, where the client requests data and the server responds to client requests.

IPC refers to a set of mechanisms supported by operating systems to enable different, local or remote, processes to communicate with each other. For example, in our case, we want to allow communications between the Electron process and the Python process.

Let's see some ways to achieve IPC.   

### Spawning a Python Process Using `child_process`

Node.js provides the {`child_process`](https://nodejs.org/api/child_process.html) module whih allows you to spawn child processes.

Let's use it to spawn a Python process and run a simple `calc.py` script.

We'll make use of [simplecalculator](https://pypi.org/project/simplecalculator/) to do simple calculations in Python, so we first run the following command to install it:

```bash
$ sudo pip install simplecalculator
```

First, inside your project's folder, create a `py` folder and create a `calc.py` file inside of it:

```bash
$ mkdir py & cd py
$ touch calc.py
```

Open the `calc.py` file and add the following Python code which performs a calculation and prints the result to the standard output:

```python
from sys import argv
from calculator.simple import SimpleCalculator

def calc(text):
	"""based on the input text, return the operation result"""
	try:
		c = SimpleCalculator()
		c.run(text)
		return c.log[-1]
	except Exception as e:
		print(e)
		return 0.0

if __name__ == '__main__':
    print(calc(argv[1]))

```

Next, create a  `renderer.js`  file, and add the following code to spawn a Python process and execute the  `py/calc.py`  script:

```js
function sendToPython() {
  var python = require('child_process').spawn('python', ['./py/calc.py', input.value]);
  python.stdout.on('data', function (data) {
    console.log("Python response: ", data.toString('utf8'));
    result.textContent = data.toString('utf8');
  });

  python.stderr.on('data', (data) => {
    console.error(`stderr: ${data}`);
  });

  python.on('close', (code) => {
    console.log(`child process exited with code ${code}`);
  });

}

btn.addEventListener('click', () => {
  sendToPython();
});

btn.dispatchEvent(new Event('click'));
```


Next, open the `index.html` file and update it as follows:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Calling Python from Electron!</title>
</head>

<body>
    <h1>Simple Python Calculator!</h1>
    <p>Input something like <code>1 + 1</code>.</p>
    <input id="input" value="1 + 1"></input>
    <input id="btn" type="button" value="Send to Python!"></input>
    </br>
    Got <span id="result"></span>
    
    <script src="./renderer.js"></script>

</body>
</html>
```

### Using `python-shell`

After seeing how to use `child_process` to do communication between Electron and Python, let's now see how to use  `python-shell` .

[`python-shell`](https://www.npmjs.com/package/python-shell)  is an npm package that provides an easy way to run Python scripts from Node.js with basic and efficient inter-process communication and error handling.

You can use  `python-shell` for:

-   Spawning Python scripts,
-   Switching between text, JSON and binary modes,
-   Doing  data transfers through  `stdin`  and  `stdout`  streams,
-   Getting stack traces in case of errors.

Go to your terminal, and run the following command to install  `python-shell`  from npm:

```bash
$ npm install --save python-shell 
```

As the time of this writing, `python-shell v1.0.8` is installed in our project.

Next, open the `renderer.js` file and update the `sendToPython()` function as follows:

```js
function sendToPython() {
  var { PythonShell } = require('python-shell');

  let options = {
    mode: 'text',
    args: [input.value]
  };

  PythonShell.run('./py/calc.py', options, function (err, results) {
    if (err) throw err;
    // results is an array consisting of messages collected during execution
    console.log('results: ', results);
    result.textContent = results[0];
  });

}
```

### Using Client-Server Communication

Let's now see another way to achieve communication betweeen Python and Electron using an HTTP server.

Head back to your terminal and run the following command to install Flask and Flask-Cors:

```bash
$ pip install flask
$ pip install Flask-Cors
```
 
Next, in the `py` folder of your project, create a  `server.py`  file and add the following code to run a Flask server that simply performs a calculation and returns the result as an HTTP response:

```python
import sys
from flask import Flask
from flask_cors import cross_origin
from calculator.simple import SimpleCalculator

def calcOp(text):
	"""based on the input text, return the operation result"""
	try:
		c = SimpleCalculator()
		c.run(text)
		return c.log[-1]
	except Exception as e:
		print(e)
		return 0.0

app = Flask(__name__)

@app.route("/<input>")
@cross_origin()
def calc(input):    
    return calcOp(input)

if __name__ == "__main__":
    app.run(host='127.0.0.1', port=5001)
```

Next, open the  `renderer.js`  file and add the following code to spawn Python and run the  `server.py` file:

```js
let input = document.querySelector('#input')
let result = document.querySelector('#result')
let btn = document.querySelector('#btn')

function sendToPython() {
  var { PythonShell } = require('python-shell');

  let options = {
    mode: 'text'
  };
  
  PythonShell.run('./py/server.py', options, function (err, results) {
    if (err) throw err;
    // results is an array consisting of messages collected during execution
    console.log('response: ', results);

  });
}

function onclick(){

  fetch(`http://127.0.0.1:5001/${input.value}`).then((data)=>{      
      return data.text();
      
  }).then((text)=>{
    console.log("data: ", text);
    result.textContent = text;
  }).catch(e=>{
    console.log(e);
  })

}
sendToPython();

btn.addEventListener('click', () => {
  onclick();
});

btn.dispatchEvent(new Event('click'))
```

## Recap 

In this tutorial, we've introduced Electron for Python developers which can be a great tool if they want to build GUIs for their Python apps with modern web technologies based on HTML, CSS and JavaScript. We've also seen different ways to connect Python and Electron such as `child_process`, `python-shell`  and an HTTP (Flask) server.






