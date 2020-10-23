# Standards naming

#### ES2015 a.k.a. ES6
#### ES2016 a.k.a. ES7
#### ES2017 a.k.a. ES8
#### ES2018 a.k.a. ES9
#### ES2019 a.k.a. ES10
#### ES2020 a.k.a. ES11

# Language core concepts

#### `Function declarations`

Should be used in global scope.

```js
function hoisted() {
  console.log('This function has been hoisted.');
};
```

#### `Function expressions`

FE avoid polluting the global scope. Name can be avoided.
Annonymous functions are invoked and forgotten immediately.
Limits where the function is available, keeps your global scope light.

```js
var expression = function() {
  console.log('Will this work?');
};
```

#### `Callback`

Function which is passed as argument to other function and executed inside or returned.

```js
const fibb = (calc) => {
    for(let i = 0; i < 10; i++) {
        calc();
    }
}
```

#### `Reflection`
  
  Language's ability to inspect and dynamically call classes, methods, attributes, etc. at runtime. If there is an option to do something without reflection - do it without reflection.

  Slower than normal code execution and in some languages requires runtime permissions.

  Allows to access things like private fields which can cause unexpected side effects.

  Key mechanism to allow an application or framework to work with code that might not have even been written yet.

  In JS `Reflect` object can't be created with `new` keyword.

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

#### `<Script>`

Browser process HTML markup from `<head>` to `<body>`. So if we have `<script>` tag between HTML markup - rendering process will be blocked until script is downloaded and executed.

```html
<p>...content before script...</p>

<script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>

<!-- This isn't visible until the script loads -->
<p>...content after script...</p>
```

Workaround for that is - adding script at bottom - whole page will be rendered and after that script will be loaded. 
In this scenario browser renders full html, starts download / execute script.

```html
<body>
  ...all content is above the script...

  <script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>
</body>
```

`defer` attribute allows to load script `in the background` and after full DOM render - execute script. Much faster becauase we rendering HTML and downloading script in the same time.

- document order - as they go in the document
- never block the page
- always execute when the DOM is ready (but before DOMContentLoaded event)
- wait for other scripts to being downloaded - keeps their relative order before execution
```js
// Download starts parallel to improve performance - but small.js must wait for long.js to be downloaded before exection.
<script defer src="https://javascript.info/article/script-async-defer/long.js"></script> 
<script defer src="https://javascript.info/article/script-async-defer/small.js"></script>
```

`async` behaves like `defer` but script is independent. 

- load first order
- browser doesn't block on `async` like `defer`
- other scripts don't wait and `async` scripts don't wait for them
- `DOmContentLoaded` and `async` scripts don't wait for each other
   - `DOMcontentLoaded` may happen before `async` script (if finishes loading after the page is complete)
   - or after `async` script (if script was small or was in HTTP-cache)
- loads `in the background` and runs when ready

`dynamic scripts` create via JavaScript. Starts loading as soon as it’s appended to the document. Behaves as `async` by default.

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

#### `Event loop`

JavaScript `concurrency model` is based on `event loop` algorythm. This model never blocks if all `hard` calculations are performed via `callbacks` as requests or `web workers` usage and if don't use legacay `synchronous` API's like `alert()`.

- `Heap` - manages memory allocation
- `Stack` - handles functions execution order
   - if a function is marked `asynchronous` it will go to the `callback queue`
- `Callback queue` - runs callback only if stack is empty

> `setTimeout()` using setTimeout with a delay of `0` means calling when the stack is empty.

![Event loop](https://miro.medium.com/max/2000/1*m5M4NV495oH4ADvpnItnVQ.png)

#### `Object descriptors`

Metadata added to an object.

`property data descriptors` - object assigned to an object’s property. Dictates how the JavaScript engine will behave regarding that property.

They can be used to dictate how to work with objects.

- `value` - the actual value we want the property to be (defaults undefined)
- `enumerable` - whether the property should show up in operations that enumerate over an object’s keys, such as `for...in` loops or `Object.keys()` (defaults false)
- `configurable` - indicates if we can later change the descriptor settings or be able to delete the property off the object (defaults false)
- `writable` - tells if the value of the property can be changed (defaults false)

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

`property accessor descriptors` - object assigned to object's property. Hiddes implementation details, allows to perform additional operations. For accessor properties, there is no value or writable, but instead there are get and set functions. 

Getters/setters can be used as wrappers over `real` property values to gain more control over operations with them.

- `get` – a function without arguments, that works when a property is read
- `set` – a function with one argument, that is called when the property is set
- `enumerable` – same as for data properties
- `configurable` – same as for data properties

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

If we try to supply both get and value in the same descriptor, there will be an error.

```js
// Error: Invalid property descriptor.
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },

  value: 2
});
```

#### Why extending build-in JS objects is bad idea ?

#### `Prototype inheritance`

#### What `transpiling` is ?

#### `Hoisting`

https://www.digitalocean.com/community/tutorials/understanding-hoisting-in-javascript#:~:text=Hoisting%20is%20a%20JavaScript%20mechanism,scope%20is%20global%20or%20local.

Mechanism where variables and function declarations are moved to the top of their scope(memory) before code execution. 
JavaScript Engine first declaring variables in memory and later initialises them.

> To avoid hoisting problems always - `declare and initialise our variable before use`.

![Hoisting](https://cdn.scotch.io/8976/bNTL1QI3RFebh7C1JPYC_variable%20hoisting.png)

> Undeclared variable is assigned the value undefined at execution and is also of type undefined.
```js
console.log(typeof variable); // Output: undefined
```
> ReferenceError is thrown when trying to access a previously undeclared variable.
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

Function declarations are hoisted over variable declarations but not over variable assignments.

> Variable assignment over function declaration
```js
var double = 22;
function double(num) {
  return (num*2);
}
console.log(typeof double); // Output: number
```

> Function declarations over variable declarations
```js
var double;
function double(num) {
  return (num*2);
}
console.log(typeof double); // Output: function
```

> Class `declarations` are hoisted. However, they remain uninitialised until evaluation.
> Declare a class before you can use it.

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

> Class `expressions` are not hoisted.

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

#### `Currying`

#### `Polyfill`

#### `Event bubbling`

#### What `scope` is ?

#### Explain `values and `types`

#### `same-origin policy` 

#### How to force `strict mode` in Node

#### `Host objects` vs `Native objects`

#### `Coercion`

#### `Memoization`

#### `Annonymous` vs `Named` functions

#### `Temporal Dead Zone`

#### `JSONP`

#### `AJAX`

#### `function Person(){}; var person = Person()` vs `var person = new Person()`

#### `this` keyword

#### `deep-freeze` - how this can be implemented ?

#### `pass by reference` vs `pass by value`

#### How compare 2 objects ?

#### Drawback of creating true `private` in js

#### `undefined` vs `non-defined`

#### `document load` vs `DOMContentLoaded` event

#### `Higher order function`

#### `function foo() {}` vs `var foo = function() {}`

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

#### `==` vs `===`

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

#### `let`

Block scoped variable - not function scoped as `var`.
`let` variables are block hoisted.
Remain uninitialised at the beginning of execution.

```js
console.log(hoist); // Output: ReferenceError: hoist is not defined ...
let hoist = 'The variable has been hoisted.';
```

```js
let hoist;
console.log(hoist); // Output: undefined
hoist = 'Hoisted'
```

#### `const`

Works same as `let`.
Value cannot be modified once assigned.

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

#### `Partial application`

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



