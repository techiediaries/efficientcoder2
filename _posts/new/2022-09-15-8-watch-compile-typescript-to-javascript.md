---
layout: post
title: "[8] Watch and compile TypeScript code to JavaScript"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "Now, with ts-node, we don't need to compile TypeScript before running it. While this is ideal for development, it will require us to restart the script every time we make a change." 
categories: [webdev]
permalink: /watch-compile-typescript-to-javascript/
previousLink: /setup-typescript-node-expressjs/
nextLink: /graphql-api-apollo-server-studio/
tags : [webdev, nodejs , javascript] 
---

Now, with ts-node, we don't need to compile TypeScript before running it. While this is ideal for development, it will require us to restart the script every time we make a change.

Real-world project development doesn't work this way, so we can take it a step further by using folder-watching utilities like nodemon and ts-node-dev. To make things simpler, let's go with the second option.

Upon returning to the terminal, you must halt any currently active server before issuing the following command from your backend folder:

```bash
npm install --save-dev ts-node-dev 
```

Next, update the `start` script in the `package.json` file, as follows: 

```json
"scripts": { 
  "start": "ts-node-dev --respawn ./src" 
}, 
```

This setup is ideal for development, but when it comes time to deploy the project, you'll only need to compile the TypeScript code to plain JavaScript once.

The tsc compiler can be called directly from another script to accomplish this. Open `package.json` and add the following script, named `build` (or any other valid name):

```json
{
  "name": "backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "ts-node-dev --respawn ./src",
    "build": "tsc --project ./",
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
    "ts-node-dev": "^2.0.0",
    "typescript": "^4.8.3"
  }
}
```

To specify the directory containing the TypeScript files to be converted to JavaScript, use the `—project` flag. 

Specifically, we told it to look in the directory `./` for the `tsconfig.json` file. The tsc compiler can be executed with this command instead of the full `./node modules/typescript/bin/tsc` path.

TypeScript can be installed system-wide with the `-g` switch of the `npm install` command, or we can use npx to launch tsc without the full path: `npx tsc`.

Return to the terminal, kill the server process, and run the following command from the backend's root directory to invoke the `build` script:

```bash
npm run build
```

Invoking the tsc compiler in this way will result in our project's files being compiled as regular JavaScript. Only one file exists so far i.e `index.ts`, which will be translated into JavaScript in the `dist/src/index.js` file.

By setting the `rootDir` property in `tsconfig.json` to `src/`, we can have a flat output of the JavaScript source files located in the `dist/` folder without the `src/` subfolder:

```json
{ 
  "compilerOptions": { 
    "target": "es6", 
    "module": "commonjs", 
    "rootDir": "./src", 
    "outDir": "./dist", 
    "esModuleInterop": true, 
    "strict": true 
  } 
} 

```
Rerunning `npm run build` will now produce results in the `dist/` folder rather than the `src/` subfolder.

This is the TypeScript code compiled to JavaScript:

```js
"use strict";
var __importDefault = (this && this.__importDefault) || function (mod) {
    return (mod && mod.__esModule) ? mod : { "default": mod };
};
Object.defineProperty(exports, "__esModule", { value: true });
const express_1 = __importDefault(require("express"));
const PORT = 8080;
const app = (0, express_1.default)();
app.get('/', (req, res) => res.send('Express &&& Node with TypeScript!'));
app.listen(PORT, () => {
    console.log(`Server is running at http://localhost:${PORT}`);
});
```

We can run the JavaScript with Node as follows:

```bash
node dist/index.js
```

The Express server should now launch and produce output. The server can be accessed at `http://localhost:8080`. **Express & Node with TypeScript!** will be displayed if you navigate to that URL in your browser.

We will be utilizing the `ts-node-dev` tool from now on (using the previous `npm start` command). 

With this setup, we can have our backend project's `src/` folder constantly monitored for any changes we make to the TypeScript source code that invokes the ts-node utility, and have it run and rerun automatically.

So far, we have launched our backend project, downloaded and installed Express.js and TypeScript, and set up a simple server listening on localhost port 8080. 

Next, we'll dive into the specifics of setting up Apollo Server to serve up a GraphQL API.