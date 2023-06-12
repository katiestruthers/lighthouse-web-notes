# Reference: DOM Traversal using jQuery

A quick, non-exhaustive list of the tree traversal options as per the [jQuery documentation](https://api.jquery.com/category/traversing/tree-traversal/).

### Traverse Down (to the Children & their descendants)
* [.children([selector])](https://api.jquery.com/children/): get all children, or optionally get all children that match the selector
* [.find(selector or element)](https://api.jquery.com/find/): get all descedants that match the selector or element

### Traverse Up (to the Parents & their ascendants)
* [.parent([selector])](https://api.jquery.com/parent/): get immediate parent, or optionally only get if matches the selector
* [.parents([selector])](https://api.jquery.com/parents/): get all ascendants, or optionally only get the ones that match selector
* [.closest(selector or element)](https://api.jquery.com/closest): gets first ascendant that matches the selector or element

### Traverse Sideways (to the Siblings)
* [.siblings([selector])](https://api.jquery.com/siblings/): get all siblings, or optionally get the ones that match selector
* [.next([selector])](https://api.jquery.com/next): get the next sibling, or optionally only get if matches selector
* [.prev([selector])](https://api.jquery.com/prev): get the previous sibling, or optionally only get if matches selector

