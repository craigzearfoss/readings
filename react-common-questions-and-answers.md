# React - Common Questions (and Answers!)

- Academind
- [https://www.youtube.com/watch?v=XI27nSXSrYs&t=9s](https://www.youtube.com/watch?v=XI27nSXSrYs&t=9s)
- Jan 17, 2018
- Completed Apr 23, 2020

---

## Do I Need a Complex Setup / Workflow?

- **Drop-in**
  - Just import react.js and react-dom.js
  - Great for multi-page applications where React should not manage the entire front end and for smaller project.
- **create-react-app**
  - Feature-rich workflow with zero setup (100% customizable!)
  - Good for bigger projects and single page applications.
- **Custom Workflow**
  - Tailor your own workflow/wokflow integration
  - Good for bigger multi-page apps or highly specific requirements

---

## {} vs {{}}

- React uses single curly braces because it just uses JSX, which is enhanced JavaScript, so you do not need a templating engine.
- For example, in React we use the **.map** method to map an array to an array of JSX elements instead of using things like directives.

---

## Functional vs "class" Components

- **Functional** (stateless)
  - Receives props as an argument.
  - No internal state.
  - No lifecycle methods.
  - Use as often as possible.
- **Class-based** (stateful)
  - Access props vis this.props.
  - Manages internal state (this.state).
  - Uses lifecycle methods.
  - Use whenever you need to manage state inside of a component.
  - Use only as containers (for stateless components).
  - If you manage your state in a only few selected stateful components it is easier to manage.
  - Try to keep the use of class-based stateful components to a minimum.

---

## setState Syntaxes

### Syntax 1

```
setState({ counter: this.state.counter + 1 })
```

- Note that setState runs asynchronously so that in Syntax 1 the old state updates might not be reflected in state yet.

### Syntax 2 (pass a function)

```
setState((prevState, prevProps) => {
  return{
    counter: prevState.counter + 1;
  }
})
```

- In Syntax 2 we are always guaranteed to get the last valid state.

---

## What does map() do?

- It maps an array into a new array, transforming every element as needed.
  - map(element => { return newElement })

```
this.props.persons.map(person => <p>{person.name}</p>)
```

- The goal is to output JSX elements ("ngFor", "v-for").
- The above will map the array ['Max', 'Anna', 'Manu'] to ['<p>Max</p>', '<p>Anna</p>', '<p>Manu</p>']

---

## Styling React

- **Inline Styles** - you add the styles directly on the JSX elements with the style props.
  - This can become cumbersome if you have more complex styles and it doesn't work for media queries.
- **Radium** - a package the allows you to use media queries and more css-like features.
- **Style Components** - a package the uses the idea of scoped styles and full css features.
- **BEM**(block, element, modifier) - a front-end naming methodology that uses css class names for scoping.
- **CSS Modules** - feature you can unlock in create-react-app. Transforms your normal css class names to component-unique class names.

---

## Adding Third-Party CSS Libraries

- Use CDN (\<link> in index.html)
- Use local file (\<link> in index.html)
- Use local file (import in JS Files)
- Use npm/yarn (import in JS Files)

---

## Adding Third-Party JavaScript Libraries

- Use CDN (\<script src> in index.html)
- Use local file (\<script src> in index.html)
- Use local file (import in JS Files)
- Use npm/yarn (import in JS Files)

---

## Immutability

- Objects and arrays are reference types in JavaScript.
  - That means that when storing an array in a new variable, you don't create an actual copy.
    - You only create a copy of a pointer to some point in memory.
- The solution is to manually clone any array/object.
- For **arrays** the solutions are:
  - You can use the **slice()** method which creates a brand new array.
  - In ES6 you can also use the spread operator **...**.

```
const oldArray = [1, 2, 3];
const newArray = oldArray;  // Doesn't work

const clonedArray = oldArray.slice();
// or use
const clonedArray2 = [...oldArray];
```

- For **objects** the solutions are:
  - You can use the **assign()** method which creates a brand new object.
  - In ES6 you can also use the spread operator **...**.

```
const oldObject = {name: 'Max'};
const newObject = oldObject;  // Doesn't work

const clonedObject = Object.assign({}, oldObject);
// or use
const clonedObject2 = {...oldArray};
```

### The above examples only use **Shallow Cloning**.

- That is, they only clone the top level.
- The solution is to clone all of the levels that you plan on changing.
- Shallow cloning example:

```
const oldObject = {name: 'Max', hobbies: ['Sports']};

const clonedObject = Object.assign({}, oldObject);
clonedObject.hobbies.push('Cooking');  // still edits the original array in oldObject
```

- To perform a deep clone of the oldObject in the previous example.

```
const clonedObject = {...oldObject, hobbies: [...oldObject.hobies]};
```

---

## Everyone Can See My Code!

- Don't put any security related information or anything that you don't want someone to see in your code.

---

## React without Redux?

- It's a popular combination but Redux is not required.
- Redux is useful for bigger apps because it makes managing state easier.

---

## React + PHP/Laravel/Node/Ruby/...

- The way you interact with the server differs so you can use any language you want.

### SPA (Single Page Appllcation)

- React controls the entire frontend, there's only one index.html file sent from the server.

```
       +-------------------+
       |     React App     |
       +-------------------+
          ^         |   ^
          |         v   | Data
index.html|     +----------+
          |     | REST API |
       +-------------------+
       |     Server        |
       +-------------------+
```

### MPA (Multi-Page Appllcation)

- React controls only parts ("widgets") of the server-side rendered views.

```
       +-------------------+    +---------+
       |     React App     |<-->|  React  |
       +-------------------+    +---------+
          ^           | Requests
Rendered  |           | yield new
by Backend|           v
       +-------------------+
       |     Server        |
       +-------------------+
```

---

## My State Is Lost After Page Reloads

- Since React state is stored on the client there is no session management.
- Two options to store state across page reloads (This is only referring to SPAs):
  1. localStorage
     - Client-side (not under your control, ie. you can't access it from the server)
     - Accessible by JavaScript
     - Simple key-value store
     - Great for storing things like a JSON web token.
     - You can store any information that you want to access on the next app start.
  2. Server-Side Database
     - Server-side (under your control)
     - Accessible only from the server
     - Can be any database that you want
     - The right solution for data that must be persisted for a long time.

---

## Can you host your app on Heroku, etc?

- For single page apps:
  - React is just a bunch of build artifacts generated by your workflow and deployment process.
    - index.html
    - app.js
    - styles.css
  - These are just static files so there is no server-side code.
  - Since they are only static files, Heroku might be overkill.
    - A better solution would be to deploy to AWS S3, Firebase Hosting or some other static host.
- For multi-page apps:
  - You can just deploy with any language that is supported.

---

## My Routes Don't Work After Deployment

- For single page apps there are no routes on the server, they all exist in the client React app.

```
            +-----------------------+    +-------------------+
            |     User (Client)     |<---|  React App (SPA)  |
            +-----------------------+    +-------------------+
                       |                          ^
   https://my-page.com |                          |
                       |                          |
                       v                          |
            +-----------------------+             |
            |       Server          |-------------+
            +-----------------------+
```

- If you hit refresh you really send a new request to the server and you'll get a 404 error back from the server.

```
                  +-----------------------+    +-------------------+
                  |     User (Client)     |<---|  React App (SPA)  |
                  +-----------------------+    +-------------------+
                             |     ^                + -------+
https://my-page.com/products |     |404 Error       | Routes |
                             |     |                +--------+
                             v     |
                  +-----------------------+
                  |       Server          |
                  +-----------------------+
                        +-----------+
                        | No Routes |
                        +-----------+
```

- So set the server up so that it alway returns the index.html file.
  - Then React can take over and render the correct page.

```
                  +-----------------------+    +-------------------+
                  |     User (Client)     |<---|  React App (SPA)  |
                  +-----------------------+    +-------------------+
                             |                      + -------+
https://my-page.com/products |                      | Routes |
                             |                      +--------+
                             v                          ^
                  +-----------------------+             |
                  |       Server          |-------------+
                  +-----------------------+
                        +-----------+ +------------------------------+
                        | No Routes | | Rule: Always load index.html |
                        +-----------+ +------------------------------+
```
