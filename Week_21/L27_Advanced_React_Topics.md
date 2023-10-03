# Lecture 27: Advanced React Topics

### React Routers
* ```npm install react-router-dom```
* Simulate a multi-page app with React

### React Context
* A feature of React (nothing to install!)
* Allows us to access props without worrying about passing through all the relevent children

### Example App to Demonstrate Concepts
index.js
```js
//...
import { BrowserRouter } from 'react-router-dom';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

App.js
```js
//...
import { useEffect, useState, createContext } from 'react';
import { Route, Routes, Link } from 'react-router-dom';
import Home from './components/Home';
import User from './components/User';

function App() {
  const [users, setUsers] = useState([]);
  const [loggedIn, setLoggedIn] = useState(true);

  useEffect(() => {
    // Assume that an API request is done to a server
    const loadUsers = () => {
      const fetchedUsers = [{
        firstName: "Alex",
        lastName: "Miller",
        id: 123
      },
      {
        firstName: "Marth",
        lastName: "Smith",
        id: 456
      }];
    };
  }, []);

  const valuesForContext = { users, loggedIn };

  return (
    <UsersContext.Provider value={valuesForContext}>
      <div className="App">
        <h1>Routes Project</h1>

        <div className="navigation">
          <Link to="/home">Home</Link>
          <Link to="/about">About</Link>
        </div>

        <Routes>
          {/* use element if using ready-made component */}
          <Route path="/home" element={<Home />}>
          <Route path="/userinfo/:userId" element={<User users={users}>}>

          {/* use Component if writing in-line JSX */}
          <Route path="/about" Component={
            return (loggedIn) ? <div>Welcome Back!</div> : <div>You need to login first.</div>;
          }>
        </Routes>
      </div>
    <UsersContext.Provider />
  );
};

export default App;
```

Home.js
```js
import { useState, useContext } from 'react';
import { useNavigate } from 'react-router-dom';

const Home = () => {
  const navigate = useNavigate();

  const [searchInput, setSearchInput] = useState('');
  const updateSearchInput = (e) => {
    setSearchInput((prev) => e.target.value);
  };
  const goToUser = (e) => {
    e.preventDefault();
    navigate()
  };

  return (
    <div>
      <h2>Welcome to the users-routes project</h2>
      <form>
        <label htmlFor="searchInput">Which user id do you want to search for?</label>
        <input type="number" id="searchInput" name="searchInput" onChange={(e) => updateSearchInput(e)}/>
        <button type="submit">Search</button>
      </form>
    </div>
  )
};

export default Home;
```

User.js
```js
import { useParams } from 'react-router-dom';

const User = (props) => {
  const { userId } = useParams();
  const currentUser = props.users.filter((user) => user.id === Number(userId));

  if (currentUser.length > 0) {
    return (
      <div>
        <h2>User name: {currentUser[0].firstName} {currentUser[0].lastName}</h2>
        <p>User id: {userId}</p>
      </div>
    );
  } else {
    return (
      <div>That user doesn't exist</div>
    );
  }
};

export default User;
```

UsersContext.js
```js
const UsersContext = createContent({
  users: [],
  isLoggedIn: false,
  login: () => {}
})

export default UsersContext;
```