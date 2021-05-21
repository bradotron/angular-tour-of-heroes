# angular-tour-of-heroes
[Angular Tutorials](https://angular.io/tutorial)

# Setup
```
npm install -g @angular/cli
```

```
ng new angular-tour-of-heroes
```

```
cd angular-tour-of-heroes
ng serve --open
```

# Introduction
navigate to src/app

## App Component Files
1. app.component.ts - the component class code
2. app.component.html - the component template
3. app.component.css - the component's private CSS styles

## Change the Title
In app.component.ts, change the value of the ```title``` property.

## Change the Template
Open app.component.html, delete all content in the default template and replace it with
```
<h1>{{title}}</h1>
```
## Add Application Styles
Open src/styles.css. Add the css below, these styles will cascade down to the entire application.
```
/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[type="text"], button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

# The Hero Editor
```
ng generate component heroes
```
This generates the folder /app/heroes
Inside the folder are 4 files
1. heroes.component.ts - the heroes (HeroesComponent) class file
2. heroes.component.html - HeroesComponent template
3. heroes.component.css - HeroesComponent private scope CSS
4. heroes.component.spec.ts - a file with unit tests maybe?

Other Info
* this command sets the CSS element selector for this component to ```'app-heroes'```
* the command generates a lifecycle hook, ```ngOnInit()``` that Angular will call shortly after creating the component

## Add A Property
Add a property to the HeroesComponent class
```
hero = "Windstorm";
```
## Show the Hero
open heroes.component.html and replace the default content with
```
<h2>{{hero}}</h2>
```
To display the HeroesCompontn, add it to the AppComponent shell.
* remember that ```app-heroes``` is the element selector. 
Add ```<app-heroes>``` to src/app/app.component.html

## Create A Hero Interface
Inside the src/app folder, create a ```hero.ts``` file. Insert this content:
```
export interface Hero {
  id: number;
  name: string;
}
```
Now go back to the ```HeroesComponent``` class and import the interface. 
Refactor the ```hero``` property to be of the type ```Hero```. Initialize it with an id of 1 and the name "Windstorm".

heroes.component.ts will look like:
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  constructor() { }

  ngOnInit() {
  }
}
```
* You will notice, if you are running the live server, the hero is not displayed. The ```hero``` property is now an object.

## Show the Hero Object
Update the HeroesComponent template to show the id and name of the ```hero``` property, which is now an object.
```
<h2>{{hero.name}} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```

## Format With the Uppercase Pipe
Modify the hero.name binding like this:
```
<h2>{{hero.name | uppercase}} Details</h2>
```
The ```|``` pipe operator activates the build-in UppercasePipe. Pipes can be used to format strings, currency amounts, dates, and other display data. There are built-in pipes and you can create custom pipes.

## Edit the Hero
The users should be able to edit the hero name in an ```<input>``` textbox. The textbox should display the name, and update that property as the user types. Data flows from the component class out to the screen, then from the screen back to the class.

To automate that flow, setup a two-way data binding between ```<input>``` and ```hero.name```.

## Two-way Binding
Refactor the HeroesComponent template to look like:
```
<div>
  <label for="name">Hero name: </label>
  <input id="name" [(ngModel)]="hero.name" placeholder="name">
</div>
```
```[(ngModel)]``` is Angular's two-way data binding syntax. In this example it binds the ```hero.name``` property to the HTML textbox so that data can flow in both directions.

### The Missing FormsModule
The app stopped working when ```[(ngModel)]``` was added.

## AppModule
You need to add the ```@NgModule``` decorators. The Angular CLI generated an ```AppModule``` class in ```src/app/app.module.ts``` when the project was created. This is the file where you opt-in to the ```FormsModule```.

### Import FormsModule
Open ```AppModule``` (```app.module.ts```) and import the ```FormsModule``` symbol from the ```@angular/forms``` library.
```
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
```

To complete the import, add ```FormsModule``` to the ```@NgModule``` metadata's ```imports``` array.
```
imports: [
  BrowserModule,
  FormsModule
],
```
Look back over at the running app, and see the HeroesComponent in is glory.

## The Hero Editor - Summary
* You used the CLI to create a second HeroesComponent.
* You displayed the HeroesComponent by adding it to the AppComponent shell.
* You applied the UppercasePipe to format the name.
* You used two-way data binding with the ngModel directive.
* You learned about the AppModule.
* You imported the FormsModule in the AppModule so that Angular would recognize and apply the ngModel directive.
* You learned the importance of declaring components in the AppModule and appreciated that the CLI declared it for you.

# Display A List
We need more heroes. We are going to create a file to import from, but this is just a stand-in for data that could come from a remote data server.

## Create Some Heroes
Create a file ```mock-heroes.ts``` in ```src/app/``` and Define a ```HEROES``` constant as an array and export it.
```
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Dr Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

## Displaying Your Heroes
Open the ```HeroesComponent``` class file, and import your mock heroes. (```app/heroes/heroes.component.ts```)
```
import { HEROES } from '../mock-heroes';
```

In the same file, define a property called ```heroes``` to expose the ```HEROES``` array for binding.
```
export class HeroesComponent implements OnInit {

  heroes = HEROES;
}
```
## List Heroes With *ngFor
Open the ```HeroesComponent``` template and make it look like this:
```
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
This will result in an error, since the ```hero``` property no longer exists. This is where ```*ngFor``` comes in. Change the ```<li>``` element by adding this:
```
<li *ngFor="let hero of heroes">
```
* the asterisk is a critical part of the syntax

Huzzah we now have a list of heroes!

## Style Your Heroes
We could add more styles to ```src/app/app.component.css```, but we would prefer to define private styles that are specific to the HeroesComponent.

Open ```heroes.component.css``` and paste in the styles for the HeroesComponent.
```
/* HeroesComponent's private CSS styles */
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes li:hover {
  color: #2c3a41;
  background-color: #e6e6e6;
  left: .1em;
}
.heroes li.selected {
  background-color: black;
  color: white;
}
.heroes li.selected:hover {
  background-color: #505050;
  color: white;
}
.heroes li.selected:active {
  background-color: black;
  color: white;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color:#405061;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}

input {
  padding: .5rem;
}
```

## View Details
When the user clicks a hero, the component should display the selected heroes details at the bottom of the page.

### Add A Click Event Binding
Add the click event binding to the ```<li>``` like this:
```
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
This errors out because we haven't created the ```onSelect(hero)``` method. Add the following to ```src/app/heroes/heroes.component.ts```
```
selectedHero?: Hero;
onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```

### Add A Details Section
Currently our component only has a list of heroes. We need a section for the details to appear. Add the folliwing after the closing tag of the list ```</ul>``` in ```heroes.component.html```.
```
<h2>{{selectedHero.name | uppercase}} Details</h2>
<div><span>id: </span>{{selectedHero.id}}</div>
<div>
  <label for="hero-name">Hero name: </label>
  <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
</div>
```
The app does not work again! This is because until a click happens, selectedHero is undefined.

### Conditionally Display The Details With *ngIf
The details should only display if ```selectedHero``` exists. Wrap the detail section in a ```<div>```. Add Angular's ```*ngIf``` to the ```<div>``` and set it to ```selectedHero```.
```
<div *ngIf="selectedHero">

  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
  </div>

</div>
```

### Style The Selected Hero
For easier identification of the selected hero, you can use the .selected CSS class in ```heroes.component.css```. To do this dynamically, you use class binding. To the element you want to style, add ```[class.some-css-class]="some-condition"``` to the element you want to style. 

We want to add style to each ```<li>``` in the HeroesComponent, like this:
```
<li *ngFor="let hero of heroes"
  [class.selected]="hero === selectedHero"
  (click)="onSelect(hero)">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
```
## Display A List - Summary
* The Tour of Heroes application displays a list of heroes with a detail view.
* The user can select a hero and see that hero's details.
* You used *ngFor to display a list.
* You used *ngIf to conditionally include or exclude a block of HTML.
* You can toggle a CSS style class with a class binding.

# Create A Feature Component

## Make the HeroDetailComponent
```
ng generate component hero-detail
```

## Create the Template
Cut the detail html from the HeroesComponent template and put it into hero-detail.component.html

## Add @Input() property
The HeroDetailComponent needs a hero to bind to the component. In the HeroDetailComponent class file, import the Hero symbol.
```
import { Hero } from '../hero';
```

The ```hero``` property must be an input, annotated with @Input() so the external ```HeroesComponent``` will bind to it like this:
```
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

But first, you need to include the ```Input``` symbol in the import statement from @angular/core, in ```hero-detail.component.ts```

```
import { Component, OnInit, Input } from '@angular/core';
```

Now add a hero property, with the @Input() decorator.
```
@Input() hero?: Hero;
```

## Show The HeroDetailComponent
We will update the HeroesComponent to use the HeroDetailComponent to display hero details. The HeroesComponent will send a new hero to the HeroDetailComopnent whenever the user clicks a hero in the list. You will only change the HeroesComponent template.

Update ```heroes.component.html``` by adding an ```<app-hero-detail>``` element at the bottom of the template.

```
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

```[hero]="selectedHero"``` is an Angular property binding. This is a one-way binding from the ```selectedHero``` property of the ```HeroesComponent``` to the ```hero``` property of the target element.

## Create a Feature Component - Summary 
* You created a separate, reusable HeroDetailComponent.
* You used a property binding to give the parent HeroesComponent control over the child HeroDetailComponent.
* You used the @Input decorator to make the hero property available for binding by the external HeroesComponent.

# Add Services
Services will fetch/save data, as an interface for Components. 

## Create the HeroService
Use the Angular CLI to create a service:
```
ng generate service hero
```

## @Injectable() Services
The new service imports the ```Injectable``` symbol and annotates the class with @Injectable(). This marks the class as one that particpates in the dependancy injection system; this class will provide an injectable service and can have its own injected dependencies. It does not have any dependencies yet.

The implementation of this service will continue to deliver the mock heroes.

Import the Hero and HEROES into src/app/hero.service.ts
```
import { Hero } from './hero';
import { HEROES } from './mock-heroes';
```
And add a ```getHeroes``` method to return the mock heroes.
```
getHeroes(): Hero[] {
  return HEROES;
}
```

## Provide the HeroService
You need to make the HeroService available to the dependency injection system, before Angular can inject it into the HeroesComponent. You must register the HeroService with a provider, the provider is something that can create or deliver a service.

By default, the Angular CLI command ```ng generate service``` registers a provider with the root injector for your service by including provider metadata, that is ```providedIn: 'root'``` in ```@Injectable()``` decorator

```
@Injectable({
  providedIn: 'root',
})
```

When you provide the service at the root level, Angular creates a single, shared instance of ```HeroService``` and injects into any class that asks for it. Registering the provider in the ```@Injectable``` metadata also allows Angular to optimize an application by removing the service if it is never used.

## Update HeroesComponent
Delete the ```Heroes``` import, and add an import of ```HeroService```.
```
import { HeroService } from '../hero.service';
```

## Inject the HeroService
If you are running the live server, the page loads, but no heroes are shown.

Add a private ```heroService``` parameter of type ```HeroService``` to the constructor.
```
constructor(private heroService: HeroService) {}
```
This parameter defines a private ```heroService``` property and identifies it as the HeroService injection site. When Angular creates a ```HeroesComponent```, the Dependency Injection system sets the ```heroService``` parameter to the singleton instance of ```HeroService```.

## Add getHeroes()
Create a method to retrieve the heroes from the service. In src/app/heroes/heroes.component.ts
```
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```

## Call it in ngOnInit()
Best practice is to call ```getHeroes()``` in  the ngOnInit() lifecycle hook. Best practice is to reserve the constructor minimal initialization like wiring constructor parameters to class properties. The constructor shouldn't do anything.

```
ngOnInit() {
  this.getHeroes();
}
```

## It worked!

## Observable Data
The ```HeroService.getHeroes()``` method has a syncronous signature, which implies that the HeroService fetches synchronously. And the HeroesComponent consumes the result of getHeroes() as if the heroes can be fetched syncrhonously.

However, in a real app, fetching the heroes from a remote server is an inherently asynchronous operation. ```HeroService.getHeroes()``` must have an asynchronous signature.

We will change ```HeroService.getHeroes()``` to return an ```Observable```. The function will eventuall use the Angular ```HttpClient.get``` method, which returns an ```Observable```.

## Observable HeroService
```Observable``` is a key class from the [RxJS Library](https://rxjs.dev/)

You can do the Angular tutorial on HTTP to learn more, for now we are going to simulate getting data from a server with the RxJS ```of()``` function.

Inport the ```Observable``` and ```of``` symbols from RxJS, in hero.service.ts.
```
import { Observable, of } from 'rxjs';
```
And replace the ```getHeroes()``` method with the following:
```
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  return heroes;
}
```

The app breaks again, since the heroes component is not expecting an Observable.

## Subscribe in HeroesComponent
The ```HeroService.getHeroes``` method was changed and now returns an ```Observable<Hero[]>```. We need to adjust to this inside ```HeroesComponent```.

In ```heroes.component.ts``` replace the getHeroes method with:
```
getHeroes(): void {
  this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes);
}
```

```.subscribe()``` is the critical difference. This now makes the HeroesComponent wait until the ```Observable``` emits the array of heroes.

# Show Messages
We can add a ```MessagesComponent``` that will display application messages at the bottom of the screen. This will require a ```MessagesComponent``` and an injectable ```MessageService```. The ```MessageService``` will be injected into the ```HeroService``` to display a message when ```HeroService``` fetches the heroes successfully.

## Create MessagesComponent
Use the Angular CLI to create a new compnent
```
ng generate component messages
```

The CLI creates the folders and files and declares the ```MessagesComponent``` in ```AppModule```. 

Modify the ```AppComponent``` template to display the generated ```MessagesComponent```. (src/app/app.component.html)
```
<h1>{{title}}</h1>
<app-heroes></app-heroes>
<app-messages></app-messages>
```
You'll see the default content of the ```MessagesComponent``` at the bottom of the page.

## Create the MessageService
Use the CLI to create a new service
```
ng generate service message
```

Open src/app/message.service.ts and replace the contents with:
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class MessageService {
  messages: string[] = [];

  add(message: string) {
    this.messages.push(message);
  }

  clear() {
    this.messages = [];
  }
}
```

The changes to the default do a couple things. The MessageService now exposes its cache of ```messages``` and two methods: one to ```add()``` a message to the cache and another to ```clear()``` the cache.

## Inject MessageService into HeroService
Import ```MessageService``` into ```HeroService```. (src/app/hero.service.ts)
```
import { MessageService } from './message.service';
```

Modify the HeroService constructor with a parameter that declares a private ```messageService``` property. Angular will inject the singleton ```MessageService``` into that property when it creates the HeroService.
```
constructor(private messageService: MessageService) { }
```

* This is a typical "service-in-service" scenario. MessageService is injected into HeroService, which is injected into the HeroesComponent.

## Send A Message From HeroService
Modify the ```getHeroes()``` method to send a message when heroes are fetched.
```
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  this.messageService.add('HeroService: fetched heroes');
  return heroes;
}
```

## Display the Message
Now we need to import the MessageService into the MessagesComponent. (src/app/messages/messages.component.ts)
```
import { MessageService } from '../message.service';
```
And modify the constructor with a parameter that declares a public ```messageService``` property. Angular will inject the ```MessageService``` singleton into that property.

(src/app/messages/messages.component.ts)
```
constructor(public messageService: MessageService) {}
```

## Bind MessagesComponent to MessageService
We haven't changed what the MessagesComponent actually displays. Replace the ```MessagesComponent``` template with:
```
<div *ngIf="messageService.messages.length">

  <h2>Messages</h2>
  <button class="clear"
          (click)="messageService.clear()">Clear messages</button>
  <div *ngFor='let message of messageService.messages'> {{message}} </div>

</div>
```
This update does things:
* ```*ngIf``` allows us to hide the component if the message cache is empty
* ```*ngFor``` presents every message in the cache in repeated <div> elements
* An angular event binding binds the button's click event to ```MessageService.clear()```

Add this to src/app/messages/messages.component.ts
```
/* MessagesComponent's private CSS styles */
h2 {
  color: #A80000;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}

.clear {
  color: #333;
  background-color: #eee;
  margin-bottom: 12px;
  padding: 1rem;
  border-radius: 4px;
  font-size: 1rem;
}
.clear:hover {
  color: white;
  background-color: #42545C;
}
```

## Add Messages to HeroService
We can add messages each time a user clicks a hero. Queueing up a history of the user's selections in the MessagesComponent.

In src/app/heroes/heroes.component.ts import MessageService
```
import { MessageService } from '../message.service';
```

Add a parameter to the constructor ```messageService``` that Angular will inject the Singleton ```MessageService``` into.
```
constructor(private heroService: HeroService, private messageService: MessageService) { }
```

Modify the ```onSelect()``` method by adding a message to the ```messageService```
```
onSelect(hero: Hero): void {
  this.selectedHero = hero;
  this.messageService.add(`HeroesComponent: Selected hero id=${hero.id}`);
}
```

## Summary
* You refactored data access to the HeroService class.
* You registered the HeroService as the provider of its service at the root level so that it can be injected anywhere in the app.
* You used Angular Dependency Injection to inject it into a component.
* You gave the HeroService get data method an asynchronous signature.
* You discovered Observable and the RxJS Observable library.
* You used RxJS of() to return an observable of mock heroes (Observable<Hero[]>).
* The component's ngOnInit lifecycle hook calls the HeroService method, not the constructor.
* You created a MessageService for loosely-coupled communication between classes.
* The HeroService injected into a component is created with another injected service, MessageService.

# Add Navigation With Routing
This section will add:
* Add a Dashboard view
* Add the ability to navigate between the Heroes and Dashboard views.
* When users click a hero name in either view, navigate to a detail view of the selected hero
* When users click a deep link in an email, open the detail view of a particular hero

## Add the AppRoutingModule
The best practice in Angular is to load and configure the router in a separate, top-level module that is dedicated to routing. This module is then imported to the root AppModule.

The conventional name is AppRoutingModule. Generate this module using the CLI:
```
ng generate module app-routing --flat --module=app
```
* Note: Sometime between the tutorial creation and when I did the tutorial, this best practice for routing was integrated into the ```ng new [app-name]``` command. You will get an error about a merge conflict.

Replace the contents of src/app/app-routing.module.ts with:
```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Routes
Routes tell the Router which view to display when a user clicks or pastes a URL into the browser address bar. app-routing.module.ts already imports and uses HeroesComponent here:
```
const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
```

A typical Angular Route has two properties:
* ```path```: a string that matches the URL in the browser address bar.
* ```component```: the component that the router should create when navigating to this route

This tells the router to match the URL to ```path: 'heroes'``` and display the ```HeroesComponent``` when the URL is something like ```localhost:4200/heroes```.

## RouterModule.forRoot()
The ```@NgModule``` metadata initializes the router and starts it listening for browser location changes. The following line adds the ```RouterModule``` to the ```AppRoutingModule``` ```imports``` array and configures it with the ```routes``` in one step by calling ```RouterModule.forRoot()```:
```
imports: [ RouterModule.forRoot(routes) ],
```

Next, AppRoutingModule exports the RouterModule to make it available to the app.
```
exports: [ RouterModule ]
```

## Add RouterOutlet
Open the ```AppComponent``` template and replace the <app-heroes> element with a <router-outlet> element.
```
<h1>{{title}}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

The ```<router-outlet>``` tells the router where to display the routed views.

## Try It
If you are still running ```ng serve``` you may see the app refresh, but no list of heroes is displayed. The address in the browser bar ends in ```/```. The path we have created to the ```HeroesComponent``` is ```/heroes```. 

Append ```/heroes``` to the URL, the app displays the ```HeroesComponent```.

## Add A Naviagtion Link
Ideally, users should click a link to navigate, rather that typing or pasting a route into the address bar.

Add a <nav> element, and an anchor element within that, that triggers navigation to the ```HeroesComponent```. Like this:
```
<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

A ```routerLink``` attribute is set to ```"/heroes"```, the string that matches the route to ```HeroesComponent```. The ```routerLink``` is the selector for the ```RouterLink``` directive that turns user clicks into router navigations. It's another of the public directives in the ```RouterModule```.

Add these CSS styles to ```app.component.css```
```
/* AppComponent's private CSS styles */
h1 {
  margin-bottom: 0;
}
nav a {
  padding: 1rem;
  text-decoration: none;
  margin-top: 10px;
  display: inline-block;
  background-color: #e8e8e8;
  color: #3d3d3d;
  border-radius: 4px;
}

nav a:hover {
  color: white;
  background-color: #42545C;
}
nav a.active {
  background-color: black;
}
```

## Add a Dashboard View
Routing makes the most sense when there are multiple views. So far, there is only the heroes view. Add a ```DashboardComponent``` using the CLI:
```
ng generate component dashboard
```

Replace the content for the created files:
```dashboard.component.html```
```
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes">
    {{hero.name}}
  </a>
</div>
```

```
dashboard.component.ts
```
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```

```
dashboard.component.css
```
```
/* DashboardComponent's private CSS styles */

h2 {
  text-align: center;
}

.heroes-menu {
  padding: 0;
  margin: auto;
  max-width: 1000px;

  /* flexbox */
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: flex-start;
  align-items: flex-start;
}

a {
  background-color: #3f525c;
  border-radius: 2px;
  padding: 1rem;
  font-size: 1.2rem;
  text-decoration: none;
  display: inline-block;
  color: #fff;
  text-align: center;
  width: 100%;
  min-width: 70px;
  margin: .5rem auto;
  box-sizing: border-box;

  /* flexbox */
  order: 0;
  flex: 0 1 auto;
  align-self: auto;
}

@media (min-width: 600px) {
  a {
    width: 18%;
    box-sizing: content-box;
  }
}

a:hover {
  background-color: #000;
}
```

The template presents a grid of hero name links.
* The ```*ngFor``` creates a link for each item in the ```heroes``` array.
* The links are styled by ```dashboard.component.css```
* The links don't go anywhere yet.

The class is similar to the HeroesComponent class
* It defines a ```heroes``` array property
* The constructor expects Angular to inject the ```HeroService``
* The ```ngOnInit()``` lifecycle hook calls ```getHeroes()```

This ```getHeroes()``` returns a sliced list of heroes at positions 1 and 5, returning only heroes

## Add The Dashboard Route
The router needs a route. Import the ```DashboardComponent``` into the ```app-routing.module.ts``` file.
```
import { DashboardComponent } from './dashboard/dashboard.component';
```

Add a route to the routes array:
```
{ path: 'dashboard', component: DashboardComponent },
```

## Add A Default Route
Currently, when the application stats, the browser points to the web site's root. There aren't any routes that match, so the router doesn't navigate anywhere.

To make the application navigate to the dashboard automatically, add the following to the routes array:
```
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
```

## Add A Dashboard Link To The Shell
The user should be able to navigate back and forth between ```DashboardComponent``` and ```HeroesComponent```. Add a dashboard navigation link to the ```AppComponent```:
```
<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

## Navigating to Hero Details
The ```HeroDetailComponent``` is only visible at the bottom of the ```HeroesComponent```.

The user should be able to get to the details in three ways:
1. Clicking a hero in the dashboard
2. Clicking a hero in the heroes list
3. Pasting/Clicking a "deep link"

Only item 2 works right now, so we will liberate the ```HeroDetailComponent``` from the ```HeroesComponent```

## Delete Hero Details from HeroesComponent
Open ```HeroesComponent``` template and delete the ```<app-hero-detail>``` element.

## Add a HeroDetail Route
We want a URL like ~/detail/11 to navigate to the hero with an id of 11.

Import ```HeroDetailComponent``` into ```app-routing.module.ts```:
```
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
```

Add a parameterized route to the ```routes``` array:
```
{ path: 'detail/:id', component: HeroDetailComponent },
```

The colon in the path indicates a placeholder for a specific hero id.

## Add Hero Links to DashboardComponent
The links in the DashboardComponent don't do anything at the moment. Modify the anchor tags in ```dashboard.component.html``` so they navigate to the newly created detail route:
```
<a *ngFor="let hero of heroes"
  routerLink="/detail/{{hero.id}}">
  {{hero.name}}
</a>
```

We are using Angular interpolation binding ```let hero of heroes``` to insert ```hero.id``` into ```routerLink="/detail/{{hero.id}}```

## Update the HeroesComponent links
The navigation in the HeroesComponent happens with the ```<li>``` elements are clicked, with the ```onSelect()``` method.

```
<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```

Remove the content between the ```<li>``` and ```</li>```, replace it with an anchor tag, similar to what was put into the dashboard.
```
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>
```

You'll need to fix the stylesheet for the HeroesComponent, since it didn't have any styles for ```<a>``` elements.

## Remove Dead Code
Inside ```HeroesComponent```, ```onSelect()``` and ```selectedHero``` are no longer used. It's a good practice to clean this up.

```
export class HeroesComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}
```

## Routable HeroDetailComponent
The ```HeroDetailComponent.hero`` property is no longer being set. So we need a new way to obtain the hero-to-display by:
1. Get the route that created it
2. Extract the ```id``` from the route
3. Retrieve the hero with that ```id``` from the ```HeroService```

Add some imports to ```hero-detail.component.ts```
```
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService } from '../hero.service';
```
And inject ```ActivatedRoute```, ```HeroService```, and ```Location``` services into the constructor.
```
`constructor(
  private route: ActivatedRoute,
  private heroService: HeroService,
  private location: Location
) {}
```

The ```ActivatedRoute``` holds information about the route to this instance of the ```HeroDetailComponent```. This component needs to extract parameters from the URL.

The ```HeroService``` gets hero data from the remote server that this compenent will use.

The ```Location``` is an Angular service for interacting with the browser, this will let the user navigate back to the view that they came from.

## Extract The id Route Parameter
In the ```ngOnInit()``` lifecycle hook, call ```getHero()``` like this:
```
ngOnInit(): void {
  this.getHero();
}

getHero(): void {
  const id = Number(this.route.snapshot.paramMap.get('id'));
  this.heroService.getHero(id)
    .subscribe(hero => this.hero = hero);
}
```

The ```route.snapshot``` is a snapshot of the route information after the component was create. From there, ```paramMap``` is a dictionary of route parameter values extracted from the URL. In this case the ```'id'``` key returns the ```id``` of the hero to fetch.

Route parameters are always strings, so the JavaScript ```Number()``` function converts the ```id``` from a string into a number.

* The application crashes after this change because the ```HeroService``` does not have a ```getHero(id)``` method.

## Add HeroService.getHero()
Add this ```getHero()``` medthod into the HeroService
```
getHero(id: number): Observable<Hero> {
  // For now, assume that a hero with the specified `id` always exists.
  // Error handling will be added in the next step of the tutorial.
  const hero = HEROES.find(h => h.id === id)!;
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(hero);
}
```
* Note the backticks (`) used in the message, to use JavaScript template literal for embedding the ```id```;

```getHero()``` also has an asynchronous signature, returning a mock hero as an Observable. This means you are able to re-implement ```getHero()``` without having to change the ```HeroDetailComponent```.

## Find The Way Back
Clicking the browser's back button returns you to whichever view sent you to the hero detail view. We can also implement a button with the same functionality, a go back button.

Add this to the HeroDetail component template:
```
<button (click)="goBack()">go back</button>
```

The app will not run until we add a ```goBack()``` method to the component class. This method uses the browser's history stack by accessing the ```Location``` service that we injected into hero-detail.component.ts previously.

```
goBack(): void {
  this.location.back();
}
```

Add some fresh style to the Hero Detail with:

```
/* HeroDetailComponent's private CSS styles */
label {
  color: #435960;
  font-weight: bold;
}
input {
  font-size: 1em;
  padding: .5rem;
}
button {
  margin-top: 20px;
  background-color: #eee;
  padding: 1rem;
  border-radius: 4px;
  font-size: 1rem;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}
```

## Summary
* You added the Angular router to navigate among different components.
* You turned the AppComponent into a navigation shell with <a> links and a <router-outlet>.
* You configured the router in an AppRoutingModule
* You defined routes, a redirect route, and a parameterized route.
* You used the routerLink directive in anchor elements.
* You refactored a tightly-coupled master/detail view into a routed detail view.
* You used router link parameters to navigate to the detail view of a user-selected hero.
* You shared the HeroService among multiple components.