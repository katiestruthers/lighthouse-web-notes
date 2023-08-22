# Lecture 23: Data Fetching & Other Side Effects

### Pure Functions
1. Identical return given identical params/arguments
2. No side effects

```js
let sum = 0;

function addTwoNums(num1, num2) {
  sum += Number(num1) + Number(num2);
  return sum;
}
```
* This is an example of an *impure* function, as the sum variable outside of the function is persistent and will result in unexpected side effects

```js
function addTwoNums(num1, num2) {
  let sum = 0;
  sum += Number(num1) + Number(num2);
  return sum;
}
```
* This is now a *pure* function with no unexpected side effects

### Side Effect Examples
* Updating variables / values defined elsewhere
* Performing input/output operations (process.argv / console.log)
* Mutating a parameter directly (don't maniputlate an array or object passed in)
* Invoking an external function that has side effects

### React Hooks
React hooks are functions that allow us to tap into React features
* `useState` - storing values in a component that might change (this change will be noticed for a re-render)
* `useEffect` - a way we can carefully and intentionally handle code that engages in side effects

### Common Side Effects in React
* Timers
* Subscriptions
* Direct updates to the DOM (index.html / HTML that is not generated via JSX, HTML outside of our components)
* Fetching data from an external resource (Ajax)
* `console.log`

### useEffect
```js
// you can only call useEffect inside of a React function / component
useEffect(() => {

  // Your side-effects go here...

  // Optionally, if you need to do any cleanup when the component
  // is unmounted (removed from the page...)
  return () => {

    // Any clean-up for this side effect goes here...

  };

}, []); // dependencies array - indicates how often should the side effect run
```

### How to Use the Dependencies Array
useEffect will run every time there is a change to the dependencies array
* [] - will only execute at the end of the the FIRST render of the componenent, and will not run subsequency times as re-renders occurs
* [state] - will only re-render if this state variable changes
* if you pass nothing, the useEffect will run every re-render

### React Hook Rules
1. Only call Hooks in the top-level
2. Only call Hooks inside of React components (functions)

### react-app Example
\src\components\TitleComponent.jsx
```js
import { useState, useEffect } from 'react';

const TitleComponent = () => {
  const [title, setTitle] = useState('Title Component');

  useEffect(() => {

    // Update the <title> element in our web page
    document.title = title;

  }, [title]); // Run side effect only when title changes

  return (
    <section>
      <h1>{title}</h1>
      <input
        type="text"

        { /* Need to pair both value & onChange for the input to always have the latest state */ }
        value={title}
        onChange={e => setTitle(e.target.value)}

      />
    </section>
  );
};

export default TitleComponent;
```

\src\components\IntervalCounter.jsx
```js
import { useState, useEffect } from 'react';

const IntervalCounter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const counterInterval = setInterval(() => {
      setCount(prev => prev + 1);
    }, 1000);

    // If the component is unmounted, we should cleanup
    return () => {
      // Delete this interval so it won't run in the background
      clearInterval(counterInterval);
    };

  }, []); // We don't want to run it again, because that will set more than one interval
  // More intervals will cause the counting to move faster than every 1 second!

  const counterInterval = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);

  return (
    <section>
      <h2>Interval Counter</h2>
      <p>{count} seconds have passed.</h2>
    </section>
  )
};

export default IntervalCounter;
```

\src\components\FoxPictures.jsx
```js
import { useState, useEffect } from 'react';

const FoxPictures = () => {
  const [foxPics, setFoxPics] = useState([]);

  useEffect(() => {
    const FOX_URL = 'https://randomfox.ca/floof/';

    // Will not render until all fox pictures are loaded
    Promise.all([

      // fetch() - Vanilla JS way of fetching data from the browser at a particular URL
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json()),
      fetch(FOX_URL).then(response => response.json())

    ]).then((allFoxPics) => setFoxPics(allFoxPics)) // 8 random fox pics are now set

  }, [])

  return(
    <section>
      <h2>Fox Pictures</h2>
      <ul>
        {foxPics.length > 0 && foxPics.map((foxPic, index) => 
          <li key={index}>
            <a href={foxPic.link}>
              <img src={foxPic.image}>
            </a>
          </li>
        )}
      </ul>
    </section>
  );
};

export default FoxPictures;
```
* Find fun data to use for your projects through this [free public API list](https://github.com/public-api-lists/public-api-lists)


\src\components\FoxPicturesAxios.jsx
```js
// ...
import axios from 'axios'; // Seperate package from React - npm install axios

const FoxPicturesAxios = () => {
  // ...

  useEffect(() => {
   // ...

    Promise.all([

      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL),
      axios.get(FOX_URL)

    ]).then((allFoxPics) => {
      const newFoxPicsArr = [];
      allFoxPics.forEach((foxPic) => {
        newFoxPicsArr.push(foxPic.data)
      });

      setFoxPics(allFoxPicsArr); 
      });
    }, [])

  // ...
};

export default FoxPicturesAxios;
```

\express-server.js
```js
const express = require('express');
const morgan = require('morgan');
const cors = require('cors');
```