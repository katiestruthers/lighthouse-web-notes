# Stretch: SASS
* SASS: Syntactically Awesome Style Sheets
* Browser doesn't understand SASS (it only understands HTML, CSS, & JS), so we will write in SCSS to convert it into CSS
* Sassy CSS (SCSS) is a superset of CSS
  * A superset takes all of the original language and adds some additional functionality on top (i.e. Typescript is a superscript of JS)
* Feed raw SCSS into a compiler, which will then spit out CSS
  * styles.scss => compiler => styles.css

### How to Get Started with SASS
* See the [SASS Documentation](https://sass-lang.com/) for full instructions.
  1. Install SASS
  2. sass input.scss output.css --no-source-map
* All valid CSS is valid SCSS

### Creating SASS Variables
* Anything that can go on the right-side of the colon in CSS can go into a variable (incl. other variables).

input.scss
```SCSS
$primary: white;
$border-color: lightgray;
$border: 2px solid $border-color;

body {
  color: $primary;
  border: $border;
}
```

output.css
```CSS
body {
  color: white;
  border: 2px solid lightgray;
}
```

### SASS Partials
* Import a file with all your variables so that you can use them on each stylesheet!
* **Convention:** all partials start with an underscore

_variables.scss
```SCSS
$primary: white;
$border-color: lightgray;
$border: 2px solid $border-color;
```

input.scss
```SCSS
/* import 'variables'; is also valid, and perhaps more common */
import '_variables';

body {
  color: $primary;
  border: $border;
}
```

### SASS Nesting
* Perhaps a more intuitive way of writing CSS styles!

input.scss
```SCSS
div {
  font-size: 24px;

  p {
    color: hotpink;
  }

  article {
    text-decoration: underline;

    h2 {
      font-weight: bold;
    }
  }
}
```
output.css
```CSS
div {
  font-size: 24px;
}
div p {
  color: hotpink;
}
div article {
  text-decoration: underline;
}
div article h2 {
  font-weight: bold;
}
```

### SASS Mixin
input.scss
```SCSS
@mixin box ($num, $color) {
  width: $num * 50px;
  height: $num * 50px;
  border-color: $color;
}

.small-box {
  @include box(1, hotpink);
}

.medium-box {
  @include box(2, yellowgreen);
}
```

output.css
```CSS
.small-box {
  width: 50px;
  height: 50px;
  border-color: hotpink;
}

.medium-box {
  width: 100px;
  height: 100px;
  border-color: yellowgreen;
}
```