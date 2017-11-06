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

A closure is a way of keeping access to variables in a function after that function has returned. Since closures keep access to the variables they can be used to save state. And things that save state look a whole lot like objects. (provides privacy and state).

* Can you describe the main difference between a `forEach` loop and a `.map()` loop and why you would pick one versus the other?

**map()** is more aligned with functional programming. It returns a new array, allowing you to chain functions after calling it. **forEach()** operates on our original array. You may be wondering why that matters. The idea is that a functional application is easier to debug because data structures are treated as immutable entities. 

* What's a typical use case for anonymous functions?

When I only need to do things one-time only or don't even invoke it in further.

Since Anonymous Functions are function expressions rather than the regular function declaration which are statements. Function expressions are more flexible. We can assign functions to variables, object properties, pass them as arguments to other functions, and even write a simple one line code enclosed in an anonymous functions.

* How do you organize your code? (module pattern, classical inheritance?)

In JavaScript, the Module pattern is used to further emulate the concept of classes in such a way that we’re able to include both public/private methods and variables inside a single object, thus shielding particular parts from the global scope.

Sometimes you don’t just want to use globals, but you want to declare them. We can easily do this by exporting them, using the anonymous function’s return value. Doing so will complete the basic module pattern, so here’s a complete example:

```javascript
var MODULE = (function () {
	var my = {},
		privateVariable = 1;

	function privateMethod() {
		// ...
	}

	my.moduleProperty = 1;
	my.moduleMethod = function () {
		// ...
	};

	return my;
}());
```

Modular code is self-contained, allowing you to hide the variable accessibility from the outside world and stand by itself.

* What's the difference between host objects and native objects?

**Host Objects** are objects supplied by a certain environment. **Native Objects or Built-in Objects** are standard built-in objects provided by Javascript. Native objects is sometimes referred to as ‘Global Objects’ since they are objects Javascript has provided natively available for use.

* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?

```javascript
function Person(){} // defines a function, probably a constructor/class

var person = new Person() // new keyword creates a new object, with its prototype set to Person's prototype, then calls constructor (instantiate an object based on a class)

var person = Person() // Declares a variable (person), invokes a function (Person) and sets the value of person to the return of the function.
```

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

For .call you pass the parameters comma separated (like normal). For .apply you pass the parameters in an array.

* Explain `Function.prototype.bind`.

**bind** allows you to set which object is treated as this within the function call.

```javascript
function Person(name){
  this.name = name;
  this.distractedGreeting = function() {
    setTimeout(function(){
      console.log("Hello, my name is " + this.name);
    }.bind(this), 500); // <-- this line!
  }
}

var alice = new Person('Alice');
alice.distractedGreeting();
// after 500ms logs "Hello, my name is Alice"
```

as opposed to this:

```javascript
function Person(name){
  this.name = name;
  this.distractedGreeting = function() {
    var self = this; // caching
    setTimeout(function(){
      console.log("Hello, my name is " + self.name);
      console.log(this.location.origin);
    }, 500); // no binding
  }
}
```

* When would you use `document.write()`?

**document.write()** writes to the document, but it will overwrite the entire page with the new content.

It seems that the only “approved” time to use document.write() is for third party code to be included (such as ads or Google Analytics). Since document.write() is always available (mostly) it is a good choice for third party vendors to use it to add their scripts.

* What's the difference between feature detection, feature inference, and using the UA string?

**Feature detection** is when you check if a feature exists. **Feature inference** is when you make an assumption that because one feature is present (or not) another one will also be present (or not). **UA string** provides information about the browser/os that the user is using (still making an assumption). Use feature detection if you're working with a feature that isn’t available across all browsers. When the browsers upgrade your code will be able to take advantage of the upgrade and your code will still work.

* Explain Ajax in as much detail as possible.

Asynchronous JavaScript and XML. Using JavaScript to send requests to the server and listen to the responses asynchronously. Can update the page after it has finished loading, so that the front end could change without a full page refresh, thus giving a much faster response time. More common for AJAX to get data and update the DOM as needed rather than doing a swap.

* What are the advantages and disadvantages of using Ajax?

Best use is the ability to send small payloads. 

Using AJAX to submit forms isn't always the best bet. Asides from not really giving you a clear advantage over posting the form normally, you break conventions such as browser history. When using AJAX, you need to handle the task of telling the user if something has gone wrong. 

Other issues to watch out for are any JavaScript errors that may prevent your events from firing - or if JavaScript is disabled, in either case ensuring that the form can submit normally before you add the AJAX code is the safest option.

* Explain how JSONP works (and how it's not really Ajax).

JSONP(as in “JSON with Padding”) is a method commonly used to bypass the cross-domain policies in web browsers (you are not allowed to make AJAX requests to a webpage perceived to be on a different server by the browser). JSON and JSONP behave differently on both the client and the server. JSONP requests are not dispatched using the XMLHTTPRequest,instead a `<script>` tag is created, whose source is set to the target URL. This script tag is then added to the DOM (normally the `<head>`).

One major issue with JSONP: you lose a lot of control of the request. For example, there is no "nice" way to get proper failure codes back. These days (2015), CORS is the recommended approach vs. JSONRequest. JSONP is still useful for older browser support, but given the security implications, unless you have no choice CORS is the better choice.

* Have you ever used JavaScript templating?
  * If so, what libraries have you used?

Yes, have used Handlebars briefly for a project in the Daily Bruin. Also used it when using Angular over the summer.

* Explain "hoisting".

Variable declarations (and declarations in general) are processed before any code is executed, declaring a variable anywhere in the code is equivalent to declaring it at the top. This also means that a variable can appear to be used before it’s declared. This behavior is called “hoisting”, as it appears that the variable declaration is moved to the top of the function or global code.

* Describe event bubbling.

vent bubbling and capturing are two ways of event propagation in the HTML DOM API, when an event occurs in an element inside another element, and both elements have registered a handle for that event. The event propagation mode determines in which order the elements receive the event. With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements. With capturing, the event is first captured by the outermost element and propagated to the inner elements.

* What's the difference between an "attribute" and a "property"?

Attributes are defined by HTML. Properties are defined by DOM. Some HTML attributes have 1:1 mapping onto properties. id is one example of such. Some do not (e.g. the value attribute specifies the initial value of an input, but the valueproperty specifies the current value).

Properties are generally simpler to deal with than attributes. An attribute value may only be a string whereas a property can be of any type.

Where both a property and an attribute with the same name exists, usually updating one will update the other, but this is not the case for certain attributes of inputs, such as value and checked: for these attributes, the property always represents the current state while the attribute (except in old versions of IE) corresponds to the default value/checkedness of the input (reflected in the defaultValue / defaultChecked property).

* Why is extending built-in JavaScript objects not a good idea?

Don’t do it because you might end up breaking other’s codes. If, in future, a browser decides to implement its own version of your method, your method might get overridden (silently) and the browser’s implementation (which is probably different from yours) would take over

* Difference between document load event and document DOMContentLoaded event?

**DOMContentLoaded** - the whole document (HTML) has been loaded. **load** — the whole document and its resources (e.g. images, iframes, scripts) have been loaded.

* What is the difference between `==` and `===`?

The identity (===) operator behaves identically to the equality (==) operator except no type conversion is done, and the types must be the same to be considered equal.

* Explain the same-origin policy with regards to JavaScript.

The same origin policy states that a web browser permits script contained in one page (or frame) to access data in another page (or frame) only if both the pages have the same origin. It is a critical security mechanism for isolating potentially malicious documents. Two pages have the same origin if the protocol, port (if one is specified), and host are the same for both pages. 

* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]

function duplicate(a) {
  return a.concat(a);
}
```
* Why is it called a Ternary expression, what does the word "Ternary" indicate?

“Ternary” means operands with three(n-ary) param. This is a one-line shorthand for an if-then statement. It is called a ternary operator or a conditional operator. Read this for example.

* What is `"use strict";`? what are the advantages and disadvantages to using it?

Strict mode is a way to opt in to a restricted variant of JavaScript. Strict mode isn’t just a subset: it intentionally has different semantics from normal code. Browsers not supporting strict mode will run strict mode code with different behavior from browsers that do, so don’t rely on strict mode.

Strict Mode is a new feature in ECMAScript 5 that allows you to place a program, or a function, in a “strict” operating context. This strict context prevents certain actions from being taken and throws more exceptions (generally providing the user with more information and a tapered-down coding experience).

Pros: It catches some common coding bloopers, throwing exceptions. It prevents, or throws errors, when relatively “unsafe” actions are taken (such as gaining access to the global object). It disables features that are confusing or poorly thought out.

Cons: The best explanation I found was when code mixed strict and “normal” modes. If a developer used a library that was in strict mode, but the developer was used to working in normal mode, they might call some actions on the library that wouldn’t work as expected. 

* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`

```javascript
for (var i=1; i <= 100; i++)
{
    if (i % 15 == 0)
        console.log("fizzbuzz");
    else if (i % 3 == 0)
        console.log("fizz");
    else if (i % 5 == 0)
        console.log("buzz");
    else
        console.log(i);
}
```

* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?

The primary reason why global variables are discouraged in javascript is because, in javascript all code share a single global namespace, also javascript has implied global variables i.e. variables which are not explicitly declared in local scope are automatically added to global namespace. Relying too much on global variables can result in collisions between various scripts on the same page.

* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?

The load event fires at the end of the document loading process. At this point, all of the objects in the document are in the DOM, and all the images, scripts, links and sub-frames have finished loading. To execute anything post document load, we fire these events. ‘DOMContentLoaded’ or jQuery’s loaded are another options.

* Explain what a single page app is and how to make one SEO-friendly.

A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluid user experience similar to a desktop application. In a SPA, either all necessary code — HTML, JavaScript, and CSS — is retrieved with a single page load, or the appropriate resources are dynamically loaded and added to the page as necessary, usually in response to user actions.

Since Googlebots will execute your JavaScript, your page contents and links will be available for them. Can also add a Sitemap file named sitemap.xml at your web server root directory to tell crawlers what links do you have in your website. Also can pre-render the page and make it available for crawlers.

* What is the extent of your experience with Promises and/or their polyfills?

Familiar with it because I worked with it over the summer at my internship with Angular. 

Basic usage:
```javascript
new Promise((resolve, rejected) => {
    if(resolve){
        resolve("successed");
    }
    else{
        rejected("failed");
    }
})
.then((result) => {
    console.log(result);
})
.then((result) => {
    console.log(result);
});
```

* What are the pros and cons of using Promises instead of callbacks?

It is fair to say promises are just syntactic sugar. Everything you can do with promises you can do with callbacks. The deep reason why promises are often better is that they’re more composeable, which roughly means that combining multiple promises “just works” and is more readable, while combining multiple callbacks often doesn’t and creates callback hell.

* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?

Pros/Cons: Syntactic sugar, readable code, and use of good patterns vs debugging and compilation/debugging issues.

* What tools and techniques do you use debugging JavaScript code?

Web/Browser console using console.log. Chrome Developer Tools, break points.

* What language constructions do you use for iterating over object properties and array items?

for loop, for..in, for each..in, map, reduce 

* Explain the difference between mutable and immutable objects.

Mutable objects are those whose state is allowed to change over time. An immutable value is the exact opposite — after it has been created, it can never change. Strings and Numbers are inherently immutable in javascript.

  * What is an example of an immutable object in JavaScript?

Strings and numbers are immutable.

  * What are the pros and cons of immutability?

Pros: Programs with immutable objects are less complicated to think about, since you don't need to worry about how an object may evolve over time. Immutable objects are good for sharing information between threads in a multi-threaded environment since they don't need to be synchronized.

Cons: Allocating lots and lots of small objects rather than modifying ones you already have can have a performance impact. Usually the complexity of either the allocator or the garbage collector depends on the number of objects on the heap. Naive implementations of immutable data structures can result in extremely poor performance. 

  * How can you achieve immutability in your own code?

Instead of passing the object and mutating it, can create a completely new object. Can use something like Immutable.js, which provides many Persistent Immutable data structures including: List, Stack, Map, OrderedMap, Set, OrderedSet and Record.

* Explain the difference between synchronous and asynchronous functions.

Synchronous: Step wise execution. Next line executed after first. Asynchronous: Execution moves to next step before first is finished. 

* What is event loop?
  * What is the difference between call stack and task queue?

JavaScript has a concurrency model based on an “event loop”. 

```javascript
while (queue.waitForMessage()) {
  queue.processNextMessage();
}
```

Call stack: In a loop, the queue is polled for the next message (each poll referred to as a “tick”) and when a message is encountered, the callback for that message is executed.

Task queue: A JavaScript runtime contains a message queue, which is a list of messages to be processed. A function is associated with each message. When the stack has enough capacity, a message is taken out of the queue and processed. The processing consists of calling the associated function (and thus creating an initial stack frame). The message processing ends when the stack becomes empty again.
  
* Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`

First one is declaration defined at parse time while the other is expression defined at run time.

* What are the differences between variables created using `let`, `var` or `const`?

`const` is a signal that the identifier won’t be reassigned.

`let`, is a signal that the variable may be reassigned, such as a counter in a loop, or a value swap in an algorithm. It also signals that the variable will be used only in the block it’s defined in, which is not always the entire containing function.

`var` is now the weakest signal available when you define a variable in JavaScript. The variable may or may not be reassigned, and the variable may or may not be used for an entire function, or just for the purpose of a block or loop.