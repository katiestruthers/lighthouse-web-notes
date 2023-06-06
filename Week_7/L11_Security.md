# Security & Real World HTTP

### Storing Password

In the previous lecture, we were storing our password in a plain text string in our user object.
* When delivering plain text over the web, any "middle people" would be able to easily read this

We want to find ways to scramble passwords to that they are not easily recognizable by people!
* We want to _hash_ a password, which is a one-way scrambling (i.e. once its scrambled, you can't unscramble it)

### Hashing vs. Encryption
* **Hashing**... one-way street! We never get to unscramble, so no one can ever read what was originally scrambled again.
* **Encryption**... two-way street! We can scramble and unscramble.

### Hashing Library: bcrypt
* See the [documentation](https://www.npmjs.com/package/bcryptjs)

```js
const bcrypt = require('bcryptjs');
const salt = bcrypt.genSaltSync(10); //**

// Example: someone is registering and we want to hash their password
const password = 'p4ss';
const hash = bcrypt.hashSync(password, salt);

// Example: someone is trying to sign-in, and we need to match their entered password
const userEntered = 'abc';
const validated = bcrypt.compareSync(userEntered, hash);
```

**Standard for genSaltSync is 10, as it is still pretty quick for the user while also remaining annoying for any hacker.
* It gets more annoying as this number gets larger, taking minutes to hash the password at 20. However, although this is perhaps more secure, we can't expect our users to wait minutes! That's why we find a middle-ground at 10.

### Cookies: Web Client vs. Web Server
* Web Client (Browser): HTML, CSS, JS
  * Stores cookies as key-value pairs

* Web Server: Operating System, Web Server Software, Web Apps / Scripts, Database
  * Sends cookies to the browser, which the browser stores

The problem: the browser has control over the cookies, so we can change the values via the developer tools and be magically in another person's account!

A couple solutions:
1. Browser stores one special session-specific encrypted-cookie that it sends to the server for verification
2. ALL key-value pairs are encrypted on the browser

### Library: Cookie Session
* See the [documentation](https://www.npmjs.com/package/cookie-session)

#### server.js
* Continuation from [last lecture](L10_Cookies.md)
* npm uninstall cookie-parser (as we are now using Cookie Session)

```js
// Previous lecture code ^
// const cookieParser = require('cookie-parser');
const bcrypt = require('bcryptjs');
const cookieSession = require('cookie-session');

app.use(cookieSession({
  name: 'session', // will appear as the cookie name in the browser developer tools
  keys: [], // can have multiple, which will rotate periodically to increase security

  // Cookie Options
  maxAge: 24 * 60 * 60 * 1000 // 24 hours
}));

// Submit login information
app.post('/login', (req, res) => {
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

  if (!foundUser) {
    return res.status(400).send('no user with that username found');
  }

  // prev: if (foundUser.password !== password)
  if (!bcrypt.compareSync(password, foundUser.password)) {
    return res.status(400).send('the passwords do not match');
  }

  // res.cookie('userId', foundUser.id);
  req.session.userId = foundUser.id; // Set session cookie

  res.redirct('/protected');
});

// Bring logged in user to protected page
app.get('/protected', (req, res) => {
  // const userId = req.cookies.userId;
  const userId = req.session.userId; // Retrieve session cookie

  if (!userId) {
    return res.status(401).send('you must have a cookie to see this page');
  }

  const templateVars = { user: users[userId] };
  res.render('protected', templateVars);
});

// Logout the user
app.post('/logout', (req, res) => {
  // res.clearCookie('userId');
  req.session.userId = null; // According to the cookie session docs, just set whichever cookies you want to clear to null
  // req.session = null; // could also do this, which drops the whole session

  res.redirect('login');
});

// Submit new user registration information
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

  const id = Math.random().toString(36).substring(2, 5);

  // Hash the password before storing it
  const salt = bcrypt.genSaltSync(10);
  const hash = bcrypt.hashSync(password, salt);

  const newUser = {
    id: id,
    username: username,
    // prev: password: password
    password: hash // Store the HASHED password! 
  }

  users[id] = newUser;

  res.redirect('/login');
});
```

### HTTP vs. HTTPS
* HTTP: HyperText Transfer Protocol
  * Port :80
  * Plain text gets sent to the server, where it is hashed from there
    * This is a huge security issue, as hackers can intercept this info before it reaches the server!
* HTTPS: HypterText Transfer Protocol Secure
  * Port :443
  * From a user perspective, you see a green icon or padlock indicating it is secure
  * End-to-end encryption for traffic, meaning the plain-text first gets encrypted via a public key _before_ sending it to the server, which the server then unencrypts using a private key and then can hash

* Browsers will often hide the port :80 or :443, but it is implied / expected based on if it is http or https
  * i.e. we don't see https://google.com:443

* In the real-world, you set up HTTPS often by opting for a paid option through your hosting provider
* Another option (free): [Let's Encrypt](https://letsencrypt.org/)

### Review: iCRUDES
* INDEX // display all of the resource
* CREATE // show the "new" form
* READ // show single of the resource
* UPDATE // save changes to existing resource
* DELETE // remove the single resource
* EDIT // show the "edit" form
* SAVE // create the new single of the resource

### REST
* Representational State Transfer
* We can use this to use other methods other than GET or POST, meaning we don't have to constantly changing the path in order to have a new, unique route
  * PUT &rarr; replace all data
  * PATCH &rarr; replace some data
  * DELETE &rarr; delete data
  * POST &rarr; save new resource

* HTTP only supports GET and POST, but by convention we can expand this to help communicate our routes' purpose.
  * We can use these other methods by using _method overrides_