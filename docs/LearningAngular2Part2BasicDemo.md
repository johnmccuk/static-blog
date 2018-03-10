06/07/2016

<i class="fa fa-github" aria-hidden="true"></i> Files are available from the <a href="https://github.com/johnmccuk/arrival-board" target="_blank">github repository</a>.

<i class="fa fa-file-code-o" aria-hidden="true"></i> The demo is available <a href="https://johnmccuk.github.io/arrival-board/" target="_blank">here</a>.

This is a demo which I used to learn the basic features of Angular2, something which want the easiest task, as there is a lot of different concepts and technologies to learn. See the [previous article](/learning-angular2-part-1-setup) for setting up the basic site using angular-cli.

The demo is an airport arrival board. When you add a flight, each flight is generated a random flight number and distance. The arrival board keeps track of the distance away each flight is, and how long it will take to land (I've substituted hours for seconds, as I assume your not that keen on realism). I've used a 747 average cruise speed (520 mph) to calculate these times. The arrival board automatically orders the flights on arrival time (closest first) and marks them green on landing.

Before we go much further, some Angular2 terminology:

* **Component** - class with a html view. Angular2 requires at least one of these.
* **Service** - singleton class which can be used to share logic over multiple Components.
* **Pipe** - class used to transform output, for example to format a date, set a string to uppercase.

I decided I needed the following design:

* **appComponent** - The base component that everything else must originate from (angular-cli creates this automatically).
* **FlightsComponent** - holds the arrival board template and controller logic needed to connect to the imported service.
* **FlightService** - Service that holds all the actual logic and data.
* **Flight** - Class and logic for each individual flight.

so to create this, I used the following commands:

```
ng generate component flights
ng generate directive flight
ng generate class flight
```

I also created a facade to hold imports called **imports-faucade.ts**. Rather than have multiple file paths all over the place, add them all in this file and import them from a centralised place. This organisational method is called Barrels apparently. In the context of this demo, its pretty useless, but it always helps to design in a scalable manner.

Each Component generated using angular-cli has 4 files:

* **component-name.css** - encapsulated css file for that the component (and child components)
* **component-name.html** - the html template
* **component-name.ts** - typescript class
* **component-name.spec.ts** - unit test class

Angular2 uses ES7 experimental feature called Decorators, this is something I was aware of vaguely from Java, but had never really used them. Thankfully all you really need to know at the basic level is that its used to add extra information to a Class or Method. As an example Angular2 uses this to add meta data to a Component class to specify the view and styles and required Services and Pipes.

```
@Component({
  moduleId: module.id,
  selector: 'app-flights',
  templateUrl: 'flights.component.html',
  styleUrls: ['flights.component.css'],
  providers: [FlightsService]
})
export class FlightsComponent implements OnInit {
    //specific class methods go here
}
```

Use **ng serve** to setup a browser refresh instance, so that your changes are automatically updated. Again, another pretty cool feature you get for free with angular-cli.

You can also run **ng test** to setup an instance of the Karma test runner with the default Jasmine unit tests. Along with the automatic test unit file creations, there really isn’t any reason not to attempt test driven development.

The code can be viewed from the Github repository and should be relatively easy to follow. All files are in the ***src/app*** directory. I’ll describe the general gist of things:

* When you click the ‘add random flight’ button,
* When you click the button, you access the **FlightsComponent** add method.
* This generates a random flight name and distance.
* It then passes these values to the **FlightService** ***addFlight*** method.
* It then adds an instance of the **Flight** class to the ***currentFlights*** array stored within the *FlightService*.

The constructor of each **Flight** class starts a timer, which subtracts the average speed of a 747 (520 mph) from the distance. The **FlightsComponent** basically just relays this information to the html view.

Pretty simple, but it demonstrates the Angular2 framework and dependancy injection.

I’m still not sold on Angular2, its relatively nice to use, but I really found it frustrating that no 2 tutorials/books did a task the same way. This led to a fair bit of confusion. I also found I really had to use Typescript to have any chance of learning the framework, luckily I really liked it and may use it more in other projects. I also liked Visual Studio Code (I used this for its Typescript features), which was a surprise since its made by Microsoft. I’m also not sure how viable Angular2 will be to integrate into an existing project as it seems to be focussed more on a single page app project.

Again, angular-cli makes life so easy, using `ng github-pages:deploy`, I deployed the demo as a single page app to Github in a single command, lovely!

## Future updates:

* Might add a form so custom flight names and distances can be added.
* might add cities to the form and an api call to work out the distance between the cities.
* Add randomised delays.

<i class="fa fa-github" aria-hidden="true"></i> Files are available from the <a href="https://github.com/johnmccuk/arrival-board" target="_blank">github repository</a>.

<i class="fa fa-file-code-o" aria-hidden="true"></i> The demo is available <a href="https://johnmccuk.github.io/arrival-board/" target="_blank">here</a>.

