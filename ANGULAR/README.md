# ANGULAR Framework

## Basics

Use Angular CLI via `npm install -g @angular/cli`
Create project using `ng new <project-name>`
Choose CSS
Run the app with `ng serve`

## VS Code extensions

Auto Rename Tag
Angular Language Server
Prettier
angular2-switcher (keymap to switch between component files)
Material Icon Theme
Angular snippets (not tested yet)
ESLint (do not install it; detect errors, style issues, allow team code consistency, etc)

## File Tree

`angular.json` -> Angular settings with ng serve config for example
`package-lock.json` -> All local dependencies with versions
`package.json` -> All dependencies
`tsconfig.json` -> main TS config
`tsconfig.json` -> extension of the TS config
`tsconfig.spec.json` -> extension of the TS config that concerns tests
`*.spec.*` -> concerns tests

## Component

1 component = 1 folder = 4 files

1. `*.html`
2. `*.css`
3. `*.ts`
4. `*.spec.ts`

Can be generated via `ng generate component <compName>` at project root

## Directives

```
@if (isServerRunning) { ... }
@else { ... }
```

```
@for (os of operatingSystems; track os.id) {
    {{ os.name }}
}
```

```
<!-- Property binding -->
Wrap the attribute name in square brackets

<img alt="photo" [src]="imageURL">

export class AppComponent {
  imageURL = 'photo.jpg';
}
```

```
<!-- Event handling -->
Bind to an event function with parentheses

@Component({
    ...
    template: `<button (click)="greet()">`
})
class AppComponent {
    greet() {
        console.log('Hello, there 👋');
    }
}
```

```
<!-- Components communication - from parent to child -->

@Component({
  ...
  selector: 'app-user'
})
class UserComponent {
  @Input() occupation = '';
}

@Component({
  ...
  template: `<app-user occupation="Angular Developer"><app-user/>`
})
class AppComponent {}
```

```
<!-- Components communication - from children to parent -->

@Component({...})
class ChildComponent {
    @Output() addItemEvent = new EventEmitter<string>();

    onClick() {
        this.addItemEvent.emit('the item');
    }
}

@Component({
  ...
  template: `
    <app-child (addItemEvent)="addItem($event)" />
  `,
  standalone: true,
  imports: [ChildComponent],
})
export class AppComponent {
  items = new Array();

  addItem(item: string) {
    this.items.push(item);
  }
}
```

```
<!-- Deferrable views -->
Load a large component only when the browser is idle or when the user scroll on it

@defer (on viewport) {
  <comments />
} @placeholder {
  <p>Future comments</p>
} @loading (minimum 2s) {
  <p>Loading comments...</p>
}
```

```
<!-- Image optimization -->
Width and Height must be specified or use fill property
Possibility to prioritize important images or use image loader to shorten URLs

import { NgOptimizedImage } from '@angular/common';
@Component({
  standalone: true,
  template: `
    ...
    <li>
      Static Image:
      <img ngSrc="/assets/logo.svg" alt="Angular logo" width="32" height="32" />
    </li>
    <li>
      Dynamic Image:
      <img [ngSrc]="logoUrl" [alt]="logoAlt" width="32" height="32" />
    </li>
    ...
  `,
  imports: [NgOptimizedImage],
})

```

```
<!-- Routing -->
All in app.routes.ts
Import RouterOutlet and add <router-outlet> in app.component.ts

export const routes: Routes = [
  {
    path: '',
    title: 'App Home Page',
    component: HomeComponent,
  },
];
```

```
<!-- Navigation -->
Avoid to fully reload page
Import RouterLink

@Component({
  ...
  template: `
    <nav>
      <a routerLink="/">Home</a>
      |
      <a routerLink="/user">User</a>
    </nav>
    <router-outlet />
  `,
  imports: [RouterOutlet, RouterLink],
})
```

```
<!-- Template Forms -->
Link a ngModel to an input so the property updates at every changes

@Component({
  ...
  template: `
    <p>{{ username }}'s favorite framework: {{ favoriteFramework }}</p>
    <label for="framework">
      Favorite Framework:
      <input id="framework" type="text" [(ngModel)]="favoriteFramework" />
    </label>
  `,
  standalone: true,
  imports: [FormsModule],
})
export class UserComponent {
  favoriteFramework = '';
  username = 'youngTech';
}
```

```
<!-- Reactive Forms -->
@Component({
  ...
  template: `
    <form [formGroup]="profileForm" (ngSubmit)="handleSubmit()">
      <input type="text" formControlName="name" />
      <input type="email" formControlName="email" />
      <button type="submit">Submit</button>
    </form>

    <h2>Profile Form</h2>
    <p>Name: {{ profileForm.value.name }}</p>
    <p>Email: {{ profileForm.value.email }}</p>
  `,
  imports: [ReactiveFormsModule],
})

export class AppComponent {
  profileForm = new FormGroup({
    name: new FormControl(''),
    email: new FormControl(''),
  });

  handleSubmit() {
    alert(this.profileForm.value.name + ' | ' + this.profileForm.value.email);
  }
}
```

```
<!-- Validating forms -->
Check validation status via formGroup.valid

export class AppComponent {
  profileForm = new FormGroup({
    name: new FormControl('', Validators.required),
    email: new FormControl('', [Validators.required, Validators.email]),
  });
}
```

```
<!-- Injectable service -->
ng generate service <serviceName>

myService = inject(<serviceName>)

or constructor injection

class PetCarDashboardComponent {
    constructor(private petCareService: PetCareService) {
        ...
    }
}

```

```
<!-- Pipes -->
Used to transform data in templates

import {UpperCasePipe, DecimalPipe, DatePipe, CurrencyPipe} from '@angular/common';
@Component({
    ...
    template: `
    <ul>
      <li>{{ loudMessage | uppercase }}</li>
      <li>Number with "decimal" {{ num | number : '3.2-2' }}</li>
      <li>Date with "date" {{ birthday | date : 'medium' }}</li>
      <li>Currency with "currency" {{ cost | currency }}</li>
    </ul>`,
    imports: [UpperCasePipe, DecimalPipe, DatePipe, CurrencyPipe],
})
class AppComponent {
    loudMessage = 'we think you are doing great!'
}

Create a custom pipe:

import {Pipe, PipeTransform} from '@angular/core';

@Pipe({
  name: 'reverse',
  standalone: true,
})
export class ReversePipe implements PipeTransform {
  transform(value: string): string {
    let reverse = '';

    for (let i = value.length - 1; i >= 0; i--) {
      reverse += value[i];
    }

    return reverse;
  }
}
```

```
<!--  -->
```

## Others

I. `<router-outlet>` (at the end of `app.component.html`) is used to change the component shown in a page without reloading the full page, to link the URL route to the right component, strange but seems mandatory in app.component, see [that example](https://stackblitz.com/edit/angular-g8do3z?file=src%2Fmain.ts) from [that article](https://medium.com/@jsmuster/angular-routing-en-5-minutes-french-22fbf1322ab1)

II. We can pass data to the view in the component class:

```
export class AppComponent {
  city = 'San Francisco';
}
```

And then use it with `{{ city }}` in the template (template=HTML)

III. Web vitals to evaluate the quality of a website [Link](https://web.dev/explore/learn-core-web-vitals)

TODO:
What is SSG and Prerendering ?
