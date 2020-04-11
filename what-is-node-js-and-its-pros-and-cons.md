# What is Node.JS and its Pros and Cons
- AltexSoft
- [https://www.youtube.com/watch?v=2gQG4cFjahw](https://www.youtube.com/watch?v=2gQG4cFjahw)
- Mar 29, 2019
- Completed Apr 11, 2020
---
### What is Node.JS
- An open source runtime enviroment based for JavaScript.
- Based on Chrome V8, an engine for Chromium browsers.
- Allows programs written in JavaScript to be executed on the server.
- Developed in 2009 by Ryan Dahl tp create dynamic web pages before they're sent to a browser.
- One of the most popular tools in back end web development.
- It is part of popular technology stacks:
  - MEAN - MongoDB, Express, Angular, Node
  - MERN - MongoDB, Express, React, Node
- There are many frameworkd built for Node
  - Express JS, Meteor, Sails, etc
- **NPM** (Node Package Manager) cna be used to source Node modules and ready-made packages.
- It has become a standard for enterprise companies like Netflix, Uber and eBay.

---
# The Strengths

### JavaScript on the server
- Opened the door to JavaScript full stack development.
- Inherits all of JavaScripts libraries and features.
- Lightweight JavaScript achieves higher performance with fewer lines of code when compared to Java or C.
- Single language is used on the front and back end.
- Easy to share and reuse code. Developers can use pre-build modules or reuse their own.

### Good for microservices.
- Highly scalable.
- Very fast. The Chrome V8 engine compiles JavaScript into machine code instead of using an interpreter.
- Event-based nature efficient for real-time apps that require constant data updates.
- Non-blocking input-output model solves performance issues.
- Performance is enhanced by concurrent request processing which uses a single thread event loop.
  - Can handle multiple requests with minimal CPU usage.

### Support and Community
- Node JS Foundation founded in 2015 by IBM, Microsoft, PayPal, Fidelity and SAP.
  - Independent community aimed at facilitating the development of Node.JS core tools.
- Well supported by Chrome with constant improvement.
- Open source with a large and engaged community.
- Very well documented.

### Packages
- **NPM** (Node Packge Manager) is a large library of modules and packages
- Encourages users to add packages.
- Ready made solutions for specific issues,

### Easy to learn
- Node.JS inherits many JavaScript features, such as libraries.
- Wide community support and popularity.
- Fast learning curve for new developers.

---
# The Drawbacks

### CPU bottleneck
- Bad for heavy computation
  - Node.JS would set up a block on other requests on the thread, causing an overall delay.
  - Major drawback
  - Threading has been introduced as an experimental feature behin the --experimental-worker flag in the worker_threads module.
    - Can now spawn additional threads for parallel processing to carry requests that block the event loop.
    - May become suitable for CPU bound tasks and used for machine learning-based calculations in the future.

### Immature tools
- NPM is quantity, not quality, driven.
  - Core products are stable but the rest of the NPM registry is poorly structured and documented.

### Few experienced developers
- Not many JavaScript developers have a lot of experience on the back end.
