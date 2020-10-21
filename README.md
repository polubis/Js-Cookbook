# Standards naming

#### ES2015 a.k.a. ES6
#### ES2016 a.k.a. ES7
#### ES2017 a.k.a. ES8
#### ES2018 a.k.a. ES9
#### ES2019 a.k.a. ES10
#### ES2020 a.k.a. ES11

# Language core concepts

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

#### `Callback`

Function which is passed as argument to other function and executed inside or returned.

```js
const fibb = (calc) => {
    for(let i = 0; i < 10; i++) {
        calc();
    }
}
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

#### Object descriptors

#### Why extending build-in JS objects is bad idea ?

#### `Prototype inheritance`

#### What `transpiling` is ?

#### What `hoisting` is ?

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

#### `const`

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



