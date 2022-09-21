---
layout: post
title: "[4] Using GraphQL and Apollo for front- and back-end integration"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "We know what Node is and that our backend server will be run by Express.js, we need to know what GraphQL is and how it fits into this stack." 
categories: [webdev]
permalink: /graphql-apollo-front-back-end-integration/
previousLink: /use-nodejs-run-javascript-servers/
nextLink: /storing-data-postgresql-typeorm/
tags : [webdev, nodejs , javascript] 
---

We know what Node is and that our backend server will be run by Express.js, we need to know what GraphQL is and how it fits into this stack.

In the same way that SQL queries databases, GraphQL queries web APIs. To put it simply, it's a query language and application programming interface specification. 

It's a runtime environment, responding to queries for information retrieval, creation, and modification.

Developers typically use REST to build APIs that can be consumed by desktop and mobile clients, but GraphQL is a more recent alternative.

By releasing GraphQL in 2015, Facebook aimed to solve problems with traditional RESTful API development practices.

For instance, GraphQL enables the frontend app to only request the information it actually needs from the server. This is because GraphQL lets you define your own types based on the built-in types (such as strings and integers) that you already use.

GraphQL is compatible with many different kinds of network protocols; WebSocket and HTTP are just two of the most common ones.

If you are familiar with Structured Query Language (SQL), you will recognize the table-based definition of data formats as being very similar.

Using a format similar to JSON, you can ask the server to return only certain fields in its HTTP response. 

Data resolution is handled by a resolver function, which executes the necessary logic to retrieve data from the database and return it to the requesting client.

As was mentioned before, GraphQL is an independent standard that can be implemented in any language or framework. Python and JavaScript are just two examples of the many popular programming languages that have implementations.

Additionally, it is database system agnostic and can be used as such with any technology stack that employs PostgreSQL or any other database management system. It will be integrated with PostgreSQL in our case.

## Implementing GraphQL with Apollo

Apollo is a GraphQL implementation. How does it stack up against the rest of our technologies?

Apollo is a GraphQL implementation for JavaScript that is widely used in the industry, and it will be used to connect the front end to the back end. It consists of a client component (called Apollo Client) and a server component (called Apollo Server).

Common Node.js frameworks are supported by Apollo Server while Apollo Client supports both plain JavaScript and modern UI libraries and frameworks (such as React, Angular, and Vue.js)

There is no longer any need for intricate logic to move data between the front and back ends of our application because of Apollo Client and Apollo Server.

Consequently, the Apollo Client will serve as an extra layer in our frontend, allowing us to send queries to the database to retrieve information and to make updates or deletes as necessary through mutations. The Apollo Client communicates not with the database itself, but with the Apollo Server and Express.js, both of which are built atop Node.js.
