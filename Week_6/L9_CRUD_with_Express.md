# Lecture 9: CRUD with Express

### What is Express?
* It is a Node.js framework for building HTTP.... ********

### Review: Client vs. Server
Client (Web Browser)
* contains HTML, CSS, & JS
* sends a request, receives response

Server (Web Server)
* contains an operating system, web server software, **web applications / scripts**, and a database
* receives the request, sends a response

*Where is Express in all of this?* &rarr; It is contained in the **web applications / scripts** portion of the server, where Node.js script runs Express functions.

### CRUD
* CREATE
* READ
* UPDATE
* DELETE

Anytime we're dealing with a resource (blog post, video, user profile), we should consider which of CRUD are appropriate.

Sometimes you see iCRUDES, which is the same four above plus:
* INDEX
* EDIT
* SAVE

Sometimes you see BREAD:
* BROWSE
* READ
* EDIT
* ADD
* DELETE

### Routes
GET -
  * Can be easily shared or bookmarked
  * Can be easily repeated

```
<form method="GET" action"/search">
  <input name = "q">
// htttps://google.com/search?q=JavaScript+MDN
```

POST -
  * Submits data in the body of your request
  * Not easily repeatable

```
<form method="POST" action="/sign-in">
  <input name="username">
  <input name="password">
```

Question to ask yourself: do you want the form submission results to be seen in the address bar?
* If YES, use GET
* If NO, use POST

iCRUDES
```
CRUD    | Method   | Path               | Purpose

======================================================================

CREATE  | GET      | /pets/new          | Display the "new pet" form
SAVE    | POST     | /pets              | Submit the "new pet" form   

INDEX   | GET      | /pets              | Display all pets
READ    | GET      | /pets/:key         | Show a single pet

UPDATE  | POST     | /pets/:key         | Submit the "edit pet" form
EDIT    | GET      | /pets/:key/edit    | Display the "edit pet" form

DELETE  | POST     | /pets/:key/delete  | Submit the "delete pet" form
// DELETE doesn't usually get its own page; usually found on the edit page instead
```

*TIP: For there to be overlap, the method AND the path need to be the same. You can have two iCRUDES going to the same path, but as long as they have different methods, you're good!

### EJS
* A back-end NPM package that allows us to format a string
  * If you want to use this alongside Express, you need to let Express know you want to use this particular template engine (using app.set)
  * Any .ejs files are subcategorized into a new directory called 'Views'

### Express + EJS Example
* npm i express
* npm i ejs
* npm i morgan

#### express-server.js
```js
// Configuration
const express = require('express');
const morgan = require('morgan');

const app = express();
const PORT = 8080;
app.set('view engine', 'ejs'); // Sets template engine to EJS

// Database
const pets = {}; // Imagine object of pets!

// Middleware
app.use(morgan('dev')); // Logs requests in our terminal
app.use(express.urlencoded({extended: true})); // Parses url-encoded data in requests

// Listener
app.listen(PORT, () => {
  console.log('Server is running on port:', PORT);
});


// INDEX: display all pets
app.get('/pets', (req, res) => {
  const templateVariables = { pets: pets };
  res.render('pets/index'); // Always looks in /views, so specify path starting from there
});

// SHOW: show single pet
app.get('/pets/:key/', (req, res) => { // :key is a key-value pair included in the req.params object
  const templateVariables = { pet: pets[req.params.key] };
  res.render('pets/show', templateVariables);
});

// CREATE: show create pet form
app.get('/pets/new', (req, res) => {
  res.render('pets/new');
});

// SAVE: submit create pet form
app.post('/pets', (req, res) => {
  const petInfo = req.body; // req.body is where we will find the information submitted by the form
  pets[petInfo.name] = petInfo; 
});
```

#### views/pets/index.ejs
```html
... <!-- Boilerplate -->
<body>
  <h1>Pets Index</h1>
  <ul>
    <% for (const petName in pets) { %> <!-- EJS can run JS in-between <% %> tags -->
      <li>
        <a href = "pets/<%= petName %>">
          <%= petName %>  <!-- '=' prints result of this expression -->
        </a>
      </li> 
    <% } %> <!-- Optional: comment saying what you are closing; in this case, for..in pets -->
</body>
```

#### views/pets/show.ejs
* Often will use the name 'show' for one individual page from the index
```html
... <!-- Boilerplate -->
<body>
  <h1>Show Pet</h1>
  <dl>
    <dt>Name</dt><dd><%= pet.name %></dd>
    <dt>Age</dt><dd><%= pet.age %></dd>
    <dt>Type</dt><dd><%= pet.type %></dd>
  </dl>
  <nav>
    <a href="/pets">Return to Pets Index</a>
  </nav>
</body>
```

#### views/pets/new.ejs
```html
... <!-- Boilerplate -->
<body>
  <h1>Create New Pet</h1>
  <form method="POST" action="/pets">
    <label> Name:
      <input name="name">
    </label>
    <label> Age:
      <input name="age">
    </label>
    <label> Type:
      <input name="type">
    </label>
    <input type="submit" value="Create New Pet"> <!-- Can create a submit button via input as well! -->
  </form>
</body>
```