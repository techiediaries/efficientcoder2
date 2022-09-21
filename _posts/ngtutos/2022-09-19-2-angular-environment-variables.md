---
layout: post
title: "[2] Setup Angular 14 environment variables"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this tutorial, we'll look at how to set up environment variables in Angular 14 and bootstrap for UI styling." 
categories: [angular-headless-cms]
permalink: /angular-environment-variables/
nextLink: /angular-routing-switchmap/
tags : [angular , angular-14 , javascript] 
---

In this tutorial, we'll look at how to set up environment variables in Angular 14 and bootstrap for UI styling.

Before we continue, let's add our Contentful [Space ID and Access Token](https://www.contentful.com/developers/docs/references/authentication/) to our Angular 14 application's environment variables.

> Note: You can create an access token using the  [Contentful web app](https://be.contentful.com/login)  or the  [Content Management API](https://www.contentful.com/developers/docs/references/content-management-api/#/reference/api-keys/create-an-api-key).

## Adding environment variables in Angular 14

Open the `src/environments.ts` file and update it as follows:

```ts
export const environment = {
  production: false,
  contentful: {
    spaceId: 'YOUR_SPACE_ID',
    accessToken: 'YOUR_ACCESS_TOKEN'
  }
};
```  

Make sure to replace `YOUR_SPACE_ID` and `YOUR_ACCESS_TOKEN` with your actual credentials.

## Installing the necessary dependencies

Let's continue by installing some necessary dependencies such as `contentful`, `bootstrap`,  [rich-text-types](https://www.npmjs.com/package/@contentful/rich-text-types) and [rich-text-html-renderer](https://www.npmjs.com/package/@contentful/rich-text-html-renderer) using the following commands after navigating to your project's folder:

```bash
npm install contentful bootstrap 
npm install @contentful/rich-text-types @contentful/rich-text-html-renderer
```

We'll use contentful to connect with Contentful APIs and bootstrap for styling the Angular 14 UI.

## Adding Bootstrap to Angular 14

Next, let's add Bootstrap to our project. There are various ways to [add Bootstrap to Angular 14](https://www.techiediaries.com/add-angular-bootstrap/), in our case, we'll include bootstrap files via the `angular.json` file so go ahead and open the configuration file that exists at the root of the project and add the following line:

```json
"styles": [
  "./node_modules/bootstrap/dist/css/bootstrap.css",
  "src/styles.css"
 ]
```

## Add a Bootstrap header bar

Open the `src/app/app.component.html` file and update it as follows:

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <h1>Developers Jobs</h1>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
</nav>
<div>
  <router-outlet></router-outlet>
</div>
```
We simply removed the boilerplate markup except the [router outlet](https://angular.io/api/router/RouterOutlet) and added a Bootstrap header bar. 

In the next tutorial, we'll learn how to develop a service for communicating between our Angular 14 app and the headless CMS backend.