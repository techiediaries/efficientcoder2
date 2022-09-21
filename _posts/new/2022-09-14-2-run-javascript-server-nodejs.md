---
layout: post
title: "How to Use Node.js to Run JavaScript on Servers"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "Node.js is an open source software platform and runtime environment that makes use of an event-driven, non-blocking input/output (I/O) architecture to enable users to run JavaScript on your server. In 2009, Ryan Dahl developed Node.js on based on Google Chrome's JavaScript Engine (V8 Engine)." 
categories: [webdev]
permalink: /use-nodejs-run-javascript-servers/
previousLink: /full-stack-monorepo-architecture/
nextLink: /graphql-apollo-front-back-end-integration/
tags : [webdev, nodejs , javascript] 
---
In this article, we'll learn how to run JavaScript on the server to build the backend of web applications.

Node.js is an open source software platform and runtime environment that makes use of an event-driven, non-blocking input/output (I/O) architecture to enable users to run JavaScript on your server. In 2009, Ryan Dahl developed Node.js on based on Google Chrome's JavaScript Engine (V8 Engine). 

You're likely acquainted with JavaScript as a programming language that is being used to develop web applications which can only be executed in the client's browser if you're a versed JavaScript programmer. Nowadays, thanks to Node.js, we can run JavaScript on the server to build web apps that run on both the client (via the browser) and the server. 

As intended, developers can now use a single programming language to develop their entire full-stack web application rather than a separate language for programming the server, such as PHP or Python. Like these languages, Node.js can be used to build the backend of your web applications by allowing developers to run JavaScript on servers. 

This allows frontend JavaScript programmers to start working on backend apps without having to study and understand a new language. 

Node.js is often used for more than just server-side applications; it is a general-purpose platform for creating all types of network apps, as well as for developing desktop tools on the developer's machine. For example, the Angular CLI is a Node.js-based command-line interface that developers use to scaffold and work with frontend Angular projects. 

## Installing and publshing libraries with npm
 
The Node.js ecosystem is expansive, and it is supported by the aptly named Node Package Manager (npm). As an example, it can be used to rapidly install packages from a central registry consisting of thousands of packages created and released by other organizations and coders to solve common development problems.

Thanks to the extensive ecosystem and the npm registry, you won't have to start from scratch when attempting to solve development challenges that have already been encountered by other developers. Popular libraries such as React, Vue.js, jQuery, Angular and Express.js can be installed with a single command thanks to the npm registry.

Developers can use npm to get things like Angular, React, and Express.js, among others, for building both frontend and backend projects.

Previosuly, we've discussed how Node.js is both a platform and a runtime environment that exposes a collection of low-level APIs that are infrequently used in the process of creating web applications. 

Instead of writing a lot of complex code ourselves or starting from scratch, we can make use of existing libraries that have already been developed. 

As we've already established, Express.js is just one example of such a library. I mean, what is it, precisely?

When it comes to building server applications, many programmers turn to Express.js, a well-liked web application framework built on top of Node.js. It has no strong preferences, weighs very little, and has all the fundamental features for building websites and web APIs.

If you need to make a server that takes in HTTP requests and returns HTTP responses, but you don't want to deal with the low-level Node.js APIs, Express.js is the right framework. It also facilitates straightforward asset serving, static file management, and routing.

Express.js is a flexible framework that does not enforce how you should organize your project, in contrast to many popular frameworks.

## How to Install Node.js

We'll need to install Node.js so that our server code can be run.

In order to run Node.js on our computer, we can choose from the following methods:

- The Node Version Manager (NVM) allows you to easily install and run multiple versions of Node.js on your development system with a single command.
- The official package manager for the operating system, like Ubuntu's APT, macOS's Homebrew, or Windows' Chocolatey.
- Downloadable binaries (for Windows, macOS, and Linux) and source code (which can be compiled) can be found at https://nodejs.org/en/download/.

We are going to assume that you are using a Debian-based operating system like Ubuntu. In the following section, you will know how to implement the first method on an Ubuntu machine.

You can access the official website at https://nodejs.org/en/download/package-manager/ if you are not running an Ubuntu or Debian-based operating system, and acquire the OS-specific installation instructions for Node.js.

### Using nvm to set up Node.js

Instead of using your operating system's built-in package manager, you can use nvm to install Node.js and npm. To put it simply, this tool does not function on a system wide scale. It instead employs a separate directory within your home directory.

Multiple versions of Node.js can be installed simultaneously, allowing you to easily toggle between them as needed. After nvm is set up, installing any version of Node.js, whether it's old or new, is as easy as typing a single command.

Head over to your command-line interface and run the following command to download the nvm installation script: 

```bash
curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh  
```

Next, use the following command to run the script: 

```bash
bash install.sh 
```

This command will clone the nvm repository under the `~/.nvm` folder and update the `~/.profile` file as necessary. 

To start using nvm, just source the following file or log out and then log back in: 

```bash
source ~/.profile 
```

You can then retrieve a list of available Node.js versions that you can install: 

```bash
nvm ls-remote 
```

Finally, you can install the desired version of Node.js as follows:

```bash
nvm install v14 
```

In the next article, we'll about the modern technologies that developers use to connect the backend and frontend.