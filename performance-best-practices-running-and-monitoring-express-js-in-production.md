# 5 Front End Interview Questions That Every NEW Developer Should Know

- Program With Erik
- [https://www.youtube.com/watch?v=z6cvj6cMIr0](https://www.youtube.com/watch?v=z6cvj6cMIr0)
- Oct 3, 2018
- Completed Apr 16, 2020

# 1. Life Cycle Hooks of React, Vue, Angular, Ember, etc.

- The basics of how things load and when things happen at different places.
- **React**
  - **constructor()** - Called before it is mounted.
  - **componentDidMount()** - Invoked immediately after a component is mounted (inserted into the tree).
  - **componentDidUpdate()** - Invoked immediately after updating occurs.
  - **componentWillUnmount()** - Invoked immediately before a component is unmounted and destroyed.
- **Vue**
  - **beforeCreate** - Called synchronously immediately after the instance has been initialized, before data observation and event/watcher setup.
  - **created** - Called synchronously after the instance is created.
  - **beforeMount** - Called right before the mounting begins: the render function is about to be called for the first time.
  - **mounted** - Called after the instance has been mounted, where el is replaced by the newly created vm.\$el.
  - **beforeDestroy** - Called right before a Vue instance is destroyed. At this stage the instance is still fully functional.
  - **destroyed** - Called after a Vue instance has been destroyed.
- **Angular**
  - **ngOnInit()** - Called after Angular has initialized all data-bound properties of a directive.
  - **ngOnDestroy()** - Called just before Angular destroys the directive/component.

# 2. CSS

- Selectors - class, id, nth element, first child (How to combine selectors)
- box-sizing, border-box
- display none, hidden, float, etc.

# 3. Promises

- An object that may produce a value sometime in the future.
- Three states for a promise:
  1. Fulfilled
  2. Rejected
  3. Pending
- **async/await**
  - An async function can contain an await expression that pauses the execution of the async function to wait for the passed Promise's resolution
- **Observables**
- **callbacks**

# 4. Scoping

- Differences between
  - **var** - global (or function) scope, depending on where it is
  - **const** - block scoping
  - **let** - block scoping (You can't reassign it but you can change properties inside of it.)

## Closures

## Prototypical inheritance vs. ES6 classes

## Currying and hoisting

- **currying** - A transformation of functions that translates a function from callable as f(a, b, c) into callable as f(a)(b)(c).
- **hoisting** - JavaScript's default behavior of moving declarations to the top.

# 5. White Board String Manipulation, Palindromes, Fibonacci

- reversing a string
- doing a palindrome
- Fibonacci sequence
- fizzbuzz

# Other Question Topics

- **Network connectivity**
- **RESTful interfaces**
- **HTTP verbs**
- **Structuring an application**
