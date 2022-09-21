---
layout: post
title: "GraphQL APIs with Apollo Server & Apollo Studio [Part 2]"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "With our Apollo Server now up and running, we can shift our focus to creating the GraphQL schema for the community app. Users, friends, posts, comments, likes, and notifications are what we'll be focusing on." 
categories: [webdev]
permalink: /graphql-api-apollo-server-studio-2/
previousLink: /graphql-api-apollo-server-studio/
tags : [webdev, nodejs , javascript] 
---


With our Apollo Server now up and running, we can shift our focus to creating the GraphQL schema for the community app. Users, friends, posts, comments, likes, and notifications are what we'll be focusing on.

Here, we'll develop the index.ts file by writing the type definitions and resolvers in their own files and then importing them. Let's modify the previous example's directory tree and make sure it still functions as expected before we add any more GraphQL types:

Return to your terminal and, from the  folder of your backend server, execute the following commands:

```bash
cd src && mkdir graphql
cd graphql && touch schema.ts && touch schema.graphql && touch resolvers.ts
```

Next, go to the `src/graphql/schema.graphql` file and add the root type:

```graphql
type Query { 
    echo: String! 
}
```

Next, go to the `src/graphql/resolvers.ts` file and add the resolver for the `echo` field: 

```ts
import { IResolvers } from '@graphql-tools/utils'; 

const resolvers: IResolvers = { 
  Query: { 
    echo: () => 'GraphQL is cool!' 
  } 
}; 

export default resolvers; 
```

We've just finished exporting the resolver for the `echo` field from the file we made, which contains a `resolvers` variable of the `IResolvers` type.

Next, go to the `src/graphql/schema.ts` file and add the following code: 

```ts
import fs from 'fs'; 
import { GraphQLSchema } from 'graphql'; 
import { makeExecutableSchema } from '@graphql-tools/schema'; 
import {  gql } from 'apollo-server-express'; 
import resolvers from './resolvers'; 

  

const typeDefs = gql`${fs.readFileSync(__dirname.concat('/schema.graphql'), 'utf8')}`; 
const schema: GraphQLSchema = makeExecutableSchema({ 
  typeDefs, 
  resolvers
}); 

export default schema; 
```

Here, we use the fs module's readFileSync method to read the query from the graphql file, and we use the import statement to bring in the resolvers from the corresponding typescript file. The typeDefs and resolvers were then passed as arguments to `makeExecutableSchema`, resulting in a schema that could be exported from the `schema.ts` file.

Go to the `src/index.ts` file and update its contents:

```ts
import express, { Application } from 'express'; 
import { ApolloServer } from 'apollo-server-express'; 
import schema from './graphql/schema'; 

async function startApolloServer() { 

  const PORT = 8080; 
  const app: Application = express(); 
  const server : ApolloServer = new ApolloServer({schema}); 
  await server.start(); 

  server.applyMiddleware({ 
    app, 
    path: '/graphql'
  }); 

  app.listen(PORT, () => { 
    console.log(`Server is running at http://localhost:${PORT}`); 
  }); 
} 

startApolloServer(); 
```

Our schema is imported via the `import` statement and sent on to the Apollo Server. Let's finish up our basic social community app by adding the necessary types.

Go to the `src/graphql/schema.graphql` file and add the `User` type, as follows: 

```graphql
type User {
  id: ID! 
  username: String!
  password: String! 
  name: String!
  about: String
  email: String!
  avatarImageURL: String!
  coverImage: String 
  posts: [Post!]
  postsCount: Int!  
  following: Int!
  follower: Int!
  datetime: String! 
}
```
Next, add the Post type:

```graphql
type Post {
  id: ID!
  postedBy: User!
  imageURL: String!
  description: String
  likes: Int!
  comments: [Comment!]
  likedByCurrentUser: Boolean
  datetime: String!
}
```

Next, add the Comment type:

```graphql
type Comment {
  id: ID!
  text: String!
  commentBy: User!
  commentOn: Post!
  datetime: String!
}

```

Then, add the Notification type:

```graphql
type Notification { 
  id: ID! 
  text: String! 
  postId: ID! 
  datetime: String! 
} 
```

Finally, update the Query type as follows:

```graphql
type Query { 
    echo: String! 
    findUser(userId: ID!): User  
    findUsers(searchQuery: String): [User] 
    findPostsByUserId(userId: ID!, offset: Int, limit: Int): [Post!]    
    feed(offset: Int, limit: Int): [Post] 
    findNotificationsByUserId(userId: ID!, offset: Int, limit: Int): [Notification]
    findCommentsByPostId(postId: ID!, offset: Int, limit: Int): [Comment]  
} 
```

Next, add the mutations as follows:

```graphql
type Mutation { 
  post(text: String, image: String): Post 
  deletePost(id: ID!): ID  
  comment(comment: String!, postId: ID!): Comment 
  deleteComment(id: ID!): Comment 
  like(postId: ID!): ID 
  deleteLike(postId: ID!): ID  
  deleteNotification(id: ID!): ID 
}
```

If you run your backend server with `npm start`, the server should start without errors!

This is the basic framework that will be refined in subsequent chapters. Before we move forward with the rest of our GraphQL API development, let's get a grasp on mocking with Apollo Server.