# Lecture 8: Web Servers 101

### HTTP (HyperText Transfer Protocol)
* Request / response protocol
* HTTP is stateless: neither party remembers any previous communication

client <===== tcp / http ====> server

### Request
* Made up of two parts:
  1. HTTP verb - GET, POST 
    * GET &rarr; I would like to receive information
    * POST &rarr; I would like to send information

  2. url / endpoint - '/about', '/home'
    * www.google.com/search

* When referring to the request, state the verb then the enpoint 
  * i.e. GET /about

### Response
* Only needs to contain a status code
  * 1xx &rarr; routing protocols (we won't deal with these)
  * 2xx &rarr; success!
  * 3xx &rarr; redirection
  * 4xx &rarr; client errors
  * 5xx &rarr; server errors

* It optionally can also include a body
  * A body can be anything! HTML, CSS, JS, JSON, XML, image, raw data, etc.

### Definition of Clients & Servers: Revisited
web client &rarr; any application that is capable of making an HTTP request 
* i.e. request, curl, wget, postman, rested

web server &rarr; any application that is capable of responding to HTTP requests 
* i.e. nginx, apache, puma, Node

### Using Chrome to Create Client Request
* type in address bar &rarr; localhost:{port#}

### Using HTTP Module To Create a Server
```js
const http = require('http');
const port = 54321;

const server = http.createServer();

server.on('request', (request, response) => {
  if (request.method === 'GET' && request.url === '/home') { // GET /home
    response.write('Welcome to our website!');
  }
  if (request.method === 'GET' && request.url === '/about') { // GET /about
    response.write('This is the about page.');
  }

  response.end(); // if you don't include this, it won't send the above response.write
});

server.listen(port, () => {
  console.log(`App is listening on port ${port}.`);
});
```

* This is the first and last time in your career as a web developer where you will work with HTTP directly!
  * Although frameworks like Express still use this under the hood, it provides us with a more efficient way of doing so on a simpler interface

### Using Express To Create a Server
* npm init -y
* npm install express

```js
const express = require('express');

const app = server();
const port = 8501;

app.get('/home', (request, response) => { // GET /home
  // Express will automatically send a status code, although we can rewrite it with response.status()
  response.status(204);

  // while you still can use response.write() & response.end(), Express has a way of combining the two with response.send()
  response.send('Welcome to the home page.');
});

app.get('/about', (request, response) => { // GET /about
  response.send('Learn more about us here!');
});

app.listen(port, () => {
  console.log(`App is listening on port ${port}`);
})
```

* The beauty of using Express is that if the endpoint doesn't exist, it will immediately respond with a 404
  * Compare this with using HTTP, where the browser was just perpetually loading, waiting for an endpoint response that doesn't exist

* Another advantage of using Express: Middleware!

### Middleware
* code (functions) that sit between the request and response
* keeps our code DRY
* useful for parsing incoming information

**Updated Diagram!*
client <=========== tcp / http =========> server
            middleware     middleware     
i.e.        cookie-parser  body-parser    app.get()                             
          request.cookies  request.body
              next()          next()

* Put your middleware at the top of the file

```js
app.use((request, respons, next) => { // all middleware take in three arguments
  request.secretKey = 'provided by the middleware';
  next();
});
```

* Middlewares allow all repetitive code to be pulled out, so that no matter what endpoint you use, the same middleware functions will run
  * i.e. a middleware checking to see if the user is logged in

### Middleware: Morgan
* HTTP request logger! &rarr; npm i morgan
* Super useful middleware to install on your projects coming up!

```js
const morgan = require('morgan');

app.use(morgan('dev')); // dev is a type of usage for the morgan middleware
```

### Common Error Responses on Express
* Attempting to connect to a port that is already in use (will crash)
* Attempting to send a response more than once (will not crash, but will send a console.log of the error)
  * Often occurs when we have mutiple response.send()'s behind conditionals, and we forget to return when a conditional is true
    * Can even do it on one line &rarr; return response.send('');