# Lecture 25: Unit & Integration Testing

### Types of Testing
* Static testing / Linting
* Unit testing - Unit = function / components testing
* Integration testing - more than one function

Integration testing runs this whole spectrum - from 2 units all the way to E2E.
* Unit Testing <=====================> End-to-End (entire application)

* Regression testing - we fixed the bug, but did it break something else?
* E2E / A-Z - testing the whole app

### Tools for Testing
What we've used so far...
* Mocha - Test Runner
* Chai - Assertion Library

What we will use for React...
* Jest - Test Runner (lightweight)
  * Made by Meta, which is one of reasons that React-Jest are often paired (as React was also made by Meta)

### Structure of Tests
* ```npm test``` - run all tests

```\src\__tests__\01-first.test.js```
```js
describe('My First Tests', () => {
  it('performs a simple test', () => {
    expect(true).toBeTruthy();
  });
});
```
* can use .skip to to skip any tests, i.e. it.skip or describe.skip
* can use .only to only test that test (in that particular file -- will still run tests in other files)

### Unit Testing
```\src\__tests__\02-list.test.js```
```js
const { getFriendCount, removeFriend } = require("helpers/list");

describe('List Helper Tests', () => {
  // Mock data to test on
  const data = [
    {name: "James Holden", uid: "1"},
    {name: "Nathan Brown", uid: "2"},
    {name: "Fox Mulder", uid: "3"},
    {name: "Tom Cruise", uid: "4"}
  ];

  it('returns count of friends', () => {
    const count = getFriendCount(data);
    expect(count).toBe(4);
  });

  it('removes unwanted friend', () => {
    const tom = {name: "Tom Cruise", uid: "4"};
    const bestFriends = removeFriend(data, "4");

    expect(bestFriends.length).toBe(3);
    expect(bestFriends).not.toContainEqual(tom);
  });
});
```
* So far, not looking too different from Mocha & Chai tests!

### Testing Functions / Components
* Need to add a Jest testing library in order to test React components
  * We will be using [Testing Library](https://testing-library.com/), which supports many front-end libraries / frameworks
  * This is automatically installed when you use ```npx-create-react-app```

```\src\__tests__\03-FriendList.test.js```
```js
const { render, prettyDOM } = require("@testing-library/react");
const { default: FriendList } = require("components/FriendList");

// Mock data to test on
const data = [
  {name: "James Holden", uid: "1"},
  {name: "Nathan Brown", uid: "2"},
  {name: "Fox Mulder", uid: "3"},
  {name: "Tom Cruise", uid: "4"}
];

describe('FriendList Tests', () => {

  it('renders without props without crashing', () => {
    render(<FriendList />);
  });

  it ('renders with data', () => {
    const {container} = render(<FriendList items={data}/>);
    console.log(prettyDOM(container)); // Manually check what the DOM looks like
  });
});
```
* Common for testing libraries to be *self-asserting*, meaning you don't need to specify if something happened or not
  * i.e. render() will return true if rendered, false otherwise
* So now we know if it renders, but we can't yet test WHAT it's rendering without manually looking at the console.log

### Querying for Elements
* Types of queries: get, find, & query (we mostly use *get*)
* To see all the options available, [read the docs on queries](https://testing-library.com/docs/queries/about)
* Generally, don't test for id - test for role, title, alt text, etc.

```\src\__tests__\03-FriendList.test.js```
```js
const {render, prettyDOM, getAllByRole, getByText} = require("@testing-library/react");
//...

describe('FriendList Tests', () => {
//...
  it('renders a list with listItems', () => {
    const {debug, container} = render(<FriendList items={data}/>);

    // Test if the list exists
    const list = getByRole(container, "list");

    // Test that there are four listItems
    const items = getAllByRoles(container, "listItem");
    expect(items.length).toBe(4);

    // Test that Nathan is listed
    const nathan = getByText(container, "Nathan Brown");
  });
});
```
* list and listItems are examples of *roles*
  * we do not test that they are ul & li elements, but rather, we test via their respective roles
* to match HTML elements with their respective roles, [read the docs on roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles)

### Test Cycle: Render - Query - Action - Expect
```\src\__tests__\03-FriendList.test.js```
```js
const {render, prettyDOM, getAllByRole, getByText, fireEvent} = require("@testing-library/react");
//...

describe('FriendList Tests', () => {
//...
  it('calls deleteItem when item is clicked', () => {
    const mockDelete = jest.fn();

    const {debug, container} = render(
      <FriendList 
        items={data}
        deleteItem={mockDelete}
      />
    );

    const tom = getByText(container, "Tom Cruise");
    fireEvent.click(tom);

    // Was the event called?
    expect(mockDelete).toHaveBeenCalled();

    // Was the right element deleted?
    expect(mockDelete).toHaveBeenCalledWith("4");

    // Was it only called once?
    expect(mockDelete).toHaveBeenCalledTimes(1);
  });
});
```

### Integration Testing
```\src\__tests__\04-App.test.js```
```js
const { default: App } = require("App");
//...

describe('App tests', () => {
  it('Renders App without crashing', () => {
    render(<App />)
  });

  it('can type into the input field and add friend', () => {
    const {container, debug} = render(<App />);

    // Textbox exists
    const input = getByRole(container, "textbox");

    // Textbox value changes on typing
    fireEvent.change(input, { target: {value: "Nathan Brown"} });

    // The text change is expected
    getByDisplayValue(container, "Nathan Brown");

    // Friend is added on button-click
    const addButton = getByText(container, "Add Item");
    fireEvent.click(addButton);
    getByText(container, "Nathan Brown");
  })
});
```