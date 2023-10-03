# Lecture 33: Advanced Topic - PHP Introduction

### What is PHP?
* Was originally developed ~1993 by Danish-Canadian Rasmus Lerdorf
* PHP originally stood for Personal Home Page tools
* Nowadays, its stands for PHP Hypertext Preprocessir
* Although it almost feels like EJS & ERB when you write it, it is still considered a general purpose programming language (specialized in web development)
  * Web Apps
  * Dynamic Web Pages
  * Command-line Programs
  * Desktop GUI Applications (not very common)
* 70% of known back-ends include PHP in their stack
  * 40% of known stacks include WordPress (which is built using PHP)
* PHP powers much of Facebook's back-end, although they use a different version to the public with their own advanced features

### The ElePHPant in the Room...
* Lots of freedom, it is weakly/dynamically typed (much like JS)
* Lots of examples, tutorials, videos, etc., and not all of them are good (much like JS)
* Inconsistences in the function names, resulting in frequently referencing the docs

### The Good!
* Elephant mascot! 🐘
* Widely used, so there are many tutorials and resources out there!
* Mature, well-documented
* Frequently updated 
* Great performance - outperforms Ruby & JS for similar use cases
* MVC - Laravel
* Package Manager - Composer
  * In saying that, A LOT of features are built-in!

### PHP Installation
* Check if PHP is already installed: `php --version`
  * Mac: `brew install php`
* Can also play around on an [online compiler](https://www.w3schools.com/php/php_compiler.asp)

### Common Command-Line Options (PHP Interpreter)
* `php this/file.php` &rarr; runs a specific file
* `php -a` &rarr; opens an interactive shell for multi-line experiments
* `php -r "[your test code here]"` &rarr; runs one line of PHP
* `php -S [address]:[port]`, i.e. `php -S localhost:3000` &rarr; serves current folder (pwd)
* `php -S [address]:[port] -t path/to/serve` &rarr; serves specific folder

hello.php
```php
// No code involved, but this will still send "Hello, World!" to the browser!
Hello, World! 

// ==

// The longer way of printing "Hello, World!"
<?php 
  // Variable names always start with '$', and are CASE-SENSITIVE
  $helloText = "Hello, World!";

  // can also use the syntax of echo($helloText)
  echo $helloText;
?>
```

### PHP Data Types
* String
* Integer
* Float (Double)
* Boolean
* Classes / Object
* Array
* Null
* Resource &rarr; open file / database

Array / List &rarr; Indexed (Numbered) Array
Hash / Map &rarr; Associative Array 

```php
// BOOL, NULL //

// Fortunately or unfortunately, these are all valid, as functions / keywords are case-insensitive
$myNull1 = NULL;
$myNull2 = null;
$myNull3 = nUlL;

$myTrue1 = TRUE;
$myTrue2 = true;
$myTrue3 = tRuE;

$myFalse1 = FALSE;
$myFalse2 = false;
$myFalse3 = fAlSe;

eCHo '<pre>'; // Keeps the white space formatting
// Display the variable + data type
VaR_dUmP($myNull1); // NULL (since variable == data type)
echo '<pre>';

ECHO '<pre>';
VAR_DUMP($myTrue1); // bool(true)
echo '<pre>';


// INTEGER, FLOAT //

$myNum1 = 3; // Integer
$myNum2 = 3.14; // Float

$mySum = $myNum1 + $myNum2; // 6.14000000001; PHP will coerce it to a float


// STRING //

$name = 'Katie';
$greeting = 'Hello, ';

// Concatenation character / operator in PHP is the `.`
// '+' is reserved for mathematical addition in PHP
$fullGreeting = $greeting . $name; // 'Hello, Katie'
// ==
// Double quotes support interpolation
$interpolatedGreeting = "$greeting$name"; // 'Hello, Katie'
// ==
// Can optionally use curly braces
$specificInterpolatedGreeting = "{$greeting}{$name}";


// OBJECT //

// JS Equivalent:
// const student = {
//   name: 'Katie',
//   school: 'Lighthouse Labs'
// }
$student = new stdClass();
$student->name = 'Katie';
$student->school = 'Lighthouse Labs';


// ARRAY //

// INDEXED ARRAY //
$animals = array('Elephant', 'Giraffe', 'Dog', 'Cat', 'Fish');
// ==
$animalsSquare = ['Elephant', 'Giraffe', 'Dog', 'Cat', 'Fish'];

array_push($animals, 'Tardigrade'); // Add 'Tardigrade' to the end of the array

// ASSOCIATIVE ARRAY //
$student = array(
  'name' => 'Katie',
  'school' => 'Lighthouse Labs'
);
// ==
$student = [
  'name' => 'Katie',
  'school' => 'Lighthouse Labs'
];

$student['school']; // 'Lighthouse Labs'
```

### Web Page Example
public/index.php
```html
<?php include '../templates/head.php'; ?>
  <h1>Welcome Home!</h1>

  <?php include '../templates/navigation.php'; ?>

  <p>This is our home page.</p>

<?php include '../templates/foot.php'; ?>
```
* php include => warning
* php require => error

templates/head.php
```html

<body>
```

public/calculator.php
```html
<?php include '../templates/head.php'; ?>
  <h1>Calculator</h1>

  <?php include '../templates/navigation.php'; ?>

  <p>Welcome to the calculator form example.</p>

  <form method="GET" action="/calculator.php">
    <label>
      Number 1:
      <input type="number" name="num-1">
    </label>

    <label>
      Number 2:
      <input type="number" name="num-2">
    </label>

    <button type="submit" value="Calculate!">
  </form>

  <p><?php echo $_GET['num-1'] + $_GET['num-2'] = Answer</p>

   <pre>
      <?php var_dump($_GET) ?>
    </pre>

<?php include '../templates/foot.php'; ?>
```
* ```php include``` => warning
* ```php require``` => error

templates/head.php
```html

<body>
```

templates/foot.php
```html
  <footer>
    <h2>Web Site Footer</h2>
    <p>
      &copy;
      <?php echo date('Y'); ?>
      Lighthouse Labs
    <p>
  </footer>
  </body>
</html>
```

templates/navigation.php
```html
  <nav>
    <h2>Web Site Navigation</h2>
    <ul>
      <li>
        <a href="/index.php">Home</a>
      </li>
    <ul>
  </nav>
```

* `php -S localhost:8080 public`

### Read / Write
```php
// Read contents from web page or API
// can also use json_decode(), json_encode()
$websiteString = file_get_contents('http://duckduckgo.com/');

// Write to a file
$programmingLanguages = ['PHP', 'JavaScript', 'Ruby'];
$myString = "# List of Programming Languages\r\n";

foreach($programmingLanguages as $index => $language) { // Can access both index and value!
  $num = $index + 1;
  $myString .= "\r\n$num. $language";
}

file_put_contents('programming-languages.md', $myString);
```

