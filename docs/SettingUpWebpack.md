19/10/2016

I've found setting up a development environment is a major hurdle to learning any of the modern javascript technologies or frameworks. The concepts and code are hard enough to learn, but the number of external libraries etc required just compound the learning curve. I'm aware React and Angular 2 (<a href="https://blog.john-mccracken.com/learning-angular2-part-1-setup/" target="_blank">Learning Angular2 Part 1: Setup</a>) have ready made environments which can be up and running from one command. This leads to the problem of never actually knowing whats going on 'behind the curtain'. And when the inevitable problem occurs, your screwed.

So I'll create a quick guide on setting up a quick environment that will:

* Allow us to write ES6 Javascript by using <a href="https://babeljs.io/" target="_blank">Babel.js</a>.
* Bundle all our source files using <a href="https://webpack.github.io" target="_blank">Webpack</a>.
* Minify our Javascript
* Watch for file changes
* Create task runners to make life easier

The only prerequisite for this is that you have <a href="https://nodejs.org/en/" target="_blank">Node.js</a> installed and are using a decent operating system (basically, not Windows).

Unless stated, any commands should be from the root of the webpage folder.

If you can't be arsed doing any typing, all code is available on <a href="https://github.com/johnmccuk/webpack-tutorial" target="_blank">GitHub</a>.

Begin by creating a folder and navigating into it. Then run the following command from the command line to setup npm.

`npm init`

Accept all defaults and you should have a new ==package.json== file. This is our Node config file, all installed modules will be listed in here.

Next we need to install Webpack itself.

`npm install webpack --save`

You should see some warnings which you will become accustomed to, npm does like to grumble. As long as there are no errors, you should see a tree of installed libraries and dependancies. If you look inside the package.json file, you should a dependency entry for Webpack.

We should now setup a config file for Webpack called ==webpack.config.js==. Add the following text.

```
//Define what libraries we need
var webpack = require('webpack');
var path = require('path');

//setup the i/o paths
var OUTPUT_DIR = path.resolve(__dirname, 'output');
var INPUT_DIR = path.resolve(__dirname, 'input');

//create a config object
var config = {
  entry: INPUT_DIR + '/start.js',
  output: {
    path: OUTPUT_DIR,
    filename: 'finish.js'
  }
};

//finally make the config object available elsewhere
module.exports = config;
```

It should be fairly obvious from looking over the above config that were taking a javascript file called ==start.js== in the input directory, which will then be compiled to ==finish.js== in the output directory.

So to begin with we need to create input and output directories. We should also create a ==start.js== file within the input directory, write the following into it: 

`alert('hello world');`. 

You could replace the `alert()` with `console.log` if your feeling that way inclined.

We'll also need a main html page to see if all this is working, so in the main folder create ==index.html== and add the following:

```
<html>
  <head>
    <meta charset="utf-8">
    <title>Webpack Setup Test</title>
  </head>
  <body>
    <script src="output/finish.js" type="text/javascript"></script>
  </body>
</html>
```

We are now ready to see the fruits of our effort. To find out if it works, run the following command.

`./node_modules/.bin/webpack`

You should see something like the following:

```
Hash: 3dc70f64d3e175c2bcef
Version: webpack 1.13.3
Time: 44ms
    Asset     Size  Chunks             Chunk Names
finish.js  1.41 kB       0  [emitted]  main
   [0] ./input/start.js 21 bytes {0} [built]
```

To make sure all's well, open the ==index.html== file in your browser (double clicking it is probably the easiest way), you should get a 'hello world' popup message.

And if you check in the output folder, there should now be a ==finish.js== file. This file contains lots of additional code we didn't actually type. The good thing is, we don't need to understand it, we can assume Webpack has worked its magical goodness on it and it all just works.

Right now, its not really doing a great deal other than taking our Javascript, adding lots of extra code and creating a new file. This now allows to do clever things to our code, we can now write shiny new ES6 javascript with all its new features, and all these features will work in the browser even if its not ES6 compatible. We use the compiler Babel.js to achieve this.

##Source Maps
If we want to add source maps to our output, run the following 

`./node_modules/.bin/webpack -d`

A source map should now have been created called ==finish.js.map==. This does have the disadvantage of taking longer for Webpack to package and output our specified files.

##ES6

`npm install babel-loader babel-preset-es2015 --save`

Again lots of warnings, but if theres no errors, who cares! We'll need a config file for Babel also, so create ==.babelrc== and add the following:

```
{
  "presets" : ["es2015"]
}
```

We next need to inform Webpack to process our code using babel, so in ==webpack.config.js== add a module object to the config object

```
//create a config object
var config = {
  entry: INPUT_DIR + '/start.js',

//*** new code starts here
  module : {
    loaders : [
      {
        test : /\.js/, //look for js files
        include : INPUT_DIR, //in this directory
        loader : 'babel' //run the babel loader
      }
    ]
  },
//*** new code ends here
  output: {
    path: OUTPUT_DIR,
    filename: 'finish.js'
  }
};
```

The loaders section is an array of modules that we can run our files through, if the file matches the test condition, we can process it through the module.

Edit the ==start.js== file and replace the content with this:

```
//constants are an ES6 feature and wont run in plain old js
const PI = 3.141593;
alert(PI);
```

now run Webpack to see if it works

`./node_modules/.bin/webpack`

Open ==index.html== in your browser, you should have an annoying alert notifying you of the value of Pi!

If you look at the ==finish.js== file, at the very end you'll see:

```
"use strict";

//constants are an ES6 feature and wont run in plain old js
var PI = 3.141593;
alert(PI);
```

From this we can see that Babel has converted our code into bog standard Javascript, but we can still get the benefits of ES6 while coding, nice!

##Minify
Lets now minify/uglify this code. This is a builtin plugin for Webpack, so we just need to edit ==webpack.config.js== to tell Webpack to use it. Update the config object to the following:

```
//create a config object
var config = {
    entry: INPUT_DIR + '/start.js',
//*** new code starts here
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            },
            sourceMap: true
        })
    ],
//*** new code ends here
    module: {
        loaders: [
            {
                test: /\.js/, //look for js files
                include: INPUT_DIR, //in this directory
                loader: 'babel' //run the babel loader
            }
        ]
    },
    output: {
        path: OUTPUT_DIR,
        filename: 'finish.js'
    }
};
```

Again run `./node_modules/.bin/webpack` and check all works. If we look at the ==finish.js== file we can see its now been minifed.

##Watch for Changes
So we now have a reasonably straightforward process of taking our code and manipulating it dependant on the plugins/loaders used. This is brilliant, but it would be nicer to have it done automatically rather than having to run a command every time. To achieve this we just need to add the --watch parameter:

`./node_modules/.bin/webpack --watch`

If we now edit the ==start.js== file to the following.

```
//constants are an ES6 feature and wont run in plain old js
const PI = 3.141593;
alert("Value is "+ PI);
```

When you hit save, you should see on the command line that Webpack is recompiling the code.

To cancel the watch process hit `ctrl+c`.

##Task Runners

We can also use npm as a task runner, this makes life easier since we don't need to remember any complex command lines. Edit the ==package.json== file

```
...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "watch": "./node_modules/.bin/webpack --watch",
    "build": "./node_modules/.bin/webpack"
  },
...
```

now we can run the following commands `npm run build` and `npm run watch` rather than the full commands.

#Development Server

`npm install webpack-dev-server --save`

This installs our development server, blissfully ignore the normal npm warnings. Now edit the ==package.json== file and add another task runner called 'server'.

```
...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "watch": "./node_modules/.bin/webpack --watch",
    "build": "./node_modules/.bin/webpack -d",
    "server": "webpack-dev-server --devtool eval --inline --hot"
  },
...
```

Now see if it works by running the task runner.

`npm run server`

Among the normal information outputted should be a url from which you can now view the site.

`http://localhost:8080/webpack-dev-server/`

#Notes
Obviously this tutorial is very rudimentary but it shows how easy it is to setup and use Webpack. 

I'm aware the used npm packages should be technically be saved using `--save-dev`, not sure why I didn't, but I can  live with it.

At some point soon I'll update this tutorial with: 

* ~~Setup a dev server.~~
* ~~Creating Source maps.~~
* Loading less/css and image files.
* Code splitting into separate files.

I've created this tutorial as a learning aid to try and reinforce what I've learned. The excellent original tutorial I used was <a href="https://www.codementor.io/tamizhvendan/tutorials/beginner-guide-setup-reactjs-environment-npm-babel-6-webpack-du107r9zr" target="_blank">Setting Up a React.js Environment Using Npm, Babel 6 and Webpack</a>.

All code is available on <a href="https://github.com/johnmccuk/webpack-tutorial" target="_blank">GitHub</a>.