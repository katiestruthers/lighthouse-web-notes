# JS Patterns for React: Quicksheet

Reference: [JavaScript: The React Parts](https://reacttraining.com/blog/javascript-the-react-parts)

### var vs. let vs. const
* var: global-scope. 
  * If you declare a new variable with the same name within a block, the variable outside of that block _WILL_ change.
* let: block-scope. 
  * If you declare a new variable with the same name within a block, the variable outside of that block _WILL NOT_ change.
* const: block-scope. 
  * It cannot be reassigned. You can however declare a new variable of the same name within a new block.

### expressions vs. statements / declarations
* expressions: 
  * resolves to a single value
  * can be assigned
  * building blocks
  * i.e. ternary operator
* statement: 
  * may or may not carry out a task (two-sided)
  * can only be declared, not assigned
  * whole structure
  * create side effects
  * i.e. if-else

* Why is this important? *React ONLY allows expressions!*

### functions
* since declarations are not used in React, only use function *expressions*

```js
const example1 = function () {

}

const example2 = () => {

}
```

### ...spread & ...rest
* spread: add elements / properties from another array / object to a new array / object
* rest: when destructuring, group multiple elements / properties together

```js
// Spread otherArray into element1, element2
const example = [...otherArray, element3, element4];

// Join element3, element4 into variable grouping
const [ element1, element2, ...grouping ] = example;
```
