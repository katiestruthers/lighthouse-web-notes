# Lecture 13: Client-Side JS

### The Browser's Window Object
* Represents the current window we have open
* The browser's version of the process object in NodeJS

Some features:
* window.history &rarr; shows some info on the browsing history (not exact addresses, but does include how many addresses)
  * history.back() & history.forward() are equivalent to pressing the backwards and forwards button in the browser window
* window.location &rarr; shows detailed info on the current page you are on, such as info on the hosting company
* window.navigator &rarr; can use things like navigator.geolocation
  * i.e. the pop-up that asks if the website can access your current location
* window.document &rarr; access the web page you are currently on (i.e., the DOM)

TIP: window.location === location, window.history === history, etc.
* Properties in window are global objects, so you don't have to include the window. when you call them

### Document Object Model (DOM)
* From the DOM perspective, each element on the page is a *node*, starting from the html tag / node at the top of the tree and branching downwards from there
  * i.e. head and body would be children of html, title and script would be children of head, and h1 and p would be children of body
* This creates a tree-like structure!

```html
<html>
  <head>
    <title>JS in the Browser</title>
    <script defer src="./js/js-in-the-browser.js"></script> <!-- Good practice to not write your JS directly in your HTML document -->
  </head>
  <body>
    <h1>JS in the Browser</h1>
    <p>Isn't this cool?</p>
  </body>
</html>
```

### HTML Runs Top-to-Bottom!
This means that your script that is placed in the head tag will run before the body tag.

Three solutions you see in industry:
1. Move scipt to the end of the body tag &rarr; this is arguably more disorganized, as script is considered metadata and as such should be in the head tag
2. Use document.eventListener('load') in your JS file, which waits until the page fully loads
3. Add the defer attribute on your script tag &rarr; what we will use! Keeps the script in the appropriate place without adding any additional JS code

js/js-in-the-browser.js
```js
document.title = 'Client-Side JS'; // Update the title

const myH1 = document.querySelector('h1'); // Grab the heading using .querySelector
myH1.textContent = 'Client-Side JS'; // Update the heading
```

### DOM & Events Example: Simple To-Do List
```html
<!-- Boilerplate -->
<head>
  <script defer src="js/dom-and-events-example.js"></script>
</head>
<body>
  <h1>Simple To-Do List</h1>
  <h2>To-Do Form</h2>

  <!-- Front-end Only Form -->
  <form>
    <label for="new-to-do">Enter a new task:</label>
    <input type="text" id="new-to-do">
    <input type="submit" value="Add To-Do">
  </form>

  <h2>To-Do List</h2>
</body>
```

js/dom-and-events-example.js
```js
// Need to access three elements in order to take input from the form and add it to the list
const ul = document.querySelector('#to-do-list');
const newToDo = document.querySelector('#new-to-do');
const form = document.querySelector('form'); // In general, try to target via the ID. This would be problematic if we had more than one form!

form.addEventListener('submit' (event) => {
  event.preventDefault(); // the default is to submit a GET request to the same page; this stops it from doing so

  // Read input form form field
  const newToDo = newToDoField.value; // Value is what is entered into our form; this property is included in every form element!

  // Create new li element
  const li = document.createElement('li'); // This is not yet displayed to the user; only created in memory

  // Add text to li element
  li.textContent = newToDo; 

  // Glue li to our existing ul element
  ul.appendChild(li); 
  // append -> at the end of content
  // prepend -> at the beginnning of content

  newToDoField.value = ''; // QOL improvement to clear out input field in preparation to add another
});
```

### Intro to jQuery
A front-end library with the following features:
* Written in JS
* Backwards compatibility
* Cross-compatibility between browsers (not as important nowadays)
* Offers us useful helpers and functions
  * i.e. document.querySelectorAll('p') is written in jQuery as $('p')
  * $ = jQuery object

Since jQuery has been around for 20+ years, it is one of the [best documented](https://jquery.com/) front-end libraries.

How To Use ([Download Options](https://jquery.com/download/)):
```html
<!-- Can pull in a CDN, here using Google's CDN -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.0/jquery.min.js"></script>
<!-- Can copy & paste the (compressed) version into our files -->
<script src="vendor/jQuery/jquery-3.7.0.min.js"></script>
```

* Find the compressed version [here](https://code.jquery.com/jquery-3.7.0.min.js)
* .min indicates its the compressed (ugly, but space-saving) version

### DOM & Events Example Using jQuery!
```js
// Select our ul
const $ul = $('#to-do-list'); // You don't have to include the $ for this to work! It is just a signal to ourselves and other developers that this variable includes the extra jQuery features.

// Select our new-to-do field
const $newToDoField = $('#new-to-do');

// Select our form
const $form = $('form');

// Set up a listener for our form "submit"
$form.on('submit', (event) => {
  // Prevent the default
  event.preventDefault(); // same as above

  // Retrieve user value from the form field
  const newToDo = $newToDoField.val(); 
  // with no arg, this GIVES us the value of our input
  // with an arg, this SETS the value of our input

  // Create an li with the info that the user entered
  const li = `<li>${newToDo}</li>`;

  // Add the li to our ul
  $ul.append(li);

  // Clear our input for easy entry of a new to-do
  $newToDoField.val('');
});
```

**Note that the variables newToDo and li do not have the $ in front of them &rarr; this is because they are not variables pulled from jQuery, but rather are just normal strings!