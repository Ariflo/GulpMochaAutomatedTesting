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
npm i  gulp mocha gulp-mocha gulp-util supertest -g
npm i  gulp mocha gulp-mocha gulp-util supertest --save
```
For those inquiring: 

- [`Gulp`](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) is a build system that will assist in running the test code. I like to think of it as a task manager that can watch for file changes and trigger tests automatically.

- [`Mocha`](https://mochajs.org/)is a test framework that will provide the instruments needed to run the actual test. Popular alternative to Mocha is Jasmine. You can learn more about the diffrence between two [here](http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html)

- [`Supertest`](https://www.npmjs.com/package/supertest) The motivation with this module is to provide a high-level of abstraction for testing HTTP (ie.routes), while still allowing you to drop down to the lower-level API provided by super-agent. In layman terms, "no crossing swords" :), or ports in this case. You can test your express app's routes and not have to worry about whether the app is running or not. It truly is super!    

##Step 2: Create a Test folder and test file

This folder will hold all of your test.js files

```
mkdir test
cd test 
touch firstTest.js 
```
##Step 3: Create and build your gulpfile.js file

You'll want to create a gulpfile.js outside of the 
```
touch gulpfile.js
```




