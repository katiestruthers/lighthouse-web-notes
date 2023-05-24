# Lecture 6: Networking with TCP

### What is Networking?
* Simplest definition: two or more machines connected together
  * every machine can talk to every other machine

* servers have information
* client wants information

### Internet Protocol (IP)
* IP address &rarr; think of this like a street address!
  * IPv4 192.168.8.8 ~ 4.3 billion addresses
  * IPv6 2001:db8:3333:4444:5555:6666:7777:8888 ~ 2^128 addresses

### Ports
* Specifies one particular app or server &rarr; think of this like an apartment number!
  * Have 65,535 ports to choose from in your machine

### DNS (Domain Name Service)
* Converts domain names into IP addresses

### Packets
* Small pieces of information that when, put together, create a larger file like an image
* More efficient to break up the large file into packets, send these packets over the network, and then put them back together once they've all reached their destination

Two ways that this happens:
1. User Datagram Protocol (UDP) &rarr; does not come up very often
2. Transport Control Protocol (TCP) &rarr; the purpose of this lecture! Generally, most internet traffic will use this.

_Think of these protocols like the string between two tin cans, i.e. the server can and the client can. They connect the two together, allowing information to be moved back and forth along the string!_

### UDP
* smaller header size
* connectionless &rarr; does not track if the packets are actually reaching their proper destination
  * since it is connectionless, there is no error recovery
* packets can arrive in any order
  * this is why UDP is often used for livestreaming, as this makes UDP faster
  * however, this is also why livestreaming can get fuzzy / laggy, as its speed is being paid for in errors

### TCP
* larger header size
* connection &rarr; must have a three way handshake in order to start sending
  * any missing packets are resent
* packets are reordered on arrival

### server.js
```js
const net = require('net'); // a Node package written in C++
const server = net.createServer(); // Just creates the server - still not turned on
server.listen(3000); // Starts the server on the port 3000
```

* .listen can also accept a second argument as a callback, which runs if the server starts successfully

```js
const port = 3000; // Try to avoid "magic numbers" by assigning to constants
server.listen(port, () => {
  console.log(`The server is running on ${port}.`); // Callback runs if successful
});
```

### client.js
```js
const net = require('net');
const config = {
  host: 'localhost', // Keyword to indicate our local machine
  port: 3000
};
net.createConnection(config); // Create connection to our own machine
```

* Much like .listen, .creatConnection can also accept a second argument as a callback which only runs if a successful connection is made

### Event-Driven Programming
* Register event handlers (aka functions) to run when / if an event occurs
  * .on &rarr; incredibly common event syntax

## Example: Chatroom App

```js
// server.js
server.on('connection', () => { // In the event of a connection...
  console.log('A new client has connected!');
});
```

* .on automatically includes a Socket object, which you can take advantage of by declaring a parameter in the callback

```js
// server.js
server.on('connection', (socket) => {
  console.log('A new client has connected!');
  socket.write('Hello new client!');
});
```

* in order for the client to receive the message from .write, it needs to be set up to receive it
  * call .on with the 'data' event, which includes the encoded message that you can catch with a parameter

```js
// client.js
const socket = net.createConnection(config);
socket.setEncoding('utf-8'); // If you don't include this, the message will remain in hexadecimal

socket.on('data', (message) => { // In the event of receiving data from the server...
  console.log(`Server says: ${message}`); 
  socket.write('Great to be here!'); // Send a message back to the server
});
```

* again, in order for server.js to receive it, we need to call .on with the 'data' event

```js
// server.js
server.on('connection', (socket) => {
  console.log('A new client has connected!');
  socket.write('Hello new client!');

  socket.setEncoding('utf-8'); // Again, have to include this in order to properly read the message

  socket.on('data', (message) => {
    console.log(`Client says: ${message}`);
  });
});
```

What if we want to send a message to ALL the clients in the server?
```js
// server.js
...
const sockets = []; // stores all connections to the server
server.on('connection', (socket) => {
  ...
  sockets.push(socket);

  socket.on('data', (message) => {
    for (const socketConnection of sockets) { // Writes a message to each connection
      socketConnection.write(message);
    }
  });
})
```

### stdin.js
```js
process.stdin.setEncoding('utf-8');

process.stdin.on('data', (messageFromTerminal) => { // In the event of the Enter key being pressed...
  console.log(`Message from terminal: ${messageFromTerminal}`) // messageFromTerminal = everything typed before pressing Enter
});
```