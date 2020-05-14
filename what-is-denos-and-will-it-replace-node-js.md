# What is Deno & Will it replace Node.js?

- Academind
- [https://www.youtube.com/watch?v=3Vl8a3zYjiw](https://www.youtube.com/watch?v=3Vl8a3zYjiw)
- May 14, 2020
- Completed May 14, 2020

---

- A **secure** runtime for JavaScript and TypeScript
- Uses the Chrome's V8 JavaScript engine and is built on Rust. 
- Created by Ryan Dahl, the creator of Node.js
- "Deno" is an anagram for Node.
- It fixes some of the flaws of Node.js.

#### Deno is in very early stages. Version 1.0.0 has not yet been released.

### Installation
```
curl -fsSL https://deno.land/x/install/install.sh | sh
...
Deno was installed successfully to /home/craig/.deno/bin/deno
Manually add the directory to your $HOME/.bash_profile (or similar)
  export DENO_INSTALL="/home/craig/.deno"
  export PATH="$DENO_INSTALL/bin:$PATH"
Run '/home/craig/.deno/bin/deno --help' to get started
```

### Deno Features
- It supports TypeScript without manual compilation because the TypeScript compiler is built in.
```
let message: string;
message = 'This works!'
console.log(message);
```
- You import from a web server.
    - When you run the script, ex. *deno run deno.ts*, it downloads what is in the url and caches it locally.
- Deno supports modern JavaScript features like **promises** or **async iterables**.
    - An **async iterable** is essentially a for loop that lets you go through an infinite array of incoming data and events.
- You can use top level **await**. 
    - That is, you don't need to wrap await in an asynchronous function.
- It is **secure**.
    - In Node.js any script can spin up a server or work with your file system.
    - You control which permissions you give to your scripts when you execute them.
        - By default there are no permissions.
        - You can add the flag *--allow_net* to allow network access.
            - *deno run --allow-net deno.ts*
    
```
import { serve } from "https://deno.land/std@.50.0/http/server.ts";
const server - serve({ port: 8000 });
for await (const req of server) {
    console.log('Incoming request');
    req.respond({body: 'Message from Deno!'}); 
}

// The code below is the functional equivalent in Node.js.
const http = require('http');
const server = http.createServer((req, res) => {
    res.senc('My response!');
});
server.listen(3000);
```

### Additional Information
- Deno team promises to maintain a stable API.
- There are currently a lot of unstable features - features that aren't finished or finalized.
- It has been in development for two years but it has a lot of room to develop and grow.
- **Deno is not compatible with Node (NPM) packages.** (This might change over time.)