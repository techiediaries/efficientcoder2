---
layout: post
title: "Modern full-stack single-page apps' architecture"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "Node.js is an open source software platform and runtime environment that makes use of an event-driven, non-blocking input/output (I/O) architecture to enable users to run JavaScript on your server. In 2009, Ryan Dahl developed Node.js on based on Google Chrome's JavaScript Engine (V8 Engine)." 
categories: [webdev]
permalink: /full-stack-monorepo-architecture/
nextLink: /use-nodejs-run-javascript-servers/
tags : [nodejs , javascript] 
---

In this article, we'll learn about some common concepts about web applications' architecture and the technologies that developers use to develop both the frontend and backend of web applications throughout a series of articles. We'll begin by becoming acquainted with the concepts of full-stack architecture and monorepo repositories (a.k.a. mono-repositories).

## Modern full-stack single-page apps' architecture

When developing a modern web application (also called single page app or SPA) with a frontend and a backend, you are building what is also known as a full-stack application. If you can build the app you are taking the role of a full-stack developer.

### A full-stack developer?

A full-stack developer is simply a web developer who can develop both the frontend (client-side) and backend (server-side) aspects of a web development project.

### Client-side and server-side apps

You need to be familiar with basic technologies such as HTML, JavaScript, and CSS, (Also called the three pillars of the web) as well as other tools such as TypeScript, Node.js, and database management systems such as MySQL or PostgreSQL. These technologies can be divided into: 

- Client-side tools and programming languages for developing browser-based applications, including HTML, CSS, JavaScript and Webpack. 

- Server-side tools and languages, such as Node.js, Python and Django, as well as database languages, such as SQL. 

## Making a modern full-stack app

Making a modern full-stack application requires two pieces:

- The frontend is the part of the application that the end user interacts with directly, via their web browser, on their own computer. Traditionally, this was achieved via server-rendered HTML pages that were then returned to the client. However, modern browsers are able to run full-fledged JavaScript applications (also called client-side apps), which perform the bulk of their work in the client's browser and depend on servers primarily to provide the application's initial files and data. In layman's terms, the frontend is the part of the system that displays the UI.

- The backend is the server-side code that receives HTTP requests, processes them, and sends back results to the user's browser in the form of HTML and/or JSON.

### The monorepo development process

You can use monorepo tools like Lerna to structure the code for your web application. Monorepo is a software development process that revolves around using a single source code repository (usually version controlled using Git) for all of your application's code in the  (the server, client, and any shared libraries).

## Delevering the web app to the client

Let's take a bird's-eye view of the process by which our modern full-stack application will reach the client's machine:

- The client initiates communication by typing the domain name associated with your application into the browser's address bar.

- The server will handle the requests and run the single-page app embedded in the HTML document.

- All of the single-page app's necessary JavaScript and CSS files will start downloading to the client's browser.

- The app will send any data requests to the server, and then display the results in the UI.
After a user interacts with the app, only then will the remaining server requests be sent.

This demonstrates the fundamentals of how our web app functions. In subsequent articles, we'll examine each of these steps in more detail.

After briefly discussing the full-stack and monorepo concepts, next, we'll take a look at the technologies and tools we can use to develop modern web applications.

## Example web development technolgies

There is always a special set of equipment and software that must be used for a specific web development project. It includes everything from programming languages to CLIs to libraries to frameworks. In a full-stack project, we'll be implementing a wide variety of technologies across the board. 

The frontend and the backend can often be built with the same set of tools in today's modern development. Let's take a closer look at these modern tools and methods. 

The front end will be written in Angular (currently at version 14), the back end in Node.js, and a number of libraries will be used to facilitate these two languages' integration, such as Express.js to initiate a web server and TypeORM to provide an abstraction layer over database operations. 

The GraphQL API is one of the primary technolgy, as it can be used to connect the backend to the frontend. It's a modern alterantive to REST APIs. Express.js is an example library that can be used on top of Node.js to provide an abstraction over the various low-level APIs required to run a web server. 

The client's web browser sends HTTP requests to the server, the vast majority of which will be GraphQL queries to retrieve data and mutations to create or update that data. 

## Building the front-end using TypeScript and Angular.

The Angular client-side framework developed by Google is free and available to the public. To replace the original Angular.js, written in vanilla JavaScript, it was built from the ground up in TypeScript. 

Angular is one of the three most widely used frontend frameworks, along with React and Vue. It has all the necessary libraries for creating cutting-edge, cross-platform web applications' front ends.

Using the official Angular CLI, we'll create a new project with a basic file structure, and then we'll use abstractions like modules, components, and services to structure our TypeScript code on the client side.
Because Angular comes with a client-side router, we can easily implement routing and navigation. In the upcoming chapters, we'll go into greater detail about these methods before we start using them.

We'll be using Apollo Client with Angular to connect our front end to the GraphQL server, which was developed with Apollo Server.

Once we combine Node.js, MySQL, Express.js, TypeORM, Apollo, and Angular, we will have a full technology stack.
After discussing the app's framework and technologies, we can set up our development environment with MySQL and Node.js.

In the next article, we'll dive into Node.js and see how it can be put to good use in your projects. 