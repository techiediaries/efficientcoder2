---
layout: post
title: "[3] Angular 14 Service for communicating with Contentful CMS"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this tutorial, let's now create the service that will encapsulate the code for communicating with Contentful." 
categories: [angular-headless-cms]
permalink: /angular-service-communicate-cms/
nextLink: /angular-routing-switchmap/
tags : [nodejs , javascript] 
---

In this tutorial, let's now create the service that will encapsulate the code for communicating with Contentful. Go back to your command-line interface and run the following command to generate the service:

```bash
ng g s content
``` 

Create a `content-types.ts` file in the `src/app/` folder and add the following code:

```ts
import * as CFRichTextTypes from "@contentful/rich-text-types";
import * as Contentful from "contentful";

export interface TypeJobListingFields {
    role?: Contentful.EntryFields.Symbol;
    description?: Contentful.EntryFields.Text;
    howToApply?: CFRichTextTypes.Block | CFRichTextTypes.Inline;
    organization?: Contentful.EntryFields.Symbol;
    date?: Contentful.EntryFields.Date;
    skills?: Contentful.EntryFields.Symbol[];
}

export type TypeJobListing = Contentful.Entry<TypeJobListingFields>;
```

This will be used to type the entries corresponding to our content model. We can either write the types manually or use automated tools for a more scalable approach:

-   [cf-content-types-generator](https://github.com/contentful-userland/cf-content-types-generator)
-   [contentful-typescript-codegen](https://github.com/intercom/contentful-typescript-codegen)
-   [contentful-ts-type-generator](https://github.com/arimkevi/contentful-ts-type-generator)
-   [contentful-ts-generator](https://github.com/watermarkchurch/contentful-ts-generator)
-   [TS Content Types Generator App](https://github.com/marcolink/cf-content-types-generator-app) 

Open the `src/app/content.service.ts` file and start by adding the following imports:

```ts
import { Injectable } from '@angular/core';
import { createClient } from 'contentful';
import { from } from 'rxjs';
import { environment } from '../environments/environment';
import { TypeJobListingFields } from './content-types';
```

Next, add the client property and call the `createClient()` function to create a client as follows:

```ts
@Injectable({
  providedIn: 'root'
})
export class ContentService {

  client = createClient({
    space: environment.contentful.spaceId,
    accessToken: environment.contentful.accessToken
  });
  constructor() {}
}
```

Then, add the following method to fetch content from Contentful:

```ts
getJobListings(query?: object){
  return from(
  this.client.getEntries<TypeJobListingFields>(Object.assign({
   content_type: 'jobListing'
  }, query))
  );
}
```

We call the [getEntries](https://contentful.github.io/contentful.js/contentful/latest/ContentfulClientAPI.html#.getEntries) method of `contentful` to retreive the entries corresponding to our model content that we pass as the first argument of the method. We use [Object.assign](https://30daysofjavascript.com/object-assign/) to create an object containing the content type and any other queries that we pass to Contentful. 

The method also accepts a [generic type](https://www.typescriptlang.org/docs/handbook/2/generics.html) that defines the type of the content that will be returned which is in our case  `TypeJobListingFields`.

> _Note If you didn't save your content type id somewhere, you can get it as follows: contentful->content model -> click your content model name -> right sidebar look for your content type id._

Also, add the following method to fetch a job listing by ID:

```ts
getJobListingById(jobListingId: string, query?: any){
 return from(
   this.client.getEntry<TypeJobListingFields>(jobListingId, query)
   );
}
```

### Add routing and display the content

After implementing the service, we need to create a component to display the job listings fetched from the backend; and add routing.
Go back to your command-line interface and run the following commands:

```bash
ng g c jobListings 
ng g c jobListing
```

This will generate the components' files and add the components to the root application module.

Next, open the `src/app/app-routing.module.ts` file and start by importing the components as follows:

```ts
import { JobListingsComponent } from './job-listings/job-listings.component';
import { JobListingComponent } from './job-listing/job-listing.component';
```

Then, add the following routes:

```ts
const routes: Routes = [
  { path: '', pathMatch: 'full', redirectTo: 'listings' },
  { path: 'listings', component: JobListingsComponent },
  { path: 'listing/:id', component: JobListingComponent }
];
```
 Here we define three routes -- the first route simply redirects the user to the second route which links to the JobListingsComponent.
 
You can read more about routing in Angular 14 from this [article](https://www.webtutpro.com/angular-12-routing-by-example-f0cfbdc7b0a8).

Open the `src/app/job-listings/job-listings.component.ts` file and import the service and the other symbols as follows:

```ts
import { Component, OnInit } from '@angular/core';
import { ContentService } from '../content.service';
import { TypeJobListingFields } from '../content-types';
import { Entry } from 'contentful';
```
Next, inside the component's class define the following array to hold the fetched job listings:

```ts
jobListings: Entry<TypeJobListingFields>[] | undefined;
```

Next, inject the service via the component's class as follows:

```ts
constructor(public contentService: ContentService) { }
```

[Injecting the service](https://angular.io/guide/dependency-injection#injecting-services) makes it available to the component.

Then, define the following method for fetching the content:

```ts
getJobListings(){
  this.contentService.getJobListings()
  .subscribe({
     next: (entryCollection) => {
       this.jobListings = entryCollection.items;
       console.log(this.jobListings);
     }
 });
}
```

Then, call the previous method inside the `ngOnInit()` life-cycle method of the component:

```ts
ngOnInit(): void {
  this.getJobListings();
}
```

Life-cycle methods are methods that hook into key events of the component. You can find more information about them from this [article](https://angular.io/guide/lifecycle-hooks).