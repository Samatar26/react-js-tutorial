When you want to change a property within your state, you call the setState method and pass in an object with the keys you want to update along with their values.
In order for our method to update the state. We have to reference it in our constructor and bind it to the this keyword. This ensure that when we call this.setState inside our method, it refers to the correct context. We also have to add an onchange handler to our input.

currentTodo is now part of our state and as we type a character, the currentTodo in our state is updated to reflect the current value of our input. We have ensured that our rendered data and our state are in sync.

```js
class App extends Component {
  constructor() {
    super();
    this.state = {
      todos: [
        {id: 1, name: 'Learn JSX', isComplete: false},
        {id: 2, name: 'Build an Awesome App', isComplete: false},
        {id: 3, name: 'Help Everyone', isComplete: false},
      ],
      currentTodo: '',

    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange (e) {
    this.setState({
      currentTodo: e.target.value
    });

  }

render(){
  return(
    <input type="text" value={this.state.currentTodo} onChange={this.handleInputChange}/>

  )
}
```

### Create a Stateless functional component for an input form

In order to access the things we defined on our app inside of our stateless functional component we need to pass that down through 'props'. We also have to pass these props into the TodoForm component.


```js
import React from 'react';


export const TodoForm = (props) => (
  <form>
    <input type="text"
      onChange={props.handleInputChange}
      value={props.currentTodo} />
  </form>

);

//inside of our app.js
<TodoForm currentTodo={this.state.currentTodo} handleInputChange={this.handleInputChange}/>


```

When looping through an array, you want the component to be directly in the loop or else you'll receive an error about looping over items without a unique key prop. So when looping over items you have to set the unique key within the loop:

```js
import React from 'react';

import {TodoItem} from './TodoItem';

export const TodoList = (props) => {
  return (
    <div className="Todo-List">
      <ul>
        {props.todos.map(todo=> <TodoItem key={todo.id} {...todo}/>)}
      </ul>
    </div>
  );
};
```

### Validating component input with Prop Types

Our todoform components represents a form that we can use to create new todo items. It functions by receiving a prop that is used to set the value of the input and a function and that's how we pass event information, input change events in this case to its parent component.

If someone else were to use our component in another part of the application, we want to ensure they're passing in the correct props. React gives us the ability to validate our components using prop types. If you were to pass in something other than the expected type, the person will receive a warning.

Accessing propTypes from the React object seems to be deprecated, so you can install an npm package called prop-types.

```js
import React from 'react';
import PropTypes from 'prop-types';

export const TodoForm = (props) => (
  <form>
    <input type="text"
      onChange={props.handleInputChange}
      value={props.currentTodo} />
  </form>

);

TodoForm.propTypes = {
  currentTodo: PropTypes.string.isRequired,
  handleInputChange: PropTypes.func.isRequired
};

```

Adding an 'isrequired' means that the property has to be passed into the components to avoid a warning in the console.

### Add data to a list without mutations

With the test below we are making sure that our startTodos and our result is not the same array. When using concat vs push it will add the item to a new array and return it.

We can also use the spread operator and because our tests are passing, we know it doesn't mutate the original array and adds the new item to our list.

```js
test('addTodo should not mutate the existing todo array', () => {
  const startTodos = [
      {id:1, name: 'one', isComplete: false},
      {id:2, name: 'two', isComplete: false},
  ];

  const newTodo = {id:3, name: 'three', isComplete: false};
  const expected = [
    {id:1, name: 'one', isComplete: false},
    {id:2, name: 'two', isComplete: false},
    {id:3, name: 'three', isComplete: false}
  ];

  const result = addTodo(startTodos, newTodo);

  expect(result).not.toBe(startTodos);

});
//Helper functions
export const addTodo = (list, item) => {
  return list.concat(item);
};

//Cleaner way
export const addTodo = (list, item) => [...list, item];



```

### Update React application state from Form input
The reason we generate a random id, is because let's say someone deletes a todo in the middle of the list. And you decide to add a todo, based on the length of the todos, you would end up with the same id for that todo as the one before it and keys have to be unique!

```js

handleSubmit(e) {
  e.preventDefault();
  const newId = generateId();
  const newTodo = {id: newId, name: this.state.currentTodo, isComplete: false };
  const updatedTodos = addTodo(this.state.todos, newTodo);
  this.setState({
    todos: updatedTodos,
    currentTodo: '',
  });
  console.dir(e.target.firstChild.value);
  // addTodo(this.state.todos, );
}

```

### Prevent empty form values with conditional submit handlers
Instead of using an if/else statement we can define a function that is invoked when the currentTodo is falsy. We have also put it at above the form as a helpful error message.


```js
handleEmptySubmit(e) {
  e.preventDefault();
  this.setState({
    errorMessage: 'Please supply a todo name!'
  });
}

//label

render() {
  const submitHandler = this.state.currentTodo ? this.handleSubmit : this.handleEmptySubmit;
  return (
    <div className="App">

      <div className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <h2>React Todos</h2>
      </div>

      <div className="Todo-App">
        {this.state.errorMessage && <span className='error'>{this.state.errorMessage}</span>}
        <TodoForm currentTodo={this.state.currentTodo}
          handleInputChange={this.handleInputChange}
          handleSubmit={submitHandler}/>
        <TodoList todos={this.state.todos}/>
      </div>


    </div>
  );
}
}

```

### Autobinding
React.createClass has a built-in magic feature that bounds all methods to 'this' automatically. So instead of doing this:

```js
class Counter extends React.Component {
  constructor() {
    super();
    this.tick = this.tick.bind(this);
  }
  tick() {
    ...
  }
  ...
}
```

You can do this:
```js
class Counter extends React.Component {
  state = {
    todo: [
      {id: 1, name: 'Sam', isComplete: false},
    ].
  }
  tick = () => {
    ...
  }
  ...
}
```
