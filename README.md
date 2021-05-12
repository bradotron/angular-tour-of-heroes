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

# Make Some Changes
navigate to src/app

## Component Files
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


