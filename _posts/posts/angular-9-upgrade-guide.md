# Angular 10 Update Guide

In this guide, we'll learn how to upgrade our project to the latest Angular 10 version and update the dependencies.

## Prerequisites

Before we can start upgrading your project to Angular 10, you need to make sure you do the following changes in your code when appropriate:

- Swap `NgForm`  selector.
- Change `@ContentChild`  and  `@ContentChildren`  hosts.
- Do not assign values to template-only variables.
- TypeScript Compiler Updates (optional).
- Remove the `Renderer`  directive and replace it with `Renderer2`.

Let's break each of these down.

## Update `ngForm` to `ng-form`

If you are using Angular forms in your templates with the `<ngForm>` directive, you need to update any instance of `<ngForm>`  with `<ng-form>` instead. 


For example, if you have declared a form like below:

```html
<ngForm #exampleForm="ngForm">
    <input [(ngModel)]="userName" name="userName" />
</ngForm>
```

You'll simply need to update it as follows:

```html
<ng-form #exampleForm="ngForm">
    <input [(ngModel)]="userName" name="userName" />
</ng-form>
```

## Update `@ContentChild`  and  `@ContentChildren`  Hosts

The  `@ContentChild`  and  `@ContentChildren`  decorator that are used to query the DOM will no longer be able to match their directive's own host node.

Before:

```typescript
@Directive({
  selector: '[swrActions]'
})
export class ActionsDirective implements AfterContentInit {
  // [TODO]: Angular v9 ContentChild will not return host element!!
  @ContentChild(ActionsDirective, { static: true, read: ElementRef })
  selfElementRef: ElementRef;

  constructor(private readonly renderer: Renderer2) {}

  ngAfterContentInit() {
    const el = this.selfElementRef.nativeElement as HTMLElement;
    if (!el) {
      return;
    }
    this.renderer.setStyle(el, 'display', 'flex');
    this.renderer.setStyle(el, 'flexDirection', 'row');
    this.renderer.setStyle(el, 'justifyContent', 'flex-end');
  }
}
```

Above, we are using the  `@ContentChild`  content query to access the host  `ElementRef`  for the directive. FWIW, this is not a best practice, which is why this is being deprecated.

Ideally, we are accessing the  `ElementRef`  via a dependency that is injected into our directive:

```typescript
@Directive({
  selector: '[swrActions]'
})
export class ActionsDirective implements AfterContentInit {
  constructor(
    private readonly elementRef: ElementRef,
    private readonly renderer: Renderer2
  ) {}

  ngAfterContentInit() {
    const el = this.elementRef.nativeElement as HTMLElement;
    if (!el) {
      return;
    }
    this.renderer.setStyle(el, 'display', 'flex');
    this.renderer.setStyle(el, 'flexDirection', 'row');
    this.renderer.setStyle(el, 'justifyContent', 'flex-end');
  }
}
```

The goal here is to avoid using the  `@ContentChild()`  and  `@ContentChildren()`  queries for accessing a host element. The migration is to rely on injecting the host's  `ElementRef`.

### Do Not Assign Values to Template-only Variables

This migration is necessary to avoid mutation and assigning unknown properties to the object.

One of the  _BIG_  updates for Angular version 9 with the new Ivy renderer is template type checking, and this is directly related to this new functionality. Before Angular version 9 and the Ivy renderer, template-only variables where basically an  `any`  type. This means, you could mutate the object, and its properties, as you saw fit. In general, this was an antipattern.

Going forward with Angular version 9 and the Ivy renderer, template-only variables will be strongly typed based on your TypeScript compiler options for strictness of template type checking. Therefore, it is imperative that we  _do not_  mutate template-only variables.

Let's look at an example that  _does_  mutate template-only variables:

```html
<button
  #translateBtn
  (click)="translateBtn.translate = !translateBtn.translate"
>
  Translate
</button>

<mat-accordion>
  <mat-expansion-panel *ngFor="let person of people">
    <mat-expansion-panel-header>
      <mat-panel-title>
        {{ person.fields.name | wookiee: translateBtn.translate }}
      </mat-panel-title>
    </mat-expansion-panel-header>
  </mat-expansion-panel>
</mat-accordion>
```

The  `translateBtn`  template-only variable with Angular version 8 is of type  `any`. With Angular version 9, the variable is strongly typed as an  `HTMLButtonElement`. And, the  `HTMLButtonElement`  interface  _does not_  have a  `translate`  property.

Our goal is to remove mutations of template only variables, and to rely on component properties and methods:

```html
<button (click)="ontTranslate()">
  Translate
</button>

<mat-accordion>
  <mat-expansion-panel *ngFor="let person of people">
    <mat-expansion-panel-header>
      <mat-panel-title>
        {{ person.fields.name | wookiee: translateToWookiee }}
      </mat-panel-title>
    </mat-expansion-panel-header>
  </mat-expansion-panel>
</mat-accordion>
```

We have removed the template-only variables. Note that the template variable notation  `#translateBtn`  has been removed.

Now, we simply implement the property and methods in our component's class:

```typescript
export class PeopleListComponent {
  /** True if the content should be translated for Chewbaka */
  translateToWookiee = false;

  onTranslate(): void {
    this.translateToWookiee = !this.translateToWookiee;
  }
}
```

### TypeScript Compiler Updates (optional)

Before updating to Angular version 9, you can opt-in to compiler updates that will make your update more seamless. Update your  _tsconfig.json_  file as follows:

```json
{
  "compilerOptions": {
    // omitted for brevity
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noFallthroughCasesInSwitch": true,
    "strictNullChecks": true
    // omitted for brevity
  }
}
```

The key compiler flags to include are:

-   `noImplicitAny`  prevent the compiler from implying that the type of an argument, variable, or assignment is of type  `any`, which is the default when the compiler either cannot infer a type or the code not does explicitly declare the type.
-   `noImplicitReturns`  prevents you from falling through a function to the end unnoticed, meaning, without your knowledge the function  _can_  return  `any`  implicitly. This ensure that you are correctly returning the proper type, no matter the branch of the code you are within where you do return a value.
-   `noImplicitThis`  ensure that we don't  _assume_  the value of  `this`. As JavaScript developers we know that the  `this`  value is dependent upon the execution context of a function. This compiler flag protects us from assuming, or allowing the compiler to imply the context of  `this`.
-   `noFallthroughCasesInSwitch`  ensures that each  `case`  statement is guarded by an appropriate  `break`  statement to prevent the case from falling through to the  `case`  statement.
-   `strictNullChecks`  ensures that we are not allowing both  `undefined`  and  `null`  values from being globally available or assignable unless explicitly declared in our TypeScript. This flag is  _highly_  recommended as it prevents bugs that can result from  `nullish`  coalescing and other truthy/falsey coercions.

### `Renderer`  Deprecation

This change has been long awaited, so this should be no surprise. The deprecated  `Renderer`  is finally being removed from the Angular codebase. This means that if your Angular project relies, via dependency injection, on the deprecated  `Renderer`  class, that you need to migrate to  `Renderer2`.

Thankfully, this migration is eased by the  `ng update`  command we are going to execute shortly. So, if you're lazy (like me), then you can skip this for now and let the Angular update migrate this for you.

Most methods are easily updated from  `Renderer`  to  `Renderer2`, but those that are not will be fixed for you.

> Be sure to review the git diff after updating for any  `Renderer`  to  `Renderer2`  migrations.

## Update to the Latest Angular 9 Patch

Before updating your project to the latest Angular 10 version, make sure to start by updating it to the latest stable release of Angular 9.

Head back to your terminal and run the following command:

```bash
$ ng update @angular/core@9 @angular/cli@9
```

Make sure you add the version number for Angular 9 to install the latest patch of this version otherwise, Angular 10 will be installed.

## Update to Angular version 9

We made it! We're ready to update our Angular project to Angular version 9 via  `ng update`:

```bash
ng update @angular/cli @angular/core
```

While the release is not final, and currently in release candidate stage, you'll need to append the  `--next`  flag:

```bash
ng update @angular/cli @angular/core --next
```

## Localization (i18n)

If you are using Angular's localization (i18n) framework, then you also need to use the  `ng add`  command to install a new  `@angular/localize`  package:

```bash
ng add @angular/localize
```

When you examine the diff of the  **angular.json**  file after updating, you'll also notice a new  `i18n`  configuration section:

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "angular-v9": {
      "i18n": {
        "locales": {
          "de": {
            "translation": "src/locale/messages.de.xlf",
            "baseHref": ""
          }
        }
      }
    }
  }
}
```

There are a few updates to your  **angular.json**  configuration you should consider.

First, specify the  `sourceLocal`  property. The default value for this is  `en-US`, so you'll want to specify the source locale for your application based on the locale of the source code:

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "angular-v9": {
      "i18n": {
        "sourceLocale": "en-US",
        "locales": {
          "de": {
            "translation": "src/locale/messages.de.xlf",
            "baseHref": ""
          }
        }
      }
    }
  }
}
```

Second, consider updating the  `baseHref`  configuration for each locale based on your needs. In most instances, we'll serve each locale of our application using either unique subdomains or unique paths.

In my use case, I wanted each locale to be served on unique paths:

-   US English: /en-US/
-   Deutsch: /de/

As such, I modified the  `baseHref`  for the  `de`  locale to:

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "angular-v9": {
      "i18n": {
        "sourceLocale": "en-US",
        "locales": {
          "de": {
            "translation": "src/locale/messages.de.xlf",
            "baseHref": "/de/"
          }
        }
      }
    }
  }
}
```

Next, my Angular version 8 project used mulitple build configurations for each locale. Based on my experience so far, you can remove these as they are no longer necessary:

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "angular-v9": {
      ...
      "architect": {
        "build": {
          ...
          "configurations": {
            "de": {
              ...
            },
            "production-de": {
              ...
            }
          }
        }
      }
    }
  }
}
```

I removed both the  `de`  and  `production-de`  configurations.

Finally, update your production build command in the project's  **package.json**  file to include the new  `--localize`  flag to build all localizations:

```json
{
  "name": "angular-v9",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    ...
    "serve:dist": "http-server -p 8080 -c-1 dist/angular-v9",
    "build": "ng build --prod --localize",
  },
```

Note:

-   First, I have a new  `serve:dist`  command where I use the http-server Node module to serve a localized version of my production build.
-   Second, I updated the  `build`  command to include the new  `--localize`  flag to build all localizations.

Let me also mention that you can create additional configuration for building either a specific locale or a group of locales.

## Post Update Checklist

After we have successfully updated to Angular version 9, there are two minor things we need to do:

1.  Migration from the deprecated  `TestBed.get()`  method to the new  `TestBed.inject<T>()`  method.
2.  Remove unnecessary  `entryComponents`  properties in our  `@NgModule()`  decorators object.

### Migrate to  `TestBed.inject()`

Post the Angular version 9 update, our first task is to migration from  `TestBed.get()`  to  `TestBed.inject<T>()`.

Before:

```typescript
describe('FilmService', () => {
  let service: FilmService;

  beforeEach(() =>
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule]
    })
  );

  it('should be created', () => {
    service = TestBed.get(FilmService);
    expect(service).toBeTruthy();
  });
});
```

After:

```typescript
describe('FilmService', () => {
  let service: FilmService;

  beforeEach(() =>
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule]
    })
  );

  it('should be created', () => {
    service = TestBed.inject<FilmService>(FilmService);
    expect(service).toBeTruthy();
  });
});
```

Note:

-   We change  `TestBed.get()`  to  `TestBed.inject<T>()`
-   We specify the generic of the type that is returned from the  `inject()`  method.

### Remove  `entryComponents`

With Angular version 9 and the new Ivy renderer, we no long need to explicitly instruct the compiler of components that are outside the component dependency graph. A good example of components that need to be declared in the  `entryComponents`  array are dialogs.

This update is simple, and who doesn't love deleting code. Just remove the  `entryComponents`  array from all  `@NgModule()`  decorators in your application.

Before:

```typescript
@NgModule({
  declarations: [
    // omitted for brevity
    PersonFilmsDialogComponent,
    PersonHomePlanetDialogComponent
  ],
  entryComponents: [PersonFilmsDialogComponent, PersonHomePlanetDialogComponent]
})
export class PeopleModule {}
```

And after we have removed the unnecessary  `entryComponents`  array:

```typescript
@NgModule({
  declarations: [
    // omitted for brevity
    PersonFilmsDialogComponent,
    PersonHomePlanetDialogComponent
  ]
})
export class PeopleModule {}
```

## Success!

We have successfully updated to Angular version 9 and the new Ivy rendering engine.