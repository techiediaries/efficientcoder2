---
layout: post
title:  "React Server Side Rendering"
---

SSR is a way of rendering web applications on the server and then sending the response and content back to the user. What this means is when a user opens a web application a request is sent to the server which returns a response together with the content i.e HTML, CSS, JavaScript, and other assets required to display the page to a user.

So unlike a Client-side rendered application, a page with the content is returned to the user. The downside of this approach is a request is always made to the server whenever a user clicks a link which may be slow as the server has to go through the process of processing the request and then return the HTML, CSS, and JavaScript files.

A solution to this approach is a hybrid of SSR and CSR which is called a Universal or Isomorphic app in some circles. In an Isomorphic app, we can eliminate the slow initial load time by Client-side rendered applications by rendering the initial HTML from the server and then letting the client take over rendering responsibilities thereby eliminating the frequent requests that have to be made to the server in SSR apps.

**Benefits of SSR**

-   Faster initial load time: because an SSR app only delivers what a user requests for when an initial request is made and also doesn’t have to wait until all the JavaScript files are loaded the Time To First Byte (which is the response time from when a user clicks a link to getting feedback) is faster.
-   Good for SEO: SSR apps are better suited for Search engines(Google, Bing, etc.) as the bots of the Search engines can crawl the entire app and index its pages, as opposed to Client-side rendered apps that load and updates just a single page.
-   Great for static sites: because the server returns a full HTML to the user, SSR can be great for building static sites.

**Cons of SSR**

-   Frequent server requests: every request made by a user has to be sent back to the server for processing which leads to performance issues.
-   Overall slower load time: because the server has to process each request the load time overall becomes slower compared to single-page applications that only need to fetch all the content needed at the initial load time. Also for large SSR applications, processing requests can take some time which may lead to a slow Time To First Byte.

## [](https://dev.to/asayerio_techblog/server-side-rendering-ssr-with-react-2oa6#getting-started)Getting Started

Now that we have an understanding of what SSR is we will be looking at building an SSR app using a popular React framework called Next.js. According to Wikipedia

> Next.js is an open-source React front-end development web framework that enables functionality such as server-side rendering and generating static websites for React based web applications.

Next.js makes creating SSR apps with React less stressful as it handles the complexities of setting everything up and also comes with some exciting features out of the box like:

-   Image Optimization
-   Internationalization
-   Next.js Analytics
-   Zero config
-   Typescript support
-   Fast refresh
-   File system routing
-   API routes
-   Built-in CSS support
-   Code-splitting and Bundling

To get started with Next.js open a terminal and run the code below  

```
npx create-next-app [app-name]

```

or  

```
yarn create next-app [app-name]

```

This code will initialize a Next.js application. Navigate to the root directory of the application and start the development server by running  

```
npm run dev

```

or if you are using yarn  

```
yarn dev

```

## [](https://dev.to/asayerio_techblog/server-side-rendering-ssr-with-react-2oa6#pages-and-routing)Pages and Routing

A page in Next.js is a React component file created in the  `pages`  directory. Next.js associates each page created to a route based on the file name. If you navigate to the pages directory you will see an  `index.js`  file that is created by default when a Next.js application is created. The  `index.js`  file is associated with  `/`  route and is by default the home page of the application.

Navigate to the  `pages`  directory and create an  `about.js`  file. Open the file and paste the code below into it and save the file  

```
import React from 'react'
const About = () => {
  return (
    <div>
      This is an About page.
    </div>
  )
}
export default About

```

Now if you navigate to  `http://localhost:3000/about`  in your browser the about page will be rendered to you. We can also create more nested routes for example  `http://localhost:3000/movies/tenet`  can be created by creating a  `tenet.js`  in the following path  `pages/movies`.

We will be creating a sample movie app to illustrate some of the major concepts of Next.js. Create a  `data.js`  file in the root directory and paste the code below  

```
export default [
  {
    slug: 'the-social-network',
    title: 'The Social Network',
    description: 'The social network is a story of how Mark Zuckerberg created Facebook and the ensuing lawsuits that followed by the twins who said he stole their idea'
  },
  {
    slug: 'malcolm-and-marie',
    title: 'Malcolm and Marie',
    description: 'A black and white romantic drama starring John David Washington and Zendaya. it tells a story of how their relationship is tested on the night of his film premiere.'
  },
  {
    slug: 'tenet',
    title: 'Tenet',
    description: 'The latest action film thriller by Christopher Nolan follows a secret agent known as the Protagonist around the world as he tries to stop a pending World war between the future and the past.'
  }
]

```

This file contains the data we will be using for our sample movie application.

Open  `index.js`  and replace the contents of the file with the code below  

```
import Link from 'next/link';
import movies from '../data';
export async function getServerSideProps() {
  return {
    props: {
      allMovies: movies,
    },
  };
}
export default function Home({ allMovies }) {
  return (
    <div>
      <main>
        <h1>Welcome to a Movie List.</h1>
        <ul>
          {allMovies.map((item) => (
            <li key={item.slug}>
              <Link href={`/movies/${item.slug}`}>
                <a>{item.title}</a>
              </Link>
            </li>
          ))}
        </ul>
      </main>
    </div>
  );
}

```

We’ve talked about creating pages and routes. To navigate between pages in Next.js we use the  `Link`  component which can be imported from  `next/link`  

```
<Link href={`/movies/${item.slug}`}>
  <a>{item.title}</a>
</Link>

```

Navigating between pages works by wrapping the  `<a>`  tag with the  `Link`  component and adding the  `href`  attribute to the  `Link`  component.

## [](https://dev.to/asayerio_techblog/server-side-rendering-ssr-with-react-2oa6#data-fetching)Data Fetching

Next.js has two ways of pre-rendering HTML they are:

-   Static Site Generation: render HTML at build time
-   Server-Side Rendering: render HTML at request time

The way data is fetched in Next.js depends on how a page is rendered. And since this article is focused on SSR we will be using a function called  `getServerSideProps`. The  `getServerSideProps`  is a method for fetching data on each request. If the  `getServerSideProps`  is exported as an  `async`  function on a page Next.js will prerender the page on each request using the data returned by  `getServerSideProps`  

```
import movies from '../data';
export async function getServerSideProps() {
  return {
    props: {
      allMovies: movies,
    },
  };
}

```

In the code snippet above we are returning the sample data we created earlier on whenever we render our page. The  `props`  object is passed to our page component so we can access the data in the component.

## [](https://dev.to/asayerio_techblog/server-side-rendering-ssr-with-react-2oa6#dynamic-routing)Dynamic Routing

After saving the file and restarting the dev server you should be presented with a page similar to the screenshot below

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--f8ArbMYn--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://asayer-content.s3.eu-central-1.amazonaws.com/4f0f58aefc434e85a37fc74bc98fc1e5.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--f8ArbMYn--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://asayer-content.s3.eu-central-1.amazonaws.com/4f0f58aefc434e85a37fc74bc98fc1e5.png)

Now if you try clicking on any of the links in the list you will be taken to a 404 page because the page doesn’t exist. We want to create the pages on the fly based on the movie data. To do that we will be creating a file called  `[id].js`  in the  `pages/movies`  directory.  

```
  cd pages
  mkdir movies
  cd movies
  touch [id].js

```

If a file name is wrapped with  `[]`  for example  `[id].js`  it tells Next.js that it is a dynamic route file. Open the  `[id].js`  file and paste the code below  

```
import { useRouter } from 'next/router';
import movies from '../../data';
const Movie = () => {
  const router = useRouter();
  const { id } = router.query;
  const getMovieById = movies.find((item) => item.slug === id);
  if (!getMovieById) {
    return <h1>Movie does not exist.</h1>;
  }
  return (
    <div>
      <h1>{getMovieById.title}</h1>
      <p>{getMovieById.description}</p>
    </div>
  );
};
export default Movie;
```

The  `useRouter`  is a react hook that gives us access to the Router object that contains information about the routes. What we are trying to do with the router object is to get the slug so we can use it to retrieve information about the movie.  
If you save and go back to the application the links should work.

## Conclusion

In this article, we learned how to render React server-side using Next.js.  Next.js offers a lot more than covered in this article so please check out the  [docs](https://nextjs.org/docs/getting-started)  to learn more about the framework.
