<div style="display: inline-block;">
<a class="link" href="http://oclipa.github.io/">&lt; home</a>
<a class="link" href="http://oclipa.github.io/toolbox.html">&lt; toolbox</a>
</div> 

## Javascript

<button type="button" id="toggle-all" value="none">Expand All Sections</button>

-------------------------------------------------------------------------------------------------------

## Paradigms

Javascript is what is termed a multi-paradigm language, which means that it supports multiple different approaches to programming.  In particular it supports:

   * Prototypal Inheritance
   * Functional Programming

<div id="pi">
<button type="button" class="collapsible">+ Prototypal Inheritance</button>   
<div class="content" style="display: none;" markdown="1">

* Objects without classes.
   * Each object instance has a prototype: Object.prototype, which is itself an object instance.
   * The prototype cannot access private variables in an child instance (they need to be accessed using getters and setters).
* Protoype delegation (the prototype chain)
   * a.k.a Object Linked to Other Objects - OLOO.
   * If a function is not found in the object, the engine will then look for it in the prototype.
* Prototype concatenation (`Object.assign()`)
   * Can pick and choose which properties to inherit (and dynamically change this over time).
* Function inheritance
   * Using a function to create a closure.

&nbsp;

-------------------------------------------------------------------------------------------------------

Note that prototypal inheritance is **not the same as classical inheritance**.  
   * In classical inheritance, classes are provided with a blueprint that is built from a hierarchical chain of sub-classes.  An instantiated class will be a single object with all of the properties of the sub-classes.
      * Pros:
         * Basic concepts easier to understand and interpret.
         * Code tends to be easier to read.
         * Better IDE support.
      * Cons:
         * Tight coupling, which makes it difficult to makes changes to base classes.
         * Multiple inheritance is very difficult to implement.
         * Brittle architecture, since bad designs are often baked into hierarchies so hard to fix.
         * Cannot choose what to inherit (Gorilla/Banana analogy, although can be minimized using interfaces).
   * In prototypal inheritance, objects concatenate properties from multiple objects in a flat (or much flatter) hierarchy. 
      * Pros:
         * Loose coupling.
         * Easier to maintain and update code (e.g. add additional properties or functionality at a later date).
         * Easier to scale across multiple processors.
      * Cons:
         * Code can be harder to read.
         * Steeper learning curve (particularly if coming from an OO background; scoping!)
   
Javascript does not support (true) classical inheritance.

*It is difficult to identify any case where classical inheritance is more appropriate than prototypal inheritance!  The only case appears to be "if the language you are using depends on classical inheritance".*

&nbsp;

-------------------------------------------------------------------------------------------------------

A more in-depth discussion regarding descriptions of prototypal inheritance, can be found [here](https://alexsexton.com/blog/2013/04/understanding-javascript-inheritance/).  The following is extracted from that article:

```js
// define a prototype object with defaults.
const defaults = {
  zero: 0,
  one: 1
};

// use the prototype to create 
// specific instances
const myOptions = Object.create(defaults);
const yourOptions = Object.create(defaults);

// change properties of one instance
myOptions.zero = 1000;

// change properties of another instance
yourOptions.one = 42;

// update the defaults
defaults.two = 2;

// the instances have picked up the new defaults.
myOptions.two; // 2
yourOptions.two; // 2

// to create a new object that inherits 
// one of the instances, use:
let myWiderOptions = Object.create(defaults);
myWiderOptions = Object.assign(
    myWiderOptions, 
    myOptions, 
    { three: 3 }
);
// or, 
// let myWiderOptions = Object.create(myOptions);
// myOptions.three = 3;
```
</div>
</div>

<div id="fp">
<button type="button" class="collapsible">+ Functional Programming</button>   
<div class="content" style="display: none;" markdown="1">

At it's core, functional programming is based on the four main principles:

   * Functional Composition
   * Avoiding Shared State
   * Avoiding Mutable Data
   * Avoiding Side-Effects

&nbsp;

-------------------------------------------------------------------------------------------------------

**Functional Composition**

* Function composition is the process of combining two or more functions in order to produce a new function, e.g. `f(g(x))`.
* There are two approaches to composition: compose and pipe.

*compose*

```js
// defining compose function
// this evalutes the functions 
// right-to-left (or bottom-to-top) 
// e.g. { ... 4th, 3rd, 2nd, 1st }
const compose = (...functions) => data =>
  functions.reduceRight((value, func) => func(value), data);

// e.g.
pipe(
  reverse,
  get6Characters,
  uppercase,
  getName,
)({ name: 'Fred Flintstone' })

// = [F DERF]
```
*pipe*

```js
// defining pipe function
// this evalutes the functions 
// left-to-right ( or top-to-bottom) 
// e.g. { 1st, 2nd, 3rd, 4th ... }
const pipe = (...functions) => data =>
  functions.reduce((value, func) => func(value), data);

// e.g.
pipe(
  getName,
  uppercase,
  get6Characters,
  reverse 
)({ name: 'Fred Flintstone' })

// = [F DERF]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**Avoiding Shared State**

* A function should not have any internal state and it should certainly not share that state with other functions.
* A function should only be aware of the data passed into it as input.
* The problem with shared state is that in order to understand the effects of a function, it is necessary to know the entire history of every shared variable that the function uses or affects.
* If state is shared, race-conditions can result, where functions compete to access resources.  This can mean that the order in which function calls can result is different output.

&nbsp;

-------------------------------------------------------------------------------------------------------

**Avoiding Mutable Data**

* Mutable data is any data that can changed after it was created.
* An immutable value or object cannot be changed, so every update creates new value, leaving the old one untouched.


&nbsp;

-------------------------------------------------------------------------------------------------------

**Avoiding Side-Effects**

In functional programming, a side effect is any application state change that is observable outside the called function other than its return value.

Examples include:
   * Modifying any external variable or object property (e.g., a global variable, or a variable in the parent function scope chain).
   * Logging to the console.
   * Writing to the screen.
   * Writing to a file.
   * Writing to the network.
   * Triggering any external process.
   * Calling any other functions with side-effects.

The goal in functional programming is to minimize side-effects; in the ideal case, the only result of calling a function should be the return value.

If side-effects cannot be avoided, best practice is to isolate them from the rest of the software.  To draw a comparision with OO programming, this is similar to the Model-View-Controller (MVC) pattern, where the View is the side-effects, the Controller the functional logic, and the Model the state.  These "components" should be kept separate and loosely coupled.

&nbsp;

-------------------------------------------------------------------------------------------------------

The ideal form of a function is the pure function, a function for which the same inputs always result in the same output, and there are no side-effects (i.e. no outside effects other than the return value).

Functional programming is supported by features such a:
   * First-class functions (i.e. functions are treated as objects)
   * Functions are arguments (i.e. the ability to pass a function as an argument)
   * Higher order functions (i.e. functions that accept other functions as input and return a function)
   
These features are typically enabled by combining **lambdas** (abstractions that can be passed into functions as data) with **closures** (encapsulation of data with functions such that they can be passed around).

</div>
</div>

<div id="purefunc">
<button type="button" class="collapsible">+ Pure Functions</button>   
<div class="content" style="display: none;" markdown="1">

A pure function is one that:

   * Given the same inputs, always returns the same output.
   * Has no side-effects.

Essentially, these are the essence of functional programming.

One of the chief advantages of pure functions is that, as long as the result is the same, they can be replaced without changing the meaning of the program, which is useful for things like micro-services and unit testing.  In addition, since they are guaranteed to be completely self-contained, they are great candidates for situations that require concurrency.

An indicator of an impure function is one that can be called without using its return value.  This implies that it produces side-effects.

In an ideal world, an application would be composed entirely of pure functions.  In practice, impure functions are required (e.g. Math.random(); Data.getTime(); anything that updates the UI or data source).

</div>
</div>

<div id="lambda">
<button type="button" class="collapsible">+ Lambdas</button>   
<div class="content" style="display: none;" markdown="1">

Lambda expressions are abstractions that enable a function to be passed around like data.  In other languages that support them, lambda expressions are normally identified by arrow notation (`=>`), but this is not the case in Javascript.

In Javascript, it is common for 'lambda expression' to be reserved for anonymous functions, but this is not strictly true.  It is perfectly possible for a named function to a lambda expression *as long as it is passed into another function and treated as data*.   In addition, it is perfectly possible for an anonymous functions to not be a lambda expression *if it is not passed into another function*.  An example of this latter case would be a Self-Executing Function.

In summary: Lambda means "function used as data".

</div>
</div>

<div id="closures">
<button type="button" class="collapsible">+ Closures</button>   
<div class="content" style="display: none;" markdown="1">

A more in-depth discussion of closures is given [here](https://oclipa.github.io/csharp-cheat-sheet/#closures?expand) (in the context of C#, but the general principles apply to Javascript).  

In summary, there are two basic use cases:
1. Storing the state of data to be used in a particular method at a later time.
1. Hiding data but leaving it accessible to a particular method.

The following are a couple of illustrative examples...

This first example demonstrates a problem that closures can solve:  
   * At first glance this might be expected to print out (0, 1, 2, ...etc), but the actual output will be (10, 10, 10, ...etc).  
   * This is because `i` exists in the scope of the `createPrinters` function, so the latest value (10) will be used when the function is evaluated.

```js
const createPrinters = () => { 
  
  const arr = []; 
  
  // i is scoped within this function,
  // and so will retain whatever value
  // was set when the for loop completes.
  let i; 
  for (i = 0; i < 10; i++)  
  { 
    // storing anonymous function 
    arr[i] = () => { 
    
      // in arrow functions, variables are
      // scoped within the parent block 
      // (in this case, within the for loop)
      return i; 
    };
  } 

  // returning the array. 
  return arr; 
}

const printers = createPrinters(); 
  
printers.map(printer => { 
              console.log(printer()); 
            });

```
This second example shows a solution to the first example:
   * `val` is scoped within a closure (basically, a couple of nested functions), so it will retain whichever value it had when the closure was instantiated.  
   * When the `createPrinters` function is evaluated, the output will be the less surprising (0, 1, 2, ...etc).

```js
const createPrinters = () => { 
    
  const createClosure = (val) => { 
    
    // val is scoped within this function,
    // and so will retain whatever value
    // was passed in via the arguments.
    return () => {
    
      // in arrow functions, variables are
      // scoped within the parent block (in
      // this case, within createClosure)
      return val; 
    }; 
  };
  
  var arr = []; 
  var i; 
  for (i = 0; i < 10; i++)  
  { 
    // storing the closure function 
    arr[i] = createClosure(i); 
  } 
  return arr; 
};

var printers = createPrinters(); 

printers.map(printer => { 
              console.log(printer()); 
            })
```

This final example demonstrates how a closure can be used for data privacy:
   * In this case, our hero's profession can only be revealed by asking him directly (with a password!), unlike his appearance, which can be seen just by looking at him.

```js
const animal = {
  animalType: 'animal',
 
  describe () {
    return 'An ' + this.animalType + ' with ' + this.furColor + 
      ' fur, ' + this.legs + ' legs, and a ' + this.tail + ' tail.';
  }
};
 
const mouse = () => {
  const secret = 'spy';

  const testPassword = (password) => {
    const privateKey = -2133058532;

    let hash = 0; 
    if (password.length === 0) return hash; 
    for (i = 0; i < password.length; i++) { 
      char = password.charCodeAt(i); 
      hash = ((hash << 5) - hash) + char; 
      hash = hash & hash; 
    } 
    return hash === privateKey; 
  };

  return Object.assign(
    Object.create(animal), 
    {
      animalType: 'mouse',
      furColor: 'brown',
      legs: 4,
      tail: 'long, skinny',
      profession: (password) => {
        return testPassword(password) ? 
                  secret : "secret";
      }
    }
  );
};
 
const peek = (animal) => {
  console.log("Peek:");
  console.log(animal);
};
const ask = (animal, password) => {
  console.log("Ask:");
  console.log(animal.profession(password));
};

const james = mouse();

peek(james); // "secret"
ask(james, "skyfall"); // "spy"
```
</div>
</div>

<div id="curry">
<button type="button" class="collapsible">+ Currying</button>   
<div class="content" style="display: none;" markdown="1">

Currying is a process in functional programming in which we can transform a function with multiple arguments into a sequence of nesting functions. It returns a new function that expects the next argument inline.

*NOTE*: the number of nested functions a curried function has depends on the number of arguments it receives. If this is not the case, it is not a *curry* but a *partial function*.

**A Simple Example**

Consider the following function that multiplies two numbers:
```js
function multiply(x, y) { return x * y; }
```
This function can be refactored in the following manner:
```js
function curriedMultiply(x) {
  return (y) => { return x * y; }
}
```
Now, `curriedMultiply` no longer performs the multiplication itself; instead, it returns a specialized multipler function.

For example, `curriedMultiply(3)` is effectively the same as the following:
```js
function(y) {
  return 3 * y;
}
```
*NOTE*: `multiply(x, y)` is equivalent to `curriedMultiply(x)(y)`.

**A More Real-World Example**

Consider how `fs.readFile(path, encoding, callback)` can be curried to separate out the callback parameter:
```js
fs.curriedReadFile(path, encoding);
```
This now returns a specialized reader function:
```js
const reader = fs.curriedReadFile("hello.txt", "utf8");

reader(callback);
```
Here, `reader` is an asynchronous function that only knows how to read `hello.txt` and has a single callback parameter.

This is useful because `curriedReadFile` can be implemented so that it starts the asynchronous read operation but we are not forced to use the reader immediately.  The function can be cached or passed around while the I/O operation progresses.  When we need the result, we will call the reader with a callback.

By currying, we have separated the initiation of the asynchronous operation from the retrieval of the result. This is very powerful because now we can initiate several operations in a close sequence, let them do their I/O in parallel, and retrieve their results afterwards. Here is an example:

```js
const reader1 = curriedReadFile(path1, "utf8");
const reader2 = curriedReadFile(path2, "utf8");
// I/O is parallelized and we can do other things while it runs

// further down the line:
reader1((err, data1) => {
  reader2((err, data2) => {
    // do something with data1 and data2
  });
`});
```

```js
function curriedReadFile(path, encoding) {
  var _done, _error, _result;

  // create a default callback that simply stores the 
  // results and any error message, and flags that the
  // I/O is complete.
  var callback = (error, result) => { 
    _done = true,
    _error = error, 
    _result = result; 
  };

  // starts reading the file asynchronously.
  // the callback is passed as a closure; when triggered this
  // will store the results and any error message (and 
  // set "done" to true)  
  fs.readFile(path, encoding, function(e, r) { 
    callback(e, r); 
  });

  // Here '_' is the function we pass to curriedReadFile
  // Where we actually do the IO stuff
  return (_) => {

    // If fs.readFile returned (_done is set), we just 
    // execute our IO code with the results
    if (_done) {
      _(_error, _result);

    // If it still hasn't return, our function that 
    // does IO stuff ('_') now becomes the callback 
    // (see fs.readFile body)
    } else {
      callback = _; 
    };
  }
}
```

**When Is Currying Useful?**

Some cases where currying is useful are:
* When you want to write little code modules that can be reused and configured with ease (e.g. for distribution by npm).
* When you want to avoid frequently calling a function with the same argument.

**Curry == Promise?**

The ability for a curry to encapsulate an asynchronous function and return the result later means that this could be considered a very simple form of a Promise.

</div>
</div>

<div id="hof">
<button type="button" class="collapsible">+ Higher Order Functions</button>   
<div class="content" style="display: none;" markdown="1">

Higher Order Functions are fundamental to the way Javascript works.  Essentially they allow functions to be composed together, to return another function. 

A Higher Order Function accepts one or more functions as arguments and returns another function.

```js
const strArray = ['JavaScript', 'Python', 'PHP', 'Java', 'C'];

// this is a higher order function
function mapForEach(arr, fn) {
  const newArray = [];
  for(let i = 0; i < arr.length; i++) {
    newArray.push(
      fn(arr[i])
    );
  }
  return newArray;
}

// pass a function to mapForEach that returns
// the number of characters in an item
const lenArray = mapForEach(strArray, function(item) {
  return item.length;
});

// prints [ 10, 6, 3, 4, 1 ]
console.log(lenArray);
```

</div>
</div>

<div id="oneway">
<button type="button" class="collapsible">+ One-Way Data Flow</button>   
<div class="content" style="display: none;" markdown="1">

* One-way data flow means that the model is the single source of truth.
* The UI can signal changes to the model but only the model can change the app's state.
* Data is input from the UI and then passed back to the model to be manipulated.  The results are then passed down the component tree to be redisplayed.
* This effectively means that the application follows the Data Down, Action Up pattern, which makes it easier to understand.

```
user notifies intention to update
=====> intention sent "up" to model 
==========> model decides what to do with intention
<========== model updates based on intention
<===== updated properties sent "down" to components
updated properties displayed to user
```

Pros:
   * Simple data flow (which makes debugging easier)
   * Scales well.

Cons:
   * Can require more code.

This is typically used in front-end frameworks, such as React, however the following is an example using vanilla javascript:

**Vanilla JS Example**

*oneway.html*

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
    <main class="main">
      <div class="quote" data-binding="quote"></div>
      <div class="author" data-binding="author"></div>
    </main>

    <script src="./oneway.js"></script>
</body>
</html>
```

*oneway.css*

```css
body {
  height: 100vh;
  margin: 0;
  padding: 0;
  display: flex;
  font-family: "Helvetica neue", Helvetica, sans-serif;
  justify-content: center;
}

.main {
  width: 60%;
  align-self: center;
}

.quote {
  font-style: italic;
}

.author {
  font-weight: 600;
  text-align: right;
}
```

*oneway.js*

```js
import './oneway.css';

// create an array of quotes
const quotes = [
  {
    author: 'Albert Einstein',
    quote: 'Strive not to be a success, but rather to be of value.'
  },
  {
    author: 'Robert Frost',
    quote: `Two roads diverged in a wood, and I—I took the one 
            less traveled by, And that has made all the difference.`
  }
];

// timer mimics user interaction
// setInterval() is a built-in function provided by the window.
setInterval(() => {

  // 1) at intervals, a random quote is "selected by the user"
  const index = Math.floor(
    Math.random() * Math.floor(quotes.length)
  );
  const { author, quote } = quotes[index];
  
  // 2) selected quote is "sent to model"
  Object.assign(state, {
    author: author,
    quote: quote
  });
}, 2000); // every 2 seconds; triggers render

// Proxy mimics model
const createStateProxy = (quote) => {
  return new Proxy(quote, {
    // get handler returns a value derived
    // from the supplied properties 
    //(e.g. validation)
    // get: (quote, property, value) => { }
    
    // set handler allows the quote to
    // to be updated based on the property
    // and value; returns a boolean to 
    // indicate success.
    set: (quote, property, value) => {
    
      // 3) quote is updated
      quote[property] = value;
      
      // 4) updated quote is "sent to UI"
      render();
      
      return true;
    }
  });
};

const stateProxy = createStateProxy(quotes[0]);

const render = () => {
  // get all data-binding ids in HTML document
  const bindings = Array.from(
    document.querySelectorAll('[data-binding]')
  ).map(
    e => e.dataset.binding
  );
  
  // if the data-binding ids match a property
  // in the stateProxy, update the HTML to display
  // the property value
  bindings.forEach(binding => {
    if (stateProxy[binding]) {
      document.querySelector(
        `[data-binding='${binding}']`
      ).innerHTML = stateProxy[binding];
    } else {
      throw new ReferenceError(
          `${binding} is a not a member of the current page`
      );
    }
  });
};

render();
```

**React Example**

*App.js*

```jsx
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import './oneway.css';
import Quote from './Quote'

class App extends Component {
  state = {
    author: '',
    quote: '',
  };

  quoteChangeHandler = (newAuthor, newQuote) => {
    this.setState({ author: newAuthor, quote: newQuote });
  };

  render() {

    let quote = (<Quote 
                  onChange={this.quoteChangeHandler} 
                  quote={this.state.quote} 
                  author={this.state.author} />);

    return (
      <div className="App"> {/* required */} 
        {quote}
      </div>
    );
  }
}

export default App;
```

*Quote.js*

```jsx
import React, { useEffect } from 'react';

const Quote = (props) => {

  const quotes = [
    {
      author: 'Albert Einstein',
      quote: 'Strive not to be a success, 
              but rather to be of value.'
    },
    {
      author: 'Robert Frost',
      quote: 'Two roads diverged in a wood, 
               and I—I took the one less 
               traveled by, And that has 
               made all the difference.'
    }
  ];
  
  function updateQuote(){
    const index = Math.floor(Math.random() * Math.floor(quotes.length));
    const { author, quote } = quotes[index];
    props.onChange(author, quote)
  }

  useEffect(() => {
    // fake user interaction
    const timer = setTimeout(() => {
      updateQuote();
    }, 2000);

    return () => {
      clearTimeout(timer); // javascript built-in function
    };
  });

  return (
    <div class="main">
      <div class="quote">{props.quote}</div>
      <div class="author">{props.author}</div>
    </div>
  );
}

export default Quote;
```


</div>
</div>

<div id="twoway">
<button type="button" class="collapsible">+ Two-Way Data Binding</button>   
<div class="content" style="display: none;" markdown="1">

Two-way binding just means that:
1. When properties in the model get updated, the updates get *immediately propagated* to the view.
1. When UI elements get updated, the changes get *immediately propagated* to the model.

```
user 1 interacts with view 1
<=====> model 1 reflects interaction
<==========> model 1 updates model 2
view 1 updates to reflect model 1 and model 2
view 2 updates to reflect model 1 and model 2
```

Pros:
   * View will always reflect model (and vice versa).
   * Makes model a safe, atomic data source for whole application.

Cons:
   * Can be difficult to debug.

</div>
</div>

<div id="mono">
<button type="button" class="collapsible">+ Monolithic vs Microservice Architectures</button>   
<div class="content" style="display: none;" markdown="1">

A monolithic application is one written as one unit of code, with components designed to work together, sharing the same memory and resources.

Pros:
  * Easier to implement features that have cross-cutting concerns (e.g. logging, security features, etc.).
  * Can have performance advantages due to shared-memory access.

Cons:
  * Componenents tend to be tightly coupled, which makes maintence and scaling difficult.
  * Can be much harder to understand dependencies and data flow.

A microservice consists of many smaller, independent applications, each of which has its own memory and resources.

Pros:
   * Tend to be better organized, with clearer separation of concerns.
   * Much easier to scale and reconfigure.
   * Easier to isolate performance bottlenecks so they are easier to handle.

Cons:
   * Can be difficult to separate or encapsulate cross-cutting concerns.
   * Tend to require management of a many virtual machines or containers.

</div>
</div>

<div id="async">
<button type="button" class="collapsible">+ Asynchronous Programming</button>   
<div class="content" style="display: none;" markdown="1">

Synchronous programming means that code is executed sequentially and will block on longer running tasks.

Asynchronous programming means that the engine runs in an event loop, from which operations are spawned.  If an operation takes a long time, the loop continues running until a result is received.  Typically this is handled through events.

Asynchronous programming is much better suited to UIs, or applications that depend on network interaction.  This is particularly relevant for web applications, which is why it is important for Javascript.

</div>
</div>

<div id="promise">
<button type="button" class="collapsible">+ Promises</button>   
<div class="content" style="display: none;" markdown="1">

A promise is an object that may produce a single value at some time in the future.  This could be a resolved value, or a reason why it was not resolved.

A promise has 3 states:
   * Fulfilled: `onFulfilled()` will be called (e.g., `resolve()` was called)
   * Rejected: `onRejected()` will be called (e.g., `reject()` was called)
   * Pending: not yet fulfilled or rejected

Callbacks are attached to promise to handle the returned value.  Once returned, a promise is considered "settled" and cannot be reused (it is immutable).

A promise will immediately start doing a task.  If there is less hurry, alternatives are Observables (which allow composition of tasks - see RxJS) and Tasks (which are similar to promises but concern computations rather than results - can start/cancel/stop the computation).

```
const wait = time => new Promise((resolve) => setTimeout(resolve, time));

wait(3000).then(() => console.log('Hello!')); // 'Hello!'
```

All promises supply a `.then()` method, which returns a new promise:

```
promise.then(
  onFulfilled?: Function,
  onRejected?: Function
) => Promise
```
</div>
</div>

### Basics

<div id="nodejs">
<button type="button" class="collapsible">+ Installing NodeJS</button>   
<div class="content" style="display: none;" markdown="1">
  
NodeJS provides many tools that aid javascript development, not least of which is the npm package management tool.
  
To install NodeJS:
* Either, download the installer from the NodeJS website: [https://nodejs.org](https://nodejs.org)
* Or, if using MacOS, install using HomeBrew: `brew install node`
* Or, if using Zsh on Unbuntu (if using Bash, just replace `zsh` with `bash` and `.zshrc` with `.bash_profile`):
   1. `sudo apt-get update`
   1. `sudo apt-get upgrade`
   1. `sudo apt-get install build-essential`
   1. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | zsh`
   1. Restart prompt (if there problems are reported with .bashrc, check the permissions on .bashrc)
   1. `nvm install --lts`
   1. `nvm use --lts`
   1. `echo "nvm use --silent --lts" >> .zshrc`

After installing NodeJS, it is recommended to also install the following packages from npm:

* http-server: `npm install -g http-server`
</div>
</div>

<div id="simple-web-app">
<button type="button" class="collapsible">+ A Simple Command-Line App</button>   
<div class="content" style="display: none;" markdown="1">
  
NodeJs must be installed in order to run javascript on the command line.

Once installed, a script can be run using `node filename.js`, e.g.:

*add.js*

```js
// Simple Addition Function in Javascript 
function add(a, b) { 
  return a+b 
}

console.log(add(4, 6)) 
```

Run command: 
```
$ node add.js
10
```

**Command-Line Arguments**

To read command-line arguments, access the `process.argv` property:

*print-args.js*

```js
console.log(process.argv);
```

Run command:
```
$ node print-args.js one two three four five`
[ 'node',
  '/home/dev/jsdemo/argv.js',
  'one',
  'two',
  'three',
  'four',
  'five' ]
```
Note that the output includes both the `node` command and the script path.

**yargs**

To simplify handling of command-line arguments, the `yargs` package can be installed using npm: 
* `npm install yargs`

A simple example of using `yargs` is the following:

*battleships.js*

```js
import argv from 'yargs';

if (argv.x === 3 && argv.y >= 2 && 5 <= argv.y) {
  console.log('You hit my battleship!')
} else {
  console.log('Missed!')
}
```

Run command: 
```
$ node battleships.js --x=3 --y=2
You hit my battleship!

$ node battleships.js --x 3 --y 3
You hit my battleship!

$ node battleships.js --x 3 --y=1
Missed!
```

There is much more that `yargs` enables; see the following for further information:
   * [http://yargs.js.org/](http://yargs.js.org/)

</div>
</div>

<div id="simple-web-app">
<button type="button" class="collapsible">+ A Simple Web App</button>   
<div class="content" style="display: none;" markdown="1">

To start a simple web server for testing javascript web apps, install the `http-server` package from npm: 
* http-server: `npm install -g http-server`

To start the web server, run the following command in the same folder as your `index.html` file:

```
$ http-server
Starting up http-server, serving ./
Available on:
  http://192.168.0.5:8080
  http://127.0.0.1:8080
Hit CTRL-C to stop the server
```

The web app can then be viewed in any browser by visiting: 
* `http://localhost:8080`

**A Simple Web App**

*index.html*

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8" />
    <title>My Awesome Page</title>
    
    <!-- 
      "defer" allows faster page loads, 
      but may not work in older browsers 
    -->
    <script defer src="src/index.js" charset="utfs-8"></script>
  </head>
  <body>
    <div id="quotes"></div>
    <button id="give-cat">GIVE ME A CAT!</button>
    <div id="cat-pic"></div>
  </body>
</html>
```

*index.js*

```js
let quotesDiv = document.getElementById('quotes');

fetch('https://api.kanye.rest')
  .then((res) => res.json())
  .then((quote) => {
    // tick marks indicate a template literal, which
    // allows a string to contain placeholders
    quotesDiv.innerHTML += `<p>${quote.quote}</p>`;
  });

let catButton = document.getElementById('give-cat');

catButton.addEventListener('click', (evt) => {
  let catDiv = document.getElementById('cat-pic');

  fetch('https://api.thecatapi.com/v1/images/search?')
    .then((res) => res.json())
    .then((cats) => {
      cats.forEach((cat) => {
        catDiv.innerHTML = `<h3>Here is your cat</h3>
            <img src="${cat.url}" alt="kitty" />`;
      });
    });
});
```

</div>
</div>

<div id="modules">
<button type="button" class="collapsible">+ Modules</button>   
<div class="content" style="display: none;" markdown="1">

**Importing in ES6**

```js
import 'helpers'
// ES5: require('···')

import Express from 'express'
// ES5: const Express = require('···').default || require('···')

import { indent } from 'helpers'
// ES5: const indent = require('···').indent

import * as Helpers from 'helpers'
// ES5: const Helpers = require('···')

import { indentSpaces as indent } from 'helpers'
// ES5: const indent = require('···').indentSpaces
```

**Exporting in ES6**

```js
export default function () { ··· }
// ES5: module.exports.default = ···

export function mymethod () { ··· }
// ES5: module.exports.mymethod = ···

export const pi = 3.14159
// ES5: module.exports.pi = ···
```

</div>
</div>

<div id="createobj">
<button type="button" class="collapsible">+ Creating An Object</button>   
<div class="content" style="display: none;" markdown="1">

There are several approaches to creating objects in Javascript.

   * Using an Object Literal.
   * Using Object.create().
   * Using a Factory Function.
   * Using Prototypal Inheritance.
   * Using Classes.

As a general rule of thumb, Prototypal Inheritance and Classes should be avoided, since these create problems for functional programming.

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using an Object literal*

This is the basic way to create an object in javascript.

```js
let X = { p1: a, p2: b, myMethod () { return `my method`; } }
```
e.g.:
```js
let mouse = {
  furColor: 'brown',
  legs: 4,
  tail: 'long, skinny',
  describe () {
    return `A mouse with ${this.furColor} 
      fur, ${this.legs} legs, and a 
      ${this.tail} tail.`;
  }
};

```

&nbsp;

-------------------------------------------------------------------------------------------------------
*Using Object.create()*

In this case, a new object is created from a base object (using `Object.create()` and the properties of the new object are set (using `Object.assign()`).

* `Object.create()` creates a new object, using an existing object as the prototype.
* `Object.assign()` copies all enumerable own properties from one or more source objects to a target object
   * An "own property" is one that is not inherited.

```js
let baseX = { p1: a, p2: b, myMethod() { return `base method` } }

let concreteSubX = return Object.assign(
    Object.create(baseX), { p1: c, p3: d }
);
```
e.g.:
```js
let animal = {
  animalType: 'animal',
  
  describe () {
    return `An ${this.animalType}, with 
      ${this.furColor} fur, ${this.legs} 
      legs, and a ${this.tail} tail.`;
  }
};

// assign(target, source1, source2 ...)
let mouse = Object.assign(
  Object.create(animal), 
  {
    animalType: 'mouse',
    furColor: 'brown',
    legs: 4,
    tail: 'long, skinny'
  }
);
```

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using a Factory Function*

A slight refinement upon using `Object.create()` directly is to wrap it in a factory function.  This allows the function to be called multiple times.

```js
let baseX = { p1: a, p2: b, myMethod() { return `base method`; } }

let subX = function subX () {
  return Object.assign(
    Object.create(baseX), { p1: c, p3: d }
  );
};

let concreteSubX = subX();
```
e.g.:
```js
let animal = {
  animalType: 'animal',
 
  describe () {
    return `An ${this.animalType}, with 
      ${this.furColor} fur, ${this.legs} 
      legs, and a ${this.tail} tail.`;
  }
};
 
let mouse = function mouse () {
  return Object.assign(
    Object.create(animal), 
    {
      animalType: 'mouse',
      furColor: 'brown',
      legs: 4,
      tail: 'long, skinny'
    }
  );
};

let mickey = mouse();
```

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using Prototypal Inheritance*

This is the core principle upon which inheritance in javascript is based.

```js
function BaseX(a, b) 
{ 
  p1: a, 
  p2: b 
}

BaseX.prototype.myMethod = function() {
  return ( "base method" );
};

function SubX(c, d, e) 
{ 
  BaseX.call(this, p1: c, p2: d); 
  p3: e;
}

SubX.prototype = Object.create(BaseX.prototype);
SubX.prototype.constructor = SubX;

SubX.prototype.myMethod = function() {
  return ( "overidden method" );
};

const baseX = new BaseX('a', 'b');

const subX = new SubX('c', 'd', e);
```
e.g.:
```js
function Rodent(rodentType, furColor, legs, tail) {
  this.rodentType = rodentType;
  this.furColor = furColor;
  this.legs = legs;
  this.tail = tail;
}

// add describe function
Rodent.prototype.describe = function() {
  return ('A ' + this.rodentType +', with ' + 
            this.furColor + ' fur, ' + 
            this.legs +' legs, and a ' + 
            this.tail +' tail.');
};

// constructor function
function Mouse(rodentType, furColor, legs, tail, food) {
  Rodent.call(this, rodentType, furColor, legs, tail);
  this.food = food;
}

// inheritance logic
Mouse.prototype = Object.create(Rodent.prototype);
Mouse.prototype.constructor = Mouse;

// override describe function
Mouse.prototype.describe = function() {
  return ('A ' + this.rodentType + ', with ' + 
            this.furColor + ' fur, ' + 
            this.legs +' legs, and a ' + 
            this.tail +' tail. It likes ' + 
            this.food + '.');
}

const rodent = new Rodent('vole', 'grey', 4, 'short, stubby');
console.log(rodent.describe());

const mouse = new Mouse('mouse', 'red', 4, 'pink', 'cheese');
console.log(mouse.describe());
```

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using Classes*

Note that `class` is simply syntactical sugar on prototype inheritance, so this example and the preceeding one are essentially identical under-the-covers.  The `class` pattern in ES6 is not the same as classical inheritance.

```js
class BaseX {
  constructor(a, b) {
    this.p1 = a;
    this.p2 = b;
  }
  
  myMethod() { return ( "base method" ); }
}

class SubX extends BaseX {
  constructor(c, d, e) {
    super(c, d);
    this.p3 = e;
  }
  
  myMethod() { return ( "overridden method" ); }
}

const baseX = new BaseX('a', 'b');

const subX = new SubX('c', 'd', e);
```
e.g.:
```js
class Rodent {
  constructor(rodentType, furColor, legs, tail) {
    this.rodentType = rodentType;
    this.furColor = furColor;
    this.legs = legs;
    this.tail = tail;
  }
  
  describe() {
    return ('A ' + this.rodentType +', with ' + 
              this.furColor + ' fur, ' + 
              this.legs +' legs, and a ' + 
              this.tail +' tail.');
  }
}

class Mouse extends Rodent {
  constructor(rodentType, furColor, legs, tail, food) {
    super(rodentType, furColor, legs, tail);
    this.food = food;
  }
  
  describe() {
    return ('A ' + this.rodentType + ', with ' + 
          this.furColor + ' fur, ' + 
          this.legs +' legs, and a ' + 
          this.tail +' tail. It likes ' + 
          this.food + '.');
  }
}

// Actual usage remains the same
const rodent = new Rodent('vole', 'grey', 4, 'short, stubby');
console.log(rodent.describe());

const mouse = new Mouse('mouse', 'red', 4, 'pink', 'cheese');
console.log(mouse.describe());
```

</div>
</div>

<div id="defaults">
<button type="button" class="collapsible">+ Default Values</button>   
<div class="content" style="display: none;" markdown="1">

Default array values:

```js
const scores = [22, 33]]
const [math = 50, sci = 50, arts = 50] = scores

// Result:
// math === 22, sci === 33, arts === 50
```

Default argument values:

```js
function greet({ name = 'Rauno' } = {}) {
  console.log(`Hi ${name}!`);
}
 
greet() // Hi Rauno!
greet({ name: 'Larry' }) // Hi Larry!
```

</div>
</div>

<div id="var">
<button type="button" class="collapsible">+ var, let &amp; const</button>   
<div class="content" style="display: none;" markdown="1">

`var`- creates a variable; doesn't differentiate between variables and constants.
   * `var` is scoped to the immediate function body (i.e. function scope).
   * `var foo = "foo1"; var foo = "foo2";` is allowed.
   * Creating a globally-scoped `var` adds a property on the global object (`window.foo`).
   * Using `var` allows variables to be hoisted (i.e. used before they are defined).

`let`- is basically the same as `var`; use this if a variable is actually variable.
   * `let` is scoped to the immediate enclosing block denoted by `{ }` (i.e. block scope).
   * `let bar = "bar1"; let bar = "bar2";` is a SyntaxError (already declared).
   * Creating a globally-scoped `let` does not add a property on the global object.
   * Using `let` does not allow variables to be hoisted.

`const`- is basically the same as `let` except it is read-only.
   * In this content, read-only means that the pointer (if a reference-type) or value (if a value-type) cannot be changed once defined.  It does not mean that the contents of a reference-type cannot be changed.
   * Other than being read-only, `const` behaviour is the same as `let`.

With the release of ES6, avoid using `var`.

</div>
</div>

<div id="function">
<button type="button" class="collapsible">+ Function Syntax</button>   
<div class="content" style="display: none;" markdown="1">

```js   
    // anonymous function 
    var multiply = function(x, y) {
        return (     // or, return x * y;
          x * y;
        )
    };

    // named function 
    var multiply = function name(x, y) {
      ...
    };
    
    // arrow function
    const multiply = (x, y) => {
        return (     // or, return x * y;
          x * y
        )
    };
    
    // self-executing expression
    (function() {
      statements
    })();
    
    // or,
    
    (() => {
      statements
    })();  
```
</div>
</div>

<div id="methods">
<button type="button" class="collapsible">+ Methods vs Functions</button>   
<div class="content" style="display: none;" markdown="1">

The difference between a method and a function (in javascript) is: 
   * functions are called in isolation (e.g. `someFunction()`)
   * methods are only called from other objects (e.g. `someObject.someFunction()`)

e.g.

```js
    var object = {
      myMethod: function() {
        console.log("What am I?");
      }
    };
    
    object.myMethod(); // method call
    
    var myFunc = object.myMethod; 
    myFunc();          // function call
```
For those coming from languages such as C#, it may be useful to think of functions as private and methods as public.
</div>
</div>

<div id="iife">
<button type="button" class="collapsible">+ Immediately-Invoked Function Expressions (IIFE)</button>   
<div class="content" style="display: none;" markdown="1">

Immediately Invoked Function Expressions (a.k.a. Self-Executing Functions) are functions which are invoked immediately after being defined, i.e. they don't need to be explicitly called elsewhere in the code.

The general form is `(function(){ })();`.  
   * The parentheses around the function are to ensure that the code within the function is contained in the private scope of the function.
   * The parentheses at the end of the function are what invokes the function.
   
Common pattern to prevent collisions when importing multiple javascript files:

```jsx
(function () {

    function MyFunc() {
    }
    
    // register MyFunc function with window scope
    window['MyFunc'] = MyFunc;

    ...etc...
})();

function onDocumentLoad() {
    new MyFunc();
}

document.addEventListener('DOMContentLoaded', onDocumentLoad);
```
</div>
</div>

<div id="arrow">
<button type="button" class="collapsible">+ Arrow Functions (=>)</button>   
<div class="content" style="display: none;" markdown="1">

"Traditional" Function (ES5):

```js
function myFunc() {
  console.log(arguments);
  return ...
}
```

Arrow Function (ES6):

```js
const myFunc = (args) => ({
  console.log(args);
  ...
})
```

Differences:
* Arrow functions avoid problems with `this` keyword (always refers to the enclosing context).
* Arrow functions have an implicit return, so the `return` keyword does not need to be used, however the function block must be wrapped in parantheses (if nothing is being returned, the parantheses can be left out).
* Arrow functions cannot be [hoisted](https://www.w3schools.com/js/js_hoisting.asp), unlike the ES5 function.
* Arguments must be explicitly passed into arrow functions (the `arguments` object is only available to ES5 functions).
* Cannot use arrow functions as constructors or methods (see below).
</div>
</div>

<div id="getset">
<button type="button" class="collapsible">+ Getters & Setters</button>   
<div class="content" style="display: none;" markdown="1">

```js
const Car = {
  get started () {
    return this.status === 'started'
  },
  set started (value) {
    this.status = value ? 'started' : 'stopped'
  }
}
```
</div>
</div>

<div id="extract-values">
<button type="button" class="collapsible">+ Extracting Values From Objects</button>   
<div class="content" style="display: none;" markdown="1">

```js
const langCode = { language: "English", code: "en" }

Object.values(langCode)
// [English, "en"]

Object.entries(langCode)
// [["language", "English"], ["code", "en"]]
```
</div>
</div>

<div id="spread">
<button type="button" class="collapsible">+ Spread and Rest Operators (...)</button>   
<div class="content" style="display: none;" markdown="1">

Both Spread and Rest use the same operator: `...`

&nbsp;

-------------------------------------------------------------------------------------------------------

**Spread:**
   * Used to split up (i.e. spread) array elements OR object properties.

```js
// create new array by splitting the old 
// array and adding objects a and b

const newArray = [...oldArray, a, b];

// create new object by splitting up the 
// old object into properties and adding 
// a new property (newProp).
// If oldObject already contains newProp, 
// the old value will be overwritten.

const newObject = ({ ...oldObject, 
                           newProp: 5 })
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**Rest:**
   * Used to merge a list of elements into an array.
   * Name comes from ability to merge "the rest of the elements" into an array.

An example of the general case is:

```js
var dogPref = ["Dogs" , "Like" , "Bones"];
const [animal , ...pref] = dogPref;
console.log(animal); // Dogs
console.log(pref); // [ "Like", "Bones"]
```

A more common case is for handling arguments passed to a function:

```js
// args can be an unlimited list of 
// arguments.
// The rest operator merges all of the 
// arguments into an array.

function sortArgs(...args) {
   return args.sort();
}
```
</div>
</div>

<div id="destruct">
<button type="button" class="collapsible">+ Destructuring ([a, b] = [x, y])</button>   
<div class="content" style="display: none;" markdown="1">

The [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

For arrays:

```js
let a, b, rest;
[a, b] = [10, 20];

console.log(a);
// expected output: 10

console.log(b);
// expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest);
// expected output: Array [30,40,50]
```

There is a similar syntax for objects (simply replaces `[]` with `{}`): 

```js
{name} = {name:'Max', age: 28};
console.log(name); // Max
console.log(age); // undefined

// The following component declarations 
// are equivalent:

function Greeting(props) {
  return <div>Hi {props.name}!</div>;
}

function Greeting({name}) {
  return <div>Hi {name}!</div>;
}

function Greeting({name, ...restProps}) {
  return <div>Hi {name}!</div>;
}
```
</div>
</div>

<div id="array">
<button type="button" class="collapsible">+ Array Functions (map() etc.)</button>   
<div class="content" style="display: none;" markdown="1">

A discussion of the mutating vs non-mutating array functions can be found here:
   * [https://lorenstewart.me/2017/01/22/javascript-array-methods-mutating-vs-non-mutating/](https://lorenstewart.me/2017/01/22/javascript-array-methods-mutating-vs-non-mutating/)


**[`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)**

* Applies a function to each element in an array and returns a new array with the result.
* This method does not mutate the array.
* Syntax: `const newArray = array.map(() => { })`

```js
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)** 

* The pop() method removes the last element from an array and returns that element. 
* **This method mutates the array.**
* Syntax: `const element = array.pop()`

```js
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];

console.log(plants.pop());
// expected output: "tomato"

console.log(plants);
// expected output: Array ["broccoli", "cauliflower", "cabbage", "kale"]

plants.pop();

console.log(plants);
// expected output: Array ["broccoli", "cauliflower", "cabbage"]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)** 

* The push() method adds one or more elements to the end of an array and returns the new length of the array.
* **This method mutates the array.**
* Syntax: `const len = array.push(element1[, ...[, elementN]])`

```js
const animals = ['pigs', 'goats', 'sheep'];

const count = animals.push('cows');
console.log(count);
// expected output: 4
console.log(animals);
// expected output: Array ["pigs", "goats", "sheep", "cows"]

animals.push('chickens', 'cats', 'dogs');
console.log(animals);
// expected output: Array ["pigs", "goats", "sheep", "cows", "chickens", "cats", "dogs"]
```

-------------------------------------------------------------------------------------------------------

**[`find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)** 

* Returns the **first** element in an array that matches the testing function. 
* This method does not mutate the array.

```js
const array1 = [5, 12, 8, 130, 44];

const res = array1.find(element => 
                          element > 10);

console.log(res);
// expected output: 12
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`findIndex()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)**

* Returns the index of the **first** element in an array that matches the testing function. 
* This method does not mutate the array.

```js
const array1 = [5, 12, 8, 130, 44];

const isLarge = (element) => 
                    element > 13;

console.log(array1.findIndex(isLarge));
// expected output: 3
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)** 

* Returns a new array that only contains elements of the input array that match the testing function. 
* This method does not mutate the array.

```js
const words = ['spray', 'limit', 'elite', 
                'enflame', 'dutiful', 
                'present'];

const res = words.filter(word => 
                          word.length > 6);

console.log(res);
// expected output: 
// Array ["enflame", "dutiful", "present"]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)** 

* Applies a [reducer](https://www.robinwieruch.de/javascript-reducer) function to each element of an array, resulting in a single output value.
* This method does not mutate the array.

```js
const array1 = [1, 2, 3, 4];
const reducer = (total, currentValue) => 
                      total + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)** 

* Returns a new array that is a concatenation of two or more arrays (using a shallow copy).
* This method does not mutate the array.
* Syntax 1: `array1.concat[array2, array3, ...arrayN]`

```js
const letters = ['a', 'b', 'c'];
const numbers = [1, 2, 3];

letters.concat(numbers);
// result in ['a', 'b', 'c', 1, 2, 3]
```

&nbsp;

* Syntax 2: `array1.concat[value1[, value2[, ...[, valueN]]]]`

```js
const letters = ['a', 'b', 'c'];

const result = letters.concat(1, [2, 3]);

console.log(result); 
// results in ['a', 'b', 'c', 1, 2, 3]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)** 

* Returns a new array containing a shallow copy of the selected elements of an array.
* This method does not mutate the array.
* Syntax: `slice[inclusive begin, exclusive end]`

```js
const animals = ['ant', 'bison', 
                  'camel', 'duck', 
                  'elephant'];

console.log(animals.slice(2));
// expected output: 
// Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: 
// Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: 
// Array ["bison", "camel", "duck", "elephant"]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)** 

* Changes the contents of an array by removing or replacing existing elements and/or adding new elements [in place](https://en.wikipedia.org/wiki/In-place_algorithm).
* **This method mutates the array.**
* Syntax: `let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

```js
const months = ['Jan', 'March', 
                'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at index 1

console.log(months);
// expected output: 
// Array ["Jan", "Feb", "March", 
//          "April", "June"]

months.splice(4, 1, 'May');
// replaces 1 element at index 4

console.log(months);
// expected output: 
// Array ["Jan", "Feb", "March", 
//          "April", "May"]
```

</div>
</div>

<div id="template">
<button type="button" class="collapsible">+ Template Literals/Strings</button>   
<div class="content" style="display: none;" markdown="1">

Template literals (a.k.a. template strings) use ticks marks to indicate that a string can contain placeholders, or is split over multiple lines.

e.g.
```js
`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`
```

An additional use of template literals is to "tag" a templated string with a function that accepts the template strings and placeholders as arguments:

```js
tag`string text ${expression} string text`
```
e.g.:
```js
function myTag(strings, ...placeHolders) {
  let str0 = strings[0]; // "the rain in "
  let str1 = strings[1]; // " falls mainly on the "
  
  let country = placeHolders[0]; // ${country}
  let location = placeHolders[1]; // ${location}

  return `${str0}${country}${str1}${location}`;
}

let country = 'Italy';
let location = 'Rome';
let text = myTag`the rain in ${country} falls mainly on ${location}`;

console.log(text); // the rain in Italy falls mainly on Rome
```
Note that the template function does not need to return a string.

Further information regarding template literals can be found here:
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
</div>
</div>

<div id="convertStringToProp">
<button type="button" class="collapsible">+ Converting a String to a Property Name</button>   
<div class="content" style="display: none;" markdown="1">

The ability to convert a string to a property name is known as "square bracket" notation.  

There are three main reasons to do this:
   * It allows the use of characters that can't be used with dot notation.
   * It is useful when dealing with property names that vary in a predictable way.
   * It can be used with properties that themselves contain a dot.

*Uncommon Characters*

```js
var foo = myForm.foo[]; // incorrect syntax
var foo = myForm["foo[]"]; // correct syntax
```

Note that this includes non-ASCII (UTF-8) characters, as in `myForm["ダ"]`.

*Predictable Property Names*

```js
for (var i = 0; i < 10; i++) {
  someFunction(myForm["myControlNumber" + i]);
}
```

```js
let event = 'click'
let handlers = {
  [`on${event}`]: true
}
// Same as: handlers = { 'onclick': true }
```

*Property Names Containing Dots*

For example, a json response could contain a property called `foo.Bar`.

```js
var foo = myResponse.foo.Bar; // incorrect syntax
var foo = myResponse["for.Bar"]; // correct syntax
```

</div>
</div>

<div id="convertStringTONum">
<button type="button" class="collapsible">+ Converting a String to a Number</button>   
<div class="content" style="display: none;" markdown="1">

Further information: [https://stackabuse.com/javascript-convert-string-to-number/](https://stackabuse.com/javascript-convert-string-to-number/)

In summary:
   * `.parseInt()` 
      * Takes a String as a first argument, and a base to which that String will be converted to. 
      * This method always returns an integer.
   * `.parseFloat()` 
      * Takes a String as an argument.
      * Returns the Float point number equivalent.
   * `Math.floor()`
      * Used to round an integer or floating point number.
      * Returns the nearest integer rounded down.
   * `Math.ceil() `
      * Used to round an integer or floating point number.
      * Returns the nearest integer rounded up.
   * `Unary Operator` 
      * By adding a `+` sign before a String, it will be converted into a number if it follows the right format.
      * e.g. `let myStr = '100.21'; +myStr; // 100.21`
      * This has good performance but lacks readability.
   * `Multiply by 1` 
      * If a String is multiplied by the primitive number 1, the string will become a number.
      * e.g. `let myStr = '100.21'; myStr * 1; // 100.21`
      * This also has good performance, and is slightly more readable (although the intention may not be clear).
</div>
</div>

<div id="convertJsonToObject">
<button type="button" class="collapsible">+ Converting JSON to an Object</button>   
<div class="content" style="display: none;" markdown="1">

Although JavaScript provides the `JSON.parse(src)` function, this throws an exception if the `src` is not valid JSON, which may not be desirable (it is generally bad practice to use try/catch for expected behaviour).  To avoid this, it is possible to add a `tryParse(src)` function to the `JSON` object.

For example,

```jsx
function tryParse(suspect) {
  suspect = typeof suspect !== 'string' ? JSON.stringify(suspect) : suspect;

  try {
    suspect = JSON.parse(suspect);
  } catch (e) {
    return { value: suspect, valid: false };
  }

  return { value: suspect, valid: typeof suspect === 'object' && suspect !== null };
}

JSON['tryParse'] = tryParse;
```

This can be used in the following manner:

```jsx
  const res = await fetch('https://api.github.com/repos/vercel/next.js');
  const src = await res.text();
  
  const result = JSON.tryParse(src);
  const parsedSrc = result.value;

  if (result.valid) {
    // handle valid JSON
  } else {
    // handle invalid JSON
  }
```

</div>
</div>

<div id="generators">
<button type="button" class="collapsible">+ Generators</button>   
<div class="content" style="display: none;" markdown="1">

A Generator simplifies creating an iterator.  It is defined using the `function*` (note that `*`) and `yield` syntax, and exposes the `next()` method.

For example:

```js
function* idMaker () {
  let id = 0
  while (true) { yield id++ }
}

let gen = idMaker()
gen.next().value  // → 0
gen.next().value  // → 1
gen.next().value  // → 2
```

Alternatively, a Generator can be iterated over using a for-of loop:

```js
for (const i of gen) {
  if (i > 100)
    break;
  console.log(`id: ${i}`);
}
```
</div>
</div>

<div id="semi">
<button type="button" class="collapsible">+ Semi-Colons!</button>   
<div class="content" style="display: none;" markdown="1">

Summarized from:
* [https://news.codecademy.com/your-guide-to-semicolons-in-javascript/](https://news.codecademy.com/your-guide-to-semicolons-in-javascript/).

Javascript is pretty flexible when it comes to the presence (or lack) of semi-colons, but there are some pitfalls.  If you following these guidelines, you shouldn't encounter any issues:

**Required**

Always put semi-colons after statements:

```js
var i;                        // variable declaration
i = 5;                        // value assignment
i = i + 1;                    // value assignment
i++;                          // same as above
var x = 9;                    // declaration & assignment
var fun = function() {...};   // var decl., assignmt, func. defin.
alert("hi");                  // function call
```

Strictly speaking, semi-colons are not required in these cases unless the statements are on the same line (e.g. `var i = 0; i++;`), but it is good practice to include them.

**Not Required**

Don't need to put semi-colons after a curly bracket *unless* something is being assigned (e.g. `var obj = {};`):

```js
if  (...) {...} else {...}
for (...) {...}
while (...) {...}

// function statement; no assignment so no semi-colon required: 
function (arg) { /*do this*/ }
```

In these cases, adding a semi-colon is harmless, but it is good practice to leave them out.

**Avoid**

After the closing parenthensis of an `if`, `for`, `while` or `switch` statement:

```js
if (0 === 1); { alert("hi") }

// equivalent to:

if (0 === 1) /*do nothing*/ ;
alert ("hi");
```

&nbsp;

**Exceptions**

* The closing parenthensis of a `do...while` loop must be terminated with a semi-colon:

`do {...} while (...);`

* The closing parenthensis of a Self-Executing Function:

```js
(function (parameters) {
    // Function body
})(parameters);
```

&nbsp;

* Inside the `()` of a `for` loop, semicolons only go after the first and second statement, never after the third:

```js
for (var i=0; i < 10; i++)  {/*actions*/} // correct
for (var i=0; i < 10; i++;) {/*actions*/} // SyntaxError
```
</div>
</div>

<div id="data-types">
<button type="button" class="collapsible">+ To Be Written Up</button>   
<div class="content" style="display: none;" markdown="1">

<div id="data-types">
<button type="button" class="collapsible">+ Data Types</button>   
<div class="content" style="display: none;" markdown="1">
  
Data Types:
primitives & objects
Primitives: number, object, string, boolean
Objects: Number, String, Function, Object, Array
Type Coercion: process of converting value from one type to another (e.g. string -> number, object -> boolean, etc.)
Implicit: 5 + "3" = "53"; true + false = 1; "6" - 1 = "5"
Explicit: Number("5"), String(true), Boolean(31)

Priority: boolean -> number -> string

Boolean("any string") == true

Loose Coercion: ==

False Positives: false, 0, 

All objects are evaluated to strings in concatentation

var myNumber = 2 // instanceof Number == false; myNumber === 2 = false; myNumber == 2 = true
var myNumber = Number(2) // instanceof Number == false
var myNumber = new Number(2) // instanceof Number == true

</div>
</div>

<div id="scope">
<button type="button" class="collapsible">+ Scope</button>   
<div class="content" style="display: none;" markdown="1">

Three levels of scope:
  * global
     * globalThis (var self = window || global;)
  * function
  * block

Scope inherits from parent to child.

Mixin:
interceptorMixin(authMixin(HttpClass))

let & const honour block level scope; var does not

</div>
</div>

<div id="polyfill">
<button type="button" class="collapsible">+ Shims & Polyfills</button>   
<div class="content" style="display: none;" markdown="1">

A shim is a library that brings a new API to an older environment, using on the means of that environment.

A polyfill is a type of shim that retrofits legacy browsers with modern HTML5/CSS3 features, usually using Javascript or Flash.

A polyfill is a property or function that is added to the prototype of an object

```js
function Runner(outerContainerId, opt_config) {
  // Singleton
  if (Runner.instance_) {
      return Runner.instance_;
  }
  Runner.instance_ = this;
}

// add polyfill
Runner.events = {
  ON_MOUSEUP: 'mouseup',
};
```

```js
class Runner {
  static get events() {
    return {
      ON_MOUSEUP: 'mouseup'
    }
  }
}
```
*NOTE*: Be careful not to overwrite existing functionality!
</div>
</div>

</div>
</div>

<div id="try-catch">
<button type="button" class="collapsible">+ Exceptions &amp; Try/Catch</button>   
<div class="content" style="display: none;" markdown="1">

This is basically the same as C# and Java:

```js
function KilledException(message, metadata) {
  const error = new Error(message);

  // code can be overridden
  error.code = "THIS_IS_A_CUSTOM_ERROR_CODE";
  
  // can also add additional properties
  error.metadata = metadata;
  
  return error;
}

KilledException.prototype = Object.create(Error.prototype);
```

```js
try {
  tryCode - Block of code to try
  
  // simplest case
  // throw "You died!";
  
  // better case
  //throw new Error("You died");
  
  // best case
  throw new KilledException("You died", { who: "Colonel Mustard", where: "Study", with: "Candlestick" } )
}
catch(err) {
  catchCode - Block of code to handle errors
  // to show details: err.message
  // to rethrow: throw err
}
finally {
  finallyCode - Block of code to be executed regardless of the try / catch result
}
```

&nbsp;

An OOP class-based alternative to the functional exception above would be:

```js
class KilledException extends Error {
  constructor(message, metadata) {
    super(message);
    this.name = 'KILLED_ERROR';
    this.code = "THIS_IS_A_CUSTOM_ERROR_CODE";
    this.metadata = metadata;  
  }
}
```

</div>
</div>

&nbsp;

&nbsp;

&nbsp;

-------------------------------------------------------------------------------------------------------

**Move along; nothing to see here...**

<script type="text/javascript">

    const loadCSS = (filename) => { 

       const file = document.createElement("link");
       file.setAttribute("rel", "stylesheet");
       file.setAttribute("type", "text/css");
       file.setAttribute("href", filename);
       document.head.appendChild(file);
    };

    const loadJS = (filename) => { 

       const file = document.createElement("script");
       file.setAttribute("type", "text/javascript");
       file.setAttribute("src", filename);
       document.head.appendChild(file);
    };
   
    //just call a function to load your CSS
    //this path should be relative your HTML location
    loadCSS("../collapse.css");
    loadJS("../collapse.js");

</script>
