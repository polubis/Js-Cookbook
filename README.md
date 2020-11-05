# Standards naming

#### ES2015 a.k.a. ES6
#### ES2016 a.k.a. ES7
#### ES2017 a.k.a. ES8
#### ES2018 a.k.a. ES9
#### ES2019 a.k.a. ES10
#### ES2020 a.k.a. ES11

## `areObjectsSame()`

```js
function isEquivalent(a, b) {
    // Create arrays of property names
    var aProps = Object.getOwnPropertyNames(a);
    var bProps = Object.getOwnPropertyNames(b);

    // If number of properties is different,
    // objects are not equivalent
    if (aProps.length != bProps.length) {
        return false;
    }

    for (var i = 0; i < aProps.length; i++) {
        var propName = aProps[i];

        // If values of same property are not equal,
        // objects are not equivalent
        if (a[propName] !== b[propName]) {
            return false;
        }
    }

    // If we made it this far, objects
    // are considered equivalent
    return true;
}

// Outputs: true
console.log(isEquivalent(bobaFett, jangoFett));
```

## Callback

Function which is passed as argument to other function and executed inside or returned.

```js
const fibb = (calc) => {
    for(let i = 0; i < 10; i++) {
        calc();
    }
}
```

## Closure

Function which returns other function. Allows to "use" variables later with next function call. **Closure** is transformed to object behind the scenes.

```js
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```


## Coercion

https://dorey.github.io/JavaScript-Equality-Table/

Type coercion is the process of converting value from one type to another (such as string to number, object to boolean, and so on). Any type, be it primitive or an object, is a valid subject for type coercion.

> One operator that does not trigger implicit type coercion is `===`.

> **Implicit coercion** is double edge sword - less code but can be buggy.

```js
String(123) // explicit, type casting
123 + ''    // implicit
```
There are tree types of **conversion** to: `string`, `boolean`, `number`.

### String conversion

To explicitly convert values to a string apply the `String()` function. Implicit coercion is triggered by the binary `+` operator, when any operand is a `string`.

```js
String(123)                   // '123'
String(-12.3)                 // '-12.3'
String(null)                  // 'null'
String(undefined)             // 'undefined'
String(true)                  // 'true'
String(false)                 // 'false'
String(Symbol('my symbol'))   // 'Symbol(my symbol)' // Explicit type conversion only
'' + Symbol('my symbol')      // TypeError is thrown
```

### Boolean conversion

To explicitly convert a value to a boolean apply the `Boolean()` function.
Implicit conversion happens in logical context, or is triggered by logical operators `( || && !)`.

> Logical operators such as `||` and `&&` do boolean conversions internally, but actually return the value of original operands, even if they are not boolean.

```js
Boolean(2)          // explicit
if (2) { ... }      // implicit due to logical context
!!2                 // implicit due to logical operator
2 || 'hello'        // implicit due to logical operator

// returns number 123, instead of returning true
// 'hello' and 123 are still coerced to boolean internally to calculate the expression
let x = 'hello' && 123;   // x === 123

Boolean('')           // false
Boolean(0)            // false     
Boolean(-0)           // false
Boolean(NaN)          // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(false)        // false
Boolean({})             // true
Boolean([])             // true
Boolean(Symbol())       // true
!!Symbol()              // true
Boolean(function() {})  // true
```

### Number conversion

For an explicit conversion just apply the `Number()` function. Implicit can be triggered by:

 - comparison operators `(>, <, <=,>=)`
 - bitwise operators `(| & ^ ~)`
 - arithmetic operators `(- + * / % )` 
 > Note, that binary `+` does not trigger numeric conversion, when any operand is a `string`.
 - unary `+` operator
 - loose equality operator `==`, `!=`
 > Note that `==` does not trigger numeric conversion when both operands are `strings`.
 
```js
Number(null)                   // 0
Number(undefined)              // NaN
Number(true)                   // 1
Number(false)                  // 0
Number(" 12 ")                 // 12
Number("-12.34")               // -12.34
Number("\n")                   // 0
Number(" 12s ")                // NaN
Number(123)                    // 123
Number(Symbol('my symbol'))    // TypeError is thrown
+Symbol('123')                 // TypeError is thrown
null == 0               // false, null is not converted to 0
null == null            // true
undefined == undefined  // true
null == undefined       // true
```

> When converting a `string` to a `number`, the engine first trims leading and trailing whitespace, `\n`, `\t` characters, returning `NaN` 
> if the trimmed string does not >represent a valid number. If `string` is empty, it returns `0`.

> Symbols cannot be converted to a number.

### Objects conversion

Always objects are converted to primitives and after converts to the final type.

Objects are converted to primitives via the internal `[[ToPrimitive]]` method, which is responsible for both numeric and string conversion.

```js
true + false             // 1
12 / "6"                 // 2
"number" + 15 + 3        // 'number153'
15 + 3 + "number"        // '18number'
[1] > null               // true
"foo" + + "bar"          // 'fooNaN'
'true' == true           // false
false == 'false'         // false
null == ''               // false
!!"false" == !!"true"    // true
['x'] == 'x'             // true 
[] + null + 1            // 'null1'
[1,2,3] == [1,2,3]       // false
{}+[]+{}+[1]             // '0[object Object]1'
!+[]+[]+![]              // 'truefalse'
new Date(0) - 0          // 0
new Date(0) + 0          // 'Thu Jan 01 1970 02:00:00(EET)0'
```

## Communication with server

### `AJAX` - Asynchronous JavaScript And XML

In a nutshell, it is the use of the` XMLHttpRequest` object to communicate with **servers**. It can send and receive information in various formats, including `JSON`, `XML`, `HTML`, and `text` files. AJAX’s most appealing characteristic is its **asynchronous** nature, which means it can communicate with the server, exchange data, and update the page without having to **refresh the page**.

- 100% supported.
- Sends cookies by default.
- Supports timeout.
- Errors always rejected.
- Progress events supported.
- Timeouts supported.

```js
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      // do something with data
      document.getElementById("demo").innerHTML =
      	this.responseText;
    }
  };
  xhttp.open("GET", "ajax_info.txt", true);
  xhttp.send();
}
```

### `fetch` API

Provides an interface for fetching resources (including across the network). It will seem familiar to anyone who has used `XMLHttpRequest`, but the new API provides a more powerful and flexible feature set.

- 100% supported.
- In some cases it does not sends cookies.
- Cannot be aborted - until `AbortController` provided.
- Errors like `404` not rejected.
- No progress events supported.
- Timeouts not supported.

```js
const controller = new AbortController();

fetch(
  'http://domain/service',
  {
    method: 'GET'
    signal: controller.signal
  })
  .then( response => response.json() )
  .then( json => console.log(json) )
  .catch( error => console.error('Error:', error) );
```

## Currying

Functional programming technique - is a transformation of functions that translates a function from callable as `f(a, b, c)` into callable as `f(a)(b)(c)`.
Increases code composition - we can create function with defined variables which will be available via **closure** and use them later.

This technique requires **unary functions** - functions which takes only one argument.
**unary functions** can be also called **1-ary** - **ary** is based of **arity** term which describes how many arguments function takes.

- Curried functions works like **iterator**.
- Can bo used to increase composition.
- Great for memoization.

```js
const req = v => !!v;
const min = (length) => v => v.length < length;
const max = (length) => v => v.length > length;
const makeValidatorsRunner = (...fns) => (value) => {
  return fns.reduce((acc, fn) => {
    return {
      ...acc,
      [idx]: fn(value)
    }
 }, {})    
};
const runLoginValidators = makeValidatorsRunner(req, min(2), max(25)); // remembers validators for later usage
const runRegisterValidators = makeValidatorsRunner(req, min(4), max(50)); // remembers validators for later usage
``` 

Own `curry()` function.

```js
function curry(f) {
  return function currify() {
    const args = Array.prototype.slice.call(arguments);
    return args.length >= f.length ? // f.length - tells how many args function needs, args.length - how many arguments were passed to function
      f.apply(null, args) :
      currify.bind(null, ...args)
  }
}
const add = (a, b, c, d) => a + b + c + d;
const curriedAdd = curry(add);
console.log(
  curriedAdd(4)(2)(16)(20)
); // 42
```

## Data persistence

![Data persistence](https://i.stack.imgur.com/cI5kT.jpg)

## Deep freeze

```js
function deepFreeze(object) {
  // Retrieve the property names defined on object
  const propNames = Object.getOwnPropertyNames(object);

  // Freeze properties before freezing self
  
  for (const name of propNames) {
    const value = object[name];

    if (value && typeof value === "object") { 
      deepFreeze(value);
    }
  }

  return Object.freeze(object);
}

const obj2 = {
  internal: {
    a: null
  }
};

deepFreeze(obj2);

obj2.internal.a = 'anotherValue'; // fails silently in non-strict mode
obj2.internal.a; // null
```

## `document load` event

Fired when the whole page has loaded, including all dependent resources such as stylesheets and images.

```js
window.addEventListener('load', (event) => {
  console.log('page is fully loaded');
});
```

## `DOMContentLoaded` event

The `DOMContentLoaded` event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

```js
function doSomething() {
  console.info('DOM loaded');
}

if (document.readyState === 'loading') {  // Loading hasn't finished yet
  document.addEventListener('DOMContentLoaded', doSomething);
} else {  // `DOMContentLoaded` has already fired
  doSomething();
}
```

## Drawback of true `private`

One drawback of creating private method in Javascript is **memory-inefficient** because a copy of the `private` method will be created every time a new instance is created.

```js
function contact(first, last) {
    this.firstName = first;
    this.lastName = last;
    this.mobile;

    // private method
    var formatPhoneNumber = function(number) {
        // format phone number based on input
    };

    // public method
    this.setMobileNumber = function(number) {
        this.mobile = formatPhoneNumber(number);
    };
}

var rob = new contact('Rob', 'Sanderson');
var don = new contact('Donald', 'Trump');
var andy = new contact('Andy', 'Whitehall');
```

## Enum in JavaScript

```js
const Days = Object.freeze({ "monday":1, "tuesday":2, "wednesday":3 });
```

## Event loop

JavaScript **concurrency model** is based on **event loop** algorythm. This model never blocks if all **hard** calculations are performed via **callbacks** as requests or **web workers** usage and if don't use legacay **synchronous** API's like `alert()`.

- **Heap** - manages memory allocation.
- **Stack** - handles functions execution order:
   - if a function is marked **asynchronous** it will go to the **callback queue**.
- **Callback queue** - runs callback only if stack is empty

> `setTimeout()` using `setTimeout` with a delay of `0` means calling when the stack is empty.

![Event loop](https://miro.medium.com/max/2000/1*m5M4NV495oH4ADvpnItnVQ.png)

## Execution context

The environment (value of `this`, **variables**, **objects**, and **functions** JavaScript code has access to at a particular time) in which the JavaScript code is executed.

- **Global execution context**

This is the default execution context in which JS code start its execution when the file first loads in the browser. There is only one **global execution context**.

- **Functional execution context**

Created after function call. Have access to **global execution context** but not **vice versa`**.

- **Eval execution context**

Context inside `eval` function.

### Execution context stack

Data structure (last in first out data structure) to store all the execution stacks created during the life cycle of the script.

```js
var a = 10;
function functionA() {
	console.log("Start function A");
	function functionB(){
		console.log("In function B");
	}
	functionB();
}
functionA();
console.log("GlobalContext");
```

![Execution context stack](https://miro.medium.com/max/700/1*bDebsOuhRx9NMyvLHY2zxA.gif)

### Execution context creation

- **Creation phase** is the phase in which the JS engine has called a function but its execution has not started. In the creation phase, JS engine is in the compilation phase and it just scans over the function code to compile the code, it doesn’t execute any code.
  - Creates the **Activation object** or the Variable object: Activation object is a special object in JS which contain all the variables, function arguments and inner functions   declaration information. As activation object is a special object it does not have the dunder proto property,
  - Creates the **scope chain**: Once the activation object gets created, the JS engine initializes the scope chain which is a list of all the variables objects inside which the current function exists. This also includes the variable object of the global execution context. Scope chain also contains the current function variable object,
  - Determines the value of `this`: After the scope chain, the JavaScript engine initializes the value of this.

```js
function funA (a, b) {
  var c = 3;
  
  var d = 2;
  
  d = function() {
    return a - b;
  }
}
funA(3, 2);

// RESULT
executionContextObj = {
 variableObject: {
  argumentObject : {
    0: a,
    1: b,
    length: 2
  },
  a: 3,
  b: 2,
  c: undefined, 
  d: undefined // then pointer to the function defintion of d
 }, // All the variable, arguments and inner function details of the funA
 scopechain: [], // List of all the scopes inside which the current function is
 this // Value of this 
}
```

- **Execution phase** JS engines will again scan through the function to update the variable object with the values of the variables and will execute the code.

```js
variableObject = {
  argumentObject : {
    0: a,
    1: b,
    length: 2
  },
  a: 3,
  b: 2,
  c: 3,
  d: undefined then pointer to the function defintion of d
}
```

## Event delegation

**DOM pattern** which can be implemented by following algorythm:

 - Put a single handler on the container.
 - In the handler – check the source element event.target.
 - If the event happened inside an element that interests us, then handle the event.
 
```html
<table>
  <tr>
    <th colspan="3"><em>Bagua</em> Chart: Direction, Element, Color, Meaning</th>
  </tr>
  <tr>
    <td class="nw"><strong>Northwest</strong><br>Metal<br>Silver<br>Elders</td>
    <td class="n">...</td>
    <td class="ne">...</td>
  </tr>
  <tr>...2 more lines of this kind...</tr>
  <tr>...2 more lines of this kind...</tr>
</table>
```
```js
table.onclick = function(event) {
  let td = event.target.closest('td'); // Gets closest element which match selector. 
  if (!td) return; // If event.target is not inside any <td>, then the call returns immediately, as there’s nothing to do.
  if (!table.contains(td)) return; // In case of nested tables, event.target may be a <td>, but lying outside of the current table. So we check if that’s actually our table’s  // <td>.
  highlight(td);
};
 ```

With data attributes usage:

```html
<div id="menu">
  <button data-action="save">Save</button>
  <button data-action="load">Load</button>
  <button data-action="search">Search</button>
</div>
```

```js
<script>
  class Menu {
    constructor(elem) {
      this._elem = elem;
      elem.onclick = this.onClick.bind(this); // (*)
    }

    save() {
      alert('saving');
    }

    load() {
      alert('loading');
    }

    search() {
      alert('searching');
    }

    onClick(event) {
      let action = event.target.dataset.action;
      if (action) {
        this[action]();
      }
    };
  }

  new Menu(menu);
</script>
```

Benefits:

- Simplifies initialization and saves memory: no need to add many handlers.
- Less code: when adding or removing elements, no need to add/remove handlers.
- DOM modifications: we can mass add/remove elements with innerHTML and the like.

Limitations:

- First, the event must be bubbling. Some events do not bubble. Also, low-level handlers should not use `event.stopPropagation()`.
- Second, the delegation may add CPU load, because the container-level handler reacts on events in any place of the container, no matter whether they interest us or not. But usually the load is negligible, so we don’t take it into account.

## Event propagation phases

The standard DOM Events describes 3 phases of event propagation:
 - **Capturing phase`=** – the event goes down to the element.
 - **Target phase** – the event reached the target element.
 - **Bubbling phase** – the event bubbles up from the element.
 
 ![Event propagation phases](https://www.w3.org/TR/2003/WD-DOM-Level-3-Events-20030331/images/eventflow.png)
 
### Event capturing

```js
elem.addEventListener(..., {capture: true})
// or, just "true" is an alias to {capture: true} // means handler is set on the capture phase
elem.addEventListener(..., true)
```
 
### Event bubbling

When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

Almost all **event bubble** expect `focus` event and some others.

Element that **caused** event is called `target`.

Element which have event assigned to is called `currentTarget` = `this`.

A bubbling event goes from the target element straight up. Normaly upwards `html` and then to `document` object.

To stop bubbling use `event.stopPropagation()`.

To stop bubbling in all handlers use `event.stopImmedietePropagation()`.

`event.eventPhase` – the current phase `(capturing=1, target=2, bubbling=3)`.

> Use only if needed.

```html
<form onclick="alert('form')">FORM // called always
  <div onclick="alert('div')">DIV // called on div / p click
    <p onclick="alert('p')">P</p> // called on p click
  </div>
</form>
```

## Function declarations

Defined as soon as its surrounding function or script is executed.

```js
// Outputs: "Hello!"
functionTwo();

function functionTwo() {
  console.log("Hello!");
}
```

## Function expressions

Defined when line of code is reached.

```js
// TypeError: functionOne is not a function
functionOne();

var functionOne = function() {
  console.log("Hello!");
};
```

## Higher order function

Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.

```js
function filter(array, test) {
  let passed = [];
  for (let element of array) {
    if (test(element)) {
      passed.push(element);
    }
  }
  return passed;
}
console.log(filter(SCRIPTS, script => script.living));
// → [{name: "Adlam", …}, …]
```

## Hoisting

https://www.digitalocean.com/community/tutorials/understanding-hoisting-in-javascript#:~:text=Hoisting%20is%20a%20JavaScript%20mechanism,scope%20is%20global%20or%20local.

Mechanism where **variables** and **function declarations** are moved to the top of their scope(memory) before code execution. 
JavaScript Engine first declaring variables in memory and later initialises them.

> To avoid hoisting problems always declare and initialise our variable before use.

![Hoisting](https://cdn.scotch.io/8976/bNTL1QI3RFebh7C1JPYC_variable%20hoisting.png)

Undeclared variable is assigned the value undefined at execution and is also of type `undefined`.

```js
console.log(typeof variable); // Output: undefined
```

`ReferenceError` is thrown when trying to access a previously `undeclared` variable.

```js
console.log(variable); // Output: ReferenceError: variable is not defined
```

```js
var a = 10; 
// is transformed to:
var a;
a = 10;
```

```js
console.log(hoist); // Output: undefined
var hoist = 'The variable has been hoisted.';
```

```js
var hoist;
console.log(hoist); // Output: undefined
hoist = 'The variable has been hoisted.';
```

```js
'use strict';
console.log(hoist); // Output: ReferenceError: hoist is not defined
hoist = 'Hoisted';
```

```js
console.log(hoist); // Output: ReferenceError: hoist is not defined ...
let hoist = 'The variable has been hoisted.';
```

```js
let hoist;
console.log(hoist); // Output: undefined
hoist = 'Hoisted'
```

```js
const PI = 3.142;
PI = 22/7; // Let's reassign the value of PI
console.log(PI); // Output: TypeError: Assignment to constant variable.
```

```js
console.log(hoist); // Output: ReferenceError: hoist is not defined
const hoist = 'The variable has been hoisted.';
```

```js
function getCircumference(radius) {
  console.log(circumference)
  circumference = PI*radius*2;
  const PI = 22/7;
}
getCircumference(2) // ReferenceError: circumference is not defined
```

```js
const PI;
console.log(PI); // Ouput: SyntaxError: Missing initializer in const declaration
PI=3.142;
```

```js
hoisted(); // Output: "This function has been hoisted."
function hoisted() {
  console.log('This function has been hoisted.');
};
```

```js
expression(); //Output: "TypeError: expression is not a function
var expression = function() {
  console.log('Will this work?');
};
```

Function declarations are hoisted over variable declarations but not over variable assignments. Variable assignment over function declaration.

```js
var double = 22;
function double(num) {
  return (num*2);
}
console.log(typeof double); // Output: number
```

Function declarations over variable declarations.

```js
var double;
function double(num) {
  return (num*2);
}
console.log(typeof double); // Output: function
```

Class `declarations` are hoisted. However, they remain uninitialised until evaluation. Declare a class before you can use it.

```js
var Frodo = new Hobbit();
Frodo.height = 100;
Frodo.weight = 300;
console.log(Frodo); // Output: ReferenceError: Hobbit is not defined
class Hobbit {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
}
```

```js
class Hobbit {
  constructor(height, weight) {
    this.height = height;
    this.weight = weight;
  }
}
var Frodo = new Hobbit();
Frodo.height = 100;
Frodo.weight = 300;
console.log(Frodo); // Output: { height: 100, weight: 300 }
```

Class `expressions` are not hoisted.

```js
var Square = new Polygon();
Square.height = 10;
Square.width = 10;
console.log(Square); // Output: TypeError: Polygon is not a constructor
var Polygon = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
```

```js
var Square = new Polygon();
Square.height = 10;
Square.width = 10;
console.log(Square); // Output: TypeError: Polygon is not a constructor
var Polygon = class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
```

```js
var Polygon = class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
Square.height = 10;
Square.width = 10;
console.log(Square);
```

## Host objects

Objects supplied by the host environment to complete the execution environment of ECMAScript.

Examples: `window`, `document`, `location`, `history`, `XMLHttpRequest`, `setTimeout`, `getElementsByTagName`, `querySelectorAll`, ...etc

## `instance of`

The `instanceof` operator tests to see if the prototype property of a constructor appears anywhere in the prototype chain of an object. The return value is a `boolean` value. 

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// expected output: true

console.log(auto instanceof Object);
// expected output: true
```

## `JSON` - JavaScript Object Notation

The `JSON` format is syntactically identical to the code for creating JavaScript objects. Because of this similarity, a JavaScript program can easily convert `JSON` data into **native JavaScript objects**.

- Data is in **name/value** pairs.
- Data is separated by **commas**.
- **Curly braces** hold objects.
- **Square brackets** hold arrays.

> `JSON` names require double quotes. JavaScript names do not.
```json
{
	"employees":[
	    {"firstName":"John", "lastName":"Doe"},
	    {"firstName":"Anna", "lastName":"Smith"},
	    {"firstName":"Peter", "lastName":"Jones"}
	]
}
```

```js
var text = '{ "employees" : [' +
'{ "firstName":"John" , "lastName":"Doe" },' +
'{ "firstName":"Anna" , "lastName":"Smith" },' +
'{ "firstName":"Peter" , "lastName":"Jones" } ]}';

const jsObj = JSON.parse(text);
const JSONtext = JSON.stringify(jsObj); // equal to text variable
```

### `JSON` data types

`string`, `number,` `object JSON`, `array`, `boolean`, `null`.

### `JSONP` - `JSON` with Padding

Technique to overcome the `XMLHttpRequest` **same domain policy**. `AJAX` (`XMLHttpRequest`) request to a different domain. Instead of using `XMLHttpRequest` we have to use `<script>` tags. 

```html
<html>
    <head>
    </head>
    <body>
        <div id = 'twitterFeed'></div>
        <script>
        function myCallback(dataWeGotViaJsonp){
            var text = '';
            var len = dataWeGotViaJsonp.length;
            for(var i=0;i<len;i++){
                twitterEntry = dataWeGotViaJsonp[i];
                text += '<p><img src = "' + twitterEntry.user.profile_image_url_https +'"/>' + twitterEntry['text'] + '</p>'
            }
            document.getElementById('twitterFeed').innerHTML = text;
        }
        </script>
        <script type="text/javascript" src="http://twitter.com/status/user_timeline/padraicb.json?count=10&callback=myCallback"></script> // triggered after get data
    </body>
</html>
```

## Lexical scoping

Every inner level can access its outer levels.

```js
function init() {
  var name = 'Mozilla'; // name is a local variable created by init
  function displayName() { // displayName() is the inner function, a closure
    alert(name); // use variable declared in the parent function
  }
  displayName();
}
init();
```

## Loose equality `==`

Compares equality after doing any necessary type conversions.

![Comparision table](https://i.stack.imgur.com/yISob.png)

## Memoization

Memoization is an optimization technique that speeds up applications by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

```js
const results = {};

const fibb = (n) => {
  if (results.hasOwnProperty(n)) {
    return results[n];
  }
  
  // some expensive calculations
}
```
 
## Modules

A **module** is a value that can be accessed by a **single reference**. If you have multiple pieces of data or functions that you want to expose in a module, they have to be **properties** on a **single object** that represents the **module**.

Module hiddes implementation details and exposing only 1 object.

### IIFE

Anonymous function which returns an object, which is the placeholder of exported APIs. 

```js
// Define IIFE module with dependencies.
const iifeCounterModule = ((dependencyModule1, dependencyModule2) => {
    let count = 0;
    return {
        increase: () => ++count,
        reset: () => {
            count = 0;
            console.log("Count is reset.");
        }
    };
})(dependencyModule1, dependencyModule2);
```

### CJS - CommonJS module, or Node.js module

Initially named **ServerJS**, is a **pattern** to define and consume modules. It is implemented by **Node,js**. By default, each `.js` file is a CommonJS module. A module variable and an exports variable are provided for a module (a file) to expose APIs. And a require function is provided to load and consume a module.

```js
// Define CommonJS module: commonJSCounterModule.js.
const dependencyModule1 = require("./dependencyModule1");
const dependencyModule2 = require("./dependencyModule2");

let count = 0;
const increase = () => ++count;
const reset = () => {
    count = 0;
    console.log("Count is reset.");
};

exports.increase = increase;
exports.reset = reset;
// Or equivalently:
module.exports = {
    increase,
    reset
};
```

```js
// Use CommonJS module.
const { increase, reset } = require("./commonJSCounterModule");
increase();
reset();
// Or equivelently:
const commonJSCounterModule = require("./commonJSCounterModule");
commonJSCounterModule.increase();
commonJSCounterModule.reset();
```

### Asynchronous Module Definition, or RequireJS module

**Pattern** to define and consume module. It is implemented by **RequireJS**. **AMD** provides a define function to define module, which accepts the module name, dependent modules’ names, and a factory function:

```js
// Define AMD module.
define("amdCounterModule", ["dependencyModule1", "dependencyModule2"], (dependencyModule1, dependencyModule2) => {
    let count = 0;
    const increase = () => ++count;
    const reset = () => {
        count = 0;
        console.log("Count is reset.");
    };

    return {
        increase,
        reset
    };
});
```

> The AMD require function is totally different from the **CommonJS** require function. **AMD** require accept the names of modules to be consumed, and pass the module to a function argument.

```js
// Use AMD module.
require(["amdCounterModule"], amdCounterModule => {
    amdCounterModule.increase();
    amdCounterModule.reset();
});
```

AMD’s define function has another overload. It accepts a callback function, and pass a CommonJS-like require function to that callback. Inside the callback function, require can be called to dynamically load the module:

```js
// Use dynamic AMD module.
define(require => {
    const dynamicDependencyModule1 = require("dependencyModule1");
    const dynamicDependencyModule2 = require("dependencyModule2");

    let count = 0;
    const increase = () => ++count;
    const reset = () => {
        count = 0;
        console.log("Count is reset.");
    };

    return {
        increase,
        reset
    };
});
```

The above define function overload can also passes the require function as well as exports variable and module to its callback function. So inside the callback, CommonJS syntax code can work:

```js
// Define AMD module with CommonJS code.
define((require, exports, module) => {
    // CommonJS code.
    const dependencyModule1 = require("dependencyModule1");
    const dependencyModule2 = require("dependencyModule2");

    let count = 0;
    const increase = () => ++count;
    const reset = () => {
        count = 0;
        console.log("Count is reset.");
    };

    exports.increase = increase;
    exports.reset = reset;
});

// Use AMD module with CommonJS code.
define(require => {
    // CommonJS code.
    const counterModule = require("amdCounterModule");
    counterModule.increase();
    counterModule.reset();
});
```

### UMD module: Universal Module Definition, or UmdJS module

https://github.com/umdjs/umd

Set of tricky patterns to make your code file work in multiple environments.

### ES module: ECMAScript 2015, or ES6 module

After all the module mess, in 2015, JavaScript’s spec version 6 introduces one more different module syntax. This spec is called ECMAScript 2015 (ES2015), or ECMAScript 6 (ES6).

```js
// Define ES module: esCounterModule.js or esCounterModule.mjs.
import dependencyModule1 from "./dependencyModule1.mjs";
import dependencyModule2 from "./dependencyModule2.mjs";

let count = 0;
// Named export:
export const increase = () => ++count;
export const reset = () => {
    count = 0;
    console.log("Count is reset.");
};
// Or default export:
export default {
    increase,
    reset
};
```

```js
// Use ES module.
// Browser: <script type="module" src="esCounterModule.js"></script> or inline.
// Server: esCounterModule.mjs
// Import from named export.
import { increase, reset } from "./esCounterModule.mjs";
increase();
reset();
// Or import from default export:
import esCounterModule from "./esCounterModule.mjs";
esCounterModule.increase();
esCounterModule.reset();
```

### ES dynamic module: ECMAScript 2020, or ES11 dynamic module

```js
// Use dynamic ES module with promise APIs, import from named export:
import("./esCounterModule.js").then(({ increase, reset }) => {
    increase();
    reset();
});
// Or import from default export:
import("./esCounterModule.js").then(dynamicESCounterModule => {
    dynamicESCounterModule.increase();
    dynamicESCounterModule.reset();
});
```

```js
// Use dynamic ES module with async/await.
(async () => {

    // Import from named export:
    const { increase, reset } = await import("./esCounterModule.js");
    increase();
    reset();

    // Or import from default export:
    const dynamicESCounterModule = await import("./esCounterModule.js");
    dynamicESCounterModule.increase();
    dynamicESCounterModule.reset();

})();
```

### System module: SystemJS module

SystemJS is a library that can enable ES module syntax for older ES. If the current runtime, like an old browser, does not support ES6 syntax, the above code cannot work. One solution is to transpile the above module definition to a call of SystemJS library API, `System.register`:

```js
// Define SystemJS module.
System.register(["./dependencyModule1.js", "./dependencyModule2.js"], function (exports_1, context_1) {
    "use strict";
    var dependencyModule1_js_1, dependencyModule2_js_1, count, increase, reset;
    var __moduleName = context_1 && context_1.id;
    return {
        setters: [
            function (dependencyModule1_js_1_1) {
                dependencyModule1_js_1 = dependencyModule1_js_1_1;
            },
            function (dependencyModule2_js_1_1) {
                dependencyModule2_js_1 = dependencyModule2_js_1_1;
            }
        ],
        execute: function () {
            dependencyModule1_js_1.default.api1();
            dependencyModule2_js_1.default.api2();
            count = 0;
            // Named export:
            exports_1("increase", increase = function () { return ++count };
            exports_1("reset", reset = function () {
                count = 0;
                console.log("Count is reset.");
            };);
            // Or default export:
            exports_1("default", {
                increase,
                reset
            });
        }
    };
});
```

## Object descriptors

Metadata added to an object.

- **property data descriptors** - object assigned to an object’s property. Dictates how the JavaScript engine will behave regarding that property. They can be used to dictate how to work with objects.
	- `value` - the actual value we want the property to be (defaults undefined).
	- `enumerable` - whether the property should show up in operations that enumerate over an object’s keys, such as `for...in` loops or `Object.keys()` (defaults false).
	- `configurable` - indicates if we can later change the descriptor settings or be able to delete the property off the object (defaults false).
	- `writable` - tells if the value of the property can be changed (defaults false).
```js

const obj = {};
obj.a = 1;
Object.getOwnPropertyDescriptor(obj, 'a');
// { value: 1,
//   writable: true,
//   enumerable: true,
//   configurable: true }

Object.defineProperty(
    obj,              // Object we're defining property on
    'a',                 // Property name
    {                     // Property descriptor
        value: 1,
        enumerable: true,
        writable: false // by default
        configurable: false // by default
    }
);

Object.defineProperties(
    obj,                    
    {                             
        a: {
            value: 1,
            writable: false,
            enumerable: true,
            configurable: false
        },
        b: {
            value: 2,
            writable: false,
            enumerable: true,
            configurable: false
        }
    }
);
```

**property accessor descriptors** - object assigned to object's property. Hiddes implementation details, allows to perform additional operations. For accessor properties, there is no value or writable, but instead there are get and set functions. **Getters/setters** can be used as wrappers over **real** property values to gain more control over operations with them.
- `get` – a function without arguments, that works when a property is read.
- `set` – a function with one argument, that is called when the property is set.
- `enumerable` – same as for data properties.
- `configurable` – same as for data properties.

```js
let user = {
  name: "John",
  surname: "Smith"
};

Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  }
});

alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```

> If we try to supply both get and value in the same descriptor, there will be an error.

```js
// Error: Invalid property descriptor.
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },

  value: 2
});
```

## Named functions

- we can get name of the function `functionInstance.name`,
- printed in stack traces,
- can be reused

## Name shadowing

Occurs when a **variable** is declared in certain scope has the same name defined on it’s outer scope.

```js
var a  = 5;

const Fn = () => {
    var a = 6; 

    console.log(a); // 6
}

Fn();
```

## `NaN`

The global NaN property is a value representing Not-A-Number.

```js
function sanitise(x) {
  if (isNaN(x)) {
    return NaN;
  }
  return x;
}

console.log(sanitise('1'));
// expected output: "1"

console.log(sanitise('NotANumber'));
// expected output: NaN
```

## Native objects

Object in an ECMAScript implementation whose semantics are fully defined by this specification rather than by the host environment.

Examples: `Date`, `Math`, `parseInt`, `eval`, `indexOf`, ...etc.

## Pass by reference vs pass by value

The variable doesn't "hold" the object - it holds a reference. You can assign that reference to another **variable**, and now both reference the same object. 

JavaScript always **pass by value** (even when that value is a reference). There's no way to alter the value held by a variable passed as a parameter, which would be possible if JavaScript supported passing by reference.

```js
function changeStuff(a, b, c)
{
  a = a * 10;
  b.item = "changed";
  c = {item: "changed"};
}

var num = 10; // 10
var obj1 = {item: "unchanged"}; 
var obj2 = {item: "unchanged"}; 

changeStuff(num, obj1, obj2);

console.log(num); // 10
console.log(obj1.item); // changed
console.log(obj2.item); // unchanged
```

## Partial application

Technique of fixing a number of arguments to a function, producing another function of smaller arguments. Partial application produces functions of arbitrary number of arguments. The transformed function is different from the original — it needs less arguments (have smaller **arity**).

```js
const partial = (fn, ...argsToApply) => {
  return (...restArgsToApply) => {
    return fn(...argsToApply, ...restArgsToApply)
  }
}
const getApiURL = (apiHostname, resourceName, resourceId) => {
  return `https://${apiHostname}/api/${resourceName}/${resourceId}`
}
const getResourceURL = partial(getApiURL, 'localhost:3000')
const getUserURL = userId => {
  return getResourceURL('users', userId)
}
const getOrderURL = orderId => {
  return getResourceURL('orders', orderId)
}
const getProductURL = productId => {
  return getResourceURL('products', productId)
}
```

## Polyfill

Piece of code (usually JavaScript on the Web) used to provide modern functionality on older browsers that do not natively support it.
For example, a polyfill could be used to mimic the functionality of an HTML Canvas element on Microsoft Internet Explorer 7 using a Silverlight plugin or mimic support for CSS rem units, or text-shadow, or whatever you want.

## Prototype inheritance

When it comes to **inheritance**, JavaScript only has one construct: **objects**. Each object has a `private` property which holds a link to another object called its **prototype**. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype. By definition, null has no prototype, and acts as **the final link in this prototype chain**.

```js
function Person(first, last, age, gender, interests) {
  // property and method definitions
  this.name = {
    'first': first,
    'last' : last
  };
  this.age = age;
  this.gender = gender;
  //...see link in summary above for full definition
}
Person.prototype.farewell = function() { // prototype definition
  alert(this.name.first + ' has left the building. Bye for now!');
};

let person1 = new Person('Bob', 'Smith', 32, 'male', ['music', 'skiing']);

console.log(person1.__proto__); // Object {}
console.log(Object.getPrototypeOf(person1)); // Object {}

console.log(person1.__proto__.__proto__);  // Object {}
console.log(Object.getPrototypeOf(person1.__proto__)); // Object {}

console.log(person1.__proto__.__proto__.__proto__); // null
console.log(Object.getPrototypeOf(person1.__proto__.__proto__)); // null
```

```js
function Student() {
    this.name = 'John';
    this.gender = 'M';
}
var studObj = new Student();
Student.prototype.sayHi= function(){
    alert("Hi");
};
var studObj1 = new Student();
var proto = Object.getPrototypeOf(studObj1);  // returns Student's prototype object
alert(proto.constructor); // returns Student function 
```

```js
function Person(firstName, lastName) {
    this.FirstName = firstName || "unknown";
    this.LastName = lastName || "unknown";            
}
Person.prototype.getFullName = function () {
    return this.FirstName + " " + this.LastName;
}
function Student(firstName, lastName, schoolName, grade)
{
    Person.call(this, firstName, lastName);

    this.SchoolName = schoolName || "unknown";
    this.Grade = grade || 0;
}
//Student.prototype = Person.prototype;
Student.prototype = new Person();
Student.prototype.constructor = Student;
var std = new Student("James","Bond", "XYZ", 10);
alert(std.getFullName()); // James Bond
alert(std instanceof Student); // true
alert(std instanceof Person); // true
```

![Person object shape](https://mdn.mozillademos.org/files/13853/object-available-members.png)

![Protype chain](https://mdn.mozillademos.org/files/13891/MDN-Graphics-person-person-object-2.png)

The browser initially checks to see if the person1 object has a `valueOf()` method available on it, as defined on its constructor, `Person()`, and it doesn't.
So the browser checks to see if the person1's prototype object has a `valueOf()` method available on it. It doesn't, then the browser checks person1's prototype object's prototype object, and it has. So the method is called.

> We want to reiterate that the **methods** and **properties** are not copied from one object to another in the prototype chain. They are accessed by walking up the chain as described above.

> The prototype chain is traversed only while retrieving properties. If properties are set or deleted directly on the object, the prototype chain is not traversed.

## Reflection

  Language's ability to inspect and dynamically call **classes, methods, attributes, etc.** at runtime. If there is an option to do something without reflection - do it without reflection.

> Slower than normal code execution and in some languages requires runtime permissions.

> Allows to access things like private fields which can cause unexpected side effects.

> Key mechanism to allow an application or framework to work with code that might not have even been written yet.

> In JS `Reflect` object can't be created with `new` keyword.

```js
  // Reflection tools before ES6
  Object.getOwnPropertyDescriptor(), Object.keys(), Object.isArray(), ...etc
  // Reflection tools after ES6
  class Person {
      constructor(firstName, lastName) {
          this.firstName = firstName;
          this.lastName = lastName;
      }
      get fullName() {
          return `${this.firstName} ${this.lastName}`;
      }
  };

  let args = ['John', 'Doe'];

  let john = Reflect.construct(
      Person,
      args
  );

  console.log(john instanceof Person);
  console.log(john.fullName); // John Doe
```

## Rounding problem

`number` type varialbes acts like `float` in JavaScript. This generates problems when comparing values or displaying.

> `Number.EPSILON` property represents the difference between 1 and the smallest floating point number greater than 1.
```js
function areEqual(num1, num2) {
	return Math.abs( num1 - num2 ) < Number.EPSILON;
}

console.log(0.1 + 0.2); // 0.3000000004
console.log(0.1 + 0.2 === 0.3); // false
console.log(areEqual(0.1 + 0.2, 0.1)); // true
```

## same-origin policy

The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.
Two URLs have the same origin if the **protocol**, **port (if specified)**, and **host** are the same for both.

- http://store.company.com/dir2/other.html - Same origin - Only the path differs.
- http://store.company.com/dir/inner/another.html - Same origin - Only the path differs.
- https://store.company.com/page.html - Failure - Different protocol.
- http://store.company.com:81/dir/page.html - Failure Different port (http:// is port 80 by default).
- http://news.company.com/dir/page.html	- Failure - Different host.

### Origin inheritance

Scripts executed from pages with an `about:blank` or `javascript: URL` inherit the origin of the document containing that URL, since these types of URLs do not contain information about an origin server.

> `about:blank` is often used as a URL of new, empty popup windows into which the parent script writes content (e.g. via the `Window.open()` mechanism). If this popup also contains JavaScript, that script would inherit the same origin as the script that created it.

### Cross-origin network access

The **same-origin policy** controls interactions between two different origins, such as when you use `XMLHttpRequest` or an `<img>` element. These interactions are typically placed into three categories:

- Cross-origin writes are typically allowed. Examples are links, redirects, and form submissions. Some HTTP requests require **preflight**.
- Cross-origin embedding is typically allowed: `<script>`, `<link>`, `<img />`, `<video>`, `<audio>`.
- Cross-origin reads are typically disallowed, but read access is often leaked by embedding. For example, you can read the dimensions of an embedded image, the actions of an embedded script, or the availability of an embedded resource.

### CORS - Cross-Origin Resource Sharing

Mechanism that uses additional `HTTP` headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin. A web application executes a **cross-origin HTTP** request when it requests a resource that has a different origin **(domain, protocol, or port)** from its own.

> Example of a cross-origin request: the front-end JavaScript code served from https://domain-a.com uses `XMLHttpRequest` to make a request for https://domain-b.com/data.json.

This **cross-origin** sharing standard can enable cross-site HTTP requests for:

- Invocations of the XMLHttpRequest or `fetch()` APIs.
- Web Fonts (for cross-domain font usage in `@font-face` within CSS),
- WebGL textures.
- Images/video frames drawn to a canvas using `drawImage()`.
- CSS Shapes from images.

### Simple requests

Some requests don’t trigger a **CORS preflight**. Those are called **simple requests** - `GET`, `HEAD`, `POST` - and many others.

### Preflight requests

First send an `HTTP` request by the `OPTIONS` method to the resource on the other domain, to determine if the actual request is safe to send. Cross-site requests are preflighted like this since they may have implications to user data.

```js
const xhr = new XMLHttpRequest();
xhr.open('POST', 'https://bar.other/resources/post-here/'); // preflighted request
xhr.setRequestHeader('X-PINGOTHER', 'pingpong');
xhr.setRequestHeader('Content-Type', 'application/xml');
xhr.onreadystatechange = handler;
xhr.send('<person><name>Arun</name></person>'); 
```

## Scope chain

The scope chain is a list of all the variable objects of functions inside which the current function exists. Scope chain also consists of the current function execution object.

```js
a = 1;
var b = 2;
cFunc = function(e) {
  var c = 10;
  var d = 15;
  console.log(c);
  console.log(a); 
  function dFunc() {
    var f = 5;
    console.log(f)
    console.log(c);
    console.log(a); 
  }
  dFunc();
}
cFunc(10);

// Scope chain of dFunc = [dFunc variable object, 
                       // cFunc variable object,
                        // Global execution context variable object]
```

## Script types

Browser process HTML markup from `<head>` to `<body>`. So if we have `<script>` tag between HTML markup - rendering process will be blocked until script is **downloaded and executed**.

```html
<p>...content before script...</p>

<script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>

<!-- This isn't visible until the script loads -->
<p>...content after script...</p>
```

Workaround for that is - adding script at bottom - whole page will be rendered and after that script will be loaded. 
In this scenario browser renders full html, **starts download / execute script**.

```html
<body>
  ...all content is above the script...

  <script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>
</body>
```

`defer` attribute allows to load script **in the background** and after full DOM render - execute script. Much faster becauase we rendering HTML and downloading script in the same time.

- Document order - as they go in the document.
- Never block the page.
- Always execute when the DOM is ready (but before DOMContentLoaded event).
- Wait for other scripts to being downloaded - keeps their relative order before execution.
```js
// Download starts parallel to improve performance - but small.js must wait for long.js to be downloaded before exection.
<script defer src="https://javascript.info/article/script-async-defer/long.js"></script> 
<script defer src="https://javascript.info/article/script-async-defer/small.js"></script>
```

`async` behaves like `defer` but script is independent. 

- Load first order.
- Browser doesn't block on `async` like `defer`.
- Other scripts don't wait and `async` scripts don't wait for them.
- `DOmContentLoaded` and `async` scripts don't wait for each other:
   - `DOMcontentLoaded` may happen before `async` script (if finishes loading after the page is complete),
   - or after `async` script (if script was small or was in HTTP-cache).
- Loads `in the background` and runs when ready.

**dynamic scripts** created via JavaScript. Starts loading as soon as it’s appended to the document. Behaves as `async` by default.

```js
let script = document.createElement('script');
script.src = "/article/script-async-defer/long.js";
document.body.append(script); // (*)
```

Behaviour can be changed via JavaScript. `async` set as false tells - "scripts will behave just like `defer`".

```js
function loadScript(src) {
  let script = document.createElement('script');
  script.src = src;
  script.async = false;
  document.body.append(script);
}

// long.js runs first because of async=false
loadScript("/article/script-async-defer/long.js");
loadScript("/article/script-async-defer/small.js");
```

## Security

### Cross-Site Scripting (XSS)

Attacker injects client-side scripts through the website into the browsers of other users.  Because the injected code comes to the browser from the site, the code is trusted and can do things like send the user's site authorization cookie to the attacker. When the attacker has the cookie, they can log into a site as though they were the user and do anything the user can, such as access their credit card details, see contact details, or change passwords.

The best defense against **XSS** vulnerabilities is to remove or disable any markup that can potentially contain instructions to run the code. For HTML this includes elements, such as `<script>, <object>, <embed>, and <link>`.

The process of modifying user data so that it can't be used to run scripts or otherwise affect the execution of server code is known as input **sanitization**.

### SQL injection

Enable malicious users to execute arbitrary `SQL` code on a database, allowing data to be **accessed, modified, or deleted** irrespective of the user's permissions.

```sql
statement = "SELECT * FROM users WHERE name = '" + userName + "';" // WORKS AS EXPECTED
SELECT * FROM users WHERE name = 'a';DROP TABLE users; SELECT * FROM userinfo WHERE 't' = 't'; // ATTACK
```

### Cross-Site Request Forgery (CSRF)

Allows a malicious user to execute actions using the credentials of another user without that user’s knowledge or consent. 

This type of attack is best explained by example. John is a malicious user who knows that a particular site allows logged-in users to send money to a specified account using an `HTTP POST` request that includes the account name and an amount of money. John constructs a form that includes his bank details and an amount of money as hidden fields, and emails it to other site users (with the Submit button disguised as a link to a "get rich quick" site).

If a user clicks the submit button, an `HTTP POST` request will be sent to the server containing the transaction details and any client-side cookies that the browser associated with the site (adding associated site cookies to requests is normal browser behavior). The server will check the cookies, and use them to determine whether or not the user is logged in and has permission to make the transaction.

One way to prevent this type of attack is for the server to require that `POST` requests include a user-specific site-generated secret. The secret would be supplied by the server when sending the web form used to make transfers.

## Shallow freeze

The result of calling `Object.freeze(object)` only applies to the immediate properties of object itself and will prevent future property addition, removal or value re-assignment operations only on object. If the value of those properties are objects themselves, those objects are not frozen and may be the target of property addition, removal or value re-assignment operations.

```js
const employee = {
  name: "Mayank",
  designation: "Developer",
  address: {
    street: "Rohini",
    city: "Delhi"
  }
};

Object.freeze(employee);

employee.name = "Dummy"; // fails silently in non-strict mode
employee.address.city = "Noida"; // attributes of child object can be modified

console.log(employee.address.city) // Output: "Noida"
```

## `strict-mode`

- Eliminates some JavaScript silent errors by changing them to `throw` errors.
- Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
- Prohibits some syntax likely to be defined in future versions of ECMAScript,
- `this` global bind as `undefined`,
- using variables without declarations throws an errror,
- changing values of readonly properties throws an error,
- some security fixes.

## Strict equality `===`

Checks whether its two operands are equal, returning a `Boolean` result. Unlike the `==`, the strict equality operator always considers operands of different types to be different.

## `var`

- Function scoped / globally scoped.
```js
    var greeter = "hey hi";
    function newFunction() {
        var hello = "hello";
    }
```
- Can be re-declared / updated
```js
    var greeter = "hey hi";
    var greeter = "say Hello instead";
    // OR
    var greeter = "hey hi";
    greeter = "say Hello instead";
```

Problem with `var`:

```js
    var greeter = "hey hi";
    var times = 4;

    if (times > 3) {
        var greeter = "say Hello instead"; 
    }
    
    console.log(greeter) // "say Hello instead"
```

## Temporal Dead Zone

Describes the state where variables are **un-reachable**. They are in **scope*, but they aren't **declared**.

```js
// This is the temporal dead zone for the age variable!
// This is the temporal dead zone for the age variable!
// This is the temporal dead zone for the age variable!
console.log(age) // ReferenceError
let age = 25; // Whew, we got there! No more TDZ
console.log(age);
```

```js
// Both the below variables will be hoisted to the top of their scope!
console.log(typeof nonsenseThatDoesntExist); // Prints undefined
console.log(typeof name); // Throws an error, cannot access 'name' before initialization

let name = "Kealan";
```

![TDZReferenceError](https://www.freecodecamp.org/news/content/images/2020/10/image-5.png)

## `this`

Refers to an object, that object which is **executing the current bit** of javascript code. To understand `this` keyword, only we need to know how, **when and from where** the function is called, does not matter **how and where** function is declared or defined. In other words, every javascript function while executing has a reference to its current execution context, called `this`.

### Default binding of `this`

In `strict mode` default value of `this` keyword is `undefined` otherwise `this` keyword act as **global object**.

```js
// "use strict"

function bike() {
  console.log(this.name);
}

var name = "Ninja";

bike(); // "Ninja"
```

```js
"use strict"

function bike() {
  console.log(this.name);
}

var name = "Ninja";

bike(); // TypeError: Cannot read property name of undefined
```

### Implict binding of `this`

When there is an object property which we are calling as a **method** then that object becomes `this` object or **execution context object** for that method.

```js
function bike() {
  console.log(this.name);
}

var obj1 = { name: "Pulsar", bike: bike };
var obj2 = { name: "Gixxer", bike: bike };

obj1.bike();      // "Pulsar"
obj2.bike();      // "Gixxer"
```

### Explicit binding of `this`

When we use `call()` and `apply()` method with calling function, both of those methods take as their first parameter as **execution context**.

```js
function bike() {
  console.log(this.name);
}

var obj = { name: "Pulsar" }

bike.call(obj);   // "Pulsar"
bike.apply(obj, ['argument1', 'argument2']); // Pulsar
```

### Fixed / Hard binding of `this`

Foces `this` object to be same always no matter from where and how it gets called.

```js
var bike = function() {
  console.log(this.name);
}
var name = "Ninja";
var obj1 = { name: "Pulsar" };
var obj2 = { name: "Gixxer" };

var originalBikeFun = bike;
bike = function() {
  originalBikeFun.call(obj1);
};

bike(); // 'Pulsar'
bike.call(obj2); // 'Pulsar'
```

### `new` keyword and `this` binding

Bind `this` with the newly created object.

```js
function bike() {
  var name = "Ninja";
  this.maker = "Kawasaki";
  console.log(this.name + " " + maker);  // undefined Bajaj
}

var name = "Pulsar";
var maker = "Bajaj";

obj = new bike();
console.log(obj.maker);                  // "Kawasaki"
```

## Transpiling

**Source-to-source** compilation, are tools that read source code written in one programming language, and produce the equivalent code in another language. Languages you write that transpile to JavaScript are often called **compile-to-JS languages**, and are said to target JavaScript.

`Babel` transpiler is example of **transpiling** or `React-Native` behaviour which takes JavaScript code and transpiles this code for `Android`, `IOS` languages.

## Types

JavaScript is a **loosely typed** and **dynamic language**. Variables in JavaScript are not directly associated with any particular **value** type, and any **variable** can be assigned (and re-assigned) values of all types.

```js
let foo = 42;    // foo is now a number
foo     = 'bar'; // foo is now a string
foo     = true;  // foo is now a boolean
```

### Data types - Primitives

- `undefined` - `typeof instance === "undefined"`
Variable which is declared but not initialized.
- `Boolean` - `typeof instance === "boolean"`
- `Number` - `typeof instance === "number"`
- `String` - `typeof instance === "string"`
- `BigInt` - `typeof instance === "bigint"`
- `Symbol` - `typeof instance === "symbol"`

### Structural types

- `Object` - `typeof instance === "object"`
Special **non-data** but **structural** type for any constructed object instance also used as data structures: `new Object, new Array, new Map, new Set, new WeakMap, new WeakSet, new Date` and almost everything made with `new` keyword.

- `Function` - a **non-data** structure, though it also answers for `typeof` operator: `typeof instance === "function"`. This is merely a special shorthand for Functions, though every Function constructor is derived from Object constructor.

### Structural root primitive

 - `null` - `typeof instance === "object"`
 Special primitive type having additional usage for its value: if object is not inherited, then `null` is shown.

# ES6 features

## `Array` methods

```js
Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"
```

## Arrow functions expression

An arrow **function expression is a compact alternative** to a **traditional function expression**, but is limited and can't be used in all situations.

- Does not have its own bindings to this or super, and should not be used as methods.
- Does not have `arguments`, or `new.target` keywords - use `...rest` operator for arguments instead.
- Not suitable for `call, apply and bind methods`, which generally rely on establishing a scope.
- Can not be used as `constructors`.
- Can not use `yield`, within its body.

```js
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

## Build-in modules syntax

```js
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}

// app.js
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
```

## `class`

Simple sugar over the prototype-based **OO** pattern. Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability. Classes support **prototype-based inheritance**, `super` calls, instance and `static` methods and `constructors`.

```js
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

## `const`

- Works same as `let`.
- Value cannot be modified once assigned.

```js
const PI = 3.142;
PI = 22/7; // Let's reassign the value of PI
console.log(PI); // Output: TypeError: Assignment to constant variable.
```

```js
console.log(hoist); // Output: ReferenceError: hoist is not defined
const hoist = 'The variable has been hoisted.';
```

```js
function getCircumference(radius) {
  console.log(circumference)
  circumference = PI*radius*2;
  const PI = 22/7;
}
getCircumference(2) // ReferenceError: circumference is not defined
```

```js
const PI;
console.log(PI); // Ouput: SyntaxError: Missing initializer in const declaration
PI=3.142;
```

## Destructuring

```js
// list matching
var [a, , b] = [1,2,3];

// object matching
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
```

## Default

```js
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```

## Enhanced object literals

```js
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

## Generators

While custom `iterators` are a useful tool, their creation requires careful programming due to the need to explicitly maintain their **internal state**. `Generator` functions provide a powerful alternative: they allow you to define an iterative algorithm by writing a single function whose execution is not continuous. Generator functions are written using the `function*` syntax.

- `yield` usage pauses execution and saves state.
- If all `yields` called - then returns `{ done: true, value: undefined }`.
- Using `return` keyword ends generator.
- Using `return` is equal to `next()` after last `yield`.

> Used in sequences which are expensive to compute.

```js
function*  generatorFunc() {
  yield 'a';
  return 'b'; // Generator ends here.
  yield 'a'; // Will never be executed. 
}
```

```js
function* generatorFunction() { // Line 1
  console.log('This will be executed first.');
  yield 'Hello, ';   // Line 2
  console.log('I will be printed after the pause');  
  yield 'World!';
}
const generatorObject = generatorFunction(); // Line 3
console.log(generatorObject.next().value); // Line 4
console.log(generatorObject.next().value); // Line 5
console.log(generatorObject.next().value); // Line 6
// This will be executed first.
// Hello, 
// I will be printed after the pause
// World!
// undefined
```

```js
function* fibonacci() {
  let current = 0;
  let next = 1;
  while (true) {
    let reset = yield current;
    [current, next] = [next, next + current];
    if (reset) {
        current = 0;
        next = 1;
    }
  }
}

const sequence = fibonacci();
console.log(sequence.next().value);     // 0
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 2
console.log(sequence.next().value);     // 3
console.log(sequence.next().value);     // 5
console.log(sequence.next().value);     // 8
console.log(sequence.next(true).value); // 0
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 2
```

## Iterables & `for...of`

An `object` is **iterable** if it defines its iteration behavior, such as what values are looped over in a `for...of` construct. Some built-in types, such as `Array` or `Map`, have a default iteration behavior, while other types (such as `Object`) do not.

```js
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

## Iterators

Object which defines a sequence and potentially a `return` value upon its termination. Specifically, an iterator is any `object` which `implements` the `Iterator` protocol by having a `next()` method that `returns` an `object` with two properties: 

Once created, an iterator object can be iterated explicitly by repeatedly calling `next()`. Iterating over an iterator is said to consume the iterator, because it is generally only possible to do once. After a terminating value has been yielded additional calls to `next()` should simply continue to return `{ done: true }`.

- `value` - next value in iteration sequence.
- `done` - this is `true` if the last value in the sequence has already been consumed. If value is present alongside done, it is the iterator's `return` value.

> Used in sequences which are expensive to compute.

```js
function makeRangeIterator(start = 0, end = Infinity, step = 1) {
    let nextIndex = start;
    let iterationCount = 0;

    const rangeIterator = {
       next: function() {
           let result;
           if (nextIndex < end) {
               result = { value: nextIndex, done: false }
               nextIndex += step;
               iterationCount++;
               return result;
           }
           return { value: iterationCount, done: true }
       }
    };
    return rangeIterator;
}

const it = makeRangeIterator(1, 10, 2);

let result = it.next();
while (!result.done) {
 console.log(result.value); // 1 3 5 7 9
 result = it.next();
}

console.log("Iterated over sequence of size: ", result.value); // [5 numbers returned, that took interval in between: 0 to 10]
```

## `let`

- Block scoped variable - not function scoped as `var`.
- `let` variables are block hoisted.
- Remain uninitialised at the beginning of execution.

```js
console.log(hoist); // Output: ReferenceError: hoist is not defined ...
let hoist = 'The variable has been hoisted.';
```

```js
let hoist;
console.log(hoist); // Output: undefined
hoist = 'Hoisted'
```

## Math methods

```js
Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2
```

## `Map`

The `Map` object holds **key-value pairs** and remembers the original insertion order of the keys. Any value (both objects and primitive values) may be used as either a key or a value. A `Map` object iterates its elements in insertion order — a for...of loop returns an array of `[key, value]` for each iteration.

> Maps mutates values - so fe. in React - you must use new Map(currMap.set()) - to trigget re-render.

- A `Map` does not contain any keys by default. It only contains what is explicitly put into it - no prototype added - only own properties.
- Key can be any value `function`, `object`, or **primitive**.
- Keys are ordered.
- The number of items in a `Map` is easily retrieved from its `size` property.
- A `Map` is an iterable, so it can be directly iterated.
- Performs better in scenarios involving frequent additions and removals of **key-value** pairs.
- Problems with garbage collecting becuase of object references.

> Don't do
```js
let wrongMap = new Map()
wrongMap['bla'] = 'blaa' // don't do
wrongMap['bla2'] = 'blaaa2' don't do

console.log(wrongMap)  // Map { bla: 'blaa', bla2: 'blaaa2' }

wrongMap.has('bla')    // false
wrongMap.delete('bla') // false
console.log(wrongMap)  // Map { bla: 'blaa', bla2: 'blaaa2' }
```

> Do
```js
let contacts = new Map()
contacts.set('Jessie', {phone: "213-555-1234", address: "123 N 1st Ave"})
contacts.has('Jessie') // true
contacts.get('Hilary') // undefined
contacts.set('Hilary', {phone: "617-555-4321", address: "321 S 2nd St"})
contacts.get('Jessie') // {phone: "213-555-1234", address: "123 N 1st Ave"}
contacts.delete('Raymond') // false
contacts.delete('Jessie') // true
console.log(contacts.size) // 1
```

```js
let myMap = new Map()
myMap.set(0, 'zero')
myMap.set(1, 'one')

for (let [key, value] of myMap) {
  console.log(key + ' = ' + value)
}
// 0 = zero
// 1 = one

for (let key of myMap.keys()) {
  console.log(key)
}
// 0
// 1

for (let value of myMap.values()) {
  console.log(value)
}
// zero
// one

for (let [key, value] of myMap.entries()) {
  console.log(key + ' = ' + value)
}
// 0 = zero
// 1 = one
```

## Module loaders

```js
// Dynamic loading – ‘System’ is default loader
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
```

## Number methods

```js
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false
```

## Object methods

```js
Object.assign(Point, { origin: new Point(0,0) })
```

## Promise

The `Promise` object represents the eventual completion (or failure) of an asynchronous operation and its resulting value. `PromiseStatus` can have three different values: `pending`, `resolved`, or `rejected`. 

> For `pending` status value will be always `undefined`.

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject();
  }, 500);
});

promise
  .then(
    () => {
      console.log("ok");
    },
    () => {
      console.log("error"); // called
      return Promise.reject();
    }
  )
  .then(
    (res) => {
      console.log("ok");
    },
    () => {
      console.log("error"); // called
    }
  )
  .catch(() => {
    console.log("error"); // not called
  })
  .finally(() => {
    console.log("finalized");
  });

```

![Promise object](https://miro.medium.com/max/469/1*n6s4IswZBVUIHc2K3apONA.png)

![Promise](https://miro.medium.com/max/875/1*0mBlni5vsYZE2wFzfVv8EA.png)

## `async` & `await`

The word `async` before a function means one simple thing: a function always returns a `promise`. Other values are wrapped in a **resolved promise automatically**.

```js
const getUsers = async () => {
  return { id: 0, firstName: "Piotr" };
};
// EQUAL TO
const getUsers = () => Promise.resolve();

getUsers().then() // { id: 0;, firstName: 'Piotr' }
```

The `await` makes JavaScript wait until that promise settles and returns its result.

```js
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });
 
  try {
     let result = await promise; // wait until the promise resolves (*) 
  }
  catch {
     // handle error
  }

  alert(result); // "done!"
}

f();
```

### `Promise.all()`

Takes an iterable of promises as an input, and returns a single `Promise`.  Runs almost **parallel** - depends on CPU.

- Resolves when **all** resolves.
- It rejects immediately upon any of the input **promises** rejecting or **non-promises** throwing an error, and will reject with this first rejection message / error.

```js
const getUsers = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        id: 0,
        firstName: "Piotr",
      });
    }, 500);
  });
};

const getBooks = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ id: 0, name: "Harry Potter" });
    }, 500);
  });
};

Promise.all([getUsers(), getBooks()]).then((res) => {
  console.log(res);
});

```

### `Promise.race()`

Returns a promise that fulfills or rejects as soon as one of the promises in an iterable fulfills or rejects, with the value or reason from that promise.

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one');
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then((value) => {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"
```

### `Promise.any()`

Takes an iterable of `Promise` objects and, as soon as one of the promises in the iterable fulfills, returns a single promise that resolves with the value from that promise. If no promises in the iterable fulfill (if all of the given promises are rejected), then the returned promise is rejected with an `AggregateError`, a new subclass of `Error` that groups together individual errors. Essentially, this method is the opposite of `Promise.all()`.

```js
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'quick'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, 'slow'));

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));

// expected output: "quick"
```

### `Promise.allSettled()`

Returns a promise that resolves after all of the given promises have either fulfilled or rejected, with an array of objects that each describes the outcome of each promise.
It is typically used when you have multiple asynchronous tasks that are not dependent on one another to complete successfully, or you'd always like to know the result of each promise.
In comparison, the Promise returned by `Promise.all()` may be more appropriate if the tasks are dependent on each other / if you'd like to immediately reject upon any of them rejecting.

```js
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
const promises = [promise1, promise2];

Promise.allSettled(promises).
  then((results) => results.forEach((result) => console.log(result.status)));

// expected output:
// "fulfilled"
// "rejected"
```

## Proxy

Enables you to create a proxy for another object, which can intercept and redefine fundamental operations for that object. `Proxy` object is created with two parameters:

- `target`: the original object which you want to proxy.
- `handler`: an object that defines which operations will be intercepted and how to redefine intercepted operations.

```js
const target = {
  message1: "hello",
  message2: "everyone"
};

const handler3 = {
  get: function (target, prop, receiver) {
    if (prop === "message2") {
      return "world";
    }
    return Reflect.get(...arguments);
  },
};

const proxy3 = new Proxy(target, handler3);

console.log(proxy3.message1); // hello
console.log(proxy3.message2); // world
```

## Reflect

```js
  // Reflection tools before ES6
  Object.getOwnPropertyDescriptor(), Object.keys(), Object.isArray(), ...etc
  // Reflection tools after ES6
  class Person {
      constructor(firstName, lastName) {
          this.firstName = firstName;
          this.lastName = lastName;
      }
      get fullName() {
          return `${this.firstName} ${this.lastName}`;
      }
  };

  let args = ['John', 'Doe'];

  let john = Reflect.construct(
      Person,
      args
  );

  console.log(john instanceof Person);
  console.log(john.fullName); // John Doe
```

## Rest operator

```js
function f(x, ...y) {
  // y is an Array
  // same as arguments
  return x * y.length;
}
f(3, "hello", true) == 6
```

## `Set`

Stores unique values of any type, whether primitive values or `object` references. Set objects are collections of values. You can iterate through the elements of a set in insertion order. A value in the Set may only occur once - it is unique in the Set's collection.

```js
let mySet = new Set()

mySet.add(1)           // Set [ 1 ]
mySet.add(5)           // Set [ 1, 5 ]
mySet.add(5)           // Set [ 1, 5 ]
mySet.add('some text') // Set [ 1, 5, 'some text' ]
let o = {a: 1, b: 2}
mySet.add(o)

mySet.add({a: 1, b: 2})   // o is referencing a different object, so this is okay

mySet.has(1)              // true
mySet.has(3)              // false, since 3 has not been added to the set
mySet.has(5)              // true
mySet.has(Math.sqrt(25))  // true
mySet.has('Some Text'.toLowerCase()) // true
mySet.has(o)       // true

mySet.size         // 5

mySet.delete(5)    // removes 5 from the set
mySet.has(5)       // false, 5 has been removed

mySet.size         // 4, since we just removed one value

console.log(mySet)
// logs Set(4) [ 1, "some text", {…}, {…} ] in Firefox
// logs Set(4) { 1, "some text", {…}, {…} } in Chrome
```

```js
// iterate over items in set
// logs the items in the order: 1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
for (let item of mySet) console.log(item)

// logs the items in the order: 1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
for (let item of mySet.keys()) console.log(item)
 
// logs the items in the order: 1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
for (let item of mySet.values()) console.log(item)

// logs the items in the order: 1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
// (key and value are the same here)
for (let [key, value] of mySet.entries()) console.log(key)

// convert Set object to an Array object, with Array.from
let myArr = Array.from(mySet) // [1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2}]

// the following will also work if run in an HTML document
mySet.add(document.body)
mySet.has(document.querySelector('body')) // true

// converting between Set and Array
mySet2 = new Set([1, 2, 3, 4])
mySet2.size                    // 4
[...mySet2]                    // [1, 2, 3, 4]

// intersect can be simulated via 
let intersection = new Set([...set1].filter(x => set2.has(x)))

// difference can be simulated via
let difference = new Set([...set1].filter(x => !set2.has(x)))

// Iterate set entries with forEach()
mySet.forEach(function(value) {
  console.log(value)
})

// 1
// 2
// 3
// 4
```

```js
// Use to remove duplicate elements from the array 

const numbers = [2,3,4,4,2,3,3,4,4,5,5,6,6,7,5,32,3,4,5]

console.log([...new Set(numbers)]) 

// [2, 3, 4, 5, 6, 7, 32]
```

## Spread operator

```js
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

## Subsclassable built-ins

```js
// Pseudo-code of Array
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// User code of Array subclass
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

## String methods

```js
"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"
```

## Symbols

The original motivation for introducing symbols to Javascript was to enable private properties. They ended up being severely downgraded. They are no longer `private`, since you can find them via `reflection`, for example, using `Object.getOwnPropertySymbol`s or `proxy`. Right now used only for avoid name clashes.

```js
let sym = new Symbol()  // TypeError
Symbol('foo') === Symbol('foo')  // false
let sym = Symbol('foo')
typeof sym      // "symbol" 
let symObj = Object(sym)
typeof symObj   // "object"
```

## Unicode

```js
// same as ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// new String ops
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
for(var c of "𠮷") {
  console.log(c);
}
```

## Tails calls

Calls in tail-position are guaranteed to not grow the stack unboundedly. Makes recursive algorithms safe in the face of unbounded inputs.

```js
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Stack overflow in most implementations today,
// but safe on arbitrary inputs in ES6
factorial(100000)
```

## `WeakMap`

The `WeakMap` object is a collection of **key/value pairs** in which the keys are weakly referenced.

- There is no option to iterate through keys.
- No option to retrieve keys.
- Keys can only have `Object` type.
- Objects doesn't avert garbage collection if there are no references to the object which is acting like a key.

```js
const wm1 = new WeakMap(),
      wm2 = new WeakMap(),
      wm3 = new WeakMap();
const o1 = {},
      o2 = function() {},
      o3 = window;

wm1.set(o1, 37);
wm1.set(o2, 'azerty');
wm2.set(o1, o2); // a value can be anything, including an object or a function
wm2.set(o3, undefined);
wm2.set(wm1, wm2); // keys and values can be any objects. Even WeakMaps!

wm1.get(o2); // "azerty"
wm2.get(o2); // undefined, because there is no key for o2 on wm2
wm2.get(o3); // undefined, because that is the set value

wm1.has(o2); // true
wm2.has(o2); // false
wm2.has(o3); // true (even if the value itself is 'undefined')

wm3.set(o1, 37);
wm3.get(o1); // 37

wm1.has(o1); // true
wm1.delete(o1);
wm1.has(o1); // false
```

## `WeakSet`

Stores weakly held objects in a collection.

- `WeakSets` are collections of objects only. They cannot contain arbitrary values of any type.
- The `WeakSet` is weak, meaning references to objects in a `WeakSet` are held weakly. If no other references to an object stored in the WeakSet exist, those objects can be garbage collected.

> Circular references detection
```js
// Execute a callback on everything stored inside an object
function execRecursively(fn, subject, _refs = null){
  if(!_refs)
    _refs = new WeakSet();
  
  // Avoid infinite recursion
  if(_refs.has(subject))
    return;

  fn(subject);
  if("object" === typeof subject){
    _refs.add(subject);
    for(let key in subject)
      execRecursively(fn, subject[key], _refs);
  }
}

const foo = {
  foo: "Foo",
  bar: {
    bar: "Bar"
  }
};

foo.bar.baz = foo; // Circular reference!
execRecursively(obj => console.log(obj), foo);
```

## Template strings

```js
const renderDomNode = (target) => {
   const div = document.createElement('div');
   const content = `
   	<div>
           Some markdown
	</div>
   `;
   
   target.appendChild(target);
}
renderDomNode();
```

# ES7 features

#### `async/await`

#### `Exponentiation Operator`

####  `Async generators`

#### `Object.getOwnPropertyDescriptors`

#### `Object.values`

#### `Object.entries`

#### `Array.prototype.includes`

#### `Typed Objects`

#### `Trailing commas in function syntax`

#### `Class decorators`

#### `Class properties`

#### `Map.prototype.toJSON`

#### `Set.prototype.toJSON`

#### `String.prototype.at`

#### `Object rest properties`

#### `Object spread properties`

#### `String.prototype.padLeft`

#### `String.prototype.padRight`

#### `String.prototype.trimLeft`

#### `String.prototype.trimRight`

#### `Regexp.escape`

#### `Bind Operator`

#### `Reflect.Realm`

# Terms

## `n+1` problem

We have 2 tables: `Author` and `Book`. They are in **one** `Author` to **many** `Books` relationship. If you use `MySql` and some `ORM` like `sequalize` in `NodeJS` your pseudo code will looks like:

```js
route: '/authors/books',
method: 'GET',
handler: async () => ORM.getAuthors().getTheirBooks(); // call to database
```

DB call is transformed to:

```sql
SELECT *
FROM authors;
-- pretend this returns 3 authors
SELECT *
FROM books
WHERE author_id in (1, 2, 3); -- an array of the author's ids
```

This is correct - only 2 queries to DB for your data. `n+1` problem occurs when behind the scenes your query looks like that:

```sql
SELECT *
FROM authors; 
SELECT *
FROM books 
WHERE author_id in (1); 
SELECT * 
FROM books 
WHERE author_id in (2); 
SELECT *
FROM books 
WHERE author_id in (3); 
```

Instead of combinig authors ids to one query we have `n` queries based on first query response and `1` which is always first query.

# SOLID

## S — Single responsibility principle

A `class`, `function`, `module` should have one and only one reason to change, meaning that a `class`, `function`, `module` should only have one job.

> Do
```js
export class Comment {
  // ...
  update({content, likesCount, isLikedByUser}) {
    saveCommentRepository
      .save(this.model)
      .then((updatedModel) => {
        this.model = updatedModel;
        this.render();
      }, (error) => {
        throw error;
      });
  }
}

export class SaveCommentRepository {
  save(model) {
    return $.post({
      url: 'some/funny/url/',
      data: model.toJSON()
    });
  }
}
```

> Don't do
```js
// How NOT TO write JS
export class Comment {
  // ...
  update(newModel) {
    $.post({
        url: 'some/funny/url/',
        data: model.toJSON()
      }).then((updatedModel) => {
        this.model = updatedModel;
      }, (error) => {
        throw new Error(error.message);
      });
  }
}
```

## O — Open closed principle

Objects or entities should be open for extension, but closed for modification. The goal is to make the system easy to extend without incurring a high impact of change.
Software systems must be allowed to change their behavior by adding new code rather than changing the existing code.

> Do
```js
const users = ['adam', 'pawel'];

const usersMaker = {
   add: (user) => {
  	users.push(user);
   }
}

export default usersMAker;

usersMaker.add('new-user');
```

> Don't do
```js
const users = ['adam', 'pawel', 'new-user'];

const usersMaker = {
  
}

export default usersMAker;
```

## L — Liskov substitution principle

Subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.
"If it looks like a duck, quacks like a duck, but needs batteries - you probably have the wrong abstraction".

> Do
```js
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

> Don't do
```js
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

## I — Interface segregation principle

A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.

> Do
```js
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```

> Don't do

```js
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

## D — Dependency Inversion principle

Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions.

> Do
```js
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```

> Don't do

```js
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

# ORP - Object oriented programming

## Abstraction

Way of creating a simple model of a more complex real-world entities, which contains the only important properties from the perspective of the context of an application. Abstraction manages complexity of a system by hiding internal details and composing it in several smaller systems.

![Abstraction](https://miro.medium.com/max/875/1*D95h8JObfAxrM0b3vxCD8w.jpeg)

```js
class Person {
    constructor({firstName, lastName, job}) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.job = job;
        this.skills = [];
        Person._amount = Person._amount || 0;
        Person._amount++;
    }

    static get amount() {
        return Person._amount;
    }
    
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    set fullName(fN) {
        if (/[A-Za-z]\s[A-Za-z]/.test(fN)) {
            [this.firstName, this.lastName] = fN.split(' ');
        } else {
            throw Error('Bad fullname');
        }
    }

    learn(skill) {
        this.skills.push(skill);
    }
}

class Job {
    constructor(company, position, salary) {
        this.company = company;
        this.position = position;
        this.salary = salary;
    }
}

const john = new Person({
    firstName: 'John',
    lastName: 'Doe',
    job: new Job('Youtube', 'developer', 200000)
});

const roger = new Person({
    firstName: 'Roger',
    lastName: 'Federer',
    job: new Job('ATP', 'tennis', 1000000)
});

john.fullName = 'Mike Smith';
john.learn('es6');
roger.learn('programming');
john.learn('es7');
```

## Inheritance

Approach of sharing common functionality within a collection of classes. It provides an ability to avoid code duplication in a class that needs the same data and functions which another class already has. At the same time, it allows us to override or extend functionality that should have a different behavior.

![Inheritance](https://miro.medium.com/max/875/1*UKdo9OtMxozL7Evz0-vORw.png)

```js
class ClassA {
    constructor() {
        this.propA = 'A';
    }

    methodA() {
        return this.propA;
    }
}

class ClassB extends ClassA {
    constructor() {
        super();
        this.propB = 'B';
    }

    methodA() {
        return 'NEW B';
    }

    methodB() {
        return this.propB + this.methodA();
    }
}

class ClassC extends ClassB {
    constructor() {
        super();
        this.propC = 'C';
         this.testProp = 1;
    }

    methodC() {
        return this.propC + super.methodB();
    }
}
```

## Polymorphism

Ability to create a property, a function, or an object that has more than one realization. In other words, it is an ability of multiple object types to implement the same functionality that can work in a different way but supports a common interface.

```js
class Car {
   drive() {
	console.log('Car driving');
   }
}

class Opel extends Car {
   drive() {
	console.log('Opel driving');
   }
}

class Nissan {
   drive() {
	console.log('Nissan driving');
   }
}
```

![Polymorphism](https://miro.medium.com/max/875/1*Pu67br76VEd7Aoa5ieMj5Q.jpeg)

## Encapsulation

Encapsulation as a concept of bundling data related variables and properties with behavioral methods in one class or code unit. Also encapsulation is an approach for restricting direct access to some of the data structure elements (fields, properties, methods, etc).

```js
class Car {
    #speed = 0;
    #engine = TURN.OFF;
    model;
    speedLimit;

    get speed() {
        return this.#speed;
    }

    constructor(model, speedLimit = 100) {
        this.model = model;
        this.speedLimit = speedLimit;
    }

    async drive(speed = 10) {
        this.#engine = TURN.ON;
        await this.setSpeed(speed);
        console.log(`Ridding with speed: ${this.#speed}`);
        return this.#speed;
    }

    async stop() {
        await this.setSpeed(0);
        this.engine = TURN.OFF;
        return this.#speed;
    }

    async setSpeed(speed) {
        return new Promise((resolve, reject) => {
            if (this.engine === TURN.OFF) {
                reject('Turn on engine!');
            } else if (this.speedLimit < speed) {
                reject(`Can't reach speed: ${speed}. Max speed limit is ${this.speedLimit}`);
            } else {
                const interval = setInterval(async () => {
                    console.log('current speed:', this.#speed);
                    if (this.#speed < speed) {
                        this.#speed++;
                    } else if (this.#speed > speed) {
                        this.#speed--;
                    } else {
                        resolve(this.#speed);
                        clearInterval(interval);
                    }
                }, 100);
            }
        });
    }

    toString() {
        return `Car: ${this.model}; Engine turned on: ${this.#engine}; ` + (this.#engine ? `Current speed: ${this.#speed}` : '');
    }
}

const TURN = {
    OFF: false,
    ON: true
}
```

# Architectural patterns

## MVC - Model View Controller

**MVC** offers architectural benefits over standard JavaScript — it helps you write better organized, and therefore more maintainable code. This pattern has been used and extensively tested over multiple languages and generations of programmers.

- **Model** - Defines what data the app should contain. If the state of this data changes, then the model will usually notify the view (so the display can change as needed) and sometimes the controller (if different logic is needed to control the updated view).
- **View** - Defines how the app's data should be displayed.
- **Controller** - Contains logic that updates the model and/or view in response to input from the users of the app.

![MVC](https://mdn.mozillademos.org/files/16042/model-view-controller-light-blue.png)

```html
<div id="root">
  <button onclick="userController.display()">
    Display user details
  </button>
</div>
```

### Partial MVC (without View-Model) connection:

```ts
interface User {
    id: number;
    firstName: string;
    lastName: string;
}

class UserModel {
    private _user: User;

    get user(): User {
        return this._user;
    }

    set user(user: User) {
        this._user = user;
    }

    load = (): Promise<void> => {
        return new Promise((resolve) => {
            setTimeout(() => {
                const user = { id: 1, firstName: 'Piotr', lastName: 'Piotrowicz' } as User;
                this.user = user;
                resolve();
            }, 500);
        });
    }
}

class UserView {
    render = (user, target: HTMLElement): void => {
        const ul = document.createElement('ul');

        ul.appendChild(document.createRange().createContextualFragment(
            `
                <li>
                    <span>${user.id}</span>
                    <b>${user.firstName}</b>
                    <b>${user.lastName}</b>
                </li>`
        ));

        target.appendChild(ul);
    }
}

class UserController {
    private _view: UserView;
    private _model: UserModel;

    constructor() {
        // UserController handles communication between View & Model
        this._view = new UserView();
        this._model = new UserModel();
    }

    display = async (): Promise<void> => {
        const isUserDisplayed = !!this._model.user;

        if (isUserDisplayed) {
            return;
        }

        await this._model.load();

        this._view.render(this._model.user, document.getElementById('root'));
    }
}

const userController = new UserController();
```

### Full MVC + Observer pattern:

```js
interface User {
    id: number;
    firstName: string;
    lastName: string;
}

type UserObserver = (user: User) => void; 
    
class UserModel {
    private _user: User;

    private _observers: UserObserver[] = [];

    get user(): User {
        return this._user;
    }

    set user(user: User) {
        this._user = user;
        this._notify();
    }

    private _notify = (): void => {
        this._observers.forEach(sb => sb(this.user));
    }

    subscribe = (observer: UserObserver): void => {
        this._observers = [...this._observers, observer];
    };

    unsubscribe = (observer: UserObserver): void => {
        this._observers = this._observers.filter(ob => ob !== observer);
    };

    load = (): Promise<void> => {
        return new Promise((resolve) => {
            setTimeout(() => {
                const user = { id: 1, firstName: 'Piotr', lastName: 'Piotrowicz' } as User;
                this.user = user;
                resolve();
            }, 500);
        });
    }
}

class UserView {
    constructor(private _model: UserModel) {
        this._model.subscribe(
            user => {
                this.render(user, document.getElementById('root'))
            }
        )
    }

    render = (user, target: HTMLElement): void => {
        const ul = document.createElement('ul');

        ul.appendChild(document.createRange().createContextualFragment(
            `
                <li>
                    <span>${user.id}</span>
                    <b>${user.firstName}</b>
                    <b>${user.lastName}</b>
                </li>`
        ));

        target.appendChild(ul);
    }
}

class UserController {
    private _view: UserView;
    private _model: UserModel;

    constructor() {
        this._model = new UserModel();
        this._view = new UserView(this._model);
    }

    display = async (): Promise<void> => {
        const isUserDisplayed = !!this._model.user;

        if (isUserDisplayed) {
            return;
        }

        await this._model.load();
    }
}

const userController = new UserController();
```

### MVC framework implementation

- Hides implementation details.
- Forces how to write code.
- Creates layers.
- Prefers composition over inheritance.
- Unifies properties/methods the nomenclature.

```js
type Observer<T> = (data: T) => void;

class Model<T> {
    private _observers: Observer<T>[] = [];

    get data(): T {
        return this._data;
    }

    set data(data: T) {
        this._data = data;
        this._notify();
    }

    constructor(private _data: T) { }

    private _notify = (): void => {
        this._observers.forEach(ob => ob(this.data));
    }

    update = (data: T) => {
        this.data = data;
    }

    sub = (observer: Observer<T>): void => {
        this._observers = [...this._observers, observer];
    };

    unsub = (observer: Observer<T>): void => {
        this._observers = this._observers.filter(ob => ob !== observer);
    };
}

type Renderer<T> = (data: T) => string;

class View<T> {
    constructor(private _model: Model<T>, private _renderer: Renderer<T>, private _target: string) {
        this._model.sub(this.render);
    }

    public render = (data: T): void => {
        const foundTarget = document.querySelector(this._target);

        if (foundTarget) {
            foundTarget.appendChild(document.createRange().createContextualFragment(
                this._renderer(data)
            ));
        }

        throw new Error('Cannot found target');

    };
}

interface Config<T> {
    model: {
        data: T;
    }
    view: {
        renderer: Renderer<T>;
        target: string;
    }
    display: (payload: { data: T, update: (data: T) => void }) => void;
}

class Controller<T> {
    private _model: Model<T>;

    private _view: View<T>;

    constructor(private _config: Config<T>) {
        this._model = new Model<T>(this._config.model.data);
        this._view = new View<T>(this._model, this._config.view.renderer, this._config.view.target);
    }

    display = (): void => {
        this._config.display({
            data: this._model.data,
            update: this._model.update,
        });
    }
}

interface User {
    id: number;
    firstName: string;
    lastName: string;
}

const { display } = new Controller<User>({
    model: {
        data: null
    },
    view: {
        renderer: (user) => `<div>${user.firstName}</div>`,
        target: '#root'
    },
    display: async ({ data, update }): Promise<void> => {
        if (!!data) {
            return;
        }

        const getUsers = () => new Promise<User>((resolve) => {
            setTimeout(() => {
                const user = { id: 1, firstName: 'Piotr', lastName: 'Piotrowicz' } as User;
                resolve(user);
            }, 500);
        });

        const user = await getUsers();

        update(user);
    }
});
```

## MVVM

![MVVM](https://miro.medium.com/max/640/0*1ZrS8t3HvPzRAuqg.png)

## MVP

Decouples different aspects in the code.

- **Presenter** - Event Handling.
- **View** - DOM Manipulation.
- **Model** - Server communication (AJAX calls).

![MVP](https://miro.medium.com/max/640/0*1ZrS8t3HvPzRAuqg.png)

```html
<div id="root">
  <button onclick="userController.display()">
    Display user details
  </button>
</div>
```

# Design patterns

## Creational patterns

### Abstract factory

An Abstract Factory creates objects that are related by a **common theme.**

Suppose we have two **factories** whose task it is to create file readers - `PDF`, `XML`. One is the `XMLReaderFactory` which creates `XMLReader` and `PDFReaderFactory` which creates `PDFReader`. Both **factories** and **products** extends from their abstractions. 

Abstract factory hide object creation details and keeps your code base unified.

```ts
// Typical abstract factory use case
const xmlReader = new XMLReader();
```

![Abstract factory](https://www.dofactory.com/img/diagrams/javascript/javascript-abstract-factory.jpg)

```ts
// ABSTRACT PRODUCT
abstract class Reader {
  abstract name: string;
  abstract read(): string;
}

// CONCRETE PRODUCT
class XMLReader extends Reader {
  constructor(public name: string) {
    super();
  }

  read(): string {
    return "Readed xml file";
  }
}

// CONCRETE PRODUCT
class PDFReader extends Reader {
  constructor(public name: string) {
    super();
  }

  read(): string {
    return "Readed pdf file";
  }
}

enum ReaderType {
  XML = "XML",
  PDF = "PDF",
}

// ABSTRACT FACTORY
abstract class ReaderFactory<T> {
  abstract create(name: string): T;

  static getFactory(type: ReaderType) {
    switch (type) {
      case ReaderType.XML:
        return new XMLReaderFactory();

      case ReaderType.PDF:
        return new PDFReaderFactory();

      default:
        throw new Error(`No implementation for given type ${type}`);
    }
  }
}

// CONCRETE FACTORY
class XMLReaderFactory extends ReaderFactory<XMLReader> {
  constructor() {
    super();
  }

  create(name: string): XMLReader {
    return new XMLReader(name);
  }
}

// CONCRETE FACTORY
class PDFReaderFactory extends ReaderFactory<PDFReader> {
  create(name: string): PDFReader {
    return new PDFReader(name);
  }
}

const pdfFactory = ReaderFactory.getFactory(ReaderType.PDF);
const xmlFactory = ReaderFactory.getFactory(ReaderType.XML);

const pdf = pdfFactory.create('My pdf file');
const xml = xmlFactory.create('My xml file');

console.log(pdf.read()); // Readed pdf file
console.log(xml.read()); // Readed xml file
```

### Builder

The Builder pattern allows a client to construct a complex object by specifying the type and content only. Construction details are hidden from the client entirely.

The most common motivation for using **Builder** is to simplify client code that creates **complex objects**. The client can still direct the steps taken by the **Builder** without knowing how the actual work is accomplished. Builders frequently encapsulate construction of **Composite objects** because the procedures involved are often repetitive and complex.

Object can be created step by step with dedicated methods. Also this pattern simplifies constructors.

```ts
// Typical builder usage case
const button = new Button(
    // target
    // styles
    // events
    // hooks
    // template
    ...
);
```

![Builder](https://www.dofactory.com/img/diagrams/javascript/javascript-builder.jpg)

```ts
interface Styles { // Helper interface
    background?: string;
    border?: string;
    color?: string;
    height?: string;
    outline?: string;
    width?: string;
}

abstract class UiElement { // abstract product
    private _events: Function[];
    private _styles: Styles = {};

    get styles(): string {
        return Object.entries(this._styles).map(([key, value]) => {
            return `${key}: ${value}`;
        }).join(';')
    }

    get template() {
        return `
            <button style=${this.styles}>

            </button>
        `
    }

    constructor(styles: Styles = {}) {
        this.addEvents = this.addEvents.bind(this);
        this.addStyles = this.addStyles.bind(this);
        this.addStyles(styles);
    }

    addEvents(...events: Function[]): void {
        this._events = [...this._events, ...events];
    }

    addStyles(styles: Partial<Styles>): void {
        this._styles = {
            ...this._styles,
            ...styles
        };
    }
}

interface Builder { // abstract builder
    create(): void;
    applySize(): void;
    applyTheme(): void;
    applyEvents(): void;
    get(): UiElement;
}

class Button extends UiElement { // Concrete product
    constructor() {
        super({
            border: 'none',
            outline: 'none'
        });
    }
}

class ButtonBuilder implements Builder { // Concrete builder
    private _button: Button;

    create(): void {
        this._button = new Button();
    }

    applySize(): void {
        this._button.addStyles({
            height: '100px',
            width: '100px'
        })
    }

    applyTheme(): void {
        this._button.addStyles({
            color: '#fff',
            background: '#333ccc'
        });
    }

    applyEvents(): void {
        this._button.addEvents(
            () => {
                console.log('Mounted')
            }
        );
    }

    get(): Button {
        return this._button;
    }
}

class ComponentsLibrary { // Director
    constructor(private _builder: Builder) { }

    buildStaticButton(): Button {
        this._builder.create();
        this._builder.applySize();

        return this._builder.get()
    }

    buildPrimaryButton(): Button {
        this._builder.create();
        this._builder.applySize();
        this._builder.applyTheme();

        return this._builder.get()
    }
}
```

### Factory method

Used in applications that manage, maintain, or manipulate collections of objects that are different but at the same time have many characteristics (methods and properties) in common. An example would be a collection of documents with a mix of **Xml documents, Pdf documents, and Rtf documents**.

![FactoryMethod](https://www.dofactory.com/img/diagrams/javascript/javascript-factory-method.jpg)

```ts
interface Doc { // Abstract product
  ext: string;
  toString(): string;
}

enum DocType {
  PDF = "pdf",
  WORD = "word",
}

class WordDoc implements Doc { // Concrete product
  ext = "doc";

  toString(): string {
    return "";
  }
}

class PdfDoc implements Doc { // Concrete product
  ext = "pdf";

  toString(): string {
    return "";
  }
}

class DocCreator { // Creator
  create(type: DocType): Doc {
    if (type === DocType.WORD) {
      return new WordDoc();
    } else if (type === DocType.PDF) {
      return new PdfDoc();
    }

    throw new Error('No document for given type');
  }
}
```

### Prototype

Used to create objects which needs some pre-defined values or to avoid boilerplate when we would like to have exact object.

![Prototype](https://www.dofactory.com/img/diagrams/javascript/javascript-prototype.jpg)

```ts
interface Cloneable<T> {
    clone(): T;
}

class Data<T> {
    constructor(public value: T, public isLoading = false, public error = '') {}
}

class DataPrototype implements Cloneable<Data<number>> {
    constructor(public proto: Data<number>) {}

    clone(): Data<number> {
        const data = new Data(
            this.proto.value,
            this.proto.isLoading,
            this.proto.error
        );

        return data;
    }
}

const dataProto = new Data(1, false, '');
const data = new DataPrototype(dataProto); // some code saved - no need to pass all constructor props

console.log(data.clone())
```

### Singleton

Limits the number of instances of a particular object to just one. This single instance is called the **singleton**.

![Singleton](https://www.dofactory.com/img/diagrams/javascript/javascript-singleton.jpg)

```js
class User {
    constructor() {}
}

class AuthProvider {
    private _user: User;

    get user(): User {
        if (this._user) {
            return this._user;
        }

        const user = new User();
        this._user = user;

        return user;
    }
}

const provider = new AuthProvider();
```

## Structural patterns

### Adapter / Wrapper

Translates one interface (an object's properties and methods) to another. Adapters allows programming components to work together that otherwise wouldn't because of mismatched interfaces. Maps old interface to new one without any changes to implementation.

![Adapter](https://www.dofactory.com/img/diagrams/javascript/javascript-adapter.jpg)

```ts
class OldImplementation {
  name: string;
  email: string;
  phone: number;

  request(name: string, email: string, phone: number): void {
    this.name = name;
    console.log(this.name);
    this.email = email;
    console.log(this.email);
    this.phone = phone;
    console.log(this.phone);
  }
}

class NewImplementation {
  name: string;
  email: string;
  phone: number;

  setName(name: string): void {
    this.name = name;
    console.log(this.name);
  }

  setEmail(email: string): void {
    this.email = email;
    console.log(this.email);
  }

  setPhone(phone: number): void {
    this.phone = phone;
    console.log(this.phone);
  }
}

class Adapter {
  request(name: string, email: string, phone: number): void {
    const newOkImplementation = new NewImplementation();

    newOkImplementation.setName(name);
    newOkImplementation.setEmail(email);
    newOkImplementation.setPhone(phone);
  }
}

new OldImplementation().request('Piotr', 'piotr@wp.pl', 112333223); // OLD IMPLEMENTATION
new Adapter().request('Piotr', 'piotr@wp.pl', 112333223); // NEW IMPLEMENTATION
```

### Bridge / Double adapter

Allows two components, a client and a service, to work together with each component having its own interface. Bridge is a high-level architectural pattern and its main goal is to write better code through two levels of abstraction. It facilitates very loose coupling of objects.

![Bridge](https://www.dofactory.com/img/diagrams/javascript/javascript-bridge.jpg)

```ts
type ControllerEvents = Partial<{
  onClick(): void;
  onHover(): void;
  onMouseLeave(): void;
}>;

interface Controller { // Abstraction
  events: Partial<ControllerEvents>;
}

class UserController implements Controller { // RefinedAbstraction 
  constructor(private _service: Service) {}

  events: ControllerEvents = {
    onClick: () => {
      this._service.get();
    },
    onHover: () => {
      this._service.post();
    }
  };
}

class PostController implements Controller { // RefinedAbstraction 
  constructor(private _service: Service) {}

  events: ControllerEvents = {
    onMouseLeave: () => {
      this._service.get();
    },
    onHover: () => {
      this._service.post();
    }
  };
}

interface Service { // Implementor 
  get(): void;
  post(): void;
}

class UserService implements Service { // ConcreteImplementor 
  get(): void {
    console.log('Get implementation');
  }

  post(): void {
    console.log('Post implementation');
  }
}

// Client

const userService = new UserService();
const userController = new UserController(userService);
const postController = new PostController(userService);

userController.events.onClick();
userController.events.onHover();
postController.events.onMouseLeave();
```

### Composite

Each item in the collection can hold other collections themselves, creating deeply nested structures. A tree control is a perfect example of a Composite pattern. The nodes of the tree either contain an individual object (leaf node) or a group of objects (a subtree of nodes).

![Composite](https://www.dofactory.com/img/diagrams/javascript/javascript-composite.jpg)

```ts
class Component {
  children: Component[] = [];

  get level(): number {
    const getLevel = (component: Component | null, level: number): number => {
      if (component !== null) {
        return getLevel(component.parent, level + 1);
      }

      return level;
    };

    return getLevel(this.parent, 0);
  }

  constructor(public name: string, public parent: Component | null = null) {
    this._traverseForLevel = this._traverseForLevel.bind(this);
    this.addChildren = this.addChildren.bind(this);
  }

  private _traverseForLevel(
    component: Component | null,
    level: number
  ): number {
    if (component !== null) {
      return this._traverseForLevel(component.parent, level + 1);
    }

    return level;
  }

  addChildren(children: Component): void {
    children.parent = this;
    this.children.push(children);
  }

  removeChildren(name: string): void {
    this.children = this.children.filter(c => c.name !== name);
  }

  findChildren(name: string): Component | null {
    const getChildren = (children: Component[]): Component | null => {
      const foundChildren = children.find(c => c.name === name);

      if (foundChildren) {
        return foundChildren;
      }

      for(let i = 0; i < children.length; i++) {
        return getChildren(children[i].children);
      }
    };

    return getChildren(this.children)
  }

  findParent(name: string): Component | null {
    const getParent = (component: Component | null): Component | null => {
      if (component === null) {
        return null;
      }

      if (component.name === name) {
        return component;
      }

      return getParent(component.parent);
    };

    return getParent(this.parent);
  }
}

const root = new Component("root");
const leaf = new Component("leaf");
leaf.addChildren(new Component("leaf1"));

root.addChildren(leaf);
root.addChildren(leaf);

console.log(root.children[0].level);
console.log(root.children[0].children[0].level);

console.log(root.children[0].children[0].findParent('root').name);

console.log(root.findChildren('leaf1').name);
```

### Module pattern

Only single object created which exposing public API's.

```js
// Define IIFE module.
const iifeCounterModule = (() => {
    let count = 0;
    return {
        increase: () => ++count,
        reset: () => {
            count = 0;
            console.log("Count is reset.");
        }
    };
})();
// Use IIFE module.
iifeCounterModule.increase();
iifeCounterModule.reset();

// Define IIFE module with dependencies.
const iifeCounterModule = ((dependencyModule1, dependencyModule2) => {
    let count = 0;
    return {
        increase: () => ++count,
        reset: () => {
            count = 0;
            console.log("Count is reset.");
        }
    };
})(dependencyModule1, dependencyModule2);
```

#### Reavealing module pattern

Same as **module pattern** with one difference - public API's are assigned into variables.

```js
// Define revealing module.
const revealingCounterModule = (() => {
    let count = 0;
    const increase = () => ++count;
    const reset = () => {
        count = 0;
        console.log("Count is reset.");
    };

    return {
        increase,
        reset
    };
})();

// Use revealing module.
revealingCounterModule.increase();
revealingCounterModule.reset();
```

## Behavioral patterns

# React

#### `VirtualDOM`

#### `context` and `Context API`

#### `Lifecycle hooks`

#### `Lifecycle methods`

#### `key` prop

#### `Element` vs `Component`

#### `diffing` algorythm

#### `Synthetic event`

#### `Performance improvements`

#### `Event handling`

#### `Why components must be capitalized`

#### `JSX`

#### `createElement`

#### `cloneElement`

#### `ErrorBoundaries`

#### `ForwardRef`

#### `useRef`

#### `createRef`

#### `strict mode` in React

#### `forceUpdate`

# React

## Event pooling

Performance improvement used by React before version 17. Properties of the event only exist while the callback is active. Adding async to the mix, or storing the event for future use, will fail.

```js
class MyComponent extends React.Component {
  handleClick(e) {
    console.log("The event currentTarget is", e.currentTarget); // DOM element
    setTimeout(() => {
      console.log("event.currentTarget was", e.currentTarget); // null
    }, 1000);
  }
  render() {
    return <button onClick={this.handleClick}>Fire event!</button>;
  }
}
```

## Refs

Refs provide a way to access DOM nodes or React elements created in the render method. In the typical React dataflow, props are the only way that parent components interact with their `children`. To modify a child, you re-render it with new `props`. However, there are a few cases where you need to imperatively modify a child outside of the typical dataflow.

Use in:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

## `SyntheticEvent`

Cross-browser wrapper class around browser's native event. Normalizes events so that they have consistent properties across different browsers.

> If you need underlying browser event - use `nativeEvent` property.

#### `HOC`

#### `Render prop`

#### `Compound component`

#### `Function as a child`

#### `Render slot`

#### `Proxy component`

#### `redux`

#### `redux-thunk`

#### `redux-observable`

#### `redux-saga`

git rebase 
git cherry pick
git merge strategies

Repeat if Needed

Nulish operator
Op chaining

		bx model

rem em	
reedux
react
big int

koolejka itp

cqrs

react angular

di

shallow equal vs depth

