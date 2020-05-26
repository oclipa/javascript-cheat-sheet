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

```javascript
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

```javascript
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

```javascript
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

Im suuary, there are two basic uses cases:
1. Storing the state of data to be used in a particular method at a later time.
1. Hiding data but leaving it accessible to a particular method.

The following are a couple of illustrative examples...

This first example demonstrates a problem that closures can solve.  At first glance this might be expected to print out (0, 1, 2, ...etc), but the actual output will be (10, 10, 10, ...etc).  This is because `i` exists in the scope of the `createPrinters` function, so the latest value (10) will be used when the function is evaluated.

```javascript
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
This second example shows a solution to the first example. `val` is scoped within a closure (basically, a couple of nested functions), so it will retain whichever value it had when the closure was instantiated.  When the `createPrinters` function is evaluated, the output will be the less surprising (0, 1, 2, ...etc).

```javascript
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

This final example demonstrates how a closure can be used for data privacy.  In this case, our hero's profession can only be revealed by asking him directly (with a password!), unlike his appearance, which can be seen just by looking at him:

```javascript
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

peek(james);
ask(james, "skyfall");
```
</div>
</div>

<div id="curry">
<button type="button" class="collapsible">+ Currying</button>   
<div class="content" style="display: none;" markdown="1">

* What is currying?

* How does currying work?

* Why is currying useful?

* https://blog.bitsrc.io/understanding-currying-in-javascript-ceb2188c339
* https://blog.bitsrc.io/understanding-currying-in-javascript-ceb2188c339
* https://bjouhier.wordpress.com/2011/04/04/currying-the-callback-or-the-essence-of-futures/

</div>
</div>

<div id="hof">
<button type="button" class="collapsible">+ Higher Order Functions</button>   
<div class="content" style="display: none;" markdown="1">

A way of composing functions.  A Higher Order Function accepts one or more functions are arguments and returns another function.

</div>
</div>

<div id="oneway">
<button type="button" class="collapsible">+ One-Way Data Flow</button>   
<div class="content" style="display: none;" markdown="1">

* One-way data flow means that the model is the single source of truth.
* The UI can signal changes to the model but only the model can change the app's state.
* Data is input from the UI and then passed back to the model to be manipulated.  The results are then passed down the component tree to be redisplayed.
* This effectively means that the application follows the Data Down, Action Up pattern, which makes it easier to understand.

user notifies intention to update
=====> intention sent "up" to model 
==========> model decides what to do with intention
<========== model updates based on intention
<===== updated properties sent "down" to components
updated properties displayed to user

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

```javascript
import './oneway.css';

const quotes = [
  {
    author: 'Albert Einstein',
    quote: 'Strive not to be a success, but rather to be of value.'
  },
  {
    author: 'Robert Frost',
    quote: 'Two roads diverged in a wood, and I—I took the one less traveled by, And that has made all the difference.'
  }
];

// timer mimics user interaction
setInterval(() => {

  // 1) at intervals, a random quote is "selected by the user"
  const index = Math.floor(Math.random() * Math.floor(quotes.length));
  const { author, quote } = quotes[index];
  
  // 2) selected quote is "sent to model"
  Object.assign(state, {
    author: author,
    quote: quote
  });
}, 2000);

// Proxy mimics model
const createState = (state) => {
  return new Proxy(state, {
    set: (state, property, value) => {
    
      // 3) state is updated
      state[property] = value;
      
      // 4) updated state is "sent to UI"
      render();
      return true;
    }
  });
};

const state = createState(quotes[0]);

const render = () => {
  const bindings = Array.from(document.querySelectorAll('[data-binding]')).map(
    e => e.dataset.binding
  );
  bindings.forEach(binding => {
    if (state[binding]) {
      document.querySelector(`[data-binding='${binding}']`).innerHTML = state[binding];
    } else {
      throw new ReferenceError(`${binding} is a not a member of the current state`);
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

user 1 interacts with view 1
<=====> model 1 reflects interaction
<==========> model 1 updates model 2
view 1 updates to reflect model 1 and model 2
view 2 updates to reflect model 1 and model 2

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

<div id="createobj">
<button type="button" class="collapsible">+ Creating An Object</button>   
<div class="content" style="display: none;" markdown="1">

There are several approaches to creating objects in Javascript.

   * Using an Object Literal.
   * Using a Factory Function.
   * Using Object.create().
   * Using Prototypal Inheritance.
   * Using Classes.

As a general rule of thumb, Prototypal Inheritance and Classes should be avoided, since these create problems for functional programming.

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using an Object literal*

```javascript
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

*Using a Factory Function*

```javascript
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

*Using Object.create()*

```javascript
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

*Using Prototypal Inheritance*

```javascript
function Rodent(rodentType, furColor, legs, tail) {
  this.rodentType = rodentType;
  this.furColor = furColor;
  this.legs = legs;
  this.tail = tail;
}

// add describe function
Rodent.prototype.describe = function() {
  return ('A ' + this.rodentType +', with ' + this.furColor +' fur, ' + this.legs +' legs, and a ' + this.tail +' tail.');
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
  return ('A ' + this.rodentType +', with ' + this.furColor +' fur, ' + this.legs +' legs, and a ' + this.tail +' tail. It likes ' + this.food +'.');
}

const rodent = new Rodent('vole', 'grey', 4, 'short, stubby');
console.log(rodent.describe());

const mouse = new Mouse('mouse', 'brown', 4, 'long, skinny', 'cheese');
console.log(mouse.describe());

```

&nbsp;

-------------------------------------------------------------------------------------------------------

*Using Classes*

Note that `class` is simply syntactical sugar on prototype inheritance, so this example and the preceeding one are essentially identical under-the-covers.  The `class` pattern in ES6 is not the same as classical inheritance.

```javascript
class Rodent {
  constructor(rodentType, furColor, legs, tail) {
  this.rodentType = rodentType;
  this.furColor = furColor;
  this.legs = legs;
  this.tail = tail;
  }
  
  describe() {
      return ('A ' + this.rodentType +', with ' + this.furColor +' fur, ' + this.legs +' legs, and a ' + this.tail +' tail.');
  }
}

class Mouse extends Rodent {
  constructor(rodentType, furColor, legs, tail, food) {
    super(rodentType, furColor, legs, tail);
    this.food = food;
  }
  
  describe() {
    return ('A ' + this.rodentType +', with ' + this.furColor +' fur, ' + this.legs +' legs, and a ' + this.tail +' tail. It likes ' + this.food +'.');
  }
}

// Actual usage remains the same
const rodent = new Rodent('vole', 'grey', 4, 'short, stubby');
console.log(rodent.describe());

const mouse = new Mouse('mouse', 'brown', 4, 'long, skinny', 'cheese');
console.log(mouse.describe());
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

```javascript   
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

```javascript
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
<button type="button" class="collapsible">+ Self-Executing Functions</button>   
<div class="content" style="display: none;" markdown="1">

Self-Executing Functions (a.k.a Immediately Invoked Function Expressions) are functions which are invoked immediately after being defined, i.e. they don't need to be explicitly called elsewhere in the code.

The general form is `(function(){ })();`.  
   * The parentheses around the function are to ensure that the code within the function is contained in the private scope of the function.
   * The parentheses at the end of the function are what invokes the function.
</div>
</div>

<div id="arrow">
<button type="button" class="collapsible">+ Arrow Functions (=>)</button>   
<div class="content" style="display: none;" markdown="1">

"Traditional" Function (ES5):

```javascript
function myFunc() {
  console.log(arguments);
  return ...
}
```

Arrow Function (ES6):

```javascript
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

<div id="spread">
<button type="button" class="collapsible">+ Spread and Rest Operators (...)</button>   
<div class="content" style="display: none;" markdown="1">

Both Spread and Rest use the same operator: `...`

&nbsp;

-------------------------------------------------------------------------------------------------------

**Spread:**
   * Used to split up (i.e. spread) array elements OR object properties.

```javascript
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

```javascript
var dogPref = ["Dogs" , "Like" , "Bones"];
const [animal , ...pref] = dogPref;
console.log(animal); // Dogs
console.log(pref); // [ "Like", "Bones"]
```

A more common case is for handling arguments passed to a function:

```javascript
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

```javascript
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

```javascript
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

**[`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)**

* Applies a function to each element in an array and returns a new array with the result.

```javascript
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)** 

* Returns the **first** element in an array that matches the testing function. 

```javascript
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

```javascript
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

```javascript
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

```javascript
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

-------------------------------------------------------------------------------------------------------

**[`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)** 

* Returns a new array that is a concatenation of two or more arrays (using a shallow copy).
* Syntax 1: `array1.concat[array2, array3, ...arrayN]`

```javascript
const letters = ['a', 'b', 'c'];
const numbers = [1, 2, 3];

letters.concat(numbers);
// result in ['a', 'b', 'c', 1, 2, 3]
```

&nbsp;

* Syntax 2: `array1.concat[value1[, value2[, ...[, valueN]]]]`

```javascript
const letters = ['a', 'b', 'c'];

const result = letters.concat(1, [2, 3]);

console.log(result); 
// results in ['a', 'b', 'c', 1, 2, 3]
```

&nbsp;

-------------------------------------------------------------------------------------------------------

**[`slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)** 

* Returns a new array containing a shallow copy of the selected elements of an array.
* Syntax: `slice[inclusive begin, exclusive end]`

```javascript
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

* Changes the contents of an array by removing or replacing existing elements and/or adding new elements [in place](https://en.wikipedia.org/wiki/In-place_algorithm) (i.e. it does not create a new array).
* Syntax: `let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

```javascript
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

<div id="semi">
<button type="button" class="collapsible">+ Template Strings</button>   
<div class="content" style="display: none;" markdown="1">

* template strings 'some text and a ${templateString} can be output ${likeThis}'

</div>
</div>

<div id="convert">
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

<div id="semi">
<button type="button" class="collapsible">+ Semi-Colons!</button>   
<div class="content" style="display: none;" markdown="1">

Summarized from:
* [https://news.codecademy.com/your-guide-to-semicolons-in-javascript/](https://news.codecademy.com/your-guide-to-semicolons-in-javascript/).

Javascript is pretty flexible when it comes to the presence (or lack) of semi-colons, but there are some pitfalls.  If you following these guidelines, you shouldn't encounter any issues:

**Required**

Always put semi-colons after statements:

```javascript
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

```javascript
if  (...) {...} else {...}
for (...) {...}
while (...) {...}

// function statement; no assignment so no semi-colon required: 
function (arg) { /*do this*/ }
```

In these cases, adding a semi-colon is harmless, but it is good practice to leave them out.

**Avoid**

After the closing parenthensis of an `if`, `for`, `while` or `switch` statement:

```javascript
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

```javascript
(function (parameters) {
    // Function body
})(parameters);
```

&nbsp;

* Inside the `()` of a `for` loop, semicolons only go after the first and second statement, never after the third:

```javascript
for (var i=0; i < 10; i++)  {/*actions*/} // correct
for (var i=0; i < 10; i++;) {/*actions*/} // SyntaxError
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
