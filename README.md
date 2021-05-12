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

Create a file ```mock-heroes.ts``` in ```src/app/``` and Define a ```HEROES``` constant as an array and export it.
