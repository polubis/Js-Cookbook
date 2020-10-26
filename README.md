# Standards naming

#### ES2015 a.k.a. ES6
#### ES2016 a.k.a. ES7
#### ES2017 a.k.a. ES8
#### ES2018 a.k.a. ES9
#### ES2019 a.k.a. ES10
#### ES2020 a.k.a. ES11

## Callback

Function which is passed as argument to other function and executed inside or returned.

```js
const fibb = (calc) => {
    for(let i = 0; i < 10; i++) {
        calc();
    }
}
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

## Primitives

`number`, `string`, `boolean`, `null`, `undefined`, `Symbol` (added in ES6).

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

## Variables

### `let`

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

### `const`

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

## Transpiling

**Source-to-source** compilation, are tools that read source code written in one programming language, and produce the equivalent code in another language. Languages you write that transpile to JavaScript are often called **compile-to-JS languages**, and are said to target JavaScript.

`Babel` transpiler is example of **transpiling** or `React-Native` behaviour which takes JavaScript code and transpiles this code for `Android`, `IOS` languages.

#### Explain `values` and `types`

#### `same-origin policy` 

#### How to force `strict mode` in Node

#### `Host objects` vs `Native objects`

#### `Annonymous` vs `Named` functions

#### `Temporal Dead Zone`

#### `JSONP`

#### `AJAX`

#### `this` keyword

#### `deep-freeze` - how this can be implemented ?

#### `pass by reference` vs `pass by value`

#### How compare 2 objects ?

#### Drawback of creating true `private` in js

#### `undefined` vs `non-defined`

#### `document load` vs `DOMContentLoaded` event

#### `IIFEs`

#### `AMD`

#### `CommonJS`

#### `Closures`

#### `Enums` in pure JS

# `ES5` Language syntax

#### `Promise`

#### `Object.freeze`

#### `call()`

#### `apply()`

#### `bind()`

#### `function constructors`

#### Loose equality `==`

#### Strit equality `===`

#### `load event` - purpose, other solutions 

#### `strict mode` vs `non strict`

#### `typeof` operator

#### `instanceof` operator

#### `new` keyword

#### `var` keyword

#### `undefined` value

#### `null` value

#### `NaN` value

#### `throw Error('msg')` vs `throw new Error('msg')`

# `ES6` Language syntax

#### `spread syntax`

#### `dynamic import`

#### `modules`

#### `rest syntax`

#### `classes`

#### `arrow functions`

#### `symbols`

#### `generators`

#### `WeakMap`

#### `Map`

#### `Set`

#### `WeakSet`

# `ES7` Language syntax

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

# Design patterns

## Module pattern

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

## Reavealing module pattern

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

#### `MVC`

#### `MVVM`

#### `MVP`

#### `Flux`

#### `Prototype`

#### `Module`

#### `Factory`

#### `Dependency injection`

#### `Observer`

#### `Mediator`

#### `Decorator`

#### `Proxy`

#### `Facade`

#### `Flyweight`

# React

#### What React is ?

#### `ref`

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

# React patterns

#### `HOC`

#### `Render prop`

#### `Compound component`

#### `Function as a child`

#### `Render slot`

#### `Proxy component`

# React ecosystem

#### `redux`

#### `redux-thunk`

#### `redux-observable`

#### `redux-saga`



