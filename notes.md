# Angular The Complete Guide

### Section 1: getting started

- video 8: editing the first app. introduction to the usage `ngModel` to add **two way binding**. imported from `FormsModule`.


### Section 2: The Basics
- video 24: Fully understanding the component selector.
```html
<app-server><app-server>
<!-- could be selected using attribute selectors -->
<div app-server><div>
<!-- could be selected using class selectors -->
<div class="app-server"><div>
```
the code
```typescript
@Component({
  selector: 'app-servers', // selecting by element name
})
// to be as
@Component({
  selector: '[app-servers]', // selecting by attribute name
})
// or
@Component({
  selector: '.app-servers', // selecting by class name
})
```
- selecting by `ID` is not supported by Angular.
- video 28: property binding in angular using `[]`
```typescript
export class ServersComponent implements OnInit {
  allowNewServer = false; // variable definition
  constructor() {
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }
}
```
```html
<!-- binding disabled property with allowNewServer -->
<button class="btn btn-primary" [disabled]="!allowNewServer">Add Server</button>
```
- video 30: event binding. 
- video 32: passing and using data with event binding. there a reserved word in templates `$event` to pass the event value to methods
```html
<input type="text" class="form-control" (input)="onUpdateServerName($event)">
```
- note 33: related to video 8.
- video 38: using ngif to output data conditionally. as ngIf is a **structural directive**, it uses a * such as `*ngIf`, unlike **attribute directive**
- video 39: enhancing ngif with an else condition. add `else` to ngIf using local reference temaplate
```html
<p *ngIf="serverCreated; else noServer">Server was created, server name is {{ serverName }}</p>
<ng-template #noServer>
  <p>No server was created!</p>
</ng-template>
```
- video 40: styling elements dynamically with ngStyle. **attribut directive** `ngStyle` can used to style dom elements.
- video 40; ngStyle directive can receieve object notation as following
```html
<p [ngStyle]="{'background-color': getColor()}"></p>
<!-- using camel case notation -->
<p [ngStyle]="{backgroundColor: getColor()}"></p>
```
- video 44; getting the index when using `ngFor`
```html
<div *ngFor="let value of items; let i = index"></div>
```

### Section 3: Course Project - The Basics
### Section 4: Debugging
### Section 5: Componenets & Databinding Deep Dive

- video 67: Binding to Custom properties, exposing a property using `@Input`
```typescript
@Input() element : {type:string, name: string, content: string};
```
- video 68: Assigning an alias to custom properties
```typescript
@Input('srvElement') element : {type:string, name: string, content: string};
```
- video 69: Binding to Custom events, fire a custom event
```typescript
@Output() serverCreated = new EventEmitter<{name: sting}>
```
- video 70: Assiging an alias to custom events
```typescript
@Output('SeverAdded') serverCreated = new EventEmitter<{name: sting}>
```
- video 73: More on View encapsulation. changing component encapsulation `ViewEncapsulation`.

- video 74: Using local references in templates
- note 75: `@ViewChild()` in Angular 8+
- video 76: getting access to the template & DOM with @ViewChild
```typescript
@ViewChild() serverContent: ElementRef<HTMLInputElement>;
```
- video 77: projecting content into Components with `ng-content`


- video 78: angular lifecycle hooks

- video 79: seeing lifecycle hooks in action. `ngOnChanges` and `SimpleChanges` arguments

### Section 6: Course Project - Componenets & Databinding Deep Dive

- video 86: Adding Navigation with Event Binding and ngIf. you could pass the data in addition to the `$event` itself.
```html
<li><a href="#" (click)="onSelect('recipe', $event)">Recipes</a></li>
```
- video 88: passing data with event and property binding (combined), you use `void` in event emitter as 
```typescript
@Output() recipeSelected = new EventEmitter<void>();
```

### Section 7: Directive Deep Dive

- video 91: Module Introduction. two types of directives; attribute directive & structural directive.

- video 93: `ngClass` and `ngStyle` recap

- video 94: creating basic attribute directive using `@Directive` decorator.

- video 97: Using `HostListener` to listen to host events.

- video 98: using `HostBinding` to bind to host properties.

- video 101: building structural directive. using `set` on a property as a setter.

- video 102: understanding `ngSwitch`

### Section 8: Course Project - Directive

- Note 104: closing the Dropdown directive
```typescript
import {Directive, ElementRef, HostBinding, HostListener} from '@angular/core';

@Directive({
  selector: '[appDropdown]'
})
export class DropdownDirective {
  @HostBinding('class.open') isOpen = false;
  @HostListener('document:click', ['$event']) toggleOpen(event: Event) {
    this.isOpen = this.elRef.nativeElement.contains(event.target) ? !this.isOpen : false;
  }
  
  constructor(private elRef: ElementRef) {}
}
```

### Section 9: Using services & Dependency Injection

- video 108: injecting the logging service into components. how the shared service is instantiated among different components using `providers`.
```typescript
import { LoggingService } from 'logging.service';
@component({
  providers: [LoggingService]
})
export class NewAccountComponent{
  constructor(private loggingService: LoggingService){}
}
```

- video 111: how many instances of service should it be. how to share only one isntance of the service among components by removing class name from `providers`

- video 112: injecting services into services. using `@Injectable` decorator.

- note 114: A Different Way Of Injecting Services
```typescript
@Injectable({providedIn: 'root'})
export class MyService { ... }
```

### Section 10: Course Project - Services & Dependency Injection

- video 119: using a service for cross-component communication; using `subscribe` on a service.
### Section 11: Changing pages with Routing

- video 127: setting up and loading routes. you could load routes definition in `app.module.ts` as following:
```typescript
import { Routes, RouterModule } from '@angular/router';

const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'servers', component: ServerComponent },
]

imports:[
  RouterModule.forRoot(appRoutes)
]
```

- video 127: using `router-outlet` tag to display route content

- video 128: navigating with router link; using `routerLink` directive, you can also use property binding.
```html
<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a routerLink="/">Home</li>
  <li role="presentation" ><a routerLink="/servers">Servers</li>
  <li role="presentation" ><a [routerLink]="'/users'">Users</li>
  <!-- or -->
  <li role="presentation" ><a [routerLink]="['/users']">Users</li>
</ul>
```
- video 129: understanding navigation paths; the difference between absolute path & relative path.

- video 130: styling active router links; using `routerLinkActive` & `routerLinkActiveOptions`
```html
<ul class="nav nav-tabs">
  <li role="presentation" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}"><a routerLink="/">Home</li>
  <li role="presentation" routerLinkActive="active"><a routerLink="/servers">Servers</li>
  <li role="presentation" routerLinkActive="active"><a [routerLink]="'/users'">Users</li>
  <!-- or -->
  <li role="presentation" ><a [routerLink]="['/users']">Users</li>
</ul>
```

- video 131: navigating programmatically; injecting the router into a component
```typescript
import { Router, ActivatedRoute} from '@angular/router';
@Component({})
export class HomeComponent implements OnInit{
  constructor(private router:Router,
              private route: ActivatedRoute){}

  AnyMethod(){
    this.router.navigate(['/servers'])  // absolute path
    this.router.navigate(['servers'], {relativeTo: this.route})  //  relative path
  }
}
```

- video 133: passing parameters to routes; dynamic path segment of the path
```typescript
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users/:id', component: UsersComponent },
  { path: 'servers', component: ServerComponent },
]

```
- video 134: Fetching Route Parameters; how the component access dynamic path segments. using `ActivatedRoute`

```typescript
import { ActivatedRoute} from '@angular/router';
@Component({})
export class HomeComponent implements OnInit{
  user : {id: number, name: string};
  constructor(private route: ActivatedRoute){}

  ngOnInit(){
    this.user = {
      id: this.route.snapshot.params['id'],
    };
  }
}
```
- video 134: subscribe to path params
```typescript
import { ActivatedRoute} from '@angular/router';
@Component({})
export class HomeComponent implements OnInit{
  user : {id: number, name: string};
  constructor(private route: ActivatedRoute){}

  ngOnInit(){
    this.route.params.subscribe((params: Params) => {
      this.user.id = params['id'];
    });
  }
}
```

- video 137; Passing query parameters and fragments; usage of `queryParams` property get query parameters from path

```html
<a [routerLink]="['/servers', 5, 'edit']"
   [queryParams]="{allowEdit: '1'}"
   fragment="loading">{{ server.name }}</a>
```

- video 137: navigation programmatically example

```typescript
export class HomeComponent implements OnInit{
  onLoadServer(id: number){
    this.router.navigate(['/servers', 'id', 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'});
  }
}
```

- video 140; setting up Child (Nested) Routes
```typescript
const appRoutes: Routes = {
  { path: 'servers',
    component: ServersComponent,
    children:[
      {path: ':id', component: ServerComponent },
      {path: ':id/edit', component: EditServerComponent },
    ]
  }
};
```

- video 140: usage of `router-outlet`

- video 141: configuring the handling of query parameters; usage of `queryParamsHandling`

- video 143: redirecting and wildcard routes

- video 147: Protecting routes with `canActivate`;
- video 148; protecting nested routes; usage of `canActivateChild`

- video 150: controlling navigation with `canDeactivate`.

- video 153: understanding location strategies; usage of `useHash` configuration.

### Section 12: Course Project - Routing

- video 156; Setting up routes; usage of `pathMatch` regarding matching strategy
```typescript
const appRoutes: Routes = [
  { path: '', redirectTo: '/recipes', pathMatch: 'full' }
]
```

