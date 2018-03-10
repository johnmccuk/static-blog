24/06/2016

I won't pretend to be a Javascript expert. I know enough to do what I need to do, and I'm relatively comfortable with AngularJS 1 and the general quirks of Javascript.

I do tend to get slightly unsettled when things move onto NodeJs, NPM, EC6 however. I just find Javascript so fast moving, you cant turn your back on it for a week and some new fangled thing has appeared. New ways to do things are created seemingly,  just for the sake of it.

So it was with some trepidation that I decided I needed to get with the cool kids and attempt to keep up to date with Javascript...

## Setup

Install <a href="https://nodejs.org/en/download/" target="_blank">Nodejs</a>, Mac and Windows installers are available. Linux users will use the default package manager I would imagine.

I initially started by following several guides and installing components individually as required. My plan was to setup a repository and use that as a base for all future projects, heres the <a href="https://github.com/johnmccuk/base-angular-setup" target="_blank">link</a> to the repository. As is normal, halfway through this it entered my mind that theres probably a better way to do this and thats someones already solved this problem in a better way.

**Note to Self**: Other people have the same problems as me, have solved them, probably in a better way. Don't reinvent the wheel.

After a bit of searching, I came across <a href="https://cli.angular.io/" target="_blank">Angular CLI</a>.

```
#install angular CLI globally
npm install -g angular-cli
```

```
#create new project and navigate to it
ng new angular2-test-folder
cd angular2-test-folder
```

lets see if its works

```
ng serve
```

If you now visit ***localhost:4200*** in your browser, a basic angular2 website should be waiting.

If it doesn't work? Walk away, cursing everything Javascript and vow to never touch the Devils code again.

## What do we get for all that hard work?

We now have a good foundation for playing around with Angular2, using the following commands we can generate everything we need

```
#a new component 
ng generate component new-component-name

#a new directive
ng generate directive new-directive-name

#a new service
ng generate service new-service-name
```

We also have a full test environment

```
ng test
```

## And Finally
We now have everything we need, now the hard part of working out what the hells going on!