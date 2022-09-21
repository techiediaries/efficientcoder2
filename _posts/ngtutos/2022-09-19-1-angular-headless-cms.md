---
layout: post
title: "[1] Build Angular 14 apps with Contentful headless CMS"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this tutorial, you'll learn how to build a full-stack app using Contentful and Angular 14." 
categories: [angular-headless-cms]
permalink: /angular-headless-cms/
nextLink: /angular-environment-variables/
tags : [angular , angular-14, javascript] 
---

In this tutorial, you'll learn how to build a full-stack app using Contentful and Angular 14.

Contentful is a cloud-based CMS that allows users to centrally manage contents that can be accessible from several platforms.

Angular 14 and Contentful are both widely used for their respective purposes, which include the development of client-side applications and the provision of services associated with the creation and management of content.


The ability to build and share code consistently while also making cross-platform web, mobile web, native mobile, and desktop apps is a major selling point for [Angular 14](https://angular.io/).


Angular 14 provides a number of advantages in terms of performance and productivity by taking advantage of the maximum performance of the current Web Platform and by adopting new approaches and APIs such as [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web Workers API/Using web workers) and [Angular Universal](https://angular.io/guide/universal) for server-side rendering.


It comes with its own [template syntax](https://angular.io/guide/template-syntax) that extends HTML, and can be further developed with custom components, and it also contains high-quality developer tools that facilitate the process of developing functionalities in efficient ways. Popular IDE and text editor also offers Angular 14-specific feedback and guidance. If you want to make strong apps, but are intimidated by the coding process, here is the solution for you.


Because of its scalability and productivity-boosting features, Angular 14 is the technology behind the front end of Google's widely used applications.


Contentful is a headless CMS that provides a project-specific framework for controlling the data on a website.


The backend and content architecture can be used with any framework, such as Angular 14, React, Vue, etc., because it is "headless," or decoupled from the front end.


It's a method of managing your website's or mobile app's content from behind the curtains. It also includes various tools for connecting the frontend (a website or application) with the backend, sparing you the expense and time of developing your custom backend, which involves the understanding of a backend language/platform like Python or Node.js as well as database administration.

## Create contents with Contentful

Let's begin with the first part of this tutorial: in the Contentful UI, we'll establish a [space](https://www.contentful.com/help/spaces-and-organizations/) with a content model and add content that we'll retrieve from the frontend later. As an example, we'll create a job postings website for developers.

We won't present a full step-by-step guide for this part, but you can learn how to carry out the procedure on [this page](https://www.contentful.com/help/contentful-101/).

We should start by developing our own content structure. The structure works identically to a schema in that it allows editors to generate recurring chunks of content. In a nutshell, you must do the following:

-   Sign up for a free Contentful account
-   Create a content type/model called `JobListing`
-   Populate the space with content based on the `JobListing` content type
    
Ou Job model has the following fields and types:

-   Name: Short Text
-   About: Long Text
-   ApplyMethod: Rich Text
-   Company: Short Text
-   Skills: Short Text & List 
-   Date: Date & time

    
Following the creation and publication of your job listings. We'll now walk you through the process of retrieving them via API and displaying them with Angular 14.

> Note: Before you can communicate with Contentful, you'll need an [API key, consisting of the Space ID, the Access Token, and the Entry ID](https://www.contentful.com/developers/docs/references/authentication/) that you can get from your Contentful space.

## Create Angular 14 Project

Now that we’ve created our content type and populated some data, let’s proceed to create the Angular 14 project that will fetch and display the job listings from Contentful.

In this section, we’ll see how to:

-   Create an Angular 14 project and modify the page to display the retrieved content in an appropriate way.
-   Create an Angular 14 service that can fetch the data.
-   Include the routes required to navigate the app.
-   Connect the service to the page and display the fetched content.
    
Before you start, you will need to have some prerequisites regarding your development environment.

Since Angular 14 requires the use of [Angular 14 CLI](https://efficientcoder.net/install-angular-cli/) to scaffold the initial projects’ files and any necessary artifacts during the development of the project, you need to have recent versions of Node.js and npm installed on your development machine.

There are various ways that you use to install Node.js in your machine:

-   Obtain an installer that is compatible with your operating system from the [official website](https://nodejs.org/).
-   Make use of the official package manager of your system.
-   Use a Node version manager such as [NVM](https://github.com/nvm-sh/nvm) which will allow you to manage [multiple versions of Node](https://www.shabang.dev/multiple-versions-node-nvm/) on your development machine. We recommend using NVM.
 
If you have Node installed on your machine; simply go to a command-line interface, Command Prompt or PowerShell (Windows) or bash terminal (Linux and macOS) and let’s get started by installing Angular 14 CLI.

Install the most recent version of the Angular 14 CLI by running the following command:

```bash
npm install -g @angular/cli  
```
> Note: You may need to add sudo (for admin access) in macOS and Linux or use a command prompt with admin access in Windows to install the Angular 14 CLI globally on your machine.

After the installation, you’ll be able to use the `ng` tool from your terminal. 

At the time of writing this article, Angular CLI **v14** is installed.

> If you want to use the same version installed in this tutorial at any time later, you simpy need to specify the version when installing the CLI .i.e   `npm install -g @angular/cli@13.3.7`. 

Let’s create our Angular 14 project. In the command-line interface run the following command:

```bash
ng new angularcontentfuldemo --routing --style=css
```

After pressing **Enter** in your keyboard, Angular 14 CLI will create your initial project’s files and install the packages and log the process on the terminal which should have the following printed after project’s creation:

```bash
✔ Packages installed successfully.
    Successfully initialized git.
```

Next, navigate inside the root folder of your project:

```bash
cd angularcontentfuldemo  
```

You can then serve your Angular 14 application using the `ng serve` command as follows:

```bash
ng serve  
```

Your app will be available from `http://localhost:4200/`. Keep your current command-line interface open for executing the development server and open a new one for running the commands in the next article.

In the next article; we'll continue developing our Angular 14 application and connect it with the headless CMS. 




