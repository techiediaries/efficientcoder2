---
layout: post
title:  "How to Build Auto-Saving Forms in Angular 14"
---

Automatically preserving the user's changes enhances the user experience by reducing loss of information. Let's look at how we can use Angular 14 forms to accomplish auto saving.

## Auto-Saving Forms in Angular 14 with RxJS Observables

Let's look at how to autosave forms in Angular 14. Because the framework makes use of RxJS, we're already in a better situation to store data in response to value changes.

When you're using [reactive forms](https://angular.io/guide/reactive-forms), any [AbstractControl](https://angular.io/api/forms/AbstractControl) (e.g. a [FormGroup](https://angular.io/api/forms/FormGroup) or single [FormControl](https://angular.io/api/forms/FormControl)) will expose an observable property `valueChanges`. Sadly, just like any other form API, this observable is still typed as `any` despite emitting the value object of your form. Recently, the [Angular team announced their work on strongly typed forms](https://github.com/angular/angular/issues/13721#issuecomment-637698836), so this might get better soon!

> `valueChanges: Observable<any>`, A multicasting observable that emits an event every time the value of the control changes, in the UI or programmatically -- Angular Documentation

You can now easily subscribe to this observable to enable autosave, convert the form value to something your server understands, and send out the data.

## Creating a new Angular 14 project

To begin, create a new Angular 14 project and generate a new component for the auto-saving form using the following commands:

```bash
ng new Angular14AutoForm --style=scss  
ng g component auto-form  
ng serve  
```

Open the `src/app/app.component.html` file and update it as follows:

```html
<app-auto-form></app-auto-form>
```

This is a screenshot of your application when visiting `http://localhost:4200` with your web browser:

![Angular 14 auto-saving form](https://miro.medium.com/max/264/1*VGJTWYjGqeOq21yP_F5PiQ.png)



## Setting up Angular 14 Material

After that, you must configure Angular Material in your project. Return to your terminal and type the following command:

```bash
ng add @angular/material
```

Next, open the `src/app/app.module.ts` file and add the following imports:

```ts
// [...]

import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';
import { MatSelectModule } from '@angular/material/select';

@NgModule({
  // [...]
  imports: [
    // [...]
    FormsModule,
    ReactiveFormsModule,
    MatFormFieldModule,
    MatInputModule,
    MatSelectModule,
  ],
  // [...]
})
export class AppModule {}
```


## Building the Angular 14 Form


Next, let's see how to buid a form with name, and description fields. Open the `src/app/auto-form/auto-form.component.ts` file and update is as follows:

```ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';

@Component({
  selector: 'app-auto-form',
  templateUrl: 'auto-form.component.html',
  styleUrls: ['auto-form.component.scss'],
})
export class AutoFormComponent implements OnInit {
  form: FormGroup;

  constructor(private formBuilder: FormBuilder) {
    this.form = this.formBuilder.group({
      name: [],
      description: [],
    });
  }

  ngOnInit(): void {}
}
```

Next, open the `src/app/auto-form/auto-form.component.html` file and update it as follows:

```html
<h1>Angular 14 Auto Saving Form</h1>
<form [formGroup]="form">
  <mat-form-field appearance="fill">
    <mat-label>Name</mat-label>
    <input matInput formControlName="name" />
  </mat-form-field>
  <br />
  <mat-form-field appearance="fill">
    <mat-label>Description</mat-label>
    <textarea matInput formControlName="description"></textarea>
  </mat-form-field>
</form>
```


We'll be using an Observable property named  `valueChanges`, which emits an event any time a value changes in one of its form controls. This Observable will be the base of the auto-saving feature in our forms.

Open the `src/app/auto-form.component.ts` file, and subscribe to `valueChanges` as follows:

```ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';

@Component({...})
export class AutoFormComponent implements OnInit, OnDestroy {

    form: FormGroup

    private unsubscribe = new Subject<void>()

    constructor(private formBuilder: FormBuilder, private service: MyService) {}

    ngOnInit() {
         this.form = this.formBuilder.group({
          name: [],
          description: [],
        });
        this.form.valueChanges.pipe(
            switchMap(formValue => service.save(formValue)),
            takeUntil(this.unsubscribe)
        ).subscribe(() => console.log('Saved'))
    }

    ngOnDestroy() {
        this.unsubscribe.next()
    }
}
```

We used the following RxJS Operators:

- [_debounceTime_](https://www.learnrxjs.io/learn-rxjs/operators/filtering/debouncetime): This operator does not allow an event to be emitted from the Observable until a set amount of time has passed until the last event was emitted. In other words,  `valueChanges` will not emit an event until theform hasn’t been modified for 1.5 consecutive seconds. This prevents constant database updates that would occur if an event was fired after every single modification of the form. 1.5 seconds is based on  [Cloud Firestore’s limitation of one write to a document per second.](https://firebase.google.com/docs/firestore/quotas)

- [_switchMap_](https://www.learnrxjs.io/learn-rxjs/operators/transformation/switchmap): This operator switches the Observable to a new Observable. For our purposes, this new Observable will be left to the imagination. However, no matter what backend technology you are using alongside Angular 14, you will almost always have one in a service that updates the user’s data. Anyways, it will be that Observable we want to subscribe to. This operator seamlessly cancels the previous Observable and subscribes to the new one.

In our previous example, every change to the form will invoke a call to save the form. However, thanks to `switchMap`, only the most recent save call will be active at one point in time. The next value changes will cancel the previous save calls if they haven't completed yet.

We could replace switchMap with mergeMap and thus have all created autosave requests run simultaneously. Similarly, we might use concatMap to execute the save calls one after another. Another option might be exhaustMap which would ignore value changes until the current save call is done.

Since we're using with a long-lived observable (meaning it doesn't just emit one time but indefinitely), we should unsubscribe from the stream once the component encapsulating our form is destroyed. We are doing this with the `takeUntil` operator.


## UX Improvements

Next, let's add a feature that allows users to see when when the form is successfully saved. We'll us a variable called `formStatus` for that purpose.

Open the `src/app/auto-form/auto-form.component.ts` file and update it as follows:

```ts
// [...]

enum FormStatus {
  Saving = 'Saving..',
  Saved = 'Saved!',
  Idle = '',
}

function sleep(ms: number): Promise<any> {
  return new Promise((res) => setTimeout(res, ms));
}

// [...]

formStatus: FormStatus.Saving | FormStatus.Saved | FormStatus.Idle = FormStatus.Idle;
// [...]

  ngOnInit(): void {
    this.form.valueChanges
      .pipe(
        tap(() => {
          this.formStatus = FormStatus.Saving;
        }),
        // [...]
      )
      .subscribe(async (value) => {
        console.log(value);
        this.formStatus = FormStatus.Saved;
        await sleep(2000);
        if (this.formStatus === FormStatus.Saved) {
          this.formStatus = FormStatus.Idle;
        }
      });
  }
...
```

We simply add the RxJS `tap` operator, to assing the saving status every time a form control is being changed. This will set the save status to “Saving”. Next, after subscriping, once the form'date is saved, the save status will be set to “Saved”, and after two additional seconds, if the status is still “Saved”, it will be set to “Idle”, which is an empty string.

Next, open the `src/app/auto-form/auto-form.component.html` file and simply add the variable as follows:

{% raw %}
```html
{{ formStatus }}
```
{% endraw %}

That's it, we have learned how to add the auto-saving functionality to our Angular 14 form using RxJS Observales and operators.

