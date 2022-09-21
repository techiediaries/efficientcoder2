---
layout: post
title: "[6] Setup the server with Node.js/Express.js/Apollo"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In the previous articles, we installed Node.js and PostgreSQL on our development machine. In this article, we'll begin by developing the API that the frontend app will consume." 
categories: [webdev]
permalink: /server-nodejs-expressjs-apollo/
previousLink: /storing-data-postgresql-typeorm/
nextLink: /setup-typescript-node-expressjs/
tags : [webdev, nodejs , javascript] 
---

In the previous articles, we installed Node.js and PostgreSQL on our development machine. In this article, we'll begin by developing the API that the frontend app will consume.

In order to get through the series in a reasonable amount of time, we'll try to develop a basic community app with just the fundamental features.

There will initially be a signup and account creation screen. An email address and password are needed to access this interface after registration or login. 

They will be asked for their full name, email address, password, and a second password to confirm their identity.

After signing in, they'll be taken to the app's main interface, where they can post contents and view their feed, which shows content created by other app users. Users can also conduct a name search to locate one another and then navigate directly to their profiles.

A user can view both their own profile, which includes a photo and posts, and the profiles of all other users. There's also a chat feature where they can type and send each other messages.

In the end, users can interact with each other's posts by commenting or liking them, and they'll be notified when others do the same.

To implement the back end, we must first prepare the development environment by installing Node.js (which is done!), and then set up a Node.js server with GraphQL/Apollo support.
 
Following this article, you will be able to successfully set up Express.js with TypeScript and GraphQL.

After that, we'll look at how to mock Apollo Server's GraphQL server so that it can be used for testing before the resolvers responsible for fetching and modifying data have been written.

## Requirements

This series assumes that you have Node.js and npm set up on your local development machine. If you have not already done so, please refer to the previous articles, for information on how to set them up.

A working knowledge of the following technologies is also required:

- JavaScript and TypeScript (Read the official docs if not)
- Familiarity with Node.js (Read the official docs if not)
- Familiarity with GraphQL concepts such as schemas and types (Read https://graphql.org/learn/.)  

## Configuring a monorepo project with lerna

To keep things simple, rather than maintaining separate repositories for our client and server apps (and any shared libraries), we'll be taking a monorepo way of managing our project. For this, we'll be employing a program called Lerna. Okay, so here we go:

To get Lerna set up, enter the following commands into your CLI:

```bash
mkdir ngcommunity
cd ngcommunity/
npx lerna init
```

You'll a similar output

```bash
erna notice cli v5.5.1
lerna info Creating .gitignore
lerna info Creating package.json
lerna info Creating lerna.json
lerna info Creating packages directory
lerna success Initialized Lerna files
lerna info New to Lerna? Check out the docs: https://lerna.js.org/docs/getting-started
```

Configuration for lerna is stored in the `lerna.json` file, while project-wide (client and server) settings are kept in the `package.json` file. Client and server applications, along with any shared libraries, should be placed in the `packages/` folder. 

This command will create a Git repository by executing the `git init` command. To learn more about how to tailor lerna to your needs, head over to https://github.com/lerna/lerna#lernajson.

You also need to create a `.gitignore` file and add the following contents:

```txt
node_modules 
.DS_Store 
```

Next, go to the `lerna.json` file and set version to “0.0.1”: 

```json
{
  "$schema": "node_modules/lerna/schemas/lerna-schema.json",
  "useNx": true,
  "useWorkspaces": true,
  "version": "0.0.1"
}
```

Next, go to the `package.json` file and set version to “0.0.1” as follows:

```json
{
  "name": "root",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "devDependencies": {
    "lerna": "^5.5.1"
  },
  "version": "0.0.1"
}
```

Please visit [https://lerna.js.org/](https://lerna.js.org/) for more details about lerna.

Next, create a new repository in your GitHub account to store the code for your project.

This is a straightforward procedure; however, if you run into any problems, be sure to read up on the topic at [https://help.github.com/articles/creating-a-new-repository/](https://help.github.com/articles/creating-a-new-repository/).

Next, using the following command, you can connect your local repository to the remote one:

```bash
git remote add origin <Insert your repository's URL here>
```

Next, stage and commit the changes using the following command: 

```bash
git add -A 
git commit -m  "Configure lerna" 
```

You can push the code by running the following command: 

```bash
git push -u origin main
```

Let's get our server-side project up and running now that we've set up a monorepo with lerna.

## Setting up our server project

In this section, we’ll understand how to setup our server project and install Express.js, which will aid us rapidly start a server without dealing with the low-level APIs provided by the Node.js platform.

Developers can use the `npm init` command to start a new project (an empty folder with a `package.json` file). 

Okay, let's see how to use it to make a `package.json` file.

We’ll start by creating an empty folder for our backend files inside the `ngcommunity/packages/` folder: 

```bash
cd packages 
mkdir backend && cd backend 
``` 

Next, run the following command: 

```bash
npm init --yes 
```

The `--yes` flag instructs npm to generate a `package.json` file with the default settings. 

You'll get the following output on the screen:

```bash
Wrote to /home/ahmed/ngcommunity/packages/backend/package.json:

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
  "license": "ISC"
}
```

Assuming you have already created a `package.json` file in your project folder, you can move on to installing Express.js and the other necessary development dependencies, such as typescript and ts-node.

## Incorporating Express.js and its prerequisites into our development environment

Installing the express package from the npm registry is the next step after we have set up our backend project. Simply run the following command:

```bash
npm install express
```

A new object called `dependencies` will be added to the `package.json` file with the following information:

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
  }
}
```

This leads us to the next step, which is to set up the necessary requisites, which are typescript and ts-node, for development.

