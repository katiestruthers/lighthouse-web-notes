# Lecture 10: HTTP Cookies & User Authentication

### HTTP and Cookies
* HTTP is a *stateless* protocol, meaning that neither party HAS TO remember any previous interaction
  * How do we get around this? &rarr; COOKIES!

* Cookies are key-value pairs stored in the browser
  * They are *domain-specific*, meaning your Facebook cookies won't be sent to the browser when opening a different website
  * They are also *port-specific*, aka localhost:8000 !== localhost:8001

#### server.js
```js
const express = require('express');
const morgan = require('morgan');
const cookieParser = require('cookie-parser');

// constants
const app = express();
const port = 8001;

// configuration
app.set('view engine', 'ejs'); // now you don't need to specify .ejs after every template

// middleware
app.use(morgan('dev'));
app.use(express.urlencoded({ extended: false })); // creates & populates req.body object; extended: false === not expecting complex data
app.use(cookieParser()); // creates & populates req.cookies object

// If req.body or req.cookies is ever undefined, it means you haven't implemented the above two lines of code yet!

// database
const users = {}; // imagine object of users!

// login endpoints
app.get('/login', (req, res) => {
  res.render('login'); // do not need to fully write login.ejs because we have defined the template engine above
});

app.post('/login', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  // deal with edge cases first
  if (!username || !password) {
    return res.status(400).send('you must provide a username and password'); // 400 - bad request
  }

  let foundUser = null; // could leave this as undefined, but setting it to null is perhaps more useful / intentional

  for (const userID in users) {
    if (users[userID].username === username) {
      foundUser = user;
    }
  }

  if (!foundUser) {
    return res.status(400).send('no user with that username found');
  }

  if (foundUser.password !== password) {
    return res.status(400).send('the passwords do not match');
  }

  // everything went well -- the happy path! 🥳
  res.cookie('userId', foundUser.id); // async, i.e. attempting to console.log(req.cookies) immediately after will not work
  res.redirct('/protected');
});

// protected endpoint
app.get('/protected', (req, res) => {
  const userId = req.cookies.userId;

  if (!userId) {
    return res.status(401).send('you must have a cookie to see this page'); // 401 - unauthorized
  }

  // happy path! Look-up user based off their userId
  const templateVars = { user: users[userId] };
  res.render('protected', templateVars);
});

// logout endpoint
app.post('/logout', (req, res) => {
  res.clearCookie('userId');
  res.redirect('login'); // Ideally with a post request, end with a .redirect instead of a .render
});

// register endpoints -- notice how similar these are to the login endpoints!
app.get('/register', (req, res) => {
  res.render('register');
});

app.post('/register', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  if (!username || !password) {
    return res.status(400).send('you must provide a username and password');
  }

  let foundUser = null;

  for (const userID in users) {
    if (users[userID].username === username) {
      foundUser = user;
    }
  }

  if (foundUser) {
    return res.status(400).send('a user with that username already exists');
  }

  const id = Math.random().toString(36).substring(2, 5); // generate random alphanumeric ID

  const newUser = {
    id: id,
    username: username,
    password: password
  }

  users[id] = newUser; // update the database with the new user
  // Testing step: console.log(users) to ensure the new user is set-up identically to its peers

  res.redirect('/login');
});

app.listen(port, () => {
  console.log(`App is listening on port ${port}`);
});
```

* TIP: Set the username and password to the easiest thing you can type to simplify testing
  * i.e. email: a@a.com, password: 123

#### /views/login.ejs
```html
... <!-- Boilerplate -->
<body>
  <h1>Login Here!</h1>
  <form method="POST" action="/login">
    <label>Username</label>
    <input name="username">
    <br/>
    <label>Password</label>
    <input name="password"> <!-- can add type="password" to turn input characters into **** as the user types -->
    <br/>
    <button type="submit">Login!</button>
  </form>
</body>
```

* URL Encoded Example: username=alice%20smith&password=1234
  * %20 - space
  * & - next key-value pair

#### /views/protected.ejs
```html
... <!-- Boilerplate -->
<body>
  <h1>This page is protected!</h1>
  <h2>You are logged in as <%= user.username %></h2>
</body>
```

#### /views/register.ejs
```html
... <!-- Boilerplate -->
<body>
  <h1>Please Register Below</h1>
  <form method="POST" action="/register">
    <label>Username</label>
    <input name="username">
    <br/>
    <label>Password</label>
    <input name="password">
    <br/>
    <button type="submit">Register!</button>
  </form>
</body>
```

* Notice we only needed to minorly change the login template to make the register template