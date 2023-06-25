# Lecture 15: Responsive Design & SASS

### What is Responsive Design?
* Making sure a website looks good on a variety of devices
* Accessibility - how well does my website translate (or not) to assistive devices? Can a colourblind person read my funky font + background colour combo? & many, many other questions!

### Viewport Meta Tag
* You need to include this line of code in the head tag on EVERY WEBPAGE you create!
* It is essential for creating good mobile design.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
* If you create every HTML page using the Emmet abbreviation ('!' in VSCode), this line of code will already come included in the boilerplate code.

### Height vs. Width Options
* Height & Width set the default
* Also can set the Max-Height / Min-Height, Max-Width / Min-Width
  * Use a combo of these to better control the shrinking / growing!

### Absolute vs. Relative Units
* Absolute - px, pt, cm, in
  * These remain the same, no matter the screen size, parent element, etc.
* Relative - %, vw/vh, em/rem
  * These vary based on screen size, parent elements, etc.

#### Relative to what?
* % &rarr; relative to the size of the parent element
* vw / vh &rarr; relative to the size of the viewport width / height
* em &rarr; relative to the font size of the parent element
* rem &rarr; relative to the font size of the root element (i.e., the html element)
  * If you haven't set an explicit font size on the html, the default is 16 px

#### Quick Tip: Width vs. Height Percentages
* Width percentages will pretty well always work
* However, unlike width percentages, height percentages will not work unless the parent element has an actual height that you have applied to it

### Media Queries
Ask two questions about the device being used to show the page
  1. Media Type: screen, print, speech
  2. Media Features: aspect-ratio, orientation (landscape vs. portrait), light-level (how light is the room you are in?), max-width/min-width, max-height/min-width

**TIP:** Put all media queries at the bottom of your CSS style sheet. That way, you have your default styles on top, with all your specifics being applied at the end.
* Apply this to "Mobile-First" design and make the defaults at the top be catered to mobile

```css
@media only screen and (min-width: 500px) {
}

/* Go smaller to bigger! If you do the opposite, the bigger sizes will get overwritten. */
@media only screen and (min-width: 800px) {
}

/* Or, if you don't want to worry about order, just be sure to set both a min and a max. */
@media only screen and (min-width: 600px) and (max-width: 700px) {
}
```