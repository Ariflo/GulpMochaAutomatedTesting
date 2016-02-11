## Automated Testing with Gulp-Mocha

This repository will outline the steps necessary to set-up an automated Gulp-Mocha testing suite for your SEAN (SQL with Knex, Express, Angular, Node) web app. 

###File Structure
This demo assumes your SEAN app follows the standard MVC file structure:

```
$tree
├── bin
├── db
├── node_modules
├── models
├── public
│   ├── css
│   ├── partials
│   ├── templates
│   └── js
│       └── main.js
│       └── services.js
│       └── directives.js
│       └── controllers.js
│       └── filters.js
├── routes (controller)
│   └── index.js
└── views
│	 ├── error.ejs
│	 └── index.ejs
├── app.js
├── gulpfile.js
├── knexfile.js
├── package.json

```

Type `yo galvanize-fullstack` into the commandline to get a basic skelton like the above file structure. [See the npm details here](https://www.npmjs.com/package/generator-galvanize-fullstack)

##Step 1: Install the Gulp Test Suite

You will need to enter the following into your terminal command line

```
$ npm i  gulp mocha chai gulp-mocha gulp-util supertest supertest-session -g
$ npm i  gulp mocha chai gulp-mocha gulp-util supertest supertest-session --save
```
For those inquiring: 

- [`Gulp`](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) is a build system that will assist in running the test code. I like to think of it as a task manager that can watch for file changes and trigger tests automatically.

- [`Mocha`](https://mochajs.org/)is a test framework that will provide the instruments needed to run the actual test. Popular alternative to Mocha is Jasmine. You can learn more about the diffrence between two [here](http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html)

- [`Supertest`](https://www.npmjs.com/package/supertest) The motivation with this module is to provide a high-level of abstraction for testing HTTP (ie.routes), while still allowing you to drop down to the lower-level API provided by super-agent. In layman terms, "no crossing swords" :), or ports in this case. You can test your express app's routes and not have to worry about whether the app is running or not. It truly is super!    


##Step 2: Create and Build your gulpfile.js file

You'll want to create a gulpfile.js outside of the test folder if you don't have it already
 
```
$ touch gulpfile.js
```

Now, you'll need to make sure to require the gulp build system you installed earlier at the top of the gulpfile:

```
var gulp = require('gulp');
var mocha = require('gulp-mocha');
var util = require('gulp-util');

```

Alright, now its task time, like I mentioned before [Gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) is at its heart a task runner that wants to excute a list of commands you give it to do. 

With this in mind, lets add a couple tasks to our gulpfile:

```
/**
 * Gulp Tasks
 */
 
gulp.task('test', function () {
    return gulp.src(['test/**/*.js'], { read: false })
        .pipe(mocha({ reporter: 'spec' }))
        .on('error', util.log);
});
 
gulp.task('watch-test', function () {
    gulp.watch(['views/**', 'public/**', 'app.js', 'routes/**', 'test/**'], ['test']);
});

// *** default task *** //
gulp.task('default', ['test', 'watch-test'], function(){});
```

Great! Now when the `$ gulp` command is entered into your terminal, it will search for the ‘default’ task and execute whatever task is provided, in this case we send it to run our two tasks *test* and *watch-test*.

- **'test'** has the duty of actually running our mocha tests. Using *'gulp.src'* it will search through our test folder and run any files inside. It'll run them under the standard 'spec' hierarchical view and log any errors.   


- **'watch-test'** has the duty of checking for changes to any of the specifed folders or files in the app. As soon as 'gulp.watch' sees a change to any of the files it will automatically run the 'test' task and report the results.

Check out the [npm page](https://www.npmjs.com/package/gulp-mocha) for all the cool stuff you can do with gulp-mocha. 

##Step 3: Create a Test folder and and build your Test file

Alright, now that you've got the tasker ready to run tasks you'll need to set-up tasks for it to run (trying saying that 3x fast)


Lets create our test folder where we'll keep all of your test.js files

```
$ mkdir test
$ cd test 
```

Now, while inside the test folder in your terminal create a test file `$ touch firstTest.js` 

At the very top of your test file you should establish the port and enviorment

```
process.env.PORT = 5000;
process.env.NODE_ENV = 'test';
```

Make sure to set the port to something other than whats been set for development in app.js. This way your development enviorment won't compete for ports being used by the testing enviorment.








 





