---
layout: post
title: "Setup TypeScript with Node.js & Express.js"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this article, we'll proceed to the next step , which is setting up the necessary requisites, typescript and ts-node with Node.js and Express.js, for development" 
categories: [webdev]
permalink: /setup-typescript-node-expressjs/
previousLink: /server-nodejs-expressjs-apollo/
nextLink: /watch-compile-typescript-to-javascript/
tags : [webdev, nodejs , javascript] 
---

In this article, we'll proceed to the next step , which is setting up the necessary requisites, typescript and ts-node with Node.js and Express.js, for development:

- The tool `typescript` allows you to convert TypeScript code into standard JavaScript.
- With `ts-node`, you can run a TypeScript-based development server in the terminal without first compiling it to vanilla JavaScript.

Now, return to the terminal and type in this command:

```bash
npm install --save-dev typescript ts-node 
```

The `--save-dev` is required to install these libraries as development packages. 

These are the contents of the `package.json` file of the backend application:

```json
{
  "name": "backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.1"
  },
  "devDependencies": {
    "ts-node": "^10.9.1",
    "typescript": "^4.8.3"
  }
}
```

Furthermore, we'll be having a brand-new program, tsc (short for "typescript compiler"), to facilitate the compilation process. It is necessary to specify the full path to the package in order to call it, as we have installed it locally. For instance, using the `-v` switch and the full path to the executable, we can determine the typescript installation version:

```bash
~/ngcommunity$ ./node_modules/typescript/bin/tsc -v 
Version 4.8.3
```

Note that npm installed the two development packages in the root-level `node_modules/` folder.

Next, we need to generate a tsconfig.json file in order to configure typescript in our project. We can do it manually or via the `tsc` compiler as follows:

```bash
~/ngcommunity/packages/backend$ ../../node_modules/typescript/bin/tsc --init 
```

You should get the following output in the terminal:

```bash
Created a new tsconfig.json with:                                               
                                                                             TS 
  target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true
```

The command will also create a `tsconfig.json` file with a lot of options which we don't actually need! A better way is to specify the typescript configuration options via the command line and use npx to invoke the typescript compiler as follows:

```bash
npx tsc --init --rootDir src --outDir dist --lib dom,es6 --module commonjs -removeComments
```

This the ouput of the command in the terminal:

```bash
Created a new tsconfig.json with:                                               
                                                                             TS 
  target: es2016
  module: commonjs
  lib: dom,es6
  outDir: dist
  rootDir: src
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true
```

As it seems the file also contains all the possible options so simply clear the file and replace with the following contents:

```json
{ 
  "compilerOptions": { 
    "target": "es6", 
    "module": "commonjs", 
    "rootDir": "./", 
    "outDir": "./dist", 
    "esModuleInterop": true, 
    "strict": true 
  } 
} 
```

Our remaining file options are as follows:

- target: This is how you tell the compiler which version of JavaScript to use when producing your code. 
- module: This is used to specify the module system of the compiled JavaScript code 
- rootDir: Placement of the TypeScript source files in the project is indicated here.
- outDir: You can use this to point the compiler to a specific directory to save the compiled JavaScript code.
- esModuleInterop: It instructs the compiler to convert ES6 modules into CommonJS modules 
- strict: This is used to enable strict type-checking.

The next step is to run the command to install the Node and Express.js type definitions:

```bash
npm install --save-dev @types/node @types/express
```

As a result, they will be included in the `package.json` file under the `devDependencies` object as follows:

```json
  "devDependencies": {
    "@types/express": "^4.17.14",
    "@types/node": "^18.7.18",
    "ts-node": "^10.9.1",
    "typescript": "^4.8.3"
  }
```

Now that typescript is configured in our project, we need to create the server using Express.js.

In the `backend/` folder's root, type the following commands to create the `src/index.ts` file:

```bash
mkdir src && touch src/index.ts 
```

It's important to keep in mind that the `.js` file extension is reserved for regular JavaScript files, and not TypeScript files (which are saved with the `.ts` extension).

The next step is to open the `src/index.ts` file and insert the necessary code to launch a server on localhost port 8080, as shown below:

```ts
import express, { Application } from 'express'; 

const PORT = 8080; 
const app: Application = express(); 

app.get('/', (req, res) => res.send('Express & Node with TypeScript!'));  
app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}`); 
}); 
```

In the line of code that you just read, we begin by importing express and Application from the express module. Next, we define a constant that will store the port number where our server will be listening.

The next thing that we do is create an instance of an application, and then we use the `get()` method of the Express application to add a route. 

When a GET request is sent to the `/` endpoint, we will be able to execute a handler method thanks to this route's functionality. At last, we make a call to the `listen()` method of the Express application so that it will begin monitoring the specified port for incoming HTTP requests.

The Express application provides us with a number of different methods, which we use to create routes in the Express.js framework. These methods include the names of the HTTP requests that the route is able to process; for instance, you can use `.get()` for GET requests, `.post()` for POST requests, and so on and so forth. A comprehensive list can be found in the documentation, which can be accessed at [https://expressjs.com/en/4x/api.html#app.METHOD](https://expressjs.com/en/4x/api.html#app.METHOD).

Because GraphQL only requires one endpoint that can listen and handle all of the requests in our app, the example that we will be building throughout this series does not require us to use a large number of different routes.

Now, we can run our backend server using ts-node by adding the following `start` script:

```json
{
  "name": "backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "ts-node ./src/index.ts", 
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.1"
  },
  "devDependencies": {
    "@types/express": "^4.17.14",
    "@types/node": "^18.7.18",
    "ts-node": "^10.9.1",
    "typescript": "^4.8.3"
  }
}
```

Then we can run our server by running the following command:

```bash
npm start
```

You should get the following output:

```bash
> backend@1.0.0 start
> ts-node ./src/index.ts

Server is running at http://localhost:8080
```

Your web browser should display a blank page with the following message if you navigate to the address `http://localhost:8080`: **Express & Node with TypeScript!**.

Now that we have our Express.js server up and running, we can learn how to set up our project to automatically recompile our code and restart the server whenever any changes are made while we work.