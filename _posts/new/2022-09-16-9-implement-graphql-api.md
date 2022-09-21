---
layout: post
title: "[9] GraphQL APIs with Apollo Server & Apollo Studio"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "As an open source GraphQL server, Apollo Server is community-maintained and will be the basis for our GraphQL API development in this article." 
categories: [webdev]
permalink: /graphql-api-apollo-server-studio/
previousLink: /watch-compile-typescript-to-javascript/
nextLink: /graphql-api-apollo-server-studio-2/
tags : [webdev, nodejs , javascript] 
---

As an open source GraphQL server, Apollo Server is community-maintained and will be the basis for our GraphQL API development in this article. To begin, a server with a basic schema will be built. Next, we'll incorporate the necessary types and resolvers to query and modify each user's posts, comments, and likes; we'll start with dummy data and then move on to the real thing.

## Installing Apollo Server with TypeScript support

First things first, let's get the required libraries set up. Get back to the terminal and, from the `backend/` folder, execute these commands:

```bash
npm install graphql apollo-server-express @graphql-tools/utils @graphql-tools/schema 

npm install --save-dev @types/graphql graphql-tag 
```

The preceding commands will install the following dependencies: graphql version 15.6.0, apollo-server-express version 3.3.0, @graphql-tools/schema version 8.2.0, @graphql-tools/utils version 8.2.3, graphql-tag version 2.12.5, and @types/graphql version 14.5.0.

In order to translate GraphQL queries into a GraphQL Abstract Syntax Tree, the graphql-tag library provides an exported JavaScript template literal tag.

Let's take the next step and figure out how to set up our GraphQL server and schema. To begin, we will make available a server with a basic application programming interface (API), and then we will develop the API for our community app.

## Making a basic GraphQL API

Having installed the required libraries, we can now learn how to expose a basic GraphQL server with a minimal schema for testing purposes.

Go to the `src/index.ts` file and replace its contents with the following code:

```ts
import express, { Application } from 'express'; 
import { ApolloServer, Config, gql } from 'apollo-server-express'; 
import { IResolvers } from '@graphql-tools/utils'; 

const typeDefs = gql`  
    type Query {  
        echo: String!  
    }  
`;

const resolvers: IResolvers = { 
  Query: { 
    echo: () => 'GraphQL is cool!' 
  } 
};  

const config: Config = { 
  typeDefs: typeDefs, 
  resolvers: resolvers 
}; 

async function startApolloServer(config: Config) { 

  const PORT = 8080; 
  const app: Application = express(); 
  const server: ApolloServer = new ApolloServer(config); 
  await server.start(); 

  server.applyMiddleware({ 
    app, 
    path: '/graphql' 
  }); 

  app.listen(PORT, () => { 
    console.log(`Server is running at http://localhost:${PORT}`); 
  }); 
} 

startApolloServer(config); 
```

As a first step, we include the required imports, including Express.js from the express package, ApolloServer from the apollo-server-express package, and gql from the same package. We then proceed to create an express application after defining our schema type and resolver.

Then, we start up an instance of Apollo Server by passing in the required typeDefs and resolvers. Then, we hook up Apollo Server with the Express app by using `applyMiddleware`, passing in the express app and the GraphQL endpoint as parameters.

A server must be awaited. Start the ApolloServer with `start()` before calling server.`applyMiddleware()`.

The only type definition in our schema is a string with the name `echo` as its only field. An GraphQL-is-cool-!-returning resolver function was added to the `echo` field of the query resolver object.

We assume you have some prior knowledge of GraphQL. If not, head over to [https://graphql.org/learn/](https://graphql.org/learn/).

The next step is to use Apollo Studio to communicate with this API by submitting requests to our backend server.

## Communicating with the GraphQL API via Apollo Studio

If your server is still up and running without any problems, open a web browser and type in http://localhost:8080/graphql. When the interface loads, you should see something like this:

![](https://codevoweb.com/wp-content/uploads/2022/06/nextjs-graphql-typegraphql-api-apollo-server-gui-937x1024.png?ezimgfmt=ng:webp/ngcb1)

From this interface, you can click on the Query your server button to access Apollo Studio, where you can send requests to your API endpoint. 

Write the following GraphQL query to send an `echo` query in the left panel:

```json
query  {
  echo
}
```

Click on the **Run** button. The following results should appear in the right panel: 

```json
{
  "data": {
    "echo": "GraphQL is cool!"
  }
}
```

Our Apollo Server is ready for action, so let's move on to developing the community app's GraphQL schema. We'll be dealing with users, friends, posts, comments, likes, and notifications.