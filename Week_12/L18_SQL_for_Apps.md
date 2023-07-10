# Lecture 18: SQL for Apps

client <====tcp/http====> server <====tcp/postgres====> rdbms (database)

* This is what's known as the "full-stack" &rarr; client, server, database

Today we will be using [ElephantSQL](https://www.elephantsql.com/), which is free to use for small databases. The benefit of using this is it is a cloud option, which won't take up space on your RAM like running psql in the terminal will.

### Setting up Postgress
See the [Node Postgress documentation](https://node-postgres.com/) for full instruction.
* npm init -y
* npm i pg

db/connection.js
```js
const pg = require('pg');

// Use either of the two options depending on your needs
// The code you write will be identical, no matter which one you pick
const Client = pg.Client; // single connection to the database (low # of requests)
const Pool = pg.Pool; // collection of clients (high # of requests, 100k+), default 5 clients, managed

// We will sub this with environmental variables later (see below) as we wouldn't want to push our code like this onto Github!!!
const config = {
  port: 5432, // default for Postgress
  host: 'rajje.db.elephantsql.com',
  database: 'labber',
  user: 'labber',
  password: 'abc123' 
};

// Postgress Connection String - note how it lines up with the info in config!
postgress://labber:abc123@rajje.db.elephantsql.com/labber

const client = new Client(config);
client.connect();

module.exports = client;
```

### Perform BREAD Actions on Database via Command Line App
index.js
```js
const client = require('./db/connection');

const verb = process.argv[2];
const id = process.argv[3];

switch(verb) {
  case 'browse':
    client.query('SELECT * FROM movie_villains') // Semi-colons at the end of queries are optional as it will still run without it (unless running multiple queries and need to distinguish between them)
      .then((response) => {
        console.log(response.rows); // An array of objects containing the data (note that this only works as intended for SELECT, as it has a return value)
        client.end(); // No more CTRL-C - it will end as soon as the result comes back
      });
    break;

  case 'read':
    client.query('SELECT * FROM movie_villains WHERE id = $1', [id]); // SQL starts at 1 in arrays, not 0
      .then((response) => {
        console.log(response.rows[0]); // This just gives us the object, not the array of one object
        client.end();
      });
    break;

  case 'edit':
    const newVillainName = process.argv[4];
    const editQuery = 'UPDATE movie_villains SET villain = $1 WHERE id = $2';
    client.query(editQuery, [newVillainName, id]);
      .then(() => {
        // Cannot use console.log(response.rows), as this will just return an empty array as UPDATE has no return value
        console.log("Villain updated successfully.");
        client.end();
      });
    break;

  case 'add':
    const villain = process.argv[3];
    const movie = process.argv[4];
    const addQuery = 'INSERT INTO movie_villains (villain, move) VALUES ($1, $2)';
    client.query(addQuery, [villain, movie]);
      .then(() => {
        console.log(`Disney+ is creating a solo series for ${villain}.`)
        client.end();
      })
    break;

  case 'delete':
    client.query('DELETE FROM movie_villains WHERE id = $1', [id]);
      .then(() => {
        console.log('Villain was defeated.')
        client.end();
      });
    break;
  
  // Can use default at the end of switch statement as a catch-all for anything not specified
  default:
    console.log('Please use a valid BREAD verb.');
    client.end();
}
```

### Hacking a Database: SQL Injection
* SQL injection is the **NUMBER ONE WAY** websites get hacked!
```js
client.query(`SELECT * FROM movie_villains WHERE id = ${id}`); // BAD! String interpolation is very easy to hack!
```
* node index.js "2; DROP TABLE movie_villains;" &rarr; while annoying, this is not even close to the most malicious thing a hacker could do!

### Serve Database Content to the Browser
* npm init -y
* npm i express morgan pg

server.js
```js
const express = require('express');
const morgan = require('morgan');
const client = require('./db/connection');

const app = express();
const port = process.env.PORT || 54321; // will default to 54321 if the port wasn't set
// Ex. $ PORT=3333 node server.js -> will set PORT to 3333 before it runs

app.use(morgan('dev'));

// GET /villains
app.get('/villains', (req, res) => {
  client.query('SELECT * FROM movie_villains ORDER BY id;')
    .then((response) => {
      const villains = response.rows;
      res.json(villains); // JSON stringifies & sends to browser
    })
});

// GET /villains/:id
app.get('/villains/:id', (req, res) => {
  const villainId = req.params.id;
  client.query('SELECT * FROM movie_villains WHERE id = $1;', [villainId])
    .then((response) => {
      const villain = response.rows[0];
      res.json(villain);
    });
});

app.listen(port, () => {
  console.log(`App is listening on port ${port}`);
});
```

### Protecting Data with Environmental Variables
* $ SECRET_KEY="something" node &rarr; put the key in before starting node

* Use the [dotenv package](https://www.npmjs.com/package/dotenv) to load environment variables from a .env file (no longer have to list in the command line!)
  * npm i dotenv

db/connection.js
```js
// Update the config with environmental variables
const config = {
  port: process.env.DB_PORT,
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASS
};
```
* All of these name (DB_PORT, DB_HOST) are arbitrary! It's whatever you decide to call them.

.env
```
DB_PORT=5432
DB_HOST=rajje.db.elephantsql.com
DB_NAME=labber
DB_USER=labber
DB_PASS=abc123
```
* Add this file to .gitignore!!! (Super important!)

Instead, you **can** push a .env.example file to Github, which can be populated with data.
.env.example
```
DB_PORT=
DB_HOST=
DB_NAME=
DB_USER=
DB_PASS=
```

server.js
```js
// VERY FIRST LINE!
require('dotenv').config(); // Populates our environmental variables
```

