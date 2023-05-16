# Mentor Assistance: Nested Callbacks

### The Problem

I need to ask multiple questions on the command line using the [readline](https://github.com/nodejs/node/blob/main/doc/api/readline.md) Node module. The two ways I attempted both didn't work.

#### Option 1 will close after the first question, never asking the second
```js
rl.question("What's your name? Nicknames are also acceptable :) ", (name) => {
  rl.close();
});
rl.question("What's an activity you like doing? ", (activity) => {
  rl.close();
});
```

#### Option 2 will _never_ close after the first question, again never asking the second
```js
rl.question("What's your name? Nicknames are also acceptable :) ", (name) => {

});
rl.question("What's an activity you like doing? ", (activity) => {
  rl.close();
});
```

### The Solution: Nested Callbacks!
```js
rl.question("What's your name? Nicknames are also acceptable :) ", (name) => {
  rl.question("What's an activity you like doing? ", (activity) => {
    console.log(`Hi my name is ${name} and I like ${activity}.`);
    rl.close();
  });
});
```

This allows us to ask the number of questions we want while still closing at the appropriate time. 

