# Tutorial: Intro to React
- [https://reactjs.org/tutorial/tutorial.html](https://reactjs.org/tutorial/tutorial.html)
- Completed Apr 26, 2020
---
## Setup
- Install Node.js
- Create React App project.
```
npx create-react-app my-app
```
- Delete all of the files in the */src* folder.
- Create the following files in the */src* folder (and use the contents from the website).
  - *src/index.css*
  - *src/index.js*
- Add the following three lines to the top of *src/index.js*.
```
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
```
- From the project folder run the following command to start the server.
```
npm start
```
- This should launch the url [http://locahost:3000](http://locahost:3000) in your browser that will display the app.

## What Is React?
- React is a declarative, efficient, and flexible JavaScript library for building user interfaces.
- 
### Components
- React has a few different kinds of components, such as React.Component.
- A component takes in parameters, called props (short for “properties”).
- A component returns a hierarchy of views to display via the render method.
  - The **render** method returns a description of what you want to see on the screen.
```
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />

// The above example is equivalent to:
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

### React element
- A lightweight description of what to render that is returned from the component **render** method.
- React elements are mostly written in **JSX** which is JavaScript embedded in HTML.
  - For example, the \<div /> syntax is transformed at build time to React.createElement('div').
- You can put any JavaScript expressions within braces inside JSX.
- You can compose and render custom React components.

### this.state
- Should be considered private to a component.
- Initialize the state in the component **constructor** method.
- All React component classes that have a constructor should start with a super(props) call.
```
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

### When you call setState in a component, React automatically updates the child components inside of it too.

## Completing the Game

### The best approach is to store the game’s state in the parent Board component instead of in each Square.
- To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. 
- The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.
```
class Board extends React.Component {
  constructor() {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  renderSquare(i) {
    return <Square value={this.state.square[i]} />;
  }
...
```

- We pass down a function from the Board to the Square, and we’ll have Square call that function when a square is clicked.
```
renderSquare(i) {
  return (
    <Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />
  );
}
```

### When a square is clicked...
- Replace this.state.value with this.props.value in Square’s render method.
- Replace this.setState() with this.props.onClick() in Square’s render method.
- Delete the constructor from Square because Square no longer keeps track of the game’s state.
- The Square is now a **controlled component** since it receives values from the Board component and no longer maintains state.
```
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
```
- We could give any name to the Square’s onClick prop or Board’s handleClick method, and the code would work the same.
- In React, it’s conventional to use on[Event] names for props which represent events and handle[Event] for the methods which handle the events.


### The Board handleClick method
- Note how in handleClick, we call .slice() to create a copy of the squares array to modify instead of modifying the existing array. 
  - By not mutating (or changing the underlying data) directly, we gain several benefits.
    - Immutability makes complex features much easier to implement.
      - Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later.
    - Detecting changes in mutable objects is difficult because they are modified directly.
    - The main benefit of immutability is that it helps you build pure components in React.
      - Immutable data can easily determine if changes have been made which helps to determine when a component requires re-rendering.



```
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }
```

### Function Components
- In React, **function components** are a simpler way to write components that only contain a render method and don’t have their own state.
- To change the Square to a function component.
- Note that we pass the props to the function so we no longer need to use **this.props**.
- Note that we can change *onClick={() => this.props.onClick()}* to *onClick={props.onClick}*.
```
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

## Adding Hitory
- Since out **squares** array is immutable (that is, we copy it instead of modifiying it) it is fairly easy to implement a move history.
- We just need to store the past squares arrays in another array called history.
  - We will need to add the history array to the top-level Game component.
```
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }
  ...
```
- We'll need to do the following:
  - Delete the constructor in Board.
  - Replace this.state.squares[i] with this.props.squares[i] in Board’s renderSquare.
  - Replace this.handleClick(i) with this.props.onClick(i) in Board’s renderSquare.

### Unlike the array **push()**, the **concat()** method doesn’t mutate the original array, so we prefer it.

### Warning: Each child in an array or iterator should have a unique "key" prop.
- Because React cannot know our intentions, we need to specify a key property.
- It’s strongly recommended that you assign proper keys whenever you build dynamic lists. 
  - If no key is specified, React will present a warning and use the array index as a key by default.
