# ES6/2015/2016/2017/Next...Huh?

## ES what?

Don't you mean JS as in Javascript and what's with 6, 2015, 2016, 2017, 'Next'. What one is it? I'll leave it up to Kyle Simpson in You Don't Know JS to explain:

```
The JavaScript standard is referred to officially as "ECMAScript" (abbreviated "ES"), and up until just recently has been versioned entirely by ordinal number (i.e., "5" for "5th edition").

The earliest versions, ES1 and ES2, were not widely known or implemented. ES3 was the first widespread baseline for JavaScript, and constitutes the JavaScript standard for browsers like IE6-8 and older Android 2.x mobile browsers. For political reasons beyond what we'll cover here, the ill-fated ES4 never came about.

In 2009, ES5 was officially finalized (later ES5.1 in 2011), and settled as the widespread standard for JS for the modern revolution and explosion of browsers.

Leading up to the expected next version of JS (slipped from 2013 to 2014 and then 2015), the obvious and common label in discourse has been ES6.

However, late into the ES6 specification timeline, suggestions have surfaced that versioning may in the future switch to a year-based schema, such as ES2016 (aka ES7) to refer to whatever version of the specification is finalized before the end of 2016. Some disagree, but ES6 will likely maintain its dominant mindshare over the late-change substitute ES2015. However, ES2016 may in fact signal the new year-based schema.
```

- Kyle Simpson, [You Don't Know JS: Es6 & Beyond](https://github.com/getify/You-Dont-Know-JS/tree/master/es6%20%26%20beyond)

## Browser Support

Feature by Feature browser support can be found here: https://kangax.github.io/compat-table/es6/

**TLDR:** if you have to support IE or Android, you're gonna have a bad time.

## Node.js Support

Feature by Feature Node.js support can be found here: http://node.green

**TLDR:** Node.js 6.4+ you should be good.

## Feature by Feature

I'm just going to do a high level feature by feature of what I think is neat or really valuable. Please dig deeper into the various specs because 'ES6' will be a regular part of the language soon without the need of compilers.

### `var` vs. `let` and `const`

* var: gets hoisted
* let: lives within block (curly braces)
* const: `constant` (immutable, can't be changed) also lives within blocks

```
var points = 50;
var winner = false;

if(points > 40) {
   var winner = true
   console.log(winner) // true
}
console.log(winner) // true
```

```
let points = 50;
let winner = false;

if(points > 40) {
   let winner = true
   console.log(winner) // true
}
console.log(winner) // false
```

```
const points = 50;
const winner = false;

if(points > 40) {
   const winner = true // winner = true would also cause an error
   console.log(winner) // ERROR
}
console.log(winner) // ERROR
```

### Arrow Functions

```
var fn1 = function() {return 2;};
var fn2 = () => 2; // Here you can omit curly braces. It means return 2. If you add curly braces then you have to put the word 'return'.
```

Parenthesis-Parameter Rules

```
var x;
x = () => {};       // No parameters, MUST HAVE PARENS
x = (val) => {};    // One parameter w/ parens, OPTIONAL
x = val => {};      // One parameter w/o parens, OPTIONAL
x = (y, z) => {};   // Two or more parameters, MUST HAVE PARENS
x = y, z => {};     // Syntax Error: must wrap with parens when using multiple params
```

REAL benefit: lexical binding of 'this'. You don't need to `.bind(this)` or `var that = this`.

Before:
```
var widget = {
  init: function() {
    var that = this;
    document.addEventListener("click", function (event) {
      that.doSomething(event.type);
    }, false);
  },
  doSomething: function(type) {
    console.log("Handling " + type + " event");
  }
};
Widget.init();
```

Now:
```
var widget = {
  init: function() {
    document.addEventListener("click", (event) => {
      this.doSomething(event.type);
    }, false);
  },
  doSomething: function(type) {
    console.log("Handling " + type + " event");
  }
};
Widget.init();
```

### Template Literals

Using Template Literals, we can now construct strings that have special characters in them without needing to escape them explicitly.

```
var text = "This string contains \"double quotes\" which are escaped.";
let text = `This string contains "double quotes" which don't need to be escaped anymore.`;
```

Template Literals also support interpolation, which makes the task of concatenating strings and values:

```
var name = 'Tiger';
var age = 13;

console.log('My cat is named ' + name + ' and is ' + age + ' years old.');
```

Much simpler:

```
const name = 'Tiger';
const age = 13;

console.log(`My cat is named ${name} and is ${age} years old.`);
```

Template Literals can accept expressions, as well:

```
let today = new Date();
let text = `The time and date is ${today.toLocaleString()}`;
```

### Destructuring

Destructuring allows us to extract values from arrays and objects (even deeply nested) and store them in variables with a more convenient syntax.

Arrays:

```
var arr = [1, 2, 3, 4];
var a = arr[0];
var b = arr[1];
var c = arr[2];
var d = arr[3];

console.log(a); // 1
console.log(b); // 2
```
```
let [a, b, c, d] = [1, 2, 3, 4];

console.log(a); // 1
console.log(b); // 2
```

Objects:

```
var luke = { occupation: 'jedi', father: 'anakin' };
var occupation = luke.occupation; // 'jedi'
var father = luke.father; // 'anakin'
```
```
let luke = { occupation: 'jedi', father: 'anakin' };
let {occupation, father} = luke;

console.log(occupation); // 'jedi'
console.log(father); // 'anakin'
```

### Modules

https://github.com/DrkSephy/es6-cheatsheet#modules

### Parameters

Default Parameters:
```
function addTwoNumbers(x, y) {
  x = x || 0;
  y = y || 0;
  return x + y;
}
```

ES6:
```
function addTwoNumbers(x=0, y=0) {
  return x + y;
}

addTwoNumbers(2, 4); // 6
addTwoNumbers(2); // 2
addTwoNumbers(); // 0
```

Rest Parameters. In ES5, we handled an indefinite number of arguments like so:
```
function logArguments() {
    for (var i=0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
}
```

Using the rest operator, we can pass in an indefinite amount of arguments:
```
function logArguments(...args) {
    for (let arg of args) {
        console.log(arg);
    }
}
```

### Spread Operator
Before the Spread Operator we would have to combine arrays like this:
```
var cities = ['San Francisco', 'Los Angeles'];
var places = ['Miami'].concat(cities, ['Chicago']);
```

Now, we can concat array literals easily with this intuitive syntax:
```
let cities = ['San Francisco', 'Los Angeles'];
let places = ['Miami', ...cities, 'Chicago']; // ['Miami', 'San Francisco', 'Los Angeles', 'Chicago']
```

We can also combine objects A LOT cleaner. We used to do something like:
```
// Let's be real, most people would use an external library like jquery or lodash for this
var _extends = Object.assign || function (target) { for (var i = 1; i < arguments.length; i++) { var source = arguments[i]; for (var key in source) { if (Object.prototype.hasOwnProperty.call(source, key)) { target[key] = source[key]; } } } return target; };

var person1 = { name: 'Brock Beldham', age: 28 };
var person2 = { name: 'Brock Beldham Jr.', role: 'kid' };

var merged = _extends({}, person1, person2);
/*
Object {
  "name": "Brock Beldham Jr.",
  "age": 28,
  "role": "kid",
}
*/
```

With Spread Operators it becomes much more straight forward:
```
const person1 = { name: 'Brock Beldham', age: 28 };
const person2 = { name: 'Brock Beldham Jr.', role: 'kid' };

const merged = {...person1, ...person2}
/*
Object {
  "name": "Brock Beldham Jr.",
  "age": 28,
  "role": "kid",
}
*/
```

**Coder Beware:** if there are conflicting keys the right most (last) keys:value pair wins out.

### Promises

Promises allow us to turn our horizontal code (callback hell):
```
func1(function (value1) {
    func2(value1, function (value2) {
        func3(value2, function (value3) {
            func4(value3, function (value4) {
                func5(value4, function (value5) {
                    // Do something with value 5
                });
            });
        });
    });
});
```

Into vertical code:
```
func1(value1)
  .then(func2)
  .then(func3)
  .then(func4)
  .then(func5, value5 => {
      // Do something with value 5
  });
```

Here is a more practical example of promises:
```
var request = require('request');

return new Promise((resolve, reject) => {
  request.get(url, (error, response, body) => {
    if (body) {
      resolve(JSON.parse(body));
    } else {
      resolve({});
    }
  });
});
```

We can also parallelize Promises to handle an array of asynchronous operations by using Promise.all():
```
let urls = [
  '/api/commits',
  '/api/issues/opened',
  '/api/issues/assigned',
  '/api/issues/completed',
  '/api/issues/comments',
  '/api/pullrequests'
];

let promises = urls.map((url) => {
  return new Promise((resolve, reject) => {
    $.ajax({ url: url })
      .done((data) => {
        resolve(data);
      });
  });
});

Promise.all(promises)
  .then((results) => {
    // Do something with results of all our promises
  });
  .catch((error) => {
    // catch and handle errors  
  })
```

### Generators

Generators are functions which can be exited and later re-entered. Useful for long iteration functions, so they can be paused to prevent blocking other functions for too long.

Basic Syntax:
```
function* myGen() { }
// or
function *myGen() { }
```

Usage:
```
function* three() {
  yield 1;
  yield 2;
  return 3;
}

var gene = three(); // starts the generator but doesn't run it
gene.next(); // runs the function for one iteration. Returns { value: 1, done: false }
gene.next(); // Returns { value: 2, done: false }
gene.next(); // Returns { value: 3, done: true }. This ends the generator.
gene.next(); // Returns { value: undefined, done: true }
```

Generator with arguments:
```
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z); // 5 + 24 + 13
}

var gene = foo(5);

console.log(gene.next()); // { value: 6, done: false }
console.log(gene.next(12)); // { value: 8, done: false }
console.log(gene.next(13)); // { value: 42, done: true }
```

## Sources

* [es6-cheatsheet](github.com/DrkSephy/es6-cheatsheet) - [David Leonard](https://github.com/DrkSephy)
* [ES6 cheatsheet](gist.github.com/vasco3/22b09ef0ca5e0f8c5996) - [Jorge Cuadra](https://github.com/vasco3)
