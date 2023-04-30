# Lecture 2: Objects in JS

### Review: Primitive Types
* stored directly in memory (RAM)
* they cannot be changed after they are set

```javascript
const str = 'hello world';
const bool = true;
const num = 3.14;
const nul = null;
const undef = undefined;

const sym = Symbol('hello'); // => very rare that you see this in industry!
const bigint = 4783784n;
```

### Objects
* objects in JS: objects, arrays, and functions
* an _object_ is a collection of key/value pairs 
  * other names in different programming languages: hash, dictionary, associative array

```js
const arr = [1, 2, 3]; // new Array()

arr[1]; // 2
arr[1] = 4; // [1, 4, 3]

const obj = {
  anything: 'value'
  username: 'alice';
};

obj['anything']; // value
obj['anything'] = 'something new'; // anything: 'something new'

obj.username; // 'alice'
obj.username = 'David'; // username: 'David'

console.log();
console['log']();

```
* if you know the name of the key, use dot notation
* if the key is in a variable, use square bracket

```js
const user = {
  username: 'alice',
  password: '1234'
}

const myKey = 'password';

user.myKey; // won't work => it's looking for a key called myKey, which does not exist
user[myKey]; // will work => it's looking for a variable called myKey, which exists
```

* when you pass a primitive to a function, it uses a copy of the _primitive_ (passed-by-value), which does not affect the original primitive 
* when you pass an object to a function, it uses a copy of the _reference_ (passed-by-reference), which leads it to the actual object and thus affects it 

### Functions in Objects
* this &rarr; refers to the object you are currently in
  * Note: arrow functions do not create 'this'

```js
const writer = {
  name: 'agatha christie',
  books: [],
  writeBook: function(title) {
    this.books.push(title);
  }
}
```

### Object Looping
* for... in &rarr; loop through keys

```js
const myObj = {
  a: 5,
  b: 10,
  c: 15
};

for (const key in myObj) {
  console.log('key:', key);
  console.log('value:', myObj[key]);
}
```
* _You can also use for... in loops for arrays! It will give the index number._