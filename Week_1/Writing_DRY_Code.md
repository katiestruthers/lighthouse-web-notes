# DRY - Don't Repeat Yourself

A principle to writing clean, organized code. To put this principle into practice, we were asked to refactor the loopyLighthouse function we had previously wrote.

## The Before:

```javascript 
function loopyLighthouse(range, multiples, words) {
  for (let x = range[0]; x <= range[1]; x++) {
    if (x % multiples[0] === 0 && x % multiples[1] === 0) {
      console.log(words[0] + words[1]);
    }
    else if (x % multiples[0] === 0) {
      console.log(words[0]);
    }
    else if (x % multiples[1] === 0) {
      console.log(words[1]);
    }
    else {
      console.log(x);
    }
  }
};
```

## The After:
```javascript
const loopyLighthouse = function(range, multiples, words) {
  for (let x = range[0]; x <= range[1]; x++) {
    let string = "";

    if (x % multiples[0] === 0) {
      string += words[0];
    }
    if (x % multiples[1] === 0) {
      string += words[1];
    }
    if (!string) {
      string = x;
    }

    console.log(string);
  }
};