---
layout: post
title: "[4] Angular 14 Routing and RxJS switchMap"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this tutorial, we'll continue building our app with Angular 14 and Contentful. We'll look at how to access route parameters using `ParamMap` and since our previous service method returns an observable, we'll see how to flatten the observable with the `switchMap` operator and subscribe to the resulting observable to get the fetched entry from the headless CMS and assign it to the jobListing property that we'll define in our Angular component below." 
categories: [angular-headless-cms]
permalink: /angular-routing-switchmap/
nextLink: /angular-pipe/
tags: [angular , angular-14 , javascript] 
---

In this tutorial, we'll continue building our app with Angular 14 and Contentful. We'll look at how to access route parameters using `ParamMap` and since our previous service method returns an observable, we'll see how to flatten the observable with the `switchMap` operator and subscribe to the resulting observable to get the fetched entry from the headless CMS and assign it to the `jobListing` property that we'll define in our Angular component below.


In the same way open the `src/app/job-listing/job-listing.component.ts` file and add the following imports:

```ts
import { ActivatedRoute, ParamMap } from '@angular/router';
import { switchMap } from 'rxjs/operators';
import { ContentService } from '../content.service';
import { TypeJobListingFields } from '../content-types';
```

Next, define the following property:

```ts
jobListing: TypeJobListingFields | null = null;
```

Next, inject the following services:

```ts
constructor(public contentService: ContentService, public route: ActivatedRoute) { }
```

The [ActivatedRoute](https://angular.io/api/router/ActivatedRoute) class provides information about active route in the outlet.

Next, update the `ngOnInit()` hook of the component as follows:

```ts
ngOnInit(): void {
  this.route.paramMap.pipe(
    switchMap((params: ParamMap) =>
      this.contentService.getJobListingById(params.get('id') as string)
    )).subscribe({
     next: (entry) => {
       this.jobListing = entry.fields;
    }});
}
```

When the component is initialized, we get the job's listing ID from the route using [ParamMap with switchMap](https://webtips101.com/angular-routing-switchmap/). Since our service method returns an observable, we flatten the observable with the `switchMap` operator and we subscribe to the resulting observable to get the fetched entry and assign it to the `jobListing` property. 

The [`switchMap` operator](https://rxjs.dev/api/operators/switchMap) also cancels previous ongoing requests. If the user navigates to the route with a different `id` while the ContentService is still fetching the content, `switchMap` removes the previous request and returns the job listing for the current `id`.

After that, we need to render the content in the component's template. Open the `src/app/job-listings/job-listings.component.html` file and update it as follows:

{% raw %}
```html
<div *ngIf="jobListing" class="card" style="margin: 10px; width: 93%;">
  <div class="card-body">
    <h5 class="card-title">
      {{ jobListing.role }}
    </h5>
    <h6 class="card-subtitle">
      {{ jobListing.organization }} - {{ jobListing.date | date }}
    </h6>
    <hr>
    <div class="card-text">
      <h5>
      Description
      </h5>
     <p>
      {{ jobListing.description }}
     </p>
     <hr>
      <h5>
      How to apply?
      </h5>
      <div [innerHtml]="jobListing.howToApply"></div>
     <hr>
    </div>
    <p>
    Skills: {{ jobListing.skills }}
    </p>
  </div>
</div>
```
{% endraw %}

You should notice that under the **How to apply** section, we are displaying the content of the `howToApply` field but we are getting `[object Object]` displayed. That's because this a Rich Text field which is a JSON format for handling complex content structures in a strongly typed manner in Contentful.
Contentful provides many tools to help you work with the [Rich Text](https://www.contentful.com/developers/docs/concepts/rich-text/) feature and we've previously installed the `@contentful/rich-text-html-renderer` package which makes it easy to display rich text fields via a `documentToHtmlString()` function.