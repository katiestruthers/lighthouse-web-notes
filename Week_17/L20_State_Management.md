# Lecture 20: State Management and Immutable Update Patterns

### Review: State

Counter.jsx
```js
// import React from 'react';
// const useState = React.useState;
// ===
import { useState } from 'react';

const Counter = () => {
  const [ count, setCount ] = useState(0);

  const clickHandler = () => {
    // DO NOT do this:
    // count += 1;
    // While count may change, React has no way of tracking this!

    setCount(count + 1);
  }

  return (
    <div>
      <h2>Counter Component</h2>
      <h2>Current Count: {count}</h2>
      <button on={clickHandler}>Click Me!</button>
    </div>
  );
};

export default Counter;
```

### Stale State
* the value has been changed, but the variable does not yet reflect those changes 
```js
 const clickHandler = () => {
    setCount(count + 1); // count = 0, state = 1
    setCount(count + 1); // count = 0, state = 1
    setCount(count + 1); // count = 0, state = 1
  }
// This will not give us +3, but +1! 
// Count will not update to 1 until the re-render at the end of the function call.
```

Solution: Callback Version with 'prev'
```js
const clickHandler = () => {
  setCount(prev => prev + 1); // prev / count = 0, state = 1
  setCount(prev => prev + 1); // prev / count = 1, state = 2
  setCount(prev => prev + 1); // prev / count = 2, state = 3
}
// This WILL give us +3!
```

### Importance of Spread Operator in React
* Whenever state or props change, any affected component is re-rendered
  * Only functions that depend on state or props will ever get re-rendered, and React knows which components are affected by state or props
* React only checks for _reference_ equality, not _content_ equality
  * i.e. {} === {} is only checking if the references are the same; it does not actually check what's inside
  * This means that in order to get React to re-render, you need to change the _reference_!

**Solution:** The Spread Operator!
```js
const arr = [1, 2, 3];
const arrCopy = [...arr, 4]; // New reference! Now React knows to re-render to reflect the change.

const user = {
  username: 'alice',
  age: 42
};
const userCopy = {
  ...user, // In objects, always spread first so we can apply changes afterwards
  username: 'bob'; // Will re-write the username key we copied, thus updating it to bob
};
```

**The Downside:** Spread Only Creates a SHALLOW Copy
* this means that any arrays within objects will only copy the reference, not creating a new array
```js
const alice = {
  age: 42,
  hobbies: ['bowling', 'curling']
}
const bob = {
  ...alice, // both alice's and bob's hobbies array is pointing to the same reference
  hobbies: [ ...alice.hobbies, 'coding' ] // New reference! Now coding will only be added to bob's hobbies, not alice's.
}
```

### useReducer
* useReducer is used to manage complex states (objects)
* ALL updates to state in one function!
* reducer is a function that tkaes in two arguments: ```1)``` current state, and ```2)``` object that specifies how to update state

* Note: 'use' &rarr; indicates to React that this is a _HOOK_
  * can re-read it as the 'reducer hook' if that helps!

```js
const reducer = (state, action) => {
  // update state
  if (action.type === 'add-a-new-hobby') {
    const copy = {
      ...state, 
      hobbies: [...state.hobbies, action.newHobby];
    }
  }

  return copy; // becomes state
};

const arr = useReducer(reducer, initialState);
const state = arr[0];
const dispatch = arr[1]; // Where you write your instructions

dispatch({ type: 'add-a-new-hobby', newHobby: 'lawn bowling' });
```

ReducerCounter.jsx
```js
import { useReducer } from 'react';

// Usually in a seperate file; in the very least, outside of the component!
const reducer = (state, action) => {
  if (action.type === 'increment') {
    return {
      ...state,
      count: state.count + 1
    }
  }

  if (action.type === 'decrement') {
    return {
      ...state,
      count: state.count - 1
    }
  }

  if (action.type === 'add-some') {
    const amountToAdd = action.payload;
    return {
      ...state,
      count: state.count + amountToAdd
    }
  }

  // REALLY important to account for error-handling!
  // If you don't, it may return undefined (which becomes the new state!)
  throw new Error('reducer called with invalid action type');
};

const initialState = {
  count: 0
};

const ReducerCounter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  // Generally, tie each action to a corresponding helper function of the same name
  const increment = () => {
    dispatch({ type: 'increment' });
  };

  const decrement = () => {
    dispatch({ type: 'decrement' });
  };

  const addSome = (amountToAdd) => {
    dispatch({ type: 'add-some', payload: amountToAdd })
  };


  return (
    <div>
      <h2>ReducerCounter Component</h2>
      <h2>Current Count: {state.count}</h2>
      <button onClick={() => addSome(2)}>Add 2</button>
      <button onClick={() => addSome(5)}>Add 5</button>
      <button onClick={() => addSome(10)}>Add 10</button>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  )
}

export default ReducerCounter;
```
* **Note:** since we are passing arguments into the functions in the first three on-clicks, we need to write them in anonymous function form
  * if we simply put ```addSome(2)```, it would call that function immediately upon page-load!
* Since ```increment``` and ```decrement``` don't take any arguments, you can just call them as-is without the braces