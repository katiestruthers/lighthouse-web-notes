# Code Review: Tweeter

The page doesn't work if the user submits long text with no spaces in-between, aka ["overflowing text"](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_text/Wrapping_breaking_text). 

!["Overflowing text on a post"](../images/Week_11_Tweeter_Before.png)

### Solution: 
```css
word-break: break-all;
```

!["Wrapping text on a post"](../images/Week_11_Tweeter_After.png)
