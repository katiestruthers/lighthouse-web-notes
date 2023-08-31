# Lecture 26: E2E Testing with Cypress

### Types of Tests
* Unit: testing surrounding a 'unit' - often a function / component running in isolation
* Integration: testing how units work *together*
* End-to-End: simulates user behaviour to evaluate if user stories are present and working

### Jest
* Testing framework (intended largely for React)
* CLI tool
* Automated testing

### Pros for Jest
* Very fast / efficient
* Once test are written, saves a lot of time
* Built specifically for React components
* **Capable of both at unit / integration testing**

### Cypress
* JavaScript web testing and component testing framework
* GUI application
* Cypress deploys a web browser (electron/chromium) in order to simulate a user walking through your website's features
* Learn more via [Cypress documentation](https://www.cypress.io/)

### Pros for Cypress
* Easy-to-use GUI makes for a simpler development experience
* Getting a good view of the user experience during testing
  * Cypress is also capable of providing screenshots / video recordings
* Widely used tool, very mature / premium, and there is support
* Opens the door to responsive testing, checking certain CSS styles, etc. is present in the page
* Able to test any page we can request in a browser (not built primarily for React, like Jest)
  * We don't necessarily need access to the source code, which makes it useful for testing any stack web app
* **Capable of both integration and end-to-end testing**

### Integration Testing
* In an ideal world, we would use both Jest & Cypress
* If you can only use one...
  * Jest: lots of custom hooks
  * Cypress: mostly relying on packages, just need to see the end-product to see if components are working together 

### Testing our Album Search App
* ```npm install cypress```
* add to scripts in package.json: ```cypress: "node_modules\\.bin\\cypress open```

```cypress\integration\01-cypress.spec.js```
```js
describe('tests for cypress', () => {

  it('cypress runs', () => {
    assert.strictEqual(true, true);
  });

  it('can visit a web page', () => {
    cy.visit('https://google.com');
  });

  it('can visit my local React app', () => {
    cy.visit('http://localhost:3000');
  });

});
```

* cypress.json &rarr; ```"baseUrl": "http://localhost:3000"```

```cypress\integration\02-checkboxes.spec.js```
```js
describe('tests for checkboxes', () => {

  // Will run before each test
  // '/' will take us to our baseUrl we set in cypress.json
  beforeEach(() => cy.visit('/'));

  it('can uncheck the "Explicit" checkbox', () => {
    cy.get('#Explicit')
      .uncheck()
      .should('not.be.checked'); // Include an assertion to confirm it is unchecked!
  });

  it('can check the "Single" checkbox', () => {
    cy.get('#Single')
      .check()
      .should('be.checked');
  });

});
```

```cypress\integration\03-search-input.spec.js```
```js
describe('tests for the search input form field', () => {
  beforeEach(() => {
    cy.visit('/');

    // Create an alias to keep the code DRY  
    cy.get('[name="search"]')
      .as('searchInput');
  });

  it('can type into the search input field', () => {
    const textValue = 'Weeknd';

    // @ allows you to access the alias
    cy.get('@searchInput') // Reminder -> [] is a CSS attribute selector
      .type(textValue, {delay: 500}) // More human-like typing speed delay
      .should('have.value', textValue);
  });

  it('can type into the search input field', () => {
    // can use keyboard keys like {backspace} to better simulate a user
    const textValue = 'Justinnnnn{backspace}{backspace}{backspace}{backspace} Bieber';
    const expectedValue = 'Justin Bieber';

    cy.get('@searchInput') 
      .type(expectedValue, {delay: 500}) 
      .should('have.value', textValue);
  });

});
```

* itunes.json &rarr; raw JSON file from one of the results
  * better to test this sample data than data from an external API, as the API data is prone to change!

```cypress\integration\04-api.spec.js```
```js
// Don't necessarily need describe() block, esp. if only one test
it('can display API results after a search', () => {

  // Visit the app
  cy.visit('/');

  // Set up a fixture
  cy.intercept(
    'GET', // METHOD
    '**/search*', // URL/PATH PATTERN
    {fixture: 'itunes', delay: 800} // CONGIG OBJECT
  ).as('itunesResponse');

  // Type in search input
  cy.get('[name="search"]')
    .type('The Pretty Reckless', {delay: 180});

  // Confirm spinning wheel shows on page
  cy.get('.spinner')
    .should('be.visible'); 

  // Wait until we receive API results
  cy.wait('@itunesResponse');

  // Assert that a particular album is present
  cy.get(':nth-child(5) > .album__info > .album__name') // Got this via API Selector Playground on Cypress GUI
    .contains('Death By Rock And Roll (Commentary Version)');
    .should('be.visible');

})
```