---
layout: post
title: "Mocking GraphQL with Apollo Server"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "" 
categories: [webdev]
permalink: /mock-graphql-apollo-server/
previousLink: /graphql-api-apollo-server-studio-2/
tags : [webdev, nodejs , javascript] 
---


Thanks to mocking, UI development can begin before the backend is fully functional. The UI can be tested without having to wait for lengthy database operations or maintaining a full-fledged GraphQL server.

There are a number of community-built tools, such as graphql-tools, graphql-faker, and Apollo Server's in-built mocking feature, that make it simple to simulate user interactions with a GraphQL API by simulating their queries and schema.

In order to test our resolvers before actually implementing them, let's use Apollo Server's mocking feature to simulate GraphQL requests.

Head to the `src/index.ts` file and simply add `mocks: true` to Apollo Server, as follows: 

```ts
const server : ApolloServer = new ApolloServer({schema , mocks: true}); 
```

The mocking functionality built into Apollo Server is simplistic. A value of the same type as the associated field is simply returned based on the type definitions.
Please enter the following query into Apollo Studio:

```ts
query { 
  findUsers(searchQuery:"ahmed"){ 
    id name email postsCount 
  } 

  findPostsByUserId(userId:"10"){ 
    id text postBy { id name } commentsCount 
    latestComment { id comment } 
    datetime 
  } 
} 
```