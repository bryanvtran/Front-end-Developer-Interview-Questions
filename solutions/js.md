# JS Questions:

* Explain event delegation

Event delegation allows you to avoid adding event listeners to specific nodes;  instead, the event listener is added to one parent.  That event listener analyzes bubbled events to find a match on child elements. 

Example:
```javascript
// Get the element, add a click listener...
document.getElementById("parent-list").addEventListener("click", function(e) {
	// e.target is the clicked element!
	// If it was a list item
	if(e.target && e.target.nodeName == "LI") {
		// List item found!  Output the ID!
		console.log("List item ", e.target.id.replace("post-", ""), " was clicked!");
	}
});
```

* Explain how `this` works in JavaScript

In a similar graceful manner, in JavaScript, we use the this keyword as a shortcut, a referent; it refers to an object; that is, the subject in context, or the subject of the executing code.

The this reference ALWAYS refers to (and holds the value of) an object—a singular object—and it is usually used inside a function or a method, although it can be used outside a function in the global scope. Note that when we use strict mode, this holds the value of undefined in global functions and in anonymous functions that are not bound to any object.

From what I understand, ‘this’ refers to itself, to its own object or global object.
Using ‘this’ are partitioned in 3 locations of code. These are in functions, outside of functions (global scope, ex: window object), and in Javascript’s eval() function.


The value of this is determined by how a function is called. ES5 introduced the bind method to set the value of a function's this regardless of how it's called, and ES2015 introduced arrow functions which do provide their own this binding. In web browsers, the window object is also the global object.

* Explain how prototypal inheritance works

Everything in Javascript is an object. Even when creating a Class is an Object via an Object Literal or Constructor Function. This is how Javascript does class-based programming as to other traditional Object-Oriented Programming languages where they use the keyword ‘class’ and ‘inheritance’.

Nearly all objects in JavaScript are instances of Object which sits on the top of a prototype chain.

```javascript
// Let's assume we have object o, with its own properties a and b:
// {a: 1, b: 2}
// o.[[Prototype]] has properties b and c:
// {b: 3, c: 4}
// Finally, o.[[Prototype]].[[Prototype]] is null.
// This is the end of the prototype chain, as null,
// by definition, has no [[Prototype]].
// Thus, the full prototype chain looks like:
// {a: 1, b: 2} ---> {b: 3, c: 4} ---> null

console.log(o.a); // 1
// Is there an 'a' own property on o? Yes, and its value is 1.

console.log(o.b); // 2
// Is there a 'b' own property on o? Yes, and its value is 2.
// The prototype also has a 'b' property, but it's not visited. 
// This is called "property shadowing."

console.log(o.c); // 4
// Is there a 'c' own property on o? No, check its prototype.
// Is there a 'c' own property on o.[[Prototype]]? Yes, its value is 4.

console.log(o.d); // undefined
// Is there a 'd' own property on o? No, check its prototype.
// Is there a 'd' own property on o.[[Prototype]]? No, check its prototype.
// o.[[Prototype]].[[Prototype]] is null, stop searching,
// no property found, return undefined.

var o = {a: 1};

// The newly created object o has Object.prototype as its [[Prototype]]
// o has no own property named 'hasOwnProperty'
// hasOwnProperty is an own property of Object.prototype. 
// So o inherits hasOwnProperty from Object.prototype
// Object.prototype has null as its prototype.
// o ---> Object.prototype ---> null

var b = ['yo', 'whadup', '?'];

// Arrays inherit from Array.prototype 
// (which has methods indexOf, forEach, etc.)
// The prototype chain looks like:
// b ---> Array.prototype ---> Object.prototype ---> null

function f() {
  return 2;
}

// Functions inherit from Function.prototype 
// (which has methods call, bind, etc.)
// f ---> Function.prototype ---> Object.prototype ---> null
```

hasOwnProperty is the only thing in JavaScript which deals with properties and does not traverse the prototype chain.

* What do you think of AMD vs CommonJS?

AMD and CommonJS are both Javascript module loader.

CommonJS is a way of defining modules with the help of an exports object, that defines the module contents. Simply put, a CommonJS implementation might work like this: 

```javascript
// someModule.js
exports.doSomething = function() { return "foo"; };

//otherModule.js
var someModule = require('someModule'); // in the vein of node    
exports.doSomethingElse = function() { return someModule.doSomething() + "bar"; };
```

AMD is more suited for the browser, because it supports asynchronous loading of module dependencies.

CommonJS is more than that - it's a project to define a common API and ecosystem for JavaScript. One part of CommonJS is the Module specification. Node.js and RingoJS are server-side JavaScript runtimes, and yes, both of them implement modules based on the CommonJS Module spec.

AMD (Asynchronous Module Definition) is another specification for modules. RequireJS is probably the most popular implementation of AMD. One major difference from CommonJS is that AMD specifies that modules are loaded asynchronously - that means modules are loaded in parallel, as opposed to blocking the execution by waiting for a load to finish.

AMD is generally more used in client-side (in-browser) JavaScript development due to this, and CommonJS Modules are generally used server-side. However, you can use either module spec in either environment - for example, RequireJS offers directions for running in Node.js and browserify is a CommonJS Module implementation that can run in the browser.

Usually, JS files are loaded in order via script tag in HTML templates, but files and code gets complicated once an application becomes large. Javascript module loaders lets us separate our code into modules and include a specific module in another module. This lets us import what module is required and load only the necessary. Better Javascript file size load and better code compartmentalization, means, JS module loader mitigates away the danger of global-namespace issue.

* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.

Immediately Invoked Function Expressions. Doesn't work because it's a function definition, it defines foo. But it’s not a function expression - that is, it’s not understood by the JS parser to actually call a function.

  * What needs to be changed to properly make it an IIFE?

```javascript
( // oh goody, we're going to call some function expressions!
  function foo(){ // here's the function definition
  }() // and here's where the function is actually called
);
```

* What's the difference between a variable that is: `null`, `undefined` or undeclared?

A variable is **undeclared** when it does not use the var keyword. It gets created on the global object (that is, the window), thus it operates in a different space as the declared variables. Doesn't exist.

Something is **undefined** when it hasn’t been defined yet. If you call a variable or function without having actually created it yet the parser will give you an not defined error. It is a primitive type. Exists, but not value.

**null** is a variable that is defined to have a null value. Exists with null value assigned to it.

  * How would you go about checking for any of these states?

```javascript

// undeclared
var declaredVariable = 1;

function scoppedVariables() {
  undeclaredVariable = 1;
  var declaredVariable = 2;
}

scoppedVariables();

undeclaredVariable; // 1
declaredVariable; // 1

// undefined
var undefinedVariable; // undefined
typeof undefinedVariable; // "undefined"

undefinedFunction(); // undefined
typeof undefinedFunction; // "undefined"

// null

var nullVariable = null; // null
typeof nullVariable // "object"

if( variable === null ) {
  console.log('variable is null');
} else {
  console.log('variable is not null');
}
```
* What is a closure, and how/why would you use one?

Closures are inner functions inside of an outer function. They have their own local scope and has access to outer function’s scope, parameters (but NOT arguments object), and they also have access to global variables.

Closures are a neat way to deal with scope issues. Because Javascript is a function-level scope rather than as with other languages, block-level scope and Javascript is an asynchronous/event driven language. Example that Closure is frequently used is jQuery (ex. click()).

A closure is a way of keeping access to variables in a function after that function has returned. Since closures keep access to the variables they can be used to save state. And things that save state look a whole lot like objects.

* Can you describe the main difference between a `forEach` loop and a `.map()` loop and why you would pick one versus the other?
* What's a typical use case for anonymous functions?
* How do you organize your code? (module pattern, classical inheritance?)
* What's the difference between host objects and native objects?
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
* What's the difference between `.call` and `.apply`?

To pass the value of this from one context to another, use call, or apply.

```javascript
// An object can be passed as the first argument to call or apply and this will be bound to it.
var obj = {a: 'Custom'};

// This property is set on the global object
var a = 'Global';

function whatsThis(arg) {
  return this.a;  // The value of this is dependent on how the function is called
}

whatsThis();          // 'Global'
whatsThis.call(obj);  // 'Custom'
whatsThis.apply(obj); // 'Custom'

function add(c, d) {
  return this.a + this.b + c + d;
}

var o = {a: 1, b: 3};

// The first parameter is the object to use as
// 'this', subsequent parameters are passed as 
// arguments in the function call
add.call(o, 5, 7); // 16

// The first parameter is the object to use as
// 'this', the second is an array whose
// members are used as the arguments in the function call
add.apply(o, [10, 20]); // 34
```

* Explain `Function.prototype.bind`.
* When would you use `document.write()`?
* What's the difference between feature detection, feature inference, and using the UA string?
* Explain Ajax in as much detail as possible.
* What are the advantages and disadvantages of using Ajax?
* Explain how JSONP works (and how it's not really Ajax).
* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
* Explain "hoisting".
* Describe event bubbling.
* What's the difference between an "attribute" and a "property"?
* Why is extending built-in JavaScript objects not a good idea?
* Difference between document load event and document DOMContentLoaded event?
* What is the difference between `==` and `===`?
* Explain the same-origin policy with regards to JavaScript.
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
* Why is it called a Ternary expression, what does the word "Ternary" indicate?
* What is `"use strict";`? what are the advantages and disadvantages to using it?
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
* Explain what a single page app is and how to make one SEO-friendly.
* What is the extent of your experience with Promises and/or their polyfills?
* What are the pros and cons of using Promises instead of callbacks?
* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
* What tools and techniques do you use debugging JavaScript code?
* What language constructions do you use for iterating over object properties and array items?
* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?
* Explain the difference between synchronous and asynchronous functions.
* What is event loop?
  * What is the difference between call stack and task queue?
* Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`
* What are the differences between variables created using `let`, `var` or `const`?