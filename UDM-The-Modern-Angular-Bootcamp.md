#### Section 2 Understanding Angular Template Syntax ####
* ng new appName
* ng serve / npm start

* Event Binding Syntax

![image](https://user-images.githubusercontent.com/5309726/226254384-caf656c4-fac5-40fa-a4db-b4a5ea7e0e23.png)

```
- app.component.html
<button (click)="onButtonClick()" />


- app.component.ts
export class AppComponent {
  onButtonClick() {
    console.log('Button was clicked');
  }
}
```

* Property Binding Syntax

![image](https://user-images.githubusercontent.com/5309726/226254287-13e080ce-e1fb-47bb-b1eb-23940a8d3c99.png)

```
- app.component.html
<input [value]="password" />


- app.component.ts
export class AppComponent {
  password = '';
}
```

* Interpolation Syntax

```
- app.component.html
{{ passoword }}}

- app.component.ts
export class AppComponent {
  password = '';
}
```

* CSS Import Statements

```
@import ''
```

* Structural Directive

```
<div *ngIf="">
  <label>Your Password</label>
</div>
```

---

#### Section 3 Building Components ####
* ng generate component helloWorld

![image](https://user-images.githubusercontent.com/5309726/226255415-c1f5de53-c6db-42dd-89bf-3056dad3d6af.png)
![image](https://user-images.githubusercontent.com/5309726/226255562-645382e9-17b9-44e1-9af8-477e99c59ecf.png)

* Tying Data to a Component

![image](https://user-images.githubusercontent.com/5309726/226255799-7b121f0f-c5e8-4259-800c-a5d0f4d10c66.png)

![image](https://user-images.githubusercontent.com/5309726/226256637-c0991013-008a-43d7-8719-1f5ebf9d599f.png)

```
- app.component.html
<app-card [title]="posts[0].title" [imageUrl]="posts[0].imageUrl" />

- app.component.ts
export class AppComponent {
  posts = [
  {
    title: 'Neat Tree',
    imageUrl: ''
  }]
}

- card.component.ts
export class CardComponent implements OnInit {
  @Input() title = '';
  @Input() imageUrl = '';
}
```

*ngFor

```
<app-card *ngFor="let post of posts" [title]="post.title" [imageUrl]="post.imageUrl" />

//alternative
<app-card *ngFor="let post of getPosts()" [title]="post.title" [imageUrl]="post.imageUrl" />
```

* Host Element Selectors
  * Used to apply styles to the host element of a component

```
:host {
  display: flex;
}
```

---

#### Section 5 Mastering Pipes ####
https://angular.io/guide/pipes-overview

![image](https://user-images.githubusercontent.com/5309726/226287662-94358577-477a-479a-8ae9-d35cab0287b8.png)

* Creating Custom Pipes
  * ng generate pipe pipeName

```
- pipe-name.pipe.ts
@Pipe({
  name: 'pipeName'
})

export class PipeName implements PipeTransform {
  transform(value: any, targetUnits: string): any {
    if(!value) {
      return '';
    }
    
    switch(targetUnits) {
      case 'km':
        return value * 1.60934;
      default:
        return value;
    }
}
```

* Two Neat Things with Pipes

```
- app.component.html
<div *ngIf="miles | convert: 'km' > 10">
  {{ miles | convert: 'cm' | number: '1.0' }}
</div>
```

---

#### Section 6 Directives in Angular ####

---

#### Section 7 The Module System ####
* ng g m Elements --routing
* ng generate component elements/ElementsHome

![image](https://user-images.githubusercontent.com/5309726/226253429-1a2cc693-6ff4-419d-af43-a461ee523a39.png)

![image](https://user-images.githubusercontent.com/5309726/226253605-f2482e2c-3f54-4a12-bd2d-ebb221193b94.png)

![image](https://user-images.githubusercontent.com/5309726/226253136-f53106ae-346d-46ea-b293-31e91995c100.png)

---

#### Section 8 Routing and Navigation Between Pages ####
* Adding Basic Routing

```
- app.component.html
<router-outlet />

- elements-routing.module.ts
const routes: Routes = [{ path: 'elements', component: ElementsHomeComponent }]
```

![image](https://user-images.githubusercontent.com/5309726/226263770-01a67499-f1bd-4c08-9285-be1758b3d6c3.png)

* Navigating with RouterLink

```
- app.component.html
<a routerLink="/collections">Collections</a>
```

* Styling an Active Link

```
<a class="item" routerLink="/elements" routerLinkActive="active">Elements</a>
```

* Reordering Routing Rules

```
- app-routing.module.ts
const routes = Routes = [
  { path: '', component: HomeComponent },
  { path: '**', component: NotFoundComponent }
]

- app.module.ts
@NgModule({
  imports: [
    BrowserModule,
    ElementsModule,
    CollectionsModule,
    AppRoutingModule
  ]
})
```

---

#### Section 9 Lazy Loading with Modules ####
* Eager Loading

![image](https://user-images.githubusercontent.com/5309726/226278846-2f2f38a4-6aa4-4452-a389-2eac003e7b08.png)

* Lazy Loading

![image](https://user-images.githubusercontent.com/5309726/226278344-fe0ddcb3-932d-4682-b697-3259b7ca4414.png)

* Implementing Lazy Loading

![image](https://user-images.githubusercontent.com/5309726/226278896-d2fe576d-cbcf-4f6a-8346-3ef6d4cb8ce1.png)

```
- app-routing.module.ts
const routes = Routes = [
  { path: 'elements', loadChildren: () => import('./elements/elements.module').then(m => m.ElementsModule) },
  { path: '', component: HomeComponent },
  { path: '**', component: NotFoundComponent }
]

- elements-routing.module.ts
const routes: Routes = [{ path: '', component: ElementsHomeComponent }]
```

* Widget Modules
  * Provide the ability for the user to create and export the component and to import it by other components for reusability

* Grabbing Content with NgContent

![image](https://user-images.githubusercontent.com/5309726/226546159-6e617d3d-a473-4c20-90fb-83db1807e987.png)

```
- divider.component.html
<h1><ng-content></ng-content></h1>

- elements-home.component.html
<app-divider>Placeholder Component</app-divider>
```

* NgContent with Select

```
- segment.component.html
<div class="ui icon header">
  <ng-content select="header"></ng-content>
</div>

  <ng-content></ng-content>

- elements-home.component.html

<app-segment>
  <header>
    <i class="pdf file outline icon"></i>
      No documents are listed for this customer.
  </header>
  
    <button class="ui primary button">Add Document</button>
</app-segment>
```

![image](https://user-images.githubusercontent.com/5309726/226550410-6bcc59f4-d392-44d1-9a1e-185877929464.png)

---

#### Section 10 Advanced Component Routing ####
![image](https://user-images.githubusercontent.com/5309726/226809705-ee517ce5-7fc0-42a8-b1e1-89cf58bcf096.png)

* Adding Child Navigation Routes

```
- collection-routing.module.ts
const routes: Routes = [
  { path: '', component: CollectionsHomeComponent, children: [
      { path: '', component: BiographyComponent }, //relative link
      { path: 'companies', component: CompaniesComponent },
      { path: 'partners', component: PartnersComponent }
    ]
]

- collections-home.component.html
<!-- Relative path -->
<a class="item" routerLink="./">Biography</a> 
<a class="item" routerLink="companies">Companies</a>
<a class="item" routerLink="partners">Patners</a>

<!-- Absolute path -->
<a class="item" routerLink="/collections">Biography</a> 
<a class="item" routerLink="/collections/companies">Companies</a>

```

![image](https://user-images.githubusercontent.com/5309726/226811264-38fa7d81-aa83-4fd2-93ab-205a4d1d971e.png)

* Alternate RouterLink Syntax

```
<a class="item" [routerLink]="['collections', 'partners', linkPropertyFromClass]">Biography</a>
```

* Matching Exact Paths

```
<a class="item" routerLink="./" [routerLinkActiveOptions]="{ exact: true }">Biography</a>
```

---

#### Section 11 Advanced Component Reusability + Hooks ####

* Natural Issues with Modal Windows 
  * The position property with a relative value in CSS applied to the parent affects the modal

![image](https://user-images.githubusercontent.com/5309726/226856691-dbdf8bcf-8b41-436b-bfc8-9004257266b7.png)

* Solving the Modal Issue

![image](https://user-images.githubusercontent.com/5309726/226858146-3336f956-dcc4-4589-b95a-d740caf5bb21.png)

* Lifecycle Hooks

![image](https://user-images.githubusercontent.com/5309726/226859347-a6511b31-e3ab-47f2-ab20-ff72259ac3be.png)

* Opening the Modal

```
- mods-home.component.html
<button (click)="onClick()">Show Modal</button>
<app-modal *ngIf="modalOpen"></app-modal>

- mods-home.component.ts
export class ModsHomeComponent {
  modalOpen = false;

  onClick() {
    this.modalOpen = !this.modalOpen;
  }
}
```

* Closing the Modal

```
- mods.home.component.html
<app-modal (close)="onClick()" *ngIf="modalOpen"></app-modal>

- mods.home.component.ts
export class ModsHomeComponent {
  modalOpen = false;

  onClick() {
    this.modalOpen = !this.modalOpen;
  }
}

- modal.component.html
<div class="ui dimmer visible active" (click)="onCloseClick()">
</div>

- modal.component.ts
export class ModalComponent implements OnInit, OnDestroy {
  @Output() close = new EventEmitter();

  constructor(private el: ElementRef) {
    console.log(el.nativeElement);
  }

  ngOnDestroy(): void {
    this.el.nativeElement.remove();
  }

  ngOnInit(): void {
    document.body.appendChild(this.el.nativeElement);
  }

  onCloseClick() {
    this.close.emit();
  }
}
```

* Stopping Event Bubbling

```
- modal.component.ts
<div class="ui dimmer visible active" (click)="onCloseClick()">
    <div class="ui modal visible active" (click)="$event.stopPropagation()">
    </div>
</div>
```

---

#### Section 13 Handling Data and HTTP Requests ####

![image](https://user-images.githubusercontent.com/5309726/227167842-10ba2a66-eb45-4426-a9d5-be3a3af57da2.png)

* Handling Form Submission

```
- search-bar.components.html
<form (submit)="onFormSubmit($event)">
  <input (input)="term = $event.target.value" />
</form>

- search-bar.component.ts
export class SearchBarComponent {
  onFormSubmit(event: Event) {
    event.preventDefault();
    console.log(this.term);
  }
}
```

* Child to Parent Communication

![image](https://user-images.githubusercontent.com/5309726/227398606-758a970f-cc85-401b-a2fc-8a24335b3748.png)

```
- search-bar.components.html
<form (submit)="onFormSubmit($event)">
  <input (input)="term = $event.target.value" />
</form>

- search-bar.component.ts
export class SearchBarComponent {
  @Output() submitted = new EventEmitter<string>();
  
  onFormSubmit(event: Event) {
    event.preventDefault();
    this.submitted.emit(this.term);
  }
}

- app.component.html
<app-search-bar (submitted)="onTerm($event)"></app-search-bar>
<app-page-list><app-page-list>

- app.component.ts
export class AppComponent {
  onTerm(term: string) {
  }
}
```

* Notes of Services

![image](https://user-images.githubusercontent.com/5309726/227401434-5a27728c-39f9-4df2-856a-68caa7e68604.png)

* Dependecy Injection

![image](https://user-images.githubusercontent.com/5309726/228477505-a93d68f2-d5aa-4d2e-8d87-550cf1d17570.png)

---

#### Section 14 App Security in Angular ####

* Escaping HTML characters

![image](https://user-images.githubusercontent.com/5309726/228481127-af06be21-709d-4d01-9311-7588a8c07040.png)

```
- page-list.component.html
<td [innerHTML]="page.snippet"></td>
```

* Another CSS Gotcha
  * It (page-list.component.css) will not style the CSS if it's not in the component (If the HTML contains CSS and it's assigned to an HTML element)
  * It works when move to styles.css
 
---

#### Section 15 RxJs From the Fundamentals  ####

* Adding RxJs Terminology

![image](https://user-images.githubusercontent.com/5309726/229417872-dc5cf413-5201-4b56-a66e-16d6c8bac3b3.png)

* Creating an Observable, https://out.stegrider.now.sh/

```
const { fromEvent } = Rx;
const { map, pluck } = RxOperators;

const input = document.createElement('input');
const container = document.querySelector('.container');
container.appendChild(input);

const observable = fromEvent(input, 'input')
                   .pipe(
                      pluck('target', 'value'),
                      map(value => parseInt(value)),
                      map(value => {
                          if (isNan(value)) {
                              throw new Error('Enter a number!');
                          }
                          
                          return value;
                      });
                   )

observable.subscribe({
  next(value) {
    console.log(`Your value ${value}`);
  },
  error(err) {
    console.error('BAD THING HAPPEN!!!', err.message)
  }
});

//This is specific to this tool
observable;
```

* Implementing the Processing Pipeline

![image](https://user-images.githubusercontent.com/5309726/229440204-99fc0682-05da-4c16-bbcd-08f37e758b97.png)

![image](https://user-images.githubusercontent.com/5309726/229448321-44299f31-eded-4833-8e60-572dd3b90522.png)

* Operator Groups

![image](https://user-images.githubusercontent.com/5309726/229450194-d2a28a80-d522-465e-a3e4-0e7b59ffe32e.png)

![image](https://user-images.githubusercontent.com/5309726/229451218-0e21aa4c-0379-4ebd-85d1-109f43db4f76.png)

* Low Level Observables

```
const { Observable } = Rx;
const observable = new Observable((subscriber) => {
  //Throw the value 1 into our pipneline
  subscriber.next(1);
  
  //Marks the observable as complete, no more values will come out
  subscriber.complete();
  
  //Emit an error, no more values will come out
  subscriber.error(new Error('Hello Error'));
});

observable.subscribe({
  next(value) {
    console.log('Got a value', value);
  },
  complete() {
    console.log('Observable is complete. Dont expect any more values');
  },
  error() {
    console.log('BAD THING!!!', err.message);
  }
});

//alternative Observer Syntax
observable.subscribe(
  (value) => console.log('Next value:', value),
  (err) => console.error('BAD THING!!!', err.message),
  () => console.log('COMPLETE')
);

observable;
```

* Unicase Observables (Cold Observable)

![image](https://user-images.githubusercontent.com/5309726/229469829-d4ac4eca-731a-4ef7-9e25-bb2c292778f3.png)

* Can easily lead bad behavior

![image](https://user-images.githubusercontent.com/5309726/229469755-89fea4e9-f1c5-45e5-89f6-b498eb2b751e.png)

* Multicase Observables (Hot Observable)

![image](https://user-images.githubusercontent.com/5309726/229470631-5cc45e72-e45c-434b-bfe7-08157d47f8ee.png)

* Take notes on point 3 & 4

![image](https://user-images.githubusercontent.com/5309726/229470699-ca8564b7-8ef8-475f-9c15-fd79f8c445e8.png)

```
const { Observable } = Rx;
const { tap, share } = RxOperators;

const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
}).pipe(
  tap(valie => console.log('From tap:', value)),
  share()
);

observable.subscribe(
  (value) => console.log('Next value:', value),
  (err) => console.error('BAD THING!!!', err.message),
  () => console.log('COMPLETE')
);

//point 4
observable.subscribe((value) => {
  console.log('From second subscribe', value);
});
```

---
