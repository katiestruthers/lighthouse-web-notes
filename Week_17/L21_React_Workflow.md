# Lecture 21: React Developer Workflow

What comes first - the data, or the structure?
* The data! Once we know what our data looks like, we can build the structure around it.

sampleData.js
```js
const sampleGroceryListItem = {
  id: 1,
  item: "Toilet Paper",
  quantity: 1,
  image: ""
}
```

* ffc - shortcut to create a component

GroceryListItem.jsx
```js
function GroceryListItem(props) {
  const [purchased, setPurchased] = useState(false);

  return (
    <li>
      {purchased && <h2>Purchased!</h2>}
      <h2>Item Name: {props.item}</h2>
      <h2>Quantity: {props.quantity}</h2>
      <img src={props.image}>
      <button>Purchase!</button>
    </li>
  )
}

export default GroceryListItem;
```

Where do we put images? Public vs. src
* src! Can do src/assets
* react only works in limited formats of images (it will give you an error if you use the wrong format)

App.js
```js
import { groceryListData } from '';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <GroceryList groceryList={sampleData}>
      </header>
    </div>
  )
}
```

* 2 locations for React apps: 1. your terminal, 2. the browser's DevTools console 
* for the unique key, never use the index of the array! Use something like id instead.

GroceryList.jsx
```js
import GroceryListItem from './components/GroceryListItem';

function GroceryList(props) {
  <div>
    <h1>Bryan's Grocery List</h1>
    {props.groceryList.map((element) => {
          return (<GroceryListItem
            key={element.id}
            item={element.item}
            quantity={element.quantity}
            image={element.image}
          />)
    })}
  </div> 
}
```

Continue to ask these two questions while building your React app:
1. What is the purpose of this component?
2. What is the purpose of the data?

Sample data -- small set of data just to see as an example
Mock data -- larger set of fake data