# Angular Overview

-
-

## The Client and the Server

-
-

The server is responsible for processing data and sending data to the client.
The client is responsible for presenting the data to the user.

<img style="width: 600px" src="img/Client-Server.png" />

-

The data we display, and the way we display it, can change depending on context of how we view it.

<img style="width: 600px" src="img/Screen-Server.png" />

-


The data we display, and the way we display it, can change depending on context of who is viewing it and under what circumstances.

<img style="width: 600px" src="img/Person-Server.png" />

-

How do we structure, display and manipulate data on the web?

<img style="width: 800px" src="img/html-css-js.jpg" />

Structure · Display · Manipulate

-

Let's look up a place to stay on airbnb.

-

Airbnb displays the same type of information for each property. They share a single ```model```.

<img style="width: 800px" src="img/airbnb-model.png" />

-

Airbnb displays its data in a set of repeated components.

<img style="width: 800px" src="img/airbnb-components.png" />

-

Depending on the platform used to access the site, the ```view```, or structure and presentation of the site, may change.

<img style="height: 500px" src="img/airbnb-mobile.png" />

-

Airbnb displays data based on context, like the location, dates and user.
The user interacts with the site and javascript ```controls``` what data
is displayed based on the context.

<img style="width: 800px" src="img/airbnb-context.png" />

-
-

## Model, View, Controller (MVC)

<img style="width: 800px" src="img/mvc.png" />

-

## Model, View, Controller (MVC)

* MVC is a *design pattern* that divides an application into three interconnected parts. 

* Separates internal representations of information from the ways information is presented to and accepted from the user.

* The MVC design pattern decouples these major components allowing for efficient code reuse and parallel development.

-

## Angular: Component-based MVC

Angular is a front-end development framework for building modular, reusable components 
that respond to user input and update the view accordingly.

Built using Typescript, and utilizes Typescript to generate Angular building blocks like modules, components and services.

-

## The DOM

The Document Object Model (DOM) is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects. That way, programming languages can connect to the page.

<img src="https://www.w3schools.com/js/pic_htmltree.gif" />

-

## The DOM

A Web page is a document. This document can be either displayed in the browser window or as the HTML source. But it is the same document in both cases. 

The Document Object Model (DOM) represents that same document so it can be manipulated. The DOM is an object-oriented representation of the web page, which can be modified with a scripting language such as JavaScript through an interface of properties and methods.

-
-

## TypeScript vs. JavaScript

* JavaScript
    * Scripting language for the web, available on every browser
    * Weakly typed
* TypeScript
    * Starts from the same syntax and semantics of JavaScript
    * Compiles to clean, simple JavaScript code which runs on any browser
    * Adds optional static typing

-

## What Is TypeScript?

* Open source
* Maintained by Microsoft
* Translates to a configurable JavaScript version
* Uses ES6/ES7 syntax if possible

-

## TypeScript classes

```javascript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet(): string {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```
-

## TypeScript vs Java

Similarities
* Concrete Classes
* Abstract Classes
* Inheritance
* Interfaces

Differences
* Typescript uses type inference when variables are initialized without a specific type
* Types precede variable names in Java, and follow variable names in TypeScript

-

## Types in TypeScript

* Boolean
* String
* Number
* Enum
* Array (Generic)
* Tuple
* Interface
* Class
* Object
* any

<a href="https://www.typescriptlang.org/docs/handbook/basic-types.html">Types in TypeScript</a>

-

## Modules in TypeScript and JavaScript

* Modules are small units of independent, reusable code. 

* Foundation of many JavaScript design patterns and are critically necessary when building any non-trivial JavaScript-based application.

-

## Modules in TypeScript and JavaScript
* Closest analog in the Java language are Java Classes
    * Like Java, modules can import from other modules
    * JavaScript modules export a value, rather than define a type. In practice, most JavaScript modules export an object literal, a function, or a constructor. Modules that export a string containing an HTML template or a CSS stylesheet are also common.

```javascript
import { cart } from 'store/customer';
import inventory from 'inventory';

export class Checkout {
    // ...
}
```

-

## Modules in TypeScript and JavaScript

<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import">Imports in JavaScript</href>
<a href="https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export">Exports in JavaScript</href>

-

## Angular Building Blocks

* ``modules``—Declares a compilation context for a set of components that is dedicated to an application domain, a workflow, or a closely related set of capabilities. An NgModule can associate its components with related code, such as services, to form functional units.

* ``components``—Defines a class that contains application data and logic, and is associated with an HTML template that defines a view to be displayed.

* ``services``—For data or logic that is not associated with a specific view, and that you want to share across components, you create a service class.

-

## Angular Building Blocks

* ``components``—Represents the ``view`` and ``controler`` in the MVC pattern.

* ``services``—Represents the ``model`` in the MVC pattern.

* ``modules``—Packages components and services into modular units.

-

## Angular Building Blocks

Modules, components and services are simply classes, with ``decorators`` that mark their type and provide metadata that tells Angular how to use them.

Closest Java analogue for ``decorators`` are ``annotations``.

-

## Decorators

Modules, components and services are designated using ``decorators``.
Decorators are functions that modify JavaScript classes. Angular defines a number of such decorators that attach 
specific kinds of metadata to classes, so that it knows what those classes mean and how they should work.

These decorators accept a single object, which represents the metadata that is used to configure the module, component or service

```javascript
@Decorator({
    metadataProperty: propertyValue,
    metadataProperty2: propertyValue2
})
```

-

## Declaring Modules, Components and Services

``module`` metadata
* defines the components that belong to the module
* defines the services the module uses
* defines other modules that are needed by the module, and elements which should be visible and usable in the component templates of other NgModules

-

## Declaring Modules, Components and Services

``component`` metadata
* Associates it with a template that defines a view
    * template combines ordinary HTML with Angular ``directives`` and binding markup that allow Angular to modify the HTML before rendering it for display.
  
-
  
## Declaring Modules, Components and Services

``service`` metadata
* Provides the information Angular needs to make it available to components through Dependency Injection (DI).

-
-

## Angular Modules

* Every Angular app has a root module, conventionally named AppModule, which provides the mechanism that launches the application. 

* An app typically contains many functional modules.
  
-

# Angular Modules

* NgModules can import functionality from other NgModules, and allow their own functionality to be exported and used by other NgModules. 
    * For example, to use the router service in your app, you import the Router NgModule.
-

# Angular Modules

* Organizing your code into distinct functional modules helps in managing development of complex applications, and in designing for reusability. 
    * Modularity lets you take advantage of lazy-loading—loading modules on demand—in order to minimize the amount of code that needs to be loaded at startup.
    
-
    
## Configuring your NgModules

* ``declarations``—The ``components``, ``directives``, and ``pipes`` that belong to this NgModule.

* ``exports``—The subset of declarations that should be visible and usable in the component templates of other NgModules.

* ``imports``—Other ``modules`` whose exported classes are needed by component templates declared in this NgModule.

-

## Configuring your NgModules

* ``providers``—Creators of ``services`` that this NgModule contributes to the global collection of services; they become accessible in all parts of the app. (You can also specify providers at the component level, which is often preferred.)

* ``bootstrap``—The main application view, called the ``root component``, which hosts all other app views. Only the root NgModule should set this bootstrap property.

-

## Example Root Module

```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { Logger } from './services/logger';

import { AppComponent } from './app.component';

@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
-
-

## Angular Components

A component controls a patch of screen called a view. 
You define a component's application logic—what it does to support the view—inside a class. The class interacts with the view through an properties and methods.

-

## Configuring your Components

* ``selector``—A CSS selector that tells Angular to create and insert an instance of this component wherever it finds the corresponding tag in template HTML. For example, if an app's HTML contains <app-todo-list></app-todo-list>, then Angular inserts an instance of the TodoListComponent view between those tags.

* ``templateUrl``—The module-relative address of this component's HTML template. Alternatively, you can provide the HTML template inline, as the value of the template property. This template defines the component's host view.

-

## Configuring your Components

* ``providers``—An array of dependency injection providers for services that the component requires. In the example, this tells Angular how to provide the TodoService instance that the component's constructor uses to get the list of todos to display.

-

## Sample Component

```javascript
@Component({
  selector:    'app-todo-list',
  templateUrl: './todo-list.component.html',
  providers:  [ TodoService ]
})
export class TodoListComponent {
  private todoService: TodoService
  todos: Todo[];
  selectedTodo: Todo;

  constructor(todoService: TodoService) { 
    this.todoService = todoService;
  }

  selectTodo(todo: Todo) { this.selectedTodo = todo; }
}
```

-

## Component Templates

Components rely on templates to render their view. 

These templates are a combination of HTML and Angular ``directives`` and binding markup.

```javascript
<h2>Hero List</h2>

<p><i>Pick a todo from the list</i></p>
<ul>
  <li *ngFor="let todo of todos" (click)="selectHero(hero)">
    {{todo.name}}
  </li>
</ul>

<app-todo-detail *ngIf="selectedTodo" [todo]="selectedTodo"></app-hero-detail>
```

-
## Component Templates

```javascript
<h2>Hero List</h2>

<p><i>Pick a todo from the list</i></p>
<ul>
  <li *ngFor="let todo of todos" (click)="selectHero(hero)">
    {{todo.name}}
  </li>
</ul>

<app-todo-detail *ngIf="selectedTodo" [todo]="selectedTodo"></app-hero-detail>
```

This template uses typical HTML elements like ``<h2>`` and ``<p>``, and also includes Angular template-syntax elements, ``*ngFor``, ``{{hero.name}}``, ``(click)``, ``[hero]``, and ``<app-hero-detail>``. The template-syntax elements tell Angular how to render the HTML to the screen, using program logic and data.

-

## Data Binding

* Without a framework, you would be responsible for pushing data values into the HTML controls and turning user responses into actions and value updates. Not fun.

* Angular supports ``two-way data binding``, a mechanism for coordinating parts of a template with parts of a component. Add binding markup to the template HTML to tell Angular how to connect both sides.

-

## Data Binding

The following diagram shows the four forms of data binding markup. Each form has a direction—to the DOM, from the DOM, or in both directions.

<img src="https://angular.io/generated/images/guide/architecture/databinding.png" />

-

## Data Binding Examples

```javascript
<li>{{todo.name}}</li>
<app-todo-detail [todo]="selectedTodo"></app-todo-detail>
<li (click)="selectTodo(todo)"></li>
```

* The ``{{todo.name}}`` interpolation displays the component's hero.name property value within the <li> element.

* The ``[todo]`` property binding passes the value of selectedHero from the parent HeroListComponent to the hero property of the child HeroDetailComponent.

* The ``(click)`` event binding calls the component's selectHero method when the user clicks a hero's name.

-

## Angular Directives

Directives can shape or reshape the DOM's structure, or change the appearance or behavior of DOM elements.

```javascript
<ul>
  <li *ngFor="let todo of todos" (click)="selectHero(hero)">
    {{todo.name}}
  </li>
</ul>
<app-todo-detail *ngIf="selectedTodo" [todo]="selectedTodo"></app-hero-detail>
```

* The ``*ngFor`` directive adds a ``<li>`` for every todo in the todos array.

* The ``*ngIf`` directive only shows the ``<app-todo-detail>`` component if a todo is selected

-
-

## Angular Services

Service is a broad category encompassing any value, function, or feature that an app needs. 

A service is typically a class with a narrow, well-defined purpose. It should do something specific and do it well. 

-

## Services vs Components

Angular distinguishes components from services in order to increase modularity and reusability.

By separating a component's view-related functionality from other kinds of processing, you can make your component classes lean and efficient.
 
-

## A Components Job 
  
* Enable the user experience and nothing more. 
* Present properties and methods for data binding
* Mediate between the view (rendered by the template) and the application logic (which often includes some notion of a model).

-

## A Service's Job

* Fetch data from the server
* Validate user input
* Log directly to the console
* Define processing tasks in an injectable service class, making it available to any component

-

## Configuring your Services

To configure your service, simply add the ``@Injectable()`` decorator.

This tells angular that this class should be made available through dependency injection when requested.

-

## Sample Service

```javascript
@Injectable()
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```
-

## Dependency Injection

```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { Logger } from './services/logger';
import { TodoService } from './services/todo-service';

import { AppComponent } from './app.component';
import { TodoComponent } from './components/todo-component';

@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent, TodoComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
-

## Dependency Injection

```javascript
@Component({
  selector:    'app-todo-list',
  templateUrl: './todo-list.component.html',
  providers:  [ TodoService, Logger ]
})
export class TodoListComponent {
  private todoService: TodoService
  todos: Todo[];
  selectedTodo: Todo;

  constructor(todoService: TodoService) { 
    this.todoService = todoService;
  }

  selectTodo(todo: Todo) { this.selectedTodo = todo; }
}
```

-
-

## Angular Router

-

## Configuring Routes

```javascript
import { NgModule }              from '@angular/core';
import { RouterModule, Routes }  from '@angular/router';
 
import { CrisisListComponent }   from './crisis-list.component';
import { HeroListComponent }     from './hero-list.component';
import { PageNotFoundComponent } from './not-found.component';

const appRoutes: Routes = [
  { path: 'crisis-center', component: CrisisListComponent },
  { path: 'hero/:id',      component: HeroDetailComponent },
  {
    path: 'heroes',
    component: HeroListComponent,
    data: { title: 'Heroes List' }
  },
  { path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
  },
  { path: '**', component: PageNotFoundComponent }
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
  ],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
-

## appRoutes

Our ``const`` ``appRoutes`` defines an array of routes. Each route is an object that contains data about that route, including it's path and the component it should route to.

```javascript
const appRoutes: Routes = [
  { path: 'crisis-center', component: CrisisListComponent },
  { path: 'hero/:id',      component: HeroDetailComponent },
  {
    path: 'heroes',
    component: HeroListComponent,
    data: { title: 'Heroes List' }
  },
  { path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
  },
  { path: '**', component: PageNotFoundComponent }
];
```
-

## Path Variables

We can define path variables (and later use those values in our components) through colon notation. ``:id``. Defines a path variable named ``id``.

```javascript
{ path: 'hero/:id', component: HeroDetailComponent },
```
-

## Redirecting

We can redirect a route using the ``redirectTo`` property.
 
A redirect route requires a pathMatch property to tell the router how to match a URL to the path of a route. In this app, the router should select the route to the HeroListComponent only when the entire URL matches '', so set the pathMatch value to 'full'.
```javascript
{ path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
},
{ path: '**', component: PageNotFoundComponent }
```
-

## Wildcard Routes

``**`` matches any route not listed in the routes configuration.

```javascript
{ path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
},
{ path: '**', component: PageNotFoundComponent }
```
-

## Imports

The ``appRoutes`` array of routes describes how to navigate. Pass it to the ``RouterModule.forRoot`` method in the module ``imports`` to configure the router.

```javascript
@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
  ],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
-

## Exports

Re-Export the Angular RouterModule by adding it to the module exports array. By re-exporting the RouterModule here and importing AppRoutingModule in AppModule, the components declared in AppModule will have access to router directives such as RouterLink and RouterOutlet.

`````javascript
@NgModule({
    imports: [
      RouterModule.forRoot(appRoutes)
    ],
    exports: [RouterModule]
})
export class AppRoutingModule { }
`````
-

## Importing into AppModule

To use your ``AppRoutingModule``, import it into your ``AppModule``.

```javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app.routing.module';

@NgModule({
  declarations: [
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
-

## Router Outlet

Add ``<router-outlet></router-outlet>`` where you want your routes to be loaded.

```html
<!-- a great place to add this is the root app component -->
<router-outlet></router-outlet>
```

-

## Router Link

Using the routerLink directive, we can navigate to a new route without refreshing the page.

We can use data interpolation within our routes using moustache notation.

```html
<div ngFor="let hero of heros">
    <a routerLink="/hero/{{hero.id}}">More info about {{hero.name}}</a>
</div>
```
-

## Accessing Router Params

We can access Url Path Variables (also known as Route params), using ``ActivatedRoute``. 

We import it first and then inject it into the constructor of BlogComponent. It exposes an Observable which we can subscribe to, like so:

```javascript
import { ActivatedRoute } from "@angular/router";

@Component({
    selector: 'app-sample',
    templateUrl: './app-sample.component.html'
})
export SampleComponent() {
    constructor(private route: ActivatedRoute) {
        this.route.params.subscribe( params => console.log(params) );
    }
}
```
-

<img src="https://i.pinimg.com/736x/2c/ac/ae/2cacae63626e91d6887608bf51217907.jpg">









