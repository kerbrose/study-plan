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