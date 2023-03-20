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


---
