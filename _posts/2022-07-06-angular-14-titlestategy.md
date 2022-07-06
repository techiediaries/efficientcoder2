---
layout: post
title: "Set title in Angular 14 with TitleStrategy"
tags: [angular, angular-14]
excerpt: ""
---

Angular 14 and previous versions provide the TitleStrategy service that you can use set the title of pages in your application. A page is simply a component that is attached to a route. 

Here is an example of how to use TitleService. Simply go to the `src/app/app-routing.module.ts` file in your project and start by adding the following import

```ts
import {Title} from "@angular/platform-browser";
```

Next, define a custom title strategy class and inject the Title service as follows:

```ts
@Injectable({providedIn: 'root'})
export class MyTitleStrategy extends TitleStrategy {
  constructor(private readonly title: Title) {
    super();
  }

  override updateTitle(routerState: RouterStateSnapshot) {
    const title = this.buildTitle(routerState);
    if (title !== undefined) {
      this.title.setTitle(`My Application | ${title}`);
    }
  }
}
```

Finally, provide the title stratgey to the router module as follows:

```ts
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
  providers: [
    {provide: TitleStrategy, useClass: MyTitleStrategy},
  ]
})
export class AppRoutingModule {
}
```


