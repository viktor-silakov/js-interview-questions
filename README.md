# JS Interview questions and answers

### 1. How to determine if an object is a Promise?

**Answer:**
You can determine if an object is a Promise by checking if it has a `then` method, which is characteristic of Promises. Additionally, using `instanceof Promise` can also be effective, though it may not work across different execution contexts.

**Example:**
```javascript
function isPromise(obj) {
  return !!obj && (typeof obj === 'object' || typeof obj === 'function') && typeof obj.then === 'function';
}

// Usage
const promise = new Promise((resolve) => resolve());
console.log(isPromise(promise)); // true

const notPromise = { then: 'not a function' };
console.log(isPromise(notPromise)); // false
```

---

### 2. Explain the difference between `var`, `let`, and `const`.

**Answer:**
- **`var`**:
  - Function-scoped.
  - Hoisted with initialization as `undefined`.
  - Can be redeclared and updated.
  
- **`let`**:
  - Block-scoped.
  - Hoisted but not initialized (`Temporal Dead Zone`).
  - Cannot be redeclared in the same scope but can be updated.
  
- **`const`**:
  - Block-scoped.
  - Hoisted but not initialized (`Temporal Dead Zone`).
  - Cannot be redeclared or updated. For objects, properties can be modified.

**Example:**
```javascript
function testVar() {
  var x = 1;
  if (true) {
    var x = 2; // Same variable
    console.log(x); // 2
  }
  console.log(x); // 2
}

function testLet() {
  let y = 1;
  if (true) {
    let y = 2; // Different variable
    console.log(y); // 2
  }
  console.log(y); // 1
}

testVar();
testLet();
```

---

### 3. What is a closure and how does it work?

**Answer:**
A closure is a feature in JavaScript where an inner function has access to the outer (enclosing) function's variables even after the outer function has returned. Closures allow functions to retain access to their lexical scope.

**Example:**
```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

---

### 4. How does the context `this` work in JavaScript?

**Answer:**
The value of `this` in JavaScript is determined by how a function is called:
- **Global Context**: In non-strict mode, `this` refers to the global object (`window` in browsers). In strict mode, it is `undefined`.
- **Object Method**: `this` refers to the object owning the method.
- **Constructor**: When using `new`, `this` refers to the newly created instance.
- **Explicit Binding**: Methods like `call`, `apply`, and `bind` can set `this` explicitly.
- **Arrow Functions**: They do not have their own `this` and inherit it from the surrounding lexical scope.

**Example:**
```javascript
const obj = {
  name: 'Alice',
  greet: function() {
    console.log(this.name);
  }
};

obj.greet(); // 'Alice'

const greet = obj.greet;
greet(); // In non-strict mode: undefined or global name if exists; in strict mode: undefined

const boundGreet = obj.greet.bind(obj);
boundGreet(); // 'Alice'

const arrowGreet = () => console.log(this.name);
arrowGreet(); // Inherits `this` from surrounding scope
```

---

### 5. What is hoisting?

**Answer:**
Hoisting is JavaScript's default behavior of moving declarations to the top of the current scope before code execution. This applies to variable and function declarations. However, only the declarations are hoisted, not the initializations.

**Example:**
```javascript
console.log(a); // undefined
var a = 5;

hoisted as:
var a;
console.log(a); // undefined
a = 5;

function foo() {
  console.log('Hello');
}
foo(); // 'Hello'
```

---

### 6. Explain the event loop and task queues.

**Answer:**
The event loop is a mechanism that manages the execution of multiple chunks of your program over time, handling asynchronous operations. It works with two main queues:
- **Call Stack**: Where the currently executing functions are placed.
- **Task Queues**:
  - **Macrotask Queue**: Includes tasks like `setTimeout`, `setInterval`, and I/O operations.
  - **Microtask Queue**: Includes tasks like Promises (`.then`, `.catch`) and `MutationObserver`.
  
The event loop continuously checks if the call stack is empty. If it is, it first processes all microtasks, then proceeds to the next macrotask.

**Example:**
```javascript
console.log('Start');

setTimeout(() => console.log('Timeout'), 0);

Promise.resolve().then(() => console.log('Promise'));

console.log('End');

// Output:
// Start
// End
// Promise
// Timeout
```

---

### 7. How to implement inheritance in JavaScript?

**Answer:**
Inheritance in JavaScript can be implemented using:
- **Prototypal Inheritance**: Using `Object.create` to set up the prototype chain.
- **ES6 Classes**: Using the `class` and `extends` keywords.
- **Constructor Functions**: Using function constructors and setting the prototype.

**Example with ES6 Classes:**
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog('Rex');
dog.speak(); // 'Rex barks.'
```

---

### 8. What is prototype inheritance?

**Answer:**
Prototype inheritance is a feature in JavaScript where objects inherit properties and methods from other objects through the prototype chain. Each object has a `[[Prototype]]` (accessible via `__proto__` or `Object.getPrototypeOf`) that points to its prototype.

**Example:**
```javascript
const parent = {
  greet() {
    console.log('Hello');
  }
};

const child = Object.create(parent);
child.greet(); // 'Hello'

child.greet = function() {
  console.log('Hi');
};

child.greet(); // 'Hi'
parent.greet(); // 'Hello'
```

---

### 9. Explain the difference between `==` and `===`.

**Answer:**
- **`==` (Loose Equality)**: Compares values after performing type coercion. It checks for value equality but allows different types to be considered equal.
- **`===` (Strict Equality)**: Compares both value and type without performing type coercion. Both operands must be of the same type and value to be considered equal.

**Example:**
```javascript
console.log(0 == '0'); // true
console.log(0 === '0'); // false

console.log(null == undefined); // true
console.log(null === undefined); // false
```

---

### 10. How does asynchrony work in JavaScript?

**Answer:**
JavaScript handles asynchronous operations using:
- **Callbacks**: Functions passed as arguments to be executed after an operation completes.
- **Promises**: Objects representing the eventual completion or failure of an asynchronous operation.
- **Async/Await**: Syntactic sugar over Promises, allowing asynchronous code to be written in a synchronous manner.

Asynchronous operations do not block the main thread, allowing other code to execute while waiting for operations like I/O, network requests, or timers to complete.

**Example with Promises:**
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

**Example with Async/Await:**
```javascript
async function getData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

getData();
```

---

### 11. What is IIFE?

**Answer:**
IIFE stands for Immediately Invoked Function Expression. It's a function that is defined and executed immediately after creation. IIFEs are used to create a new scope, encapsulating variables and avoiding polluting the global namespace.

**Example:**
```javascript
(function() {
  var x = 10;
  console.log(x); // 10
})();

// x is not accessible here
console.log(typeof x); // 'undefined'
```

---

### 12. How do generators work in JavaScript?

**Answer:**
Generators are special functions that can be paused and resumed, allowing for iterative or asynchronous programming patterns. They are defined using the `function*` syntax and use the `yield` keyword to pause execution.

**Example:**
```javascript
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
console.log(gen.next().done);  // true
```

---

### 13. Explain the difference between `call`, `apply`, and `bind`.

**Answer:**
- **`call`**: Invokes a function with a specified `this` value and arguments provided individually.
- **`apply`**: Invokes a function with a specified `this` value and arguments provided as an array.
- **`bind`**: Returns a new function with a bound `this` value and, optionally, preset initial arguments.

**Example:**
```javascript
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };

greet.call(person, 'Hello', '!'); // 'Hello, Alice!'
greet.apply(person, ['Hi', '!!']); // 'Hi, Alice!!'

const greetAlice = greet.bind(person, 'Hey');
greetAlice('?'); // 'Hey, Alice?'
```

---

### 14. What is memoization?

**Answer:**
Memoization is an optimization technique that caches the results of expensive function calls and returns the cached result when the same inputs occur again. It avoids redundant computations by storing previously computed results.

**Example:**
```javascript
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache[key]) {
      return cache[key];
    }
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}

const factorial = memoize(function(n) {
  return n <= 1 ? 1 : n * factorial(n - 1);
});

console.log(factorial(5)); // 120
console.log(factorial(6)); // 720 (uses cached factorial(5))
```

---

### 15. How to avoid global scope collisions?

**Answer:**
To avoid polluting the global namespace and prevent variable collisions:
- **Use Modules**: Encapsulate code within modules using ES6 `import`/`export`.
- **IIFE (Immediately Invoked Function Expressions)**: Create local scopes.
- **Namespaces**: Use objects to group related functions and variables.
- **Closures**: Encapsulate variables within function scopes.

**Example with IIFE:**
```javascript
(function() {
  var localVar = 'I am local';
  console.log(localVar); // 'I am local'
})();

console.log(typeof localVar); // 'undefined'
```

---

### 16. Explain the different ways to create objects in JavaScript.

**Answer:**
- **Object Literal**:
  ```javascript
  const obj = { key: 'value' };
  ```
- **Constructor Function**:
  ```javascript
  function Person(name) {
    this.name = name;
  }
  const person = new Person('Alice');
  ```
- **ES6 Classes**:
  ```javascript
  class Person {
    constructor(name) {
      this.name = name;
    }
  }
  const person = new Person('Bob');
  ```
- **`Object.create`**:
  ```javascript
  const proto = { greet() { console.log('Hello'); } };
  const obj = Object.create(proto);
  obj.greet(); // 'Hello'
  ```
- **Factory Functions**:
  ```javascript
  function createPerson(name) {
    return { name, greet() { console.log(`Hello, ${name}`); } };
  }
  const person = createPerson('Carol');
  ```

---

### 17. What is "use strict"?

**Answer:**
`"use strict"` is a directive that enables strict mode in JavaScript. Strict mode enforces stricter parsing and error handling, helping to write more secure and optimized code by:
- Preventing the use of undeclared variables.
- Disallowing duplicate parameter names.
- Making `this` undefined in functions that are not methods.
- Eliminating silent errors by throwing exceptions.
- Prohibiting certain syntax likely to be defined in future versions.

**Example:**
```javascript
"use strict";
x = 10; // ReferenceError: x is not defined
```

---

### 18. How does garbage collection work in JavaScript?

**Answer:**
Garbage collection (GC) in JavaScript is an automatic memory management process that frees up memory by removing objects that are no longer reachable or needed. The primary algorithm used is **Mark-and-Sweep**, which:
1. **Mark Phase**: Identifies all reachable objects starting from root references (e.g., global variables).
2. **Sweep Phase**: Collects and frees memory occupied by objects that were not marked.

**Example:**
```javascript
function createObject() {
  const obj = { name: 'Alice' };
  return obj;
}

const myObj = createObject();
// If `myObj` is set to null, the object becomes unreachable and eligible for GC.
myObj = null;
```

---

### 19. What is currying?

**Answer:**
Currying is a functional programming technique that transforms a function with multiple arguments into a sequence of functions, each taking a single argument. It allows partial application of functions and enhances reusability.

**Example:**
```javascript
function add(a) {
  return function(b) {
    return a + b;
  };
}

const add5 = add(5);
console.log(add5(3)); // 8

// Using arrow functions
const multiply = a => b => a * b;
const double = multiply(2);
console.log(double(4)); // 8
```

---

### 20. Explain the principles of modules in JavaScript.

**Answer:**
Modules in JavaScript allow developers to encapsulate code, promoting reusability and maintainability. The core principles include:
- **Encapsulation**: Each module has its own scope, preventing global namespace pollution.
- **Import/Export**: Modules can export functionalities which can be imported by other modules.
- **Dependency Management**: Clearly defines dependencies between modules.
- **Lazy Loading**: Modules can be loaded on-demand, optimizing performance.

**Example:**
**module.js**
```javascript
export function greet(name) {
  return `Hello, ${name}`;
}

export const pi = 3.14;
```

**main.js**
```javascript
import { greet, pi } from './module.js';
console.log(greet('Alice')); // 'Hello, Alice'
console.log(pi); // 3.14
```

---

### 21. How does strict mode work?

**Answer:**
Strict mode can be enabled by adding `"use strict";` at the beginning of a script or function. It changes previously accepted "bad syntax" into real errors, helps catch common coding mistakes, and prevents the use of certain JavaScript features that are considered problematic.

**Benefits:**
- Prevents accidental global variable declarations.
- Throws errors for attempts to delete undeletable properties.
- Disallows duplicate parameter names.
- Makes `this` undefined in functions that are not methods.
- Prevents usage of future reserved keywords.

**Example:**
```javascript
"use strict";
function test() {
  x = 3.14; // ReferenceError: x is not defined
}
test();
```

---

### 22. What is hoisting with functions?

**Answer:**
Function declarations are hoisted entirely, meaning both the function's name and its implementation are moved to the top of their scope during the compilation phase. Function expressions, however, are only hoisted as variables, not the function definition itself.

**Example:**
```javascript
foo(); // 'Hello'

function foo() {
  console.log('Hello');
}

bar(); // TypeError: bar is not a function
var bar = function() {
  console.log('World');
};
```

---

### 23. What is `prototype` in JavaScript?

**Answer:**
In JavaScript, every object has an internal `[[Prototype]]` property that points to another object. This is known as the prototype. Objects inherit properties and methods from their prototype. The `prototype` property is used in constructor functions to define properties and methods that should be shared across all instances.

**Example:**
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log(`Hello, ${this.name}`);
};

const alice = new Person('Alice');
alice.greet(); // 'Hello, Alice'

console.log(alice.__proto__ === Person.prototype); // true
```

---

### 24. How does `bind` work?

**Answer:**
The `bind` method creates a new function that, when called, has its `this` keyword set to the provided value. It can also preset initial arguments. This is useful for ensuring that a function retains the correct `this` context when passed around.

**Example:**
```javascript
const obj = { name: 'Alice' };

function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const boundGreet = greet.bind(obj, 'Hello');
boundGreet('!'); // 'Hello, Alice!'
```

---

### 25. What is shadowing?

**Answer:**
Shadowing occurs when a variable declared within a certain scope (e.g., function or block) has the same name as a variable in an outer scope. The inner variable "shadows" the outer one, making the outer variable inaccessible within the inner scope.

**Example:**
```javascript
let x = 10;

function test() {
  let x = 20; // Shadows the outer x
  console.log(x); // 20
}

test();
console.log(x); // 10
```

---

### 26. Explain destructuring.

**Answer:**
Destructuring is a syntax that allows unpacking values from arrays or properties from objects into distinct variables. It simplifies the extraction of multiple properties or elements in a concise manner.

**Example with Arrays:**
```javascript
const [a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

**Example with Objects:**
```javascript
const { name, age } = { name: 'Alice', age: 25 };
console.log(name); // 'Alice'
console.log(age); // 25
```

---

### 27. What is the spread operator and where can it be used?

**Answer:**
The spread operator (`...`) allows an iterable such as an array or string to be expanded in places where zero or more arguments or elements are expected. It's useful for copying, merging, and passing elements.

**Use Cases:**
- **Copying Arrays:**
  ```javascript
  const arr1 = [1, 2];
  const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]
  ```
- **Merging Arrays:**
  ```javascript
  const merged = [...arr1, ...arr2];
  ```
- **Passing Arguments:**
  ```javascript
  function sum(a, b, c) {
    return a + b + c;
  }
  const numbers = [1, 2, 3];
  console.log(sum(...numbers)); // 6
  ```
- **Copying Objects (Shallow Copy):**
  ```javascript
  const obj1 = { a: 1 };
  const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }
  ```

---

### 28. How does `async/await` work?

**Answer:**
`async/await` is syntactic sugar over Promises, making asynchronous code appear synchronous. An `async` function returns a Promise, and `await` pauses the function execution until the Promise resolves or rejects.

**Example:**
```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

fetchData();
```

**Key Points:**
- Only `async` functions can use `await`.
- `await` can only be used inside `async` functions.
- `await` pauses the execution of the `async` function until the Promise settles.

---

### 29. What is WeakMap and how does it differ from Map?

**Answer:**
`WeakMap` is a collection of key-value pairs where the keys are objects and the values can be arbitrary values. Unlike `Map`, `WeakMap` holds weak references to keys, allowing garbage collection if there are no other references to the key objects.

**Differences:**
- **Key Types**: `WeakMap` keys must be objects; `Map` keys can be of any type.
- **Garbage Collection**: `WeakMap` allows keys to be garbage collected; `Map` holds strong references.
- **Iteration**: `WeakMap` is not iterable; `Map` can be iterated.
- **Size Property**: `WeakMap` does not have a `size` property; `Map` does.

**Example:**
```javascript
const wm = new WeakMap();
let obj = {};
wm.set(obj, 'value');
console.log(wm.get(obj)); // 'value'
obj = null; // The object can now be garbage collected
```

---

### 30. What is Symbol and what is it used for?

**Answer:**
`Symbol` is a primitive data type introduced in ES6, used to create unique identifiers. Symbols are often used as property keys to avoid name collisions and to implement private-like properties.

**Example:**
```javascript
const sym1 = Symbol('description');
const sym2 = Symbol('description');

console.log(sym1 === sym2); // false

const obj = {
  [sym1]: 'value1',
  [sym2]: 'value2'
};

console.log(obj[sym1]); // 'value1'
console.log(obj[sym2]); // 'value2'
```

**Use Cases:**
- **Unique Property Keys**: Prevent accidental property name collisions.
- **Meta-properties**: Implementing custom behavior in libraries or frameworks.
- **Symbol-based Iterators**: Such as `Symbol.iterator` for defining custom iteration.

---

### 31. What are Service Workers?

**Answer:**
Service Workers are scripts that run in the background, separate from the web page, enabling features like offline functionality, push notifications, and background synchronization. They act as a network proxy, intercepting network requests to cache resources and manage network interactions.

**Key Features:**
- **Offline Support**: Cache resources to enable offline access.
- **Push Notifications**: Receive push messages from the server.
- **Background Sync**: Synchronize data when the network is available.
- **Intercept Network Requests**: Control how network requests are handled.

**Example of Registration:**
```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(registration => {
      console.log('Service Worker registered with scope:', registration.scope);
    })
    .catch(error => {
      console.error('Service Worker registration failed:', error);
    });
}
```

---

### 32. What are Web Workers and how to use them?

**Answer:**
Web Workers allow JavaScript to run in background threads, enabling parallel execution of code without blocking the main UI thread. They are useful for performing computationally intensive tasks.

**Usage:**
1. **Create a Worker**:
   ```javascript
   const worker = new Worker('worker.js');
   ```
2. **Send Messages to the Worker**:
   ```javascript
   worker.postMessage('Hello Worker');
   ```
3. **Receive Messages from the Worker**:
   ```javascript
   worker.onmessage = function(event) {
     console.log('Message from worker:', event.data);
   };
   ```
4. **Worker Script (`worker.js`)**:
   ```javascript
   onmessage = function(event) {
     console.log('Message received from main script:', event.data);
     postMessage('Hello Main');
   };
   ```

**Example:**
```javascript
// main.js
const worker = new Worker('worker.js');
worker.postMessage(10);

worker.onmessage = function(event) {
  console.log('Result:', event.data); // Result: 20
};

// worker.js
onmessage = function(event) {
  const result = event.data * 2;
  postMessage(result);
};
```

---

### 33. Explain Shadow DOM.

**Answer:**
Shadow DOM is a part of the Web Components standard that allows developers to create encapsulated and reusable components with their own isolated DOM trees and styles. It prevents styles and scripts from leaking in or out of the component, ensuring that the component's internal structure is hidden from the main document.

**Advantages:**
- **Encapsulation**: Styles and DOM structure are isolated.
- **Reusability**: Create modular and reusable components.
- **Avoids Conflicts**: Prevents style and script collisions with the global scope.

**Example:**
```javascript
const host = document.createElement('div');
const shadow = host.attachShadow({ mode: 'open' });
shadow.innerHTML = `
  <style>
    p { color: blue; }
  </style>
  <p>Shadow DOM content</p>
`;
document.body.appendChild(host);
```

---

### 34. What is a polyfill?

**Answer:**
A polyfill is a piece of code (usually JavaScript) that implements a feature on web browsers that do not natively support it. Polyfills allow developers to use modern JavaScript features while maintaining compatibility with older browsers.

**Example:**
Polyfill for `Array.prototype.includes`:
```javascript
if (!Array.prototype.includes) {
  Array.prototype.includes = function(element, fromIndex) {
    return this.indexOf(element, fromIndex) !== -1;
  };
}
```

**Usage:**
Include polyfills in your project to ensure compatibility:
```html
<script src="https://polyfill.io/v3/polyfill.min.js"></script>
```

---

### 35. How do `import` and `export` work in ES6 modules?

**Answer:**
`import` and `export` are used to share code between different JavaScript files (modules).

- **`export`**: Used to expose functions, objects, or primitives from a module.
- **`import`**: Used to bring in exported functionalities from other modules.

**Example:**
**module.js**
```javascript
export function greet(name) {
  return `Hello, ${name}`;
}

export const pi = 3.14;

export default class Person {
  constructor(name) {
    this.name = name;
  }
}
```

**main.js**
```javascript
import Person, { greet, pi } from './module.js';

const alice = new Person('Alice');
console.log(greet(alice.name)); // 'Hello, Alice'
console.log(pi); // 3.14
```

**Key Points:**
- **Named Exports**: Export multiple values using their names.
- **Default Export**: Export a single default value from a module.
- **Import Syntax**: Use curly braces for named imports and without for default imports.

---

### 36. What is optional chaining?

**Answer:**
Optional chaining (`?.`) is a syntax that allows safe access to nested object properties. If any part of the chain is `null` or `undefined`, it short-circuits and returns `undefined` without throwing an error.

**Example:**
```javascript
const user = {
  profile: {
    name: 'Alice'
  }
};

console.log(user.profile?.name); // 'Alice'
console.log(user.address?.street); // undefined
```

**Use Cases:**
- Accessing deeply nested properties.
- Safely invoking functions.
- Handling cases where some properties might not exist.

---

### 37. Explain the nullish coalescing operator (`??`).

**Answer:**
The nullish coalescing operator (`??`) returns the right-hand side operand when the left-hand side operand is `null` or `undefined`. Otherwise, it returns the left-hand side operand. This differs from the logical OR (`||`) operator, which returns the right-hand side for any falsy value.

**Example:**
```javascript
const a = null;
const b = a ?? 'default';
console.log(b); // 'default'

const c = 0;
const d = c ?? 42;
console.log(d); // 0
```

**Use Cases:**
- Providing default values without overriding falsy values like `0` or `''`.
- Safely assigning variables that might be `null` or `undefined`.

---

### 38. Explain Event Bubbling and Event Capturing.

**Answer:**
Event Bubbling and Event Capturing are two phases of event propagation in the DOM.

- **Event Capturing**:
  - The event starts from the window and traverses down to the target element.
  - Handlers are executed during the descent.
  - Set the third parameter of `addEventListener` to `true` to use capturing.

- **Event Bubbling**:
  - After reaching the target, the event propagates back up to the window.
  - Handlers are executed during the ascent.
  - Default phase for event listeners (`false`).

**Example:**
```javascript
document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent Capturing');
}, true);

document.getElementById('child').addEventListener('click', () => {
  console.log('Child Bubbling');
}, false);

// Clicking on the child element will log:
// 'Parent Capturing'
// 'Child Bubbling'
```

**Use Cases:**
- **Event Delegation**: Leveraging bubbling to handle events at a higher level in the DOM.
- **Specific Event Handling**: Using capturing for certain scenarios where early interception is needed.

---

### 39. How to handle errors in promises?

**Answer:**
Errors in promises can be handled using the `.catch` method or by using `try-catch` blocks within `async/await` functions.

**Example with `.catch`:**
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

**Example with `async/await`:**
```javascript
async function getData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) throw new Error('Network response was not ok');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error);
  }
}

getData();
```

**Key Points:**
- Always handle promise rejections to avoid unhandled promise rejection errors.
- In `async` functions, use `try-catch` to manage errors effectively.

---

### 40. Explain callback hell and how to avoid it.

**Answer:**
**Callback Hell** refers to the situation where multiple nested callbacks make the code hard to read and maintain. It often occurs when dealing with asynchronous operations that depend on each other.

**Example of Callback Hell:**
```javascript
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log(finalResult);
    });
  });
});
```

**Solutions to Avoid Callback Hell:**
- **Promises**: Chain asynchronous operations using `.then` and `.catch`.
- **Async/Await**: Write asynchronous code in a synchronous style.
- **Modularization**: Break down code into smaller, reusable functions.
- **Use of Control Flow Libraries**: Libraries like `async.js` can help manage asynchronous flows.

**Example with Promises:**
```javascript
doSomething()
  .then(result => doSomethingElse(result))
  .then(newResult => doThirdThing(newResult))
  .then(finalResult => console.log(finalResult))
  .catch(error => console.error(error));
```

**Example with Async/Await:**
```javascript
async function process() {
  try {
    const result = await doSomething();
    const newResult = await doSomethingElse(result);
    const finalResult = await doThirdThing(newResult);
    console.log(finalResult);
  } catch (error) {
    console.error(error);
  }
}

process();
```

---

### 41. Explain CORS and how to bypass it.

**Answer:**
**CORS (Cross-Origin Resource Sharing)** is a security mechanism that restricts web pages from making requests to a different domain than the one that served the web page. It prevents malicious websites from accessing sensitive data on another site.

**How CORS Works:**
- The browser sends an `Origin` header with the request.
- The server responds with appropriate `Access-Control-Allow-Origin` headers.
- If the response does not include the correct headers, the browser blocks the request.

**Ways to Bypass CORS:**
- **Server-Side Proxy**: Create a proxy on your server that forwards requests to the target server.
- **CORS Headers**: Configure the target server to include appropriate CORS headers.
- **JSONP**: (Deprecated) Use `<script>` tags to fetch data. Only supports GET requests.
- **Browser Extensions**: Use extensions that disable CORS (not recommended for production).

**Example of Server-Side Proxy:**
```javascript
// Express.js example
const express = require('express');
const request = require('request');
const app = express();

app.get('/proxy', (req, res) => {
  const url = 'https://api.example.com/data';
  request(url).pipe(res);
});

app.listen(3000);
```

**Client-Side Request:**
```javascript
fetch('/proxy')
  .then(response => response.json())
  .then(data => console.log(data));
```

---

### 42. What is GraphQL and how does it differ from REST?

**Answer:**
**GraphQL** is a query language for APIs and a runtime for executing those queries with your existing data. It allows clients to request exactly the data they need, reducing over-fetching and under-fetching issues common with REST APIs.

**Differences from REST:**
- **Flexible Queries**: Clients can specify the exact data structure they need.
- **Single Endpoint**: GraphQL typically uses a single endpoint, unlike REST which uses multiple endpoints.
- **Strong Typing**: GraphQL APIs are strongly typed, providing a clear schema.
- **Real-time Updates**: Supports subscriptions for real-time data.

**Example of a GraphQL Query:**
```graphql
{
  user(id: "1") {
    name
    email
    posts {
      title
    }
  }
}
```

**Comparison with REST:**
- **REST**: Multiple endpoints (e.g., `/users/1`, `/users/1/posts`).
- **GraphQL**: Single endpoint with nested queries.

**Use Cases:**
- Applications requiring complex data retrieval.
- Scenarios where API clients need flexibility in data fetching.

---

### 43. How to cancel a fetch request?

**Answer:**
To cancel a `fetch` request, use the `AbortController` API. It allows you to abort one or more DOM requests as and when desired.

**Example:**
```javascript
const controller = new AbortController();
const signal = controller.signal;

fetch('https://api.example.com/data', { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => {
    if (error.name === 'AbortError') {
      console.log('Fetch aborted');
    } else {
      console.error('Fetch error:', error);
    }
  });

// To abort the fetch
controller.abort();
```

**Key Points:**
- Create an instance of `AbortController`.
- Pass `controller.signal` to the fetch options.
- Call `controller.abort()` to cancel the request.
- Handle the `AbortError` in the catch block.

---

### 44. What is Tail Call Optimization?

**Answer:**
Tail Call Optimization (TCO) is a feature in some programming languages, including JavaScript (in strict mode), that optimizes recursive function calls by reusing the current function's stack frame for the next function call if it's in the tail position. This prevents stack overflow errors and allows for infinite recursion.

**Example:**
```javascript
"use strict";

function factorial(n, acc = 1) {
  if (n <= 1) return acc;
  return factorial(n - 1, n * acc); // Tail call
}

console.log(factorial(5)); // 120
```

**Notes:**
- TCO is only supported in strict mode.
- Not all JavaScript engines implement TCO yet.
- Ensures that recursive functions can run efficiently without increasing the call stack.

---

### 45. What is BOM (Browser Object Model)?

**Answer:**
The Browser Object Model (BOM) is a set of JavaScript objects that provide interaction with the browser environment outside of the web page's DOM. It includes objects like `window`, `navigator`, `screen`, `location`, `history`, and `alert`.

**Key Components:**
- **`window`**: The global object representing the browser window.
- **`navigator`**: Information about the browser and user agent.
- **`screen`**: Information about the user's screen.
- **`location`**: Represents the current URL and provides methods to navigate.
- **`history`**: Allows navigation through the session history.
- **`alert`, `prompt`, `confirm`**: Dialog boxes for user interaction.

**Example:**
```javascript
console.log(window.innerWidth); // Width of the browser window
console.log(navigator.userAgent); // User agent string
console.log(screen.height); // Screen height
console.log(location.href); // Current URL

location.href = 'https://www.example.com'; // Navigate to a new URL
```

---

### 46. How to use `querySelector` and `querySelectorAll`?

**Answer:**
`querySelector` and `querySelectorAll` are DOM methods used to select elements using CSS selectors.

- **`querySelector`**: Returns the first element that matches the selector.
- **`querySelectorAll`**: Returns a static `NodeList` of all elements that match the selector.

**Example:**
```javascript
// Select the first <div> element
const firstDiv = document.querySelector('div');
console.log(firstDiv);

// Select all <div> elements
const allDivs = document.querySelectorAll('div');
allDivs.forEach(div => console.log(div));

// Select an element with a specific ID
const header = document.querySelector('#header');
console.log(header);

// Select elements with a specific class
const buttons = document.querySelectorAll('.btn');
buttons.forEach(button => console.log(button));
```

**Notes:**
- `querySelectorAll` returns a static `NodeList`, not live.
- Supports complex selectors, enabling powerful and flexible element selection.

---

### 47. What is the `DOMContentLoaded` event?

**Answer:**
The `DOMContentLoaded` event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. It signifies that the DOM is ready for manipulation.

**Example:**
```javascript
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM fully loaded and parsed');
  // Safe to manipulate the DOM here
});
```

**Use Cases:**
- Initialize scripts that depend on the DOM structure.
- Ensure that elements are available before attaching event listeners or manipulating them.

---

### 48. How to change the style of an element via JavaScript?

**Answer:**
You can change the style of an element using the `style` property or by manipulating CSS classes with `classList`.

**Using the `style` Property:**
```javascript
const element = document.getElementById('myElement');
element.style.backgroundColor = 'blue';
element.style.fontSize = '20px';
```

**Using `classList`:**
```javascript
const element = document.getElementById('myElement');
element.classList.add('active'); // Adds the 'active' class
element.classList.remove('hidden'); // Removes the 'hidden' class
element.classList.toggle('visible'); // Toggles the 'visible' class
```

**Example with CSS Classes:**
```css
/* CSS */
.active {
  background-color: blue;
  font-size: 20px;
}
```

```javascript
// JavaScript
element.classList.add('active');
```

**Notes:**
- Using `classList` is generally preferred for managing multiple style changes and maintaining separation of concerns.
- The `style` property is useful for dynamic or one-off style changes.

---

### 49. How to implement inheritance via prototypes?

**Answer:**
Inheritance via prototypes can be implemented by setting the prototype of a constructor function to an instance of the parent constructor or using `Object.create`.

**Example with Constructor Functions:**
```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  console.log(`${this.name} makes a noise.`);
};

function Dog(name) {
  Animal.call(this, name); // Call parent constructor
}

Dog.prototype = Object.create(Animal.prototype); // Inherit prototype
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  console.log(`${this.name} barks.`);
};

const dog = new Dog('Rex');
dog.speak(); // 'Rex makes a noise.'
dog.bark();  // 'Rex barks.'
```

**Example with `Object.create`:**
```javascript
const parent = {
  greet() {
    console.log('Hello');
  }
};

const child = Object.create(parent);
child.greet(); // 'Hello'
child.sayBye = function() {
  console.log('Bye');
};
child.sayBye(); // 'Bye'
```

**Key Points:**
- Use `Object.create` to set up the prototype chain.
- Ensure the constructor property is correctly set after inheritance.
- Call the parent constructor within the child constructor to inherit properties.

---

### 50. What are ES6 Classes and how do they work?

**Answer:**
ES6 Classes are syntactic sugar over JavaScript's prototypal inheritance, providing a more familiar and cleaner syntax for creating constructor functions and managing inheritance.

**Key Features:**
- **Class Declaration**: Define a class using the `class` keyword.
- **Constructor**: Special method for initializing new instances.
- **Methods**: Define functions within the class.
- **Inheritance**: Use `extends` to create subclasses and `super` to call parent class methods.

**Example:**
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, ${this.name}`);
  }
}

class Student extends Person {
  constructor(name, grade) {
    super(name); // Call parent constructor
    this.grade = grade;
  }

  study() {
    console.log(`${this.name} studies.`);
  }
}

const student = new Student('Alice', 'A');
student.greet(); // 'Hello, Alice'
student.study(); // 'Alice studies.'
```

**Advantages:**
- Cleaner and more intuitive syntax.
- Improved readability and maintainability.
- Facilitates inheritance and method definitions.

**Notes:**
- Under the hood, classes still use prototypes.
- Classes are not hoisted; they must be declared before use.

---

### 51. How does inheritance work via `Object.create`?

**Answer:**
Inheritance via `Object.create` involves creating a new object with its prototype set to the specified object. This method establishes the prototype chain, allowing the new object to inherit properties and methods from its prototype.

**Example:**
```javascript
const parent = {
  greet() {
    console.log('Hello from parent');
  }
};

const child = Object.create(parent);
child.greet(); // 'Hello from parent'
child.sayBye = function() {
  console.log('Bye from child');
};
child.sayBye(); // 'Bye from child'
```

**Key Points:**
- `Object.create` creates a new object with the specified prototype.
- Useful for creating objects that inherit directly from other objects.
- Does not require constructor functions or classes.

---

### 52. What is Encapsulation and how to achieve it in JavaScript?

**Answer:**
Encapsulation is an object-oriented principle that restricts direct access to an object's internal state and allows interaction through well-defined interfaces. It promotes data hiding and protects object integrity.

**Ways to Achieve Encapsulation:**
- **Closures**: Encapsulate private variables within function scopes.
- **Modules**: Use ES6 modules to create private and public members.
- **Private Fields (ES2022)**: Use the `#` syntax to define private properties in classes.

**Example with Closures:**
```javascript
function createCounter() {
  let count = 0; // Private variable

  return {
    increment() {
      count++;
      console.log(count);
    },
    decrement() {
      count--;
      console.log(count);
    }
  };
}

const counter = createCounter();
counter.increment(); // 1
counter.decrement(); // 0
// count is not accessible directly
console.log(counter.count); // undefined
```

**Example with Private Fields:**
```javascript
class Counter {
  #count = 0; // Private field

  increment() {
    this.#count++;
    console.log(this.#count);
  }

  decrement() {
    this.#count--;
    console.log(this.#count);
  }
}

const counter = new Counter();
counter.increment(); // 1
counter.decrement(); // 0
console.log(counter.#count); // SyntaxError
```

---

### 53. What are mixins and how to implement them?

**Answer:**
Mixins are a design pattern that allows objects or classes to borrow methods and properties from other objects or classes without using inheritance. They enable the composition of behaviors from multiple sources.

**Implementation with Object.assign:**
```javascript
const canGreet = {
  greet() {
    console.log('Hello');
  }
};

const canBye = {
  sayBye() {
    console.log('Bye');
  }
};

class Person {}
Object.assign(Person.prototype, canGreet, canBye);

const person = new Person();
person.greet(); // 'Hello'
person.sayBye(); // 'Bye'
```

**Advantages:**
- Promote code reuse without deep inheritance hierarchies.
- Allow combining multiple behaviors in a single class or object.
- Provide flexibility in composing functionality.

**Notes:**
- Mixins can lead to name collisions if multiple mixins have methods with the same name.
- Care must be taken to manage dependencies and method overriding.

---

### 54. How to implement the singleton pattern in JavaScript?

**Answer:**
The singleton pattern ensures that a class has only one instance and provides a global point of access to it. This can be implemented using closures or static properties.

**Example with Closure:**
```javascript
const Singleton = (function() {
  let instance;

  function createInstance() {
    return { name: 'Singleton Instance' };
  }

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

const instance1 = Singleton.getInstance();
const instance2 = Singleton.getInstance();
console.log(instance1 === instance2); // true
```

**Example with ES6 Classes:**
```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    this.name = 'Singleton Instance';
    Singleton.instance = this;
  }
}

const instance1 = new Singleton();
const instance2 = new Singleton();
console.log(instance1 === instance2); // true
```

**Key Points:**
- Prevents multiple instances of a class.
- Useful for managing shared resources like configurations or connections.
- Ensures controlled access to the single instance.

---

### 55. How to implement private methods in classes?

**Answer:**
Private methods can be implemented using the `#` syntax introduced in ES2022. These methods are only accessible within the class and cannot be called from outside.

**Example:**
```javascript
class Counter {
  #count = 0; // Private field

  increment() {
    this.#count++;
    this.#log();
  }

  #log() { // Private method
    console.log(`Count is now ${this.#count}`);
  }
}

const counter = new Counter();
counter.increment(); // 'Count is now 1'
counter.#log(); // SyntaxError: Private field '#log' must be declared in an enclosing class
```

**Notes:**
- Private methods cannot be accessed or modified from outside the class.
- They help in encapsulating internal logic and preventing external interference.
- The `#` prefix is mandatory for private fields and methods.

---

### 56. What are getters and setters?

**Answer:**
Getters and setters are special methods in JavaScript classes that allow you to access and mutate private or internal properties in a controlled manner. They provide a way to define computed properties and enforce encapsulation.

**Example:**
```javascript
class Person {
  constructor(name) {
    this._name = name;
  }

  // Getter
  get name() {
    return this._name;
  }

  // Setter
  set name(newName) {
    if (newName.length > 0) {
      this._name = newName;
    } else {
      console.error('Name must be a non-empty string');
    }
  }
}

const person = new Person('Alice');
console.log(person.name); // 'Alice'
person.name = 'Bob';
console.log(person.name); // 'Bob'
person.name = ''; // 'Name must be a non-empty string'
```

**Key Points:**
- **Getters (`get`)**: Accessor methods to retrieve property values.
- **Setters (`set`)**: Mutator methods to set or update property values.
- Enhance encapsulation by controlling how properties are accessed and modified.
- Can include validation or transformation logic.

---

### 57. Explain composition vs inheritance.

**Answer:**
- **Inheritance**:
  - Establishes an "is-a" relationship.
  - Subclasses inherit properties and behaviors from parent classes.
  - Can lead to tight coupling and issues with multiple inheritance.

- **Composition**:
  - Establishes a "has-a" relationship.
  - Combines simple objects or functions to build more complex ones.
  - Promotes flexibility and code reuse without deep inheritance chains.
  - Reduces coupling between components.

**Example of Inheritance:**
```javascript
class Animal {
  speak() {
    console.log('Animal speaks');
  }
}

class Dog extends Animal {
  bark() {
    console.log('Dog barks');
  }
}

const dog = new Dog();
dog.speak(); // 'Animal speaks'
dog.bark();  // 'Dog barks'
```

**Example of Composition:**
```javascript
const canSpeak = {
  speak() {
    console.log('Can speak');
  }
};

const canBark = {
  bark() {
    console.log('Can bark');
  }
};

const dog = Object.assign({}, canSpeak, canBark);
dog.speak(); // 'Can speak'
dog.bark();  // 'Can bark'
```

**Key Points:**
- **Inheritance** is suitable for shared behavior and properties but can be rigid.
- **Composition** offers more flexibility and promotes reusable components.
- Favor composition over inheritance to create more maintainable and modular code.

---

### 58. What is the difference between `Object.create` and class inheritance?

**Answer:**
- **`Object.create`**:
  - Creates a new object with the specified prototype.
  - Provides direct control over the prototype chain.
  - Does not involve constructor functions or classes.
  - Suitable for simple inheritance scenarios.

- **Class Inheritance (ES6 Classes)**:
  - Uses the `class` and `extends` syntax to define classes and subclasses.
  - Provides a clearer and more structured syntax for inheritance.
  - Includes constructor functions, methods, and static properties.
  - More suitable for complex inheritance hierarchies and object-oriented designs.

**Example with `Object.create`:**
```javascript
const parent = {
  greet() {
    console.log('Hello from parent');
  }
};

const child = Object.create(parent);
child.greet(); // 'Hello from parent'
```

**Example with ES6 Classes:**
```javascript
class Parent {
  greet() {
    console.log('Hello from Parent');
  }
}

class Child extends Parent {
  greet() {
    console.log('Hello from Child');
  }
}

const child = new Child();
child.greet(); // 'Hello from Child'
```

**Key Points:**
- `Object.create` is more flexible for creating objects with a specific prototype.
- Classes provide a more formal structure for object-oriented programming.
- Choose based on the complexity and requirements of your inheritance model.

---

### 59. Explain microtasks and macrotasks.

**Answer:**
**Microtasks** and **macrotasks** are types of tasks managed by the event loop, determining the order of execution for asynchronous operations.

- **Microtasks**:
  - Include operations like Promise callbacks (`.then`, `.catch`) and `MutationObserver`.
  - Have higher priority and are executed immediately after the current script completes, before rendering and before any macrotasks.
  
- **Macrotasks**:
  - Include operations like `setTimeout`, `setInterval`, `I/O`, and user interactions.
  - Executed after all microtasks are completed.

**Execution Order:**
1. Execute the current script.
2. Execute all microtasks in the microtask queue.
3. Render updates if needed.
4. Execute the next macrotask from the macrotask queue.
5. Repeat the cycle.

**Example:**
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Macrotask: setTimeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Microtask: Promise');
});

console.log('End');

// Output:
// 'Start'
// 'End'
// 'Microtask: Promise'
// 'Macrotask: setTimeout'
```

**Key Points:**
- Microtasks are processed before macrotasks.
- Promises and `async/await` primarily use microtasks.
- Understanding the distinction helps in optimizing asynchronous code execution.

---

### 60. How to protect against XSS attacks in JavaScript?

**Answer:**
**Cross-Site Scripting (XSS)** attacks involve injecting malicious scripts into web pages. To protect against XSS:
- **Escape User Input**: Sanitize and encode user-generated content before rendering it in the DOM.
- **Use Safe Methods**: Prefer `textContent` over `innerHTML` to insert text without parsing HTML.
- **Content Security Policy (CSP)**: Implement CSP headers to restrict the sources of executable scripts.
- **Validate Input**: Enforce strict validation on both client and server sides.
- **Avoid Inline Scripts**: Refrain from using inline JavaScript which can be exploited.
- **Use Frameworks**: Utilize frameworks that automatically handle XSS protection.

**Example of Safe DOM Manipulation:**
```javascript
const userInput = '<img src=x onerror=alert(1)>';

// Unsafe
element.innerHTML = userInput; // Executes the onerror script

// Safe
element.textContent = userInput; // Renders as text without executing
```

**Implementing CSP:**
```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com
```

**Key Points:**
- Always treat user input as untrusted.
- Combine multiple XSS prevention techniques for robust security.
- Regularly review and update security measures to address new threats.

---

### 61. What is Content Security Policy (CSP)?

**Answer:**
Content Security Policy (CSP) is a security standard that helps prevent cross-site scripting (XSS), clickjacking, and other code injection attacks by specifying allowed sources of content. CSP is implemented via HTTP headers that dictate which resources the browser is permitted to load and execute.

**Example of CSP Header:**
```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://apis.example.com; style-src 'self' 'unsafe-inline'
```

**Directives:**
- **`default-src`**: Default policy for fetching resources.
- **`script-src`**: Allowed sources for JavaScript.
- **`style-src`**: Allowed sources for CSS.
- **`img-src`**: Allowed sources for images.
- **`connect-src`**: Allowed sources for AJAX, WebSockets, etc.
- **`font-src`**, **`media-src`**, etc.: Allowed sources for fonts and media.

**Benefits:**
- Mitigates XSS by restricting script execution.
- Controls where resources can be loaded from.
- Enhances overall security posture of web applications.

**Implementation:**
Set the CSP header on the server side:
```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com
```

**Key Points:**
- CSP should be tailored to the specific needs of the application.
- Use reporting (`report-uri` or `report-to`) to monitor and enforce policies.
- Test CSP policies thoroughly to avoid breaking legitimate functionality.

---

### 62. How to use Intl and internationalization?

**Answer:**
The `Intl` object in JavaScript provides language-sensitive string comparison, number formatting, and date and time formatting. It enables internationalization (i18n) by supporting multiple locales and formatting options.

**Examples:**

- **Number Formatting:**
  ```javascript
  const number = 123456.789;

  const formatter = new Intl.NumberFormat('de-DE', {
    style: 'currency',
    currency: 'EUR',
  });

  console.log(formatter.format(number)); // '123.456,79 €'
  ```

- **Date Formatting:**
  ```javascript
  const date = new Date();

  const formatter = new Intl.DateTimeFormat('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  });

  console.log(formatter.format(date)); // e.g., 'December 17, 1995'
  ```

- **Plural Rules:**
  ```javascript
  const pluralRules = new Intl.PluralRules('en-US');

  console.log(pluralRules.select(1)); // 'one'
  console.log(pluralRules.select(2)); // 'other'
  ```

**Key Points:**
- Supports a wide range of locales and options.
- Helps in building applications that cater to a global audience.
- Provides consistency in formatting across different regions.

---

### 63. What is Tail Call Optimization?

**Answer:**
Tail Call Optimization (TCO) is an optimization technique where the compiler reuses the current function's stack frame for a function call that is in the tail position (the last action in a function). This prevents the growth of the call stack, allowing for efficient recursion without stack overflow.

**Example:**
```javascript
"use strict";

function factorial(n, acc = 1) {
  if (n <= 1) return acc;
  return factorial(n - 1, n * acc); // Tail call
}

console.log(factorial(5)); // 120
```

**Notes:**
- TCO is only supported in strict mode.
- Not all JavaScript engines implement TCO yet.
- Enables writing recursive functions without worrying about stack limits.

---

### 64. Explain the event loop.

**Answer:**
The event loop is a fundamental part of JavaScript's concurrency model. It continuously monitors the call stack and task queues to manage the execution of asynchronous operations.

**Process:**
1. **Call Stack**: Executes synchronous code.
2. **Task Queues**:
   - **Macrotask Queue**: Includes tasks like `setTimeout`, `setInterval`, and I/O operations.
   - **Microtask Queue**: Includes tasks like Promises (`.then`, `.catch`) and `MutationObserver`.
3. **Event Loop Cycle**:
   - If the call stack is empty, the event loop first processes all microtasks.
   - After microtasks, it processes the next macrotask.
   - This cycle repeats indefinitely.

**Example:**
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Macrotask: setTimeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Microtask: Promise');
});

console.log('End');

// Output:
// 'Start'
// 'End'
// 'Microtask: Promise'
// 'Macrotask: setTimeout'
```

**Key Points:**
- Microtasks have higher priority than macrotasks.
- Understanding the event loop helps in managing asynchronous code execution and avoiding common pitfalls.
