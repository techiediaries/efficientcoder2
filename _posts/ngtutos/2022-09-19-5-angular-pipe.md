---
layout: post
title: "[5] Transform data with Angular pipes"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "In this tutorial, we'll see how to use a pipe in Angular 14 to transform and display data in our components' templates. " 
categories: [angular-headless-cms]
permalink: /angular-pipe/
tags : [angular , angular-14 , javascript] 
---

In this tutorial, we'll see how to use a pipe in Angular 14 to transform and display data in our components' templates. 

Since under the **How to apply** part, we are displaying the content of the `applyMethod` field but we are getting `[object Object]` displayed. That's because this a Rich Text field which is a JSON format for handling complex content structures in a strongly typed manner in Contentful.

Contentful provides many tools to help you work with the [Rich Text](https://www.contentful.com/developers/docs/concepts/rich-text/) feature and we've previously installed the `@contentful/rich-text-html-renderer` package which makes it easy to display rich text fields via a `documentToHtmlString()` function.

Since, we are using Angular 14, we need to use the  function with a [custom pipe](https://angular.io/guide/pipes) that we can apply to the field in the component's template to display it correctly.
Go back to your command-line interface and run the following command to generate a pipe:

```bash
ng g pipe toHtml
```

Open the `src/app/to-html.pipe.ts` file and update it as follows:

```ts
import { Pipe, PipeTransform } from '@angular/core';
import { documentToHtmlString } from '@contentful/rich-text-html-renderer';
import { Document } from '@contentful/rich-text-types';

@Pipe({
  name: 'toHtml'
})
export class ToHtmlPipe implements PipeTransform {

  transform(value: unknown, ...args: unknown[]): unknown {
    return documentToHtmlString(value as Document);
  }
}
```

You can then go back to the file and display the rich text field using the following code:

```html
<div [innerHtml]="jobListing.fields.howToApply | toHtml"></div>
```
Since the result of applying our custom `toHtml` pipe to the `howToApply` field is HTML content, we need to [bind to the innerHtml](https://www.techiediaries.com/angular-innerhtml-binding/) attribute to render it inside the `<div>`. 

## Conclusion

We learned how to use Contentful as our Angular 14 application's backend for managing content.

Software like Wordpress sparked the rise of content management systems. Even though Wordpress is the most popular content management system in the world, Contentful allows you to accomplish more and separate your front-end or view layer from managing content. For more information, you can read [Is Contentful a WordPress alternative?](https://www.contentful.com/blog/2020/03/26/contentful-wordpress-alternative/) or [The Problems with Headless WordPress](https://www.webtutpro.com/the-problems-with-headless-wordpress-196630f5f8f6).

You'll save time and money because you simply need to write and update content once for all platforms that are integrated with your CMS.