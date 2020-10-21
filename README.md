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

#### Script types and loading order

#### `Event loop`

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



