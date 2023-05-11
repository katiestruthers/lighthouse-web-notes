# Lecture 4: Unit Testing
We often don't test functions that use console.log, as its difficult to test. Instead, we test a return value.

### Types of Testing
1. Manual - run the code and inspect the output
2. Unit testing - tests for the smallest units of code (aka functions)
  * Actual vs. expected

### Assertion Library
JS library filled with helper functions
* Assertion === forceful statement of fact or belief 
* If assertions fail, an error is thrown

### What is Node? 
* A JS interpreter aimed at server-side code
* When JS was born in 1995, it was primarily a front-end only language for a decade before Node came about
* Node wraps JS in C++ and allows it to be used as a back-end language

### Import from Node Library

```js
const assert = require('assert'); // Brings in the Node Assert library
assert.equal(actual, expected); // If actual == expected, will run as normal; if not, will throw an error
assert.strictEqual(actual, expected); // Same except actual === expected
```

**_The reason you can return at any point in the program is because everything written in Node is wrapped in a function!_

### Import from Your Own Code
Example of sharing a function you want to test:

```js
// in sayHello.js
module.exports = sayHello;
```

```js
// in sayHelloTest.js
require('./sayHello');
```

You can also pass in multiple exports, in either an array or an object:
```js
module.exports = [sayHello, phrase];
```
```js
module.exports = {
  sayHello: sayHello,
  phrase: phrase
}
```
```js
module.exports.sayHello = sayHello;
module.exports.phrase = phrase;
```

### Import from Other People's Code
NPM (node package manager) allows us to easily do this!
  * npm is automatically downloaded with Node

In order to download a package, we need to make our code a package by adding a package.json file to our directory.
  * 'package.json' contains information about the project
    * The Simple Way: 'touch package.json' 
    * The More Thorough Way: 'npm init' &rarr; allows you to give info about the package

#### Installation Options
1. global install ('npm install -g package') on our operating system
  * not usually best practice, as its locked into the current version when you installed it
2. project dependency ('npm install package') &rarr; something our project directly depends on to run
3. development dependency ('npm install --save-dev package') &rarr; will not be installed in production 
  * just like test code also doesn't appear in production

### The Ultimate Testing Library: Mocha ☕️
'.node_modules/mocha/bin/mocha.js' &rarr; runs all tests

```js
describe(); // grouping related tested together; optional 

it(); // actual execution of a test
```

Example: sayHello.test.js
```js
const sayHello = require('../sayHello');
const assert = require('assert');

describe('tests for the sayHello function', () => {
  it('returns "hello there alice" when given the string "alice"', () => {
    const actual = sayHello('alice');
    const expected = 'hello there alice';

    assert.strictEqual(actual, expected);
  });
});
```

### The Ultimate Assertion Library: Chai 🍵
While Node Assert gives about ~20 assertion options, Chai gives us 100s more.

### Ignore Files
'touch .gitignore' &rarr; don't want to include the Node modules, so we add the names of those to this file

### TDD: Test-Driven Development
In test-driven development, tests are written first, and then the code
  * This is known as 'red, green', as all our tests will be failing at first until we write code to turn them green
* n-driven development 
  * vs. componant development (React)