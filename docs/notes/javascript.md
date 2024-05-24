# Javascript

## Variables

```js
// comment

let variable = value;
const constant = value;

// old and not used anymore
var variable = value;
```

## Primitive Types
1. Number - includes `NaN`
2. String - zeroth index
    - `[]` to access index
    - `+` for concatenation
3. Boolean - `true` or `false`
4. `null` - use intentionally
5. `undefined` - "I don't know the data."



## Operators

### Math
```
+           addition
-           subtraction
/           division
*           multiplication
%           modulo
**          exponentiation
++          increment
-          decrement
```

### Comparison
```js
>
<

>=
<=

== // doesn't care about types
!=

=== // strict; cares about types
!==
```

### Spread

#### As spread
Spreads an iterable as individual items:

- array to a function args
- array to a new array
- array to an object; index is used as key and item as value

Spreads an object as individual keys and values:

- object to a new object
- order matters for several spreads, latter wins for conflicting keys

#### As rest
Collects *other* arguments passed to a function
```js
function func(argN, ...args) {
    console.log(args);
    ...
}
```



## String Template Literals
```js
`hi ${variable}`
```



## Common Methods
```js
// browser
console.log(...)
console.warn(...)
console.alert(...)
alert(...)
prompt()
parseInt(...)

// async timeout
setTimeout(callback, milliseconds)

// repeat callback for every interval
const id = setInterval(callback, milliseconds) 
// stops interval callback
clearInterval(id)
```

## HTML Script
```html
<body>
    ...
    <script src='script.js'></script>
</body>

<!- or ->

<head>
    <script>...</script>
</head>
```



## Data Structure

### Arrays
```js
let array = [value1, valueN];
array[0]
```

**Methods**  
Array methods can be chained.

```js
// applies callback to each element
// no returns
// ex: build on top of an existing variable, e.g. total, list
array.forEach(callback)

// similar to forEach but returns a new array
const newArray = array.map(callback)

// returns a new array that 'passes' the callback
// callback must return a boolean
const newArray = array.filter(callback)

// test; returns true if ALL items `pass` the callback
// callback must return a boolean
const bool = array.every(callback) 

// test; returns true if ANY of the items passes the callback
// callback must return a boolean
const bool = array.some(callback) 

// reduces an array to a single value by succeedingly pairing adjacent items and performing the callback's expression
// callback returns a new value for the accumulator
const val = array.reduce((accumulator, current) => expression, initialValue) 
```

### Objects
All keys are automatically converted to strings.

```js
let objectLiteral = {
    key: value,
    keyN: valueN,
};

objectLiteral['key'] // allows expression inside the brackets
objectLiteral.key
```

### Object with properties and methods
```js
const variable = {
    property1: value1,
    method1: function () {
        ...
    },
    ...
}

// or

const variable = {
    property1: value1:
    method1 () {
        ...
    },
    ...
}
```

`this` refers to the object that invoked the method, not necessarily the object where the method was *initialized* to.

```js
const test = {
    PI: 3.14,
    print() { console.log(this.PI) };
}

const other = test.print;
// `this` here will refer to `window` and not to `test`
// thus test.PI will not be displayed
// other() is similar to window.other()
other()
```


## Control Flow

### If
```js
if (condition) {
    ...
} else if (condition) {
    ...
} else {
    ...
}
```

### Switch Case
```js
switch (value) {
    case valueN:
        ...
        break;
    default:
        ...
}
```

### Logical Operators
```js
&& // and
|| // or
! // not
```

### Falsy Values
```js
false
0
''
null
undefined
NaN
```



## Loops

### for
```js
for (start; end; step) {
    ...
}
```

### for...of (iterable)
```js
for (let item of iterables) {
    ...
}
```

### for...in (object)
```js
for (let key in object) {
    ...
}
```

### while
```js
while (condition) {
    ...
}
```

### do...while
```js
do {
    ...
} while(condition);
```

### Label
Identifier that lets you refer to it elsewhere in your program.
```js
label:
...
```

### Break
Terminates a loop
```js
break;
break label;
```

### Continue
Skips or restarts an iteration
```js
continue;
continue label;
```



## Function

### Format
```js
function label(argN, kwargN = defN) {
    ...
    return value;
}
```

### Scopes
1. **Function Scope** - prioritizes initialized variable inside the function.
2. **Block Scope** - scope within blocks of code (e.g. conditionals, loops. `var` bypasses this as it forces function scope)
3. **Lexical Scope** - inner functions have access to variables in parent functions.

### Function Expression
```js
const variable = function (argN) {
    ...
}
```

### Arrow Function
Similar to function expressions  
Compact function definition

```js 
const func = (argN) => {
    ...
};
```

`this` refers to where it was executed, instead of what invoked it.  
Use regular function and arrow function appropriately. See [this Context](#this-context)

#### Implict Return
```js
const func = (argN) => (
    value/expression;
)

// or

const func = (argN) => value/expression
```

### Higher Order Functions
Functions that can accept and/or return functions

## Error Handling
```js
try {
    ...
} catch (e) {
    ...
}
```



## Destructuring
```js
// arrays
const [var1, var2, ...varN] = array; // ... in varN is a rest operator

// objects
const { key1, key2, key3: newKey3 = defaultVal } = object;

// argumets
// only the specified keys from an object argument will be accepted as input to the function
function func({ key1, key2, key3 = defaultVal3 }) {
    ...
}
```



## DOM
Document Object Model  
JS representation of a webpage

### Methods
```js
document.getElementById(id)
document.getElementsByTagName('tag')
document.getElementsByClassName('class')

document.querySelector('#id')
document.querySelector('tag')
document.querySelector('.class')
document.querySelector('tag[attribute=value]')

document.querySelectorAll(query)
```

### Element Properties and Methods
```js
const element = document.querySelector(query)
```

#### Properties
```js
// content
element.innerText
element.textContent
element.innerHTML

// attributes
element.getAttribute('attribute')
element.setAttribute('attribute', 'value')

// styles
element.style
element.style.attribute = 'value'; // avoid because inline; use css class methods instead

// css classes
element.classList.add(class)
element.classList.remove(class)
element.classList.toggle(class)

// get all effective css classes
window.getComputedStyle(element)

// selecting a relative element
element.parentElement
element.children
element.nextElementSibling
element.previousElementSibling
```

#### Create and remove elements
```js
// create a new element
const elem = document.createElement(tag)

// add as child
element.appendChild(elem) 
element.append(elem, ...) // newer; append text easily; append multiple elements
element.prepend(elem, ...) 

// add as sibling
element.insertAdjacentElement(position, elem)
element.after(elem)
element.before(elem)

// remove element
const elem = document.querySelector(query)
elem.parentElement.removeChild(elem) // older
elem.remove() // newer
```

### Event Handler

#### Inline (avoid)
```js
<button onclick="alert('hi')">Click</button>
```

#### Property
Only supports one callback for each property.

```js
elem = document.querySelector(query);
elem.onclick = function () {
    ...
}
```

#### Event Listener
Supports multiple callbacks  
More arguments/options

`this` inside the callback refers to the element.  
Use `this` when defining a separate callback function.

```js
elem = document.querySelector(query);
elem.addEventListener(event, callback);

// e callback argument
elem.addEventListener(event, function(e) {
    // e will contain an event object with details about the event
    // e.g. coordinates, keys
    ...
})
```

**Prevent Default**  
Prevents default behavior of the element (e.g. stop redirection upon submitting forms)
```js
elem.addEventListener(event, function(e) {
    e.preventDefault()
})
```

**Event Bubbling**  
When nested event handlers bubble up executing each in order
```js
elem.addEventListener(event, function(e) {
    // stops event bubbling
    e.stopPropagation()
})
```

**Event Delegation**  
Strategically applying event handlers so they work correctly.

Example:

- handler that removes an element also removes the handler itself
- when new elements are created, they won't have the same handler

Solution: apply event handler on the parent that will persist

```js
parentElem.addEventListener('click', (e) => {
    e.target.nodeName === 'LI' && e.target.remove()
})
```



## Promise
Object representation of the eventual completion or failure of an asynchronous operation.  
Solves callback hell by chaining promises

```js
promise1
    .then(data => {
        ...
        return promise2
    })
    .then(data => {
        ...
        return promise3
    })
    .then(data => {
        ...
    })
    .catch(err => {
        ...
    })
```

### Create a promise
```js
const request = (url) => {
    return new Promise((resolve, reject) => {
        ...
        if (success) {
            ...
            resolve(data)
        } else {
            ...
            reject(err)
        }
    })
}
```

### Async Functions
Syntactic sugar for promises  
Use async and await to bundle promises instead of chaining them

`async` - creates an async function  
`await` - pauses execution until a promise is resolved or rejected

```js
const func = async (url) => {
    try {
        let data = await fetch(url)
        ...
        return data
    } catch (e) {
        throw new Error(`error: ${e}`) // or throw `error: ${e}`
    }
}
```



## HTTP

### AJAX
Asynchronous Javascript and XML (JSON)  
Making requests behind the scenes without going to a new webpage (e.g. infinite scrolling, live search)

### JSON
JavaScript Object Notation
```js
// from json string to object
JSON.parse(json)

// from json object to string
JSON.stringify(object)
```

### Response
- content
- status code
- headers (metadata)

### Query String
Additional info for the request

### Requests

#### XHR (old)
XML Http Request  
Old way of making requests  
Does not support promises
```js
const req = new XMLHttpRequest();

req.onload = function () {
    ...
    const data = JSON.parse(this.responseText);
    ...
}

req.onerror = function () {
    ...
}

req.open(method, url);
req.send();
```

#### Fetch API (built-in)
Better way of making requests than XHR  
Support promises

```js
fetch(url)
    .then(res => {
        ...
        // parse the JSON string
        return res.json()
    })
    .then(data => {
        // here data has been parsed to JSON
        ...
    })
    .catch(e => {
        ...
    })

// using async
const fetchAsync = async () => {
    try {
        const res = await fetch(url);
        const data = await res.json();
        ...
    } catch (e) {
        ...
    }
}
```

#### Axios
Library for making HTTP requests  
Better than fetch  
Data is already parsed

```js
axios.get(url)
    .then(res => {
        // already parsed
        const data = res.data;
    })
    .catch(e => {
        ...
    })

// using async
const axiosAsnyc = async () => {
    try {
        const config = { 
            headers: { ... },
            params: { ... }
        };
        const res = await axios.get(url, config);
        const data = res.data;
    } catch (e) {
        ...
    }
}
```



## Class

### Factory Function (old)
A function that returns an object attached with its properties and methods.  
Each method is defined in each returned object unlike with prototypes (bad practice).

```js
function func(args, ...) {
    const obj = {};
    obj.arg1 = arg1;
    ...

    obj.method1 = function() {
        const { properties, ... } = this;
        ...
    }
    ...

    return obj;
}

instance = obj(args);
```

### Constructor Function (old)
A function that creates a new object with properties and methods using prototypes.  
Uses `new` keyword.  
Methods are attached by using prototypes.
```js
Object.prototype.method = function () { ... }
```

```js
function Func(args) {
    this.arg1 = arg1;
    ...
}

Func.prototype.method1 = function() {
    const { prop1, prop2, ... } = this;
    ...
}

instance = new Func(args);
```

### Classes {#classes}
Syntactic sugar for constructor functions

```js
class Class {
    constructor(args) {
        this.arg1 = arg1;
        ...
    }

    method1() {
        const { properties, ... } = this;
        ...
    }
}

instance = new Class(args);

// inheritance
class SubClass extends Class {
    constructor(args) {
        super(args);
        this.arg1 = arg1;
        ...
    }

    ...
}
```



## Notes
### CamelCase Naming Convention
### `window` is the top level or global object
### Call Stack
How code is tracked and executed  
LIFO - recent function put into the call stack is executed and removed from the stack  
[Loupe](http://latentflip.com/loupe) - visualize call stack

### JS is single-threaded
At any given point in time, the thread is running at most a single line of JS code.  
Browsers help JS to work around single threadedness via *web apis*. It will remind JS to run a callback function at a certain point of time.

### Callback Hell
Multiple nested callbacks  
When a callback depends on a callback which depends on a callback, and so on.

### JS is a prototype-based language
Objects and data structures are created from prototypes/blueprints sharing properties and methods.

```js
// type.prototype.property/method
String.prototype.length
```

### `this` Context

#### Global Context
Refers to the global object whether in strict mode or not.

```js
this.b = "MDN";
console.log(window.b) // "MDN"
console.log(b) // "MDN"
```

#### Function Context
When not in strict mode, it will default to the global object.

```js
function f1() {
    return this;
}

// in a browser
f1() === window; // true
// in Node
f1() === globalThis; // true
```

In strict mode, it's `undefined`.

```js
function f2() {
    'use strict';
    return this;
}

f2() === undefined; // true
```

This can be overriden by `call` or `apply` methods.

```js
// an object can be passed as the first argument to call or apply and this will be bound to it.
var obj = {a: 'Custom'};

var a = 'Global';
function whatsThis() {
    // The value of this is dependent on how the function is called
    return this.a;
}

whatsThis();          // 'Global' as `this` in the function isn't set, so it defaults to the global/window object
whatsThis.call(obj);  // 'Custom' as `this` in the function is set to obj
whatsThis.apply(obj); // 'Custom' as `this` in the function is set to obj
```

#### Class Context
Similar to function except for few caveats.  
Within a class constructor, `this` is a regular object that can be accessed by the class methods. 

To retain the reference to the instance, it may be necessary to use `bind`.
```js
class Car {
  constructor() {
    // Bind sayBye but not sayHi to show the difference
    this.sayBye = this.sayBye.bind(this);
  }
  sayHi() {
    console.log(`Hello from ${this.name}`);
  }
  sayBye() {
    console.log(`Bye from ${this.name}`);
  }
  get name() {
    return 'Ferrari';
  }
}

class Bird extends Car {
  get name() {
    return 'Tweety';
  }
}

const car = new Car();
const bird = new Bird();

// The value of 'this' in methods depends on their caller
car.sayHi(); // Hello from Ferrari

bird.sayHi = car.sayHi;
bird.sayHi(); // Hello from Tweety

// For bound methods, 'this' doesn't depend on the caller
bird.sayBye = car.sayBye;
bird.sayBye();  // Bye from Ferrari
```

#### Arrow Functions
`this` retains the value of the enclosing lexical context's `this` - where it was created.  
This is useful when retaining the reference to the object where it was created, instead of an instance where it was called.  
In global code, it will be set to the global object.

#### As an object method
When a function is called as a method of an object, its `this` is set to the object the method is called on.

```js
// calling the method directly from the object
var o = {
    prop: 37,
    f: function() {
        return this.prop;
    }
};

console.log(o.f()); // 37

// referencing `this` from the object the method was called on
var o = {prop: 37};
function independent() {
  return this.prop;
}

o.f = independent;
console.log(o.f()); // 37
```

#### As a DOM event handler
`this` is set to the element on which the listener is placed.