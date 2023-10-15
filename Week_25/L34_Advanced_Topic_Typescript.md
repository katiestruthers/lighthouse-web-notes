# Lecture 34: Advanced Topic - Intro to Typescript

### What is TypeScript?
* superset of JavaScript -- all valid JS is valid TS!
* what other superset have we seen in the program? &rarr; SASS / SCSS!
* TypeScript adds static types (that's it!)
  * static types means that you have to declare the data type
  * .ts &rarr; typescript compiler (tsc) &rarr; .js
* owned by Microsoft, who also own VSCode! So there's a lot of in-built functionality within our favourite IDE.
* According to Octoverse 2022, it is no. 4 most popular progamming language in the world!

### TypeScript Basics
* ```npm i typescript```
* tsc [file_name].ts --target es6 &rarr; converts to .js
  * if you remove the target, the default target will result in a compiled .js that uses ```var``` instead of ```let```

```ts
//// PRIMITIVES ////

// Default JS type is 'any'; this is just like normal JS
let username: any = 'Alice'; 

let username: string = 'Alice';

username = 42; // Error! This is not a string.

// Use single pipe '|' to assign two data types 
let username: string | number = 'Alice';

username = 42; // This works just fine now.

//// ARRAYS ////

let numbers: number[] = [];

numbers.push('hello'); // Error! This is not a number.
numbers.push(42);

const myNum = numbers.pop(); // You can trust this will always return a number!

// Include both types in the array
let numbers: (number | string)[] = [];

numbers.push('hello'); // This works now!
numbers.push(42);

//// OBJECTS ////

const myObj: {
  username: string; // Can use ';' or ',' here in the type definition
  password: stirng;
} = {
  username: 'bob',
  password: '1234'
};

// or, we can add a new INTERFACE -- a new data type!
// often, we store all our interfaces in one file and import that file in

interface User { // can also name as IUser to indicate this is an interface, not a class
  id?: number; // optional property
  username: string;
  password: string;
  bff?: User; // can recursively use interfaces!
  friends?: User[];
}

const myObj: User = {
  username: 'bob',
  password: '1234'
}

// Now can use your interface wherever you would use any other type!
const users: User[] = [];

//// FUNCTIONS ////

// TS will error if you don't call the function with the specified number of arguments
// This function accepts one number parameter and returns a string
// Can have a 'void' return value, which means it returns nothing
const myFunc = (age: number): string => {
  return 'hello world';
};

const returnVal = myFunc(42); // Will always return a string

// Can use Promise<unknown> to give flexibility to what the Promise is returning
const returningPromise = (message: string, age?: number): Promise<string> => {
  return new Promise((resolve) => {
    resolve(message);
  });
};

// Not including age works, since we have set it to optional with the '?'
returningPromise('hello')
  .then((data) => {});

const higherOrderFunc = (callback: (message: string) => number) => {};

//// METHODS ////

interface Writer {
  penName: string;
  realName: string;
  books: string[];
  writeBook: (title: string) => boolean; // Declare method
}

const agatha: Writer = {
  penName: 'agatha christie',
  realName: 'joan mary',
  books: [],
  writeBook: (title: string) => {
    return false;
  }
}
```

### Duck-Typing
* Found in all staticly-typed languages
* "If it looks like a duck... its probably a duck"

```ts
interface PotentialUser {
  username: string;
  password: string;
}

const login = (user: PotentialUser): boolean => {
  return true;
};

// Even though this is not a User, it just needs to have the info they are looking for
// I.e., in this situation, it NEEDS to have a username and password of type string
// Can add other info and it will still 'look like a duck'
const user = {
  username: 'jstamos',
  password: '1234',
  age: 42
};

login(user); // This works!
```

### Generics
* A generic is an interface where you are unsure what some of the types will be
  * Not likely that you will create these -- much more likely that you will be using them instead!

```ts
interface Container<T> { // Can call this type, Type, T, or anything else you like
  name: string;
  contents: T;
};

const stringContainer: Container<string> = {
  name: 'string container',
  contents: 'hello world'
};

const booleanContainer: Container<boolean> = {
  name: 'boolean container',
  contents: true
};
```

### tsconfig.json
* ```tsc --init```
* tons and tons of potential options to uncomment and have turned on!
  * if you want to have compilation to happen automatically (much like SASS): ```tsc --watch```
  * if you want to use TS in your final project, you can turn on "allowJs" to allow you to stop using TS at any time and switch back to vanilla JS

### React + TS
* if you use Angular or Vue, it automatically uses TS
* if you want to use React, you have a couple options:
  * make a new React app with TypeScript with it: [https://create-react-app.dev/docs/adding-typescript/](https://create-react-app.dev/docs/adding-typescript/)
  * if you want to add it to an existing project: 
    * ```npm i express @types/express```, ```npm i morgan @types/morgan```, etc. Most packages you need will have this ```@types``` that you can simply install with it.
    * in fact, a lot of packages are simply shipping with a type definition file (.d.ts file)! So you don't have to worry about specificying anything, just install as normal