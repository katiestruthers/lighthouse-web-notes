# Mentor Assistance: $(document).ready()

If you want to apply jQuery to your webpage, you need to ensure the DOM is loaded & ready to go! If it's not, your jQuery will not effect it, as the elements you are attempting to target are not present when it runs.

You need to wrap all relevent code in a $(document).ready() block to ensure it runs after the DOM has loaded.

Two ways of doing so:
```js
// 1. A normal document.ready block
$(document).ready(function() {
...
});

// 2. A shorthad document.ready block
$(function() {
...
});
```

* See the [documentation](https://learn.jquery.com/using-jquery-core/document-ready/) for more info