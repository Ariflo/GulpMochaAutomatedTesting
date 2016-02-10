## Automated Testing with Gulp-Mocha

This repository will outline the steps necessary to set-up an automated Gulp-Mocha testing suite for your SEAN (SQL with Knex, Express, Angular, Node) web app. 

###File Structure
This demo assumes your SEAN app follows the standard MVC file structure:

```
$tree
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
├── test
│   └── dashboard.test.js
└── views
│	 ├── error.ejs
│	 └── index.ejs
├── .gitignore
├── package.json
├── knexfile.js
├── gulpfile.js
├── server.js
 
```


