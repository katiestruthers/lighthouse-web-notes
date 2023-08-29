# Lecture 24: Class-based Components

### Review: Classes in JS
```js
class Person {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  printInfo = () => {
    console.log(`First name: ${this.firstName}`);
    console.log(`Last name: ${this.lastName}`);
    console.log(`Age: ${this.age}`);
  }
}

class Student extends Person {
  constructor(firstName, lastName, age, specialty) {
    super(firstName, lastName, age);
    this.specialty = specialty;
  }

  printStudentInfo() {
    this.printInfo();
    console.log(`Specialty: ${this.specialty}`);
  }

}

const student1 = new Student('Alex', 'Miller', 25, 'Web Development');
const student2 = new Student('Martha', 'Smith', 24, 'Software Engineering');
```

### React Example: Student Project
student-project/src/App.js
```js
import { Component } from 'react';
import './App.css';
import Student from './components/Student';
import Dog from './components/Dog';
import axios from 'axios';

class App extends Component {
  constructor(props) {
    super(props);  // Allows us to receive props, which can be accessed via this.props
    this.state = {
      students: [{
        firstName: 'Alex',
        lastName: 'Miller',
        age: 25
      }, 
      {
        firstName: 'Martha',
        lastName: 'Smith',
        age: 24
      }],
      title: 'Lighthouse Labs Web Development Course',
      count: 0,
      dogs: []
    }
    /* Equivalent to functional components and hooks
      const [firstName, setFirstName] = useState('Alex');
      const [lastName, setLastName] = useState('Miller');
      const [age, setAge] = useState(25);
    */
  }

  // Will trigger ONCE after page loads (like useEffect w/ [])
  componentDidMount() {
    axios.get('https://dog.ceo/api/breeds/image/random/5')
      .then((response) => {

      });
  }

  addOneToCount = () => {
    let currentCount = this.state.count;
    currentCount++;

    this.setState({count: currentCount});
  }

  addStudent = (event) => {
    event.preventDefault();

    const newStudent = {
      firstName: event.target.firstName.value,
      lastName: event.target.lastName.value,
      age: Number(event.target.age.value)
    }

    // Clean up the form values after adding the new student
    event.target.firstName.value = '';
    event.target.lastName.value = '';
    event.target.age.value = '';

    // const currentStudents = this.state.students;
    // currentStudents.push(newStudent);
    // this.setState({students: currentStudents});
    // ===
    this.setState({students: [...this.state.students, newStudent]});
  }

  render() {
    return (
      <div>
        <h1>Hey there, welcome to the {this.state.title} students app!</h1>
        <ul>
          {this.state.students.map((student, index) => {
            return (
              <Student 
                firstName={student.firstName}
                lastName={student.lastName}
                age={student.age}
                key={index}
              />
            );
          })}
        </ul>

        <h3>Counter Button</h3>
        <div>
          <p>Count so far: {this.state.count}</p>
          <button onClick={this.addOneToCount}>Click Here!</button>
        </div>

        <form onSubmit={this.addStudent}>
          <label htmlFor="firstName">First Name:
            <input id="firstName" name="firstName">
          </label>
          <label htmlFor="lastName">Last Name:
            <input id="lastName" name="lastName">
          </label>
          <label htmlFor="age">Age:
            <input id="age" name="age">
          </label>
          <button type="submit">
        </form>

        <h2>Dog Images from the API</h2>
        {this.state.dogImages.map((dogUrl, index) => {
          
        })}
      </div>
    )
  }
}

export default App;
```

student-project/src/components/Student.js
```js
import { Component } from 'react';

class Student extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
        <h2>Full Name: {this.props.firstName} {this.props.lastName}</h2>
        <p>Age: {this.props.age}</p>
      </div>
    )
  }
}

export default Student;
```

student-project/src/components/Dog.js
```js
const Dog = (props) => {
  return (
    <div>
      <img src={props.dogUrl} alt="An image of a dog">
    </div>
  )
};

export default Dog;
```