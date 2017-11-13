*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```

1020

*Question: What will be the output of the code below?*
```javascript
console.log(0.1 + 0.2 == 0.3);
```

false, floating point numbers not exactly precise

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```

'''javascript
function add(x, y) {
    if (y) {
        return x+y;
    }
    else {
        return function(y2) {
            return x+y2;
        }
    }
}
```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

same thing but reversed

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```

bar

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

hello world, then undefined

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

2

*Question: What is the value of `foo.x`?*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

foo.x is undefined, but bar.x is {n: 2}. This is because `foo.x = foo = {n: 2};` determines that foo.x refers to a property x of the {n: 1} object, assigns {n: 2} to foo, and assigns the new value of foo – {n: 2} – to the property x of the {n: 1} object.

The foo that foo.x refers is a different object. 

*Question: What does the following code print?*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

one, three, two - has to do with the call stack, in that settimeout executes the function after everything else has been executed with 0 delay (setTimeout() re-queues the new JavaScript at the end of the execution queue)

*Question: What is the difference between these four promises?*
```javascript
doSomething().then(function () {
  return doSomethingElse();
});

doSomething().then(function () {
  doSomethingElse();
});

doSomething().then(doSomethingElse());

doSomething().then(doSomethingElse);
```

* first one does something after dosomething returns, and then returns the result of that...so u can chain functions after that then as well

* second one does not return anything, so you cannot chain after it because the return is undefined (also anything after that will not have to wait for return value because it automatically returns undefined)

* third one runs the two functions at the same time because it calls the function rather than passing it as a callback

* last one is similar to the first one...runs in the correct order