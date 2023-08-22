# Lecture 22: Custom Hooks

### Review Definitions
- React Component: A function that returns JSX
- React Render: calling / running a component function
- useState: so stuff can survive a render
- useEffect: document.ready of React! Code runs after browser is ready
- useReducer: 

### Custom Hooks
- Custom Hooks: function that we write (aka, a helper function for React!)
  - a custom hook is different from a regular helper function in that it does React stiff, i.e. call other Hooks
- Rules of Hooks:
  - must always be called in the same order
  - starts with use
  - only called inside components or other hooks

/hooks/useCounter.js
```js
import {useState} from 'react';

// Example of custom hook - a function doing React stuff!
const useCounter = function() {
  const [counter, setCounter] = useState(0);

  const increment = function() {
    setCounter(counter + 1);
  }

  return {counter, increment};
  // Could also do return [counter, increment];**
};

export default useCounter;
```
** There's a design tradeoff here! 
  - Object &rarr; just able to get what you need
  - Array &rarr; can use multiple times with custom names

/hooks/useMousePosition.js
```js
import {useEffect, useState} from 'react';

const useMousePosition = function() {
  const [coords, setCoords] = useState({x: 0, y: 0});

  useEffect(() => {
    const mouseMoveHandler = function(event) {
    setCoords({x: event.clientX, y: event.clientY});
    }

    // This is a crucial step!!!
    const cleanup = function() {
      document.removeEventListener("mousemove", mouseMoveHandler);
    };

    document.addEventListener("mousemove", mouseMoveHandler);
    return cleanup;
  }, [])

  return coords;
};

export default useMousePosition;
```

/hooks/useInput.js
```js
import {useState} from 'react';

const useInput = function(initial) {
  const [value, setValue] = useState(initial);

  const onChange = function(event) {
    setValue(event.target.value);
  }

  return {value, onChange};
}

export default useInput;
```

/hooks/useAxios.js
```js
import 'axios' from 'axios';
import {useEffect, useState} from 'react';

const useAxios = function(url) {
  const [body, setBody] = useState(null);
  const [pending, setPending] = useState(true);

  useEffect(() => {
    setPending(true);

    axios.get(url)
      .then(res => {
        setBody(res.data);
      })

    setPending(false);
  }, []);
  
  return {body, pending};
};

export default useAxios;
```


