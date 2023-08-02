# Lecture 19: React

### What is React?
* A library for creating user interfaces (React-DOM, React-Native, React Ink)
* We will be using React-DOM, which specifically targets web browsers, but there's other versions of React as well, such as React-Native (for mobile apps) or React Ink (for the terminal)
  * If you learn how to use one aspect of React, it is often very easy to apply it to a different aspect!
* _Declarative_ - specifiy what we want to see, not how it's implemented
  * i.e. other declarative languages we have worked with are CSS & SQL
  * we have been doing _Imperative_ programming with JavaScript
* Component-based - divide up the UI into smaller pieces (reusable)

### Create Your React App
**npx create-react-app**
* npx === node package executor
* npx will first look at your node modules, and if the node package is there, it will install it. If you don't have it installed on your system, then npx will download it temporary, create the app, and then uninstall it.
* A benefit of using npx is that you will always be downloading the latest version

### Tour of the App
public/
* favicon.ico - this is all ready to be served up! Change as you see fit.
* index.html - includes a #root where the entire React app latches onto: ```<div id="root"></div>```
* manifest.json - instructions for when you shortcut something (i.e., icon on a mobile screen)
* robots.txt - instructions for web-crawlers / search engines (i.e., do you want this to show up on Google)

src/
* App.test.js - all tests are ran from here
* setupTests.js - add in additional functionality to your testing suite
* reportWebVitals.js - will tell you how well the app is running in production (how quickly is it rendering? How often does it 404? etc.)
* index.css - default CSS for the app
* index.js - brains of the operation! Calls the main App component and all relevant CSS, creates the root node, and renders our app.
  * ```<React.StrictMode></React.StrictMode>``` &rarr; This is optional! It is only there for development purposes.
  * If you are seeing doubles, this is because strict mode calls every function twice to ensure we aren't updating data incorrectly.

* app.js - all components are imported & organized in here in the order they will appear
  * Here at Lighthouse, we like to create a /components directory within src/ and put all our components in there
  * Each component should start with a capital letter, and so each file should also start with a capital letter

### What to Delete (for now)
* public/ &rarr; manifest.json, robots.txt, relevant code from those files in index.html
* src/ &rarr; App.test.js, setupTests.js, reportWebVitals.js, relevent code from those files in index.js, App.js

### JSX
* JavaScript and XML (eXtensible)
  * Extensible means that you are EXTENDING the language to add custom elements
* XML is way more strict than vanilla JS or HTML - i.e., every element needs a closing tag, as it won't just fill-in-the-blanks for you like HTML often does
* While both .js & .jsx files will work in a React app, you do get additional shortcuts using .jsx files

```JS
const App = () => {
  // Can use this just like you would in EJS, in curly braces
  const name = 'Alice';

  // Can only return one element, but that element can have many children!
  return (
    // Where you would write "class" in HTML, write "className"
    // Where you would write "for" in HTML, write "htmlFor"
    <div className="App">
      <label htmlFor="username">username:</label>
      <input id="username">
      <p>{name}</p>
    </div>
  )
};
```

### CommonJS Syntax (CJS) vs. ECMAScript Modules (ESM)
* CJS: Exclusive & created by Node
* ESM: Modern JavaScript &rarr; this is the future! Recommended to use this syntax, as even the most recent versions of Node have switched over to this.

|   |  import |  export | export key | export function |
|:-:|:-:|:-:|:-:|:-:|
|  CJS |  require | module.exports | ```module.exports.myVar = 'Hello';``` | ```module.exports = Header;```
| ESM  |  import |  export / export default | ```export const myVar = 'Hello';``` | ```export default Header;```

### Quick Destructuring
* Destructuring &rarr; breaking down a data structure to get something specific
```js
import HeaderObj from './components/Header';
const Header = HeaderObj.Header;
// ===
import { Header } from './components/Header';
```

### Ways to Call Functions in JSX
```jsx
<Header><Header/>
<Header />
{ Header() }
```
If you console.logged each one, this is what would return:
* ```<Header><Header/>``` === ```{}```
* ```<Header />``` === ```{}```
* ```{ Header() }``` === ```undefined```

### Props
* props &rarr; data from outside the component / function

```jsx
{/*These are all identical!*/}

<Header heading="about us" ><Header/>
<Header heading="about us" />
{ Header(heading: "about us" ) }
```
* If you console.logged each one, they would all return ```{ heading: "about us" }```

```jsx
<Header>About Us</Header>
```
* console.log === ```{ children: "about us" }```

### State
* state &rarr; data that belongs to a component / function

How Class-Based Components Used to Be:
```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count += 1;
    console.log(this.count);
  }
}

const counter = new Counter();
console.log(counter); // Counter { count: 0 }

counter.increment(); // 1
counter.increment(); // 2
counter.increment(); // 3
counter.increment(); // 4
counter.increment(); // 5
```

How Function-Based Components Are Now:
```js
const increment = () => {
  let count = 0;
  count += 1;
  console.log(count);
}

increment(); // 1
increment(); // 1
increment(); // 1
increment(); // 1
increment(); // 1
// Not what we want in this situation. We can solve this using CLOSURES.
```

### Closure
* A variable in the outside scope that our function has access to
```js
let count = 0;

const increment = () => {
  count += 1;
  console.log(count);
}

increment(); // 1
increment(); // 2
increment(); // 3
increment(); // 4
increment(); // 5
```

### Closure in React: Hooks
* hook &rarr; helper function that interacts with React
* Example: **useState**
  * gives us back an array of values, of which the first is a getter and the second is a setter
```jsx
import React from 'react';

const Counter = () => {
  const arr = React.useState(0); // Set count to 0

  // If React sees a difference between these two values, the page will re-render with the new value
  const count = arr[0]; // The current count value
  const setCount = arr[1]; // Updating the current count value

  // Can rewrite the above three lines of code in a destructured manner:
  // const [count, setCount] = React.useState(0);

  const increment = () => {
    // Note that you never update count directly; you call the setter (setCount) and get the current value via the getter (count) to use as a value for setCount
    setCount(count + 1); 
  }

  return (
    <div>
      <h2>Counter Component</h2>
      <h2>Count: {count}</h2>
      <button onClick={increment}>Click Me!</button>
    </div>
  )
};

export default Counter;
```

* Big secret of React: you are only updating data!!! You do not have to worry about re-rendering the page, as React will do that for you.
  * If you find yourself updating the webpage yourself when using React, something has gone wrong.

### Other Examples using .useState
```jsx
const [username, setUsername] = React.useState('');
const [isVisible, isNotVisible] = React.useState(false);
```

### jQuery: Do we need it now?
* While jQuery was great to get you to learn more about what was happening behind-the-scenes and getting you practice with event listeners, there is really not much you can do with jQuery nowadays that you can't do with JS
  * https://youmightnotneedjquery.com/
* Plus, with modern libraries like React, Angular, etc., you don't have to worry about page re-rendering like jQuery!