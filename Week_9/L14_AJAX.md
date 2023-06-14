# Lecture 14: AJAX

### What is AJAX?
In our [Tinyapp](https://github.com/katiestruthers/tinyapp), when we wanted to update the user interface, each request triggered a page reload.
```html
<form method="POST" action="/register">
```

With AJAX, we can update the user interface *without reloading* the whole document!
```js
$('#search-form').on('submit', function(event) {
  // Stop the form from being submitted
  event.preventDefault();
  ...
  XMLHttpRequest();
})
```

### Single-Page Application (SPA) Model
1. Web browser sends an HTTP request (AJAX) to the server
2. Server sends a SQL Query to the database
3. Database sends back the relevant data to the server
4. Server sends the data back in JSON form to the browser

### AJAX Demo
```js
// Helper function
const addDrink = function(drinkObject) {
  let outputString = `
    <div class="drink">
      <h2>${drinkObject.strDrink}</h2>
      <img src="${drinkObject.strDrinkThumb}" alt="${drinkObject.strDrink}">
      <ul>`;
  
  // Our data we chose to use didn't give us a nice array, so we have to manipulate the data to work for what we need it to do!
  for (let i = 1; i < 16; i++) {
    const keyName = `strIngredient${i}`;
    if (drinkObject[keyName] !== null) {
      outputString += `<li>${drinkObject[keyName]}</li>`;
    }
  }

  outputString += `
      </ul>
      <p>${drinkObject.strInstructions}</p>
    </div>`;

  // Append new HTML into the element with id="display"
  $('#display').append(outputString);
};

// Run when document loads
$(document).ready(function() {
  // When the form is submitted...
  $('form').on('submit', function(event) {
    // Stop the default page refresh
    event.preventDefault();

    // Make the AJAX request
    $.ajax({
      url: 'https://www.thecocktaildb.com/api',
      type: 'GET'
    }).then(function(data) {
      for (let drink of data.drinks) {
        addDrink(drink); // Call helper function
      }
    });
  });
});
```

TIP: Instead of thinking to yourself, "is this function asynchronous?", think, "does this function have any asynchronous effects?"
* i.e. $.ajax will SYNCHRONOUSLY schedule itself, but will ASYNCHRONOUSLY run later

### Using jQuery for DOM Element Creation

In the course so far, we have been using jQuery to access particular DOM elements via CSS selectors. 

You can also use jQuery to *create* a new DOM element, like so:

```js
const $newDiv = $('<div>Hi!</div>'); // New DOM element created in memory

// 2 ways of adding the DOM element into HTML file:
$('#display').append($newDiv); // 1
$newDiv.appendTo('#display'); // 2
```

The benefit of doing this is that you now have access to all of the jQuery functionality (whereas with a plain ol' string, like in the AJAX demo above, you do not).

### Pros / Cons of AJAX
Pros:
* Improve user experience &rarr; no page reloads, faster page renders, improved response times
* Client side rendering, which reduces the network load

Cons:
* Can be tricky to create &rarr; dynamic content and async programming can be more complex to code
* History is not updated automatically, so you as the developer needs to decide what gets saved
  * i.e. if someone interacts with a page and then bookmarks it for later, what exactly gets re-rendered when the user revists?

### Cross-Origin Resource Sharing (CORS)
* Browsers restrict cross-origin HTTP requests for security reasons
* AJAX requests follow the *same-origin* policy, which means that, by default, it can only request resources from the same origin it was loaded from
* If you would like to include resources from other origins, you need to ensure these responses include the **correct CORS headers**.
  * You may run into CORS issues during your midterms so it's something to be aware of! Read more on the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).