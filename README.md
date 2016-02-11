
##<center>How to get a Life in 3 Easy steps!</center>
<center>Do you feel like every development project you undertake is another chunk of your life being sucked into the black hole that is your laptop? 

Do you suffer from PTDD (Post-Traumatic Deployment Disorder) and just can't seem to walk away from a computer screen, thinking about that inevitable post-deployment crash to come. 

Well then I've got 4 words for you: </center>

<center>![calm](http://sd.keepcalm-o-matic.co.uk/i/keep-calm-and-tdd-1.png)</center>

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

If not, feel free to type `yo galvanize-fullstack` into your terminal's command line to get a basic SEAN app skeleton. [See the npm details here](https://www.npmjs.com/package/generator-galvanize-fullstack)

##Step 1: Install the Gulp Test Suite

You will first need to enter the following into your terminal command line

```
$ npm i  gulp mocha chai gulp-mocha gulp-util supertest supertest-session -g
$ npm i  gulp mocha chai gulp-mocha gulp-util supertest supertest-session --save
```

For the inquisitive among us: 

- [`Gulp`](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) is a build system that will assist in running the test code. I like to think of it as a task manager that can watch for file changes and trigger tests automatically.

- [`Mocha`](https://mochajs.org/)is a test framework that will provide the instruments needed to run the actual test. A popular alternative to Mocha is Jasmine. You can learn more about the diffrence between the two [here](http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html).

- [`Chai`](http://chaijs.com/) is a BDD / TDD assertion library for node and the browser that, as noted on the site, can be delightfully paired with any javascript testing framework. Not sure how something can be 'delightfully paired' but it certainly helps to have chai around when building your actual tests.

- [`Supertest`](https://www.npmjs.com/package/supertest) is a  module that can provide a high-level of abstraction for testing HTTP (ie.routes), while still allowing you to drop down to the lower-level API provided by super-agent. In layman terms, no crossing ports! You can test your express app's routes and not have to worry about whether the app is running or not. It truly is super!    


##Step 2: Create and Build your gulpfile.js

You'll want to create a gulpfile.js file outside of the test folder if you don't have it already
 
```
$ touch gulpfile.js
```

Now, you'll need to make sure to require the gulp build system you installed earlier at the top of the gulpfile:

```
var gulp = require('gulp');
var mocha = require('gulp-mocha');
var util = require('gulp-util');

```

###<center> Its task time! </center>

Like I mentioned before [`gulp`](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) is essentially a task runner that wants to excute a list of commands you've provided for it to do. 

With this in mind, lets add a couple of tasks to our gulpfile:

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

Great! Now when you run `$ gulp`, it will excecute the ‘default’ tasks provided, which in this case is to simultaneously run our two other tasks *test* and *watch-test*.

- **'test'** has the duty of actually running our mocha tests. Using *'gulp.src'* it will search through our test folder and run any files inside. It'll run them under the standard 'spec' hierarchical view and log any errors.   


- **'watch-test'** has the duty of checking for changes to any of the specifed folders or files in the app. As soon as 'gulp.watch' sees a change to any of the files it will automatically run the 'test' task and report the results in the terminal.

<center> Check out the [ gulp-mocha npm page](https://www.npmjs.com/package/gulp-mocha) to learn more about the cool stuff you can do. </center>

##Step 3: Create a Test Folder and and Build your Test File

Alright, now that you've got the tasker ready to run tasks you'll need to set-up tasks for it to run (trying saying THAT 3x fast!)


Lets create our test folder where we'll keep all of our test.js files

```
$ mkdir test
$ cd test 
```

While inside the test folder in your terminal create a test file: 
`$ touch firstTest.js` 

At the very top of your test file you should establish the port and enviorment

```
process.env.PORT = 5000;
process.env.NODE_ENV = 'test';
```

Make sure to set the port to something other than what's been set for development in your app.js (server) file.

I cannot re-emphasize this enough: 
**<center>Make sure to set the port to something other than what's been set up for development!!!</center>**

This way you can gurantee the development and testing enviorments won't compete for ports.

Next, you will need to bring in a few requirements:

```
var request = require('supertest')
	, app = require('../app')
	, expect = require('chai').expect
	, session = require('supertest-session');
```
As I mentioned before, [`supertest`](https://www.npmjs.com/package/supertest) is our main testing framework. In order to properly send routes to your sever you will need to also require the server in the test file (in this case app.js).[`Chai`](http://chaijs.com/)gives us the ability to use 'expect' (see my sample code below) and finally [`Supertest`](https://www.npmjs.com/package/supertest) provides a handy cookie-based session persistence tool called [`supertest-session`](https://www.npmjs.com/package/supertest-session) to envolpe our app (ie. server). 

It isn't particularly mandatory, but when starting out I would set-up a beforeEach where you assign a variable to the session. It cleans up the code a little and makes it easier to understand what your test is doing.  

```
beforeEach(function () {
  testSession = session(app);
})
```

**<center>Congrats! You are offcially ready to build your tests! </center>**

In the sample that I've attached to this repository I test to see that my app is able to load the dashboard (homepage) successfully:

```
describe('this test', function(){

	it("should load the dashboard", function(done){
		testSession.get('/')
		.expect(200)
		.end(function(err, res){
			if(err){
				return done(err);
			}
			expect(res.text).to.contain('Galvanize');
			done();
		})

	})

})
```

##Welcome to the Wonderful World of Test Driven Developement (TDD)

Now that everything is in place, feel free to run your development enivorment sever using `nodemon` in one terminal window. While on another window simply type `gulp` into your command line and hit enter. 

<center> You should now feel the weight of the world lift from your shoulders. </center>

<center>![over](https://s-media-cache-ak0.pinimg.com/564x/9b/56/80/9b56805ffca1fa1a144ed1acc8dbb84d.jpg)</center>


When you return to your code, you can make any and all necessary changes, save your code, and simply look at your terminal window and hope to see those magnificant beautiful green checkmarks assuring you that it's ok to walk away from the laptop. You're free!  

<center>[More on writing test in mocha](https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-mocha)  
[More on writing test in jasmine](http://jasmine.github.io/2.0/introduction.html)</center>









 





