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