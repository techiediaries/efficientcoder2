---
layout: post
title: "Storing data with PostgreSQL and TypeORM"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "A PostgreSQL database will be used to store the information. While a local PostgreSQL server will be used during development, a cloud-hosted relational database such as Amazon Relational Database Service (Amazon RDS) or another service of your choosing can be used during production." 
categories: [webdev]
permalink: /storing-data-postgresql-typeorm/
previousLink: /graphql-apollo-front-back-end-integration/
nextLink: /server-nodejs-expressjs-apollo/
tags : [webdev, nodejs , javascript] 
---

A PostgreSQL database will be used to store the information. While a local PostgreSQL server will be used during development, a cloud-hosted relational database such as Amazon Relational Database Service (Amazon RDS) or another service of your choosing can be used during production.

Since PostgreSQL is one of the most popular open source relational databases, we figured that most developers would be familiar with it and might have even used it before in one of their web projects.

Local installation is a breeze on all of the supported operating systems. Thanks to cloud services like Amazon RDS and DigitalOcean, it's also easy to set up and scale in production.

With Amazon RDS, you can start off with a free tier, and any additional usage is billed to you on a pay-as-you-go basis. You'll be able to devote more time and energy to creating useful applications rather than worrying about database management tasks like backups, monitoring, scalability, and replication.

We won't be making use of SQL for anything, so you can forget about making tables or running queries. Instead of using SQL to manage our data, we will be employing an Object Relational Mapper (ORM) to build our database tables and perform operations like querying, inserting, and deleting from those tables. In this scenario, we'll be employing TypeORM, an ORM written in TypeScript.

With the database, the last major part of our backend, now covered, let's take a look at Angular, another part of our web development stack.

The previous articles introduced the full-stack and monorepo architectures of the app we'll be developing throughout the rest of the modern web development series, as well as the technologies we'll be using to develop the app.

We learned about Node.js, a platform necessary for both the frontend and backend of our full-stack application.

After establishing a development environment and installing Node.js, the next step is to develop a server that can run GraphQL in order to implement the backend. Next, we'll set up Express.js, add Apollo Server, and learn about testing and debugging options for our GraphQL server.

