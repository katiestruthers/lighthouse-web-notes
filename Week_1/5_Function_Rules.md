# 5 Rules for Defining Functions
1. Give your functions precise verb-based names
2. Use camelCasedNames
3. Use proper indendation
4. Functions should either return a value _or_ cause a side effect (not both!)
5. Data needed by functions should be passed in as parameters

### Example:
```javascript
const joinList = function(array) {
  let string = "";

  for (let i = 0; i < array.length; i++) {
    string += array[i];

    if (i < array.length - 1) {
      string += ", ";
    }
  }

  return string;
};