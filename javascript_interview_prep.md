# The Ultimate FAANG JavaScript & React Interview Prep Guide
## 150 High-Yield Questions & Answers for Python/Java Developers

This guide is designed for developers coming from **Python and Java** who need to prepare for full-stack JavaScript and React interviews. 

---

## 📂 Table of Contents
1. [JavaScript Concept Glossary (Core Architecture & Advanced Terms)](#javascript-concept-glossary-core-architecture--advanced-terms)
2. [Category 1: JavaScript Fundamentals (1-25)](#category-1-javascript-fundamentals-1-25)
3. [Category 2: Scope, Hoisting, Closures & `this` (26-50)](#category-2-scope-hoisting-closures--this-26-50)
4. [Category 3: Objects, Prototypes & OOP (51-75)](#category-3-objects-prototypes--oop-51-75)
5. [Category 4: Async JS, Event Loop & Promises (76-100)](#category-4-async-js-event-loop--promises-76-100)
6. [Category 5: ES6+, Modules, Browser APIs & Performance (101-125)](#category-5-es6-modules-browser-apis--performance-101-125)
7. [Category 6: Advanced & FAANG Trick Questions + Final Cheat Sheet (126-150)](#category-6-advanced--faang-trick-questions--final-cheat-sheet-126-150)

---

## JavaScript Concept Glossary (Core Architecture & Advanced Terms)

Use this glossary to study basic, intermediate, and compiler-level terms often tested in senior or FAANG interviews:

*   **V8 Engine:** Chrome and Node.js's open-source JavaScript execution engine. It compiles JavaScript code directly into native machine code at runtime instead of interpreting bytecode.
*   **JIT (Just-In-Time) Compilation:** The compilation model used by modern JS engines. V8 compiles code while executing it, using an **Interpreter (Ignition)** to run code quickly and a **JIT Compiler (TurboFan)** to optimize hot/frequently run functions into fast machine code.
*   **Hidden Classes / Shapes:** An optimization V8 uses. Since JS objects are dynamic, V8 assigns hidden classes ("Shapes") to objects behind the scenes. Objects with the exact same properties added in the exact same order share the same shape, which allows the engine to optimize property lookup offsets.
*   **Inline Caching (IC):** An optimization built on shapes. V8 caches the memory offsets of object property lookups. If a function is consistently called with objects sharing the same Shape, V8 bypasses property lookups and loads the property directly from the cache.
*   **Memory Stack vs. Memory Heap:**
    *   **Stack:** Small, organized, fast memory space. Stores primitive values and reference pointers. Managed automatically (LIFO).
    *   **Heap:** Large, unstructured, slower memory space. Stores dynamic data like Objects, Arrays, and Functions. Managed by the Garbage Collector.
*   **Garbage Collection (Mark-and-Sweep):** The memory-management algorithm in JS. The engine starts at "roots" (global variables), recursively marks all reachable objects in memory, and then sweeps away (releases) all unmarked, unreachable objects.
*   **`globalThis`:** A unified global variable identifier introduced in ES2020. It provides a standard way to access the global object across any environment (resolves to `window` in browsers, `global` in Node.js, and `self` in Web Workers).
*   **`Object.is()`:** A value comparison method. Unlike strict equality (`===`), `Object.is()` handles floating-point signs and invalid numbers correctly: `Object.is(NaN, NaN)` returns `true` (whereas `NaN === NaN` is `false`), and `Object.is(0, -0)` returns `false` (whereas `0 === -0` is `true`).
*   **Arity:** The number of parameters a function expects. Accessible via the `.length` property of a function declaration (e.g., `function add(a, b) {}` has an arity of 2).
*   **Parameters vs. Arguments:**
    *   **Parameters:** Variables declared in the function signature (e.g., `x`, `y` in `function add(x, y)`).
    *   **Arguments:** The actual values passed to the function when it is called (e.g., `5`, `10` in `add(5, 10)`).
*   **Prototype Pollution:** A security vulnerability occurring in prototype-based languages when an attacker injects properties into `Object.prototype`, causing them to inherit automatically across all objects.
*   **Layout Thrashing (Forced Synchronous Layout):** A browser performance bug that occurs when you write to the DOM (triggering a layout reflow) and immediately read a layout property (forcing the browser to recalculate the layout immediately before the frame is finished).
*   **Strict Mode (`"use strict"`):** A configuration that restricts bad JS design choices: throws errors on global variable leaks (`x = 5`), prevents deleting unconfigurable properties, blocks duplicate function parameter names, and sets default `this` to `undefined`.
*   **JavaScript Module History:**
    *   **IIFE (Immediate Invoked Function Expression):** Encapsulated private scope prior to module standards.
    *   **AMD (Asynchronous Module Definition):** Browser-first module loading standard (e.g., RequireJS).
    *   **CommonJS (CJS):** Node.js standard using `require()` and `module.exports` (synchronous execution).
    *   **UMD (Universal Module Definition):** Wrapper code pattern that automatically detects whether to use CJS or AMD.
    *   **ES Modules (ESM):** Modern language standard using `import`/`export` (asynchronous execution, supports static analysis).
*   **Iterable Protocol:** An object interface that allows JS to iterate over its values using loops like `for...of`. The object must have a method with key `[Symbol.iterator]` that returns an iterator.
*   **Iterator Protocol:** An object interface defining how to produce a sequence of values. It must contain a `.next()` method returning `{ value: nextValue, done: boolean }`.

---

## Category 1: JavaScript Fundamentals (1-25)

#### 1. What are the primitive vs. non-primitive types in JS?
*   **Primitives:** `Number`, `String`, `Boolean`, `Undefined`, `Null`, `Symbol`, `BigInt` (immutable, passed by value).
*   **Non-Primitives:** `Object` (including Arrays, Functions, Dates) (mutable, passed by reference).
*   *Java/Python Parallel:* Similar to primitives vs. wrappers/objects in Java.

#### 2. Explain Dynamic vs. Static typing (JS vs. Java).
JS is dynamically typed; variable types are checked at runtime, and a variable can change its type. Java is statically typed; types are declared and checked at compile time.
```javascript
let x = 5;
x = "hello"; // Valid in JS
```

#### 3. What is type coercion (Implicit vs. Explicit)?
*   **Implicit:** The JS engine automatically converts types (e.g., `5 + "5" = "55"`).
*   **Explicit:** The developer manually casts types (e.g., `Number("5")`).

#### 4. What is the difference between `==` and `===`?
*   `==` (Loose equality): Converts types before comparing (coercion). E.g., `1 == true` is `true`.
*   `===` (Strict equality): Compares both value and type. E.g., `1 === true` is `false`.

#### 5. Explain `null` vs. `undefined`.
*   `undefined`: The variable has been declared but has no assigned value.
*   `null`: An assigned value representing the intentional absence of an object.
*   *Python Parallel:* `None`.

#### 6. What are the falsy values in JS?
`false`, `0`, `-0`, `0n` (BigInt), `""`, `null`, `undefined`, and `NaN`. Everything else is **truthy**, including `[]` and `{}`.
> [!WARNING]
> In Python, empty lists/dicts are falsy. In JS, they are truthy!

#### 7. What is `NaN`? How do you test for it?
`NaN` (Not-a-Number) is a special numeric value indicating an invalid math operation.
*   Use `Number.isNaN(val)` to test. Do not use `val === NaN` because `NaN === NaN` is false!

#### 8. What are block, function, and global scopes?
*   **Global:** Accessible anywhere in the application.
*   **Function:** Accessible only inside the declaring function (`var`, `let`, `const`).
*   **Block:** Accessible only inside curly braces `{}` (`let`, `const` only).

#### 9. Compare `var`, `let`, and `const`.
| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Scope** | Function | Block | Block |
| **Hoisting** | Yes (initialized to `undefined`) | Yes (Temporal Dead Zone) | Yes (Temporal Dead Zone) |
| **Reassignable** | Yes | Yes | No |

#### 10. What is the Temporal Dead Zone (TDZ)?
The period between a block entering scope and a `let`/`const` variable being declared, during which accessing the variable causes a `ReferenceError`.

#### 11. How does variable hoisting differ from function hoisting?
*   Variables declared with `var` are hoisted and assigned `undefined`.
*   Function declarations are hoisted in their entirety (can be called before written).
*   Variable declarations with `let`/`const` are hoisted but uninitialized (TDZ).

#### 12. Explain short-circuiting in `&&` and `||`.
*   `&&` returns the first falsy value, or the last value if all are truthy.
*   `||` returns the first truthy value, or the last value if all are falsy.
```javascript
const name = "" || "Guest"; // "Guest"
```

#### 13. What is the Nullish Coalescing Operator (`??`)?
It returns its right-hand operand when its left-hand operand is `null` or `undefined`. Unlike `||`, it does not trigger for other falsy values (like `0` or `""`).
```javascript
const count = 0 ?? 10; // returns 0
const countOr = 0 || 10; // returns 10
```

#### 14. What is Optional Chaining (`?.`)?
It reads nested property values without throwing an error if an intermediate reference is nullish (`null` or `undefined`).
```javascript
const street = user?.address?.street; // Returns undefined instead of throwing error
```

#### 15. What are template literals?
Backtick (`` ` ``) strings allowing multi-line strings and interpolation using `${expression}`.

#### 16. Compare mutable vs. immutable array methods.
*   **Mutable (mutates original array):** `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `reverse()`, `sort()`.
*   **Immutable (returns new array):** `concat()`, `slice()`, `map()`, `filter()`, spread operator `[...]`.

#### 17. Explain reference types vs. value types.
*   **Value types (Primitives):** Values are stored directly on the stack.
*   **Reference types (Objects):** The variable holds a pointer to a location on the heap.

#### 18. What are array-like objects?
Objects with numeric keys and a `.length` property (like DOM NodeLists or the `arguments` object). Convert them to real arrays using `Array.from(likeObj)` or `[...likeObj]`.

#### 19. Why does `0.1 + 0.2 !== 0.3` in JavaScript?
JS uses IEEE 754 double-precision floats. Numbers are represented in binary, and fractions like `0.1` and `0.2` result in repeating decimals, causing precision errors when added.

#### 20. What are the quirks of the `typeof` operator?
*   `typeof null` returns `"object"` (historical bug).
*   `typeof NaN` returns `"number"`.
*   `typeof []` returns `"object"`.

#### 21. What is the difference between `typeof` and `instanceof`?
*   `typeof` checks the primitive type of a variable.
*   `instanceof` checks if an object is an instance of a constructor class in its prototype chain.

#### 22. What does strict mode (`"use strict"`) do?
Prevents syntax errors from passing silently (e.g., throwing error on undeclared variables: `x = 5`), prevents duplicating parameters, and binds default `this` to `undefined`.

#### 23. Explain dynamic object property access.
Accessing object properties dynamically using bracket notation with variables:
```javascript
const key = "age";
const user = { name: "Alice", age: 20 };
console.log(user[key]); // 20
```

#### 24. What are bitwise operators in JS?
Bitwise operators (e.g., `&`, `|`, `^`, `~`, `<<`, `>>`) treat arguments as 32-bit integers and operate at the binary level.

#### 25. Compare type casting functions vs. parsing functions.
*   `Number("12px")` returns `NaN`.
*   `parseInt("12px", 10)` parses up to the non-numeric character and returns `12`.

---

## Category 2: Scope, Hoisting, Closures & `this` (26-50)

#### 26. Explain scope chain and lexical environment.
Every execution context has a reference to its outer parent environment. When a variable is accessed, JS traverses this scope chain outward until the variable is found or the global scope is reached.

#### 27. What is a closure?
A function that remembers and accesses its outer lexical scope even when it is executed outside its original environment.
```javascript
function makeAdder(x) {
  return function(y) { return x + y; };
}
const add5 = makeAdder(5);
console.log(add5(2)); // 7
```

#### 28. How do you implement private variables using closures?
By declaring variables inside a function scope and returning inner methods that access them, hiding them from the outer scope:
```javascript
function counter() {
  let count = 0;
  return { increment: () => ++count };
}
```

#### 29. How does `let` solve the loop `var` problem?
`var` is function-scoped; all loop iterations share the exact same variable container. `let` is block-scoped; it creates a brand new variable binding for each loop iteration.

#### 30. How do closures cause memory leaks?
If a closure retains a reference to a large outer variable that is no longer needed, garbage collection cannot free that memory until the closure itself is destroyed.

#### 31. What is the `this` keyword?
A reference pointing to the object execution context. It is bound dynamically at runtime based on the method execution call site.

#### 32. Explain `this` inside methods vs. regular functions.
*   Inside a method (e.g., `obj.method()`), `this` refers to the parent object `obj`.
*   Inside a regular function (e.g., `func()`), `this` refers to the global object (`window` or `global`), or `undefined` in strict mode.

#### 33. What is the difference between `call()`, `apply()`, and `bind()`?
*   `call()`: Invokes a function immediately, specifying `this` and arguments one-by-one.
*   `apply()`: Invokes a function immediately, specifying `this` and arguments as an array.
*   `bind()`: Returns a new function with `this` permanently bound, to be invoked later.

#### 34. What is currying?
A functional technique that translates a function with $N$ arguments into $N$ nested functions that each accept a single argument.
```javascript
const curryAdd = a => b => c => a + b + c;
```

#### 35. What is the memoization pattern?
Caching the outputs of expensive function calls based on inputs using closures:
```javascript
function memoize(fn) {
  const cache = {};
  return (...args) => {
    const key = JSON.stringify(args);
    return cache[key] || (cache[key] = fn(...args));
  };
}
```

#### 36. Why do arrow functions not have their own `this`?
Arrow functions bind `this` lexically. They inherit their `this` context from their enclosing scope at declaration time. They cannot be used as constructors.

#### 37. Compare function declarations vs. function expressions.
*   Function Declarations: `function foo() {}` (fully hoisted, can call before definition).
*   Function Expressions: `const foo = function() {}` (variable is hoisted, but function is not initialized).

#### 38. What is an IIFE (Immediately Invoked Function Expression)?
A function that runs immediately upon declaration, creating a private local scope to avoid global scope pollution.
```javascript
(function() {
  console.log("IIFE");
})();
```

#### 39. What is variable shadowing?
When a variable declared in an inner scope has the same name as a variable in an outer scope, blocking access to the outer variable.

#### 40. What is an execution context?
An environment containing the current executing code, split into two phases: **creation phase** (hoisting, setting scope, binding `this`) and **execution phase** (running code line-by-line).

#### 41. How does the call stack manage execution contexts?
The Call Stack tracks function execution. When a function is invoked, its execution context is pushed onto the stack. When the function returns, its context is popped off.

#### 42. Explain closures in async loops.
If you run an async operation (like `setTimeout`) inside a loop using a `var` counter, the closure accesses the final iterated value of the variable. Using `let` fixes this by binding a unique value to each block iteration.

#### 43. What is the danger of dynamically creating functions inside loops?
It is computationally expensive and memory intensive, as a new function object (and closure scope) is instantiated on every iteration.

#### 44. What happens when you pass an object method as a callback?
It loses its implicit object context binding. When invoked later, `this` will default to the global scope or `undefined`.
```javascript
const obj = { name: "A", log() { console.log(this.name); } };
setTimeout(obj.log, 100); // Logs undefined (context is lost)
setTimeout(obj.log.bind(obj), 100); // Fixed!
```

#### 45. What is the difference between static and lexical scope?
They are synonymous. It means variable scope is resolved based on the physical position of functions at write time, not their execution time.

#### 46. What is the `arguments` object?
An array-like object available inside non-arrow functions containing all passed parameters.

#### 47. Rest parameters vs. the `arguments` object.
*   Rest parameters (`...args`) yield a real array, include only unnamed parameters, and work in arrow functions.
*   `arguments` is an array-like object, contains all arguments, and does not work in arrow functions.

#### 48. What is the environment record in execution contexts?
The structure inside a lexical environment that maps identifier names directly to variable/function values in that scope.

#### 49. What is `this` in strict mode?
If a function is called unbound (e.g., `func()`), `this` is `undefined` instead of the global `window` object, preventing accidental edits to global properties.

#### 50. How does garbage collection work with closures?
A lexical scope remains in memory as long as there is at least one active reference path (like a returned closure) pointing to any variable declared within it.

---

## Category 3: Objects, Prototypes & OOP (51-75)

#### 51. Compare Prototype vs. Class-based inheritance.
*   Class-based (Java): Classes are blueprints. Objects are instances copy-constructed from them.
*   Prototype-based (JS): Objects are linked directly to other objects. Properties are shared dynamically via lookup paths.

#### 52. Explain the Prototype Chain.
If a property is missing on an object, JS checks the object's parent prototype link (`__proto__`). This chain climbs up until it finds the property or hits `Object.prototype.__proto__` which is `null`.

#### 53. Diff between `.prototype` and `.__proto__`.
*   `__proto__` is the link reference on an *object instance* pointing to its parent prototype.
*   `.prototype` is a property on *constructor functions* that becomes the `__proto__` of any instance created with `new`.

#### 54. How does `Object.create(proto)` work?
It creates a new object and sets its `__proto__` link directly to the target `proto` object.

#### 55. What does the `new` keyword do under the hood?
1. Creates a empty object `{}`.
2. Links its `__proto__` to the constructor's `.prototype` object.
3. Binds the constructor's `this` context to the new object.
4. Executes constructor logic and returns the object.

#### 56. Are ES6 classes just syntactic sugar?
Yes. They compile down to standard constructor functions and prototype property attachments in the background.

#### 57. How do `extends` and `super` work?
`extends` links the prototype of the child class to the parent class. `super()` calls the parent constructor and binds the parent's properties to the child class instance's `this` context.

#### 58. How do getter and setter methods work?
Accessors look like standard object properties but trigger getter/setter function calls behind the scenes.
```javascript
const user = {
  get age() { return 25; }
};
```

#### 59. Explain property descriptors.
Objects have metadata configurations:
*   `writable`: Can overwrite value.
*   `enumerable`: Iterates in loops.
*   `configurable`: Can delete/reconfigure property.

#### 60. Compare `Object.freeze()` vs. `Object.seal()` vs. `const`.
*   `const` prevents reference re-assignment.
*   `Object.seal()` prevents adding/deleting properties, but allows mutating existing properties.
*   `Object.freeze()` locks the object completely (no mutations, no deletes, no additions).

#### 61. How do you check if a property exists on an object?
*   `"prop" in obj`: Returns true if property is on the object or its prototype chain.
*   `obj.hasOwnProperty("prop")`: Returns true only if the property exists directly on the object.

#### 62. Shallow copy vs. Deep copy of objects.
*   Shallow copy (`{ ...obj }`) copies primitive values, but keeps reference references to nested objects.
*   Deep copy (`structuredClone(obj)`) recursively duplicates everything, isolating the new object.

#### 63. What is `structuredClone()`?
A native JS engine function to perform deep clones on complex objects, including handling nested Arrays, Dates, and Maps correctly (unlike JSON tricks).

#### 64. What are static methods and fields?
Methods/fields declared on the Class constructor itself rather than on instances. Call them using `ClassName.methodName()`.

#### 65. How are private fields created in ES6 classes?
By prefixing the field name with a hash symbol `#`. These cannot be accessed outside the class scope.
```javascript
class User {
  #id = 123;
}
```

#### 66. How is polymorphism achieved in JS?
By overriding inherited parent prototype methods in child classes.

#### 67. How are mixins implemented in JS?
JS does not support multiple inheritance, but you can copy functions onto class prototype targets to reuse code blocks:
```javascript
Object.assign(User.prototype, swimMixin);
```

#### 68. How do you iterate over object keys and values?
*   `Object.keys(obj)`: Array of keys.
*   `Object.values(obj)`: Array of values.
*   `Object.entries(obj)`: Array of `[key, value]` tuples.

#### 69. Compare `for...in` vs. `Object.keys()`.
*   `for...in` iterates over all enumerable properties, **including those on prototypes**.
*   `Object.keys()` returns only properties owned directly by the object.

#### 70. Compare `Object.assign()` vs. Spread (`{...}`).
Both perform shallow copies, but the spread operator does not trigger setter methods on target objects.

#### 71. Factory functions vs. Constructor functions.
*   Factory: Standard function returning a new object (`return {}`).
*   Constructor: Invoked with `new` and implicitly returns `this`.

#### 72. What is the performance impact of prototypes?
Prototypes save memory because methods are declared once on the parent prototype, rather than duplicated inside every object instance.

#### 73. Why is modifying built-in prototypes bad?
"Prototype pollution" can break native library code, cause naming collisions, and disrupt browser optimization routines.

#### 74. How do you get/set prototypes dynamically?
Use `Object.getPrototypeOf(obj)` and `Object.setPrototypeOf(obj, newProto)`. Avoid assigning to `__proto__` directly as it is slow.

#### 75. How are objects serialized?
Using `JSON.stringify(obj)` to convert objects to strings, and `JSON.parse(str)` to reconstruct them.

---

## Category 4: Async JS, Event Loop & Promises (76-100)

#### 76. How does JS handle concurrency if it is single-threaded?
By delegating asynchronous operations (fetch calls, timers) to the host environment (browser/Node APIs) and processing completed callbacks inside the Event Loop.

#### 77. Explain the Event Loop workflow.
1. Sync tasks run on the **Call Stack**.
2. Async tasks run in background host threads and queue completed callbacks.
3. Once the Call Stack is empty, JS executes all callbacks in the **Microtask Queue**.
4. Once the Microtask Queue is empty, JS pulls **one** callback from the **Macrotask Queue** and executes it.

#### 78. Diff between Microtask and Macrotask queues.
*   **Microtasks (higher priority):** Promises (`.then`), `queueMicrotask`, `MutationObserver`.
*   **Macrotasks (lower priority):** `setTimeout`, `setInterval`, UI rendering, user actions.

#### 79. Explain "Callback Hell".
Deeply nested callback functions that become difficult to read and track when chaining multiple async tasks.

#### 80. What are the three states of a Promise?
1. `pending`: Action not completed yet.
2. `fulfilled`: Operation completed successfully.
3. `rejected`: Operation failed with an error.

#### 81. How does Promise chaining work?
Every `.then()` returns a **new Promise**. If a handler returns a value, the new Promise resolves with that value. If it returns a Promise, the chain waits for it to settle.

#### 82. Compare `Promise.all()` vs. `Promise.allSettled()`.
*   `Promise.all()`: Rejects immediately if **any** promise fails.
*   `Promise.allSettled()`: Waits for **all** to complete, returning status objects for each.

#### 83. Compare `Promise.any()` vs. `Promise.race()`.
*   `Promise.any()`: Resolves on the first **successful** promise.
*   `Promise.race()`: Settles (resolves or rejects) as soon as the first promise resolves or rejects.

#### 84. What is the benefit of `async`/`await`?
It is syntactic sugar that makes asynchronous promise chains read sequentially like synchronous code, making debugging and structure simpler.

#### 85. How does error handling differ in async/await?
Instead of `.catch()`, you wrap asynchronous operations in standard `try...catch` blocks.

#### 86. How are unhandled promise rejections caught globally?
By listening to the global `unhandledrejection` window/process event:
```javascript
window.addEventListener("unhandledrejection", (e) => { ... });
```

#### 87. What is Promisification?
Converting a callback-based API into a promise-based API:
```javascript
const fsPromise = (path) => new Promise((res, rej) => {
  fs.readFile(path, (err, data) => err ? rej(err) : res(data));
});
```

#### 88. Why is `setTimeout` not accurate?
Because its callback has to wait in the Macrotask Queue. If the Call Stack is blocked by expensive calculations, the timer callback will execute late.

#### 90. `requestAnimationFrame` vs. `setTimeout` for UI updates.
`requestAnimationFrame` is synchronized directly with the browser's refresh rate (typically 60Hz/120Hz) to ensure smooth animations. `setTimeout` runs out-of-sync, leading to layout stutter.

#### 91. What are Generator functions?
Functions that can pause execution using `yield` and resume later, returning an iterator control object:
```javascript
function* count() {
  yield 1;
  yield 2;
}
```

#### 92. Python `asyncio` / Java threads vs. JS Event Loop.
Java allocates dedicated system threads. Python `asyncio` runs single-threaded async code but must be managed manually. JS has a built-in event loop running automatically inside the host browser/Node engine.

#### 93. Event Bubbling vs. Event Capturing.
*   **Bubbling:** Event fires on target and bubbles up the DOM tree (child to parent).
*   **Capturing:** Event starts at window level and travels down the DOM tree (parent to child).

#### 94. What is Event Delegation?
A design pattern where you attach a single listener on a parent container to manage events bubbled up from child elements.

#### 95. What is Debouncing?
Delaying a function's execution until a specified delay period has elapsed since the last time the function was invoked.

#### 96. What is Throttling?
Restricting a function to execute at most once in a specified time window.

#### 97. What is a Web Worker?
A browser API that runs a JS file in a separate background thread, isolated from the UI main thread, communicating using message channels.

#### 98. How do you cancel a `fetch` request?
Using `AbortController` and passing its signal reference to the fetch query options:
```javascript
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort(); // Cancels the request
```

#### 99. What is `process.nextTick()` in Node.js?
A Node-specific method that queues a callback to run immediately after the current operation finishes, executing before the Event Loop enters microtask evaluation.

#### 100. How does a custom debounce function look?
```javascript
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

---

## Category 5: ES6+, Modules, Browser APIs & Performance (101-125)

#### 101. Compare ES Modules (ESM) vs. CommonJS (CJS).
*   ESM: `import/export` (asynchronous, statically analyzed, standard in modern JS).
*   CJS: `require/module.exports` (synchronous, parsed at runtime, standard in legacy Node.js).

#### 102. Explain tree shaking.
Static analysis of ESM module imports allows compilers to drop unused exported methods, reducing the bundle size of the application.

#### 103. Transpilers vs. Polyfills.
*   **Transpilers** (Babel): Rewrite modern syntax (e.g., optional chaining `?.`) into older equivalents.
*   **Polyfills**: Inject runtime implementations of newer global functions (e.g., `Promise`) when missing in older environments.

#### 104. What is a Bundler?
A developer tool (like Webpack or Vite) that bundles project files, resources, and dependencies into a minimized web package.

#### 105. How does dynamic import work?
By invoking `import(path)` which resolves to a Promise, enabling dynamic, runtime loading of code chunks:
```javascript
import("./module.js").then(m => m.run());
```

#### 106. Object Destructuring with renaming and default values.
```javascript
const { name: userName, status = "active" } = user;
```

#### 107. Array Destructuring and rest/spread.
```javascript
const [first, ...rest] = [10, 20, 30]; // first = 10, rest = [20, 30]
```

#### 108. What are Tagged Template Literals?
Passing template literals directly to a parser function, allowing custom string formatting:
```javascript
myTag`Hello ${name}`;
```

#### 109. Compare `Map` vs. Plain Object.
`Map` allows key lookups of **any type** (including Objects), tracks keys in insertion order, and scales better for high-frequency key operations.

#### 110. Compare `Set` vs. Array.
`Set` guarantees uniqueness of entries and offers fast $O(1)$ lookups, whereas Arrays require $O(N)$ searches.

#### 111. What are `WeakMap` and `WeakSet`?
Collections where key/value references are weakly bound. If there are no other references to the key objects, they are garbage-collected, preventing memory leaks.

#### 112. What is a `Symbol`?
A primitive data type yielding unique, immutable identifiers, useful for creating non-clashing object property keys.

#### 113. Compare Spread vs. Rest syntax.
*   Spread (`...`): Expands arrays/objects.
*   Rest (`...`): Gathers separate function parameters into an array.

#### 114. Explain `map()`, `filter()`, and `reduce()`.
Functional array operators that run without mutating the original array.
*   `map`: Transforms each item.
*   `filter`: Drops items based on boolean logic.
*   `reduce`: Combines items down to a single value.

#### 115. Compare `forEach()` vs. `map()`.
`map()` returns a new transformed array; `forEach()` returns `undefined` and is used to trigger side effects.

#### 116. Compare `localStorage`, `sessionStorage`, and `Cookies`.
*   `localStorage`: Persistent data, up to 5MB, stays after window closes.
*   `sessionStorage`: Cleared when tab closes, up to 5MB.
*   `Cookies`: Sent to server on every request, capacity up to 4KB.

#### 117. How do you parse JSON responses in `fetch`?
```javascript
fetch(url).then(res => res.json()).then(data => console.log(data));
```

#### 118. What is CORS and the preflight request?
Cross-Origin Resource Sharing is a browser security mechanism. For non-simple HTTP requests, browsers send a preflight request (`OPTIONS` request) to verify the server permits cross-origin traffic.

#### 119. Compare `querySelector` vs. `getElementById`.
`getElementById` is optimized and faster but works only on IDs. `querySelector` is more flexible, accepting any CSS selector query.

#### 120. Explain Reflow vs. Repaint.
*   **Reflow (Layout):** Browser recalculates positions and geometries of elements on the page (expensive).
*   **Repaint:** Browser redraws elements with new styling properties (like color or opacity) without layout changes (cheaper).

#### 121. What is the Critical Rendering Path?
The sequence of steps the browser takes to translate HTML, CSS, and JS into pixels: DOM -> CSSOM -> Render Tree -> Layout -> Paint.

#### 122. How do you optimize DOM operations?
By minimizing layout writes, batching DOM updates using `DocumentFragment` templates, and styling changes via CSS classes rather than inline style overrides.

#### 123. What is a Service Worker?
A script running in a background thread of the browser, independent of the web page, enabling offline capabilities, asset caching, and push notifications.

#### 124. How does the Intersection Observer API work?
It monitors element positions relative to parent bounds or viewport intersections, enabling performant lazy loading or infinite scrolling:
```javascript
const observer = new IntersectionObserver(entries => { ... });
observer.observe(targetElement);
```

#### 125. How do you measure rendering execution times?
Using `performance.now()` for sub-millisecond precision testing.

---

## Category 6: Advanced & FAANG Trick Questions + Final Cheat Sheet (126-150)

#### 126. Why does `0.1 + 0.2 === 0.3` return `false`? How do you fix it?
Because of floating-point inaccuracies. Fix it by rounding:
```javascript
Number((0.1 + 0.2).toFixed(12)) === 0.3 // true
```

#### 127. What do these return: `[] + []`, `[] + {}`, `{} + []`, and `{} + {}`?
*   `[] + []` yields `""` (arrays coerced to empty strings).
*   `[] + {}` yields `"[object Object]"` (object coerced to string).
*   `{} + []` yields `0` (interprets `{}` as empty block, evaluates `+[]` as coerced zero).
*   `{} + {}` yields `NaN` or `"[object Object][object Object]"` depending on the browser context.

#### 128. What does `typeof null` evaluate to, and why?
`"object"`. This is a legacy bug from JS's initial version where values had type tags, and the tag for null matched the tag for object references. It cannot be fixed without breaking existing websites.

#### 129. Write a custom `bind` polyfill.
```javascript
Function.prototype.myBind = function(context, ...args) {
  const self = this;
  return function(...newArgs) {
    return self.apply(context, [...args, ...newArgs]);
  };
};
```

#### 130. Write a custom `map` polyfill.
```javascript
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      result.push(callback(this[i], i, this));
    }
  }
  return result;
};
```

#### 131. Write a custom `Promise.all` polyfill.
```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let completed = 0;
    if (promises.length === 0) return resolve(results);
    
    promises.forEach((p, index) => {
      Promise.resolve(p)
        .then(val => {
          results[index] = val;
          completed++;
          if (completed === promises.length) resolve(results);
        })
        .catch(reject);
    });
  });
}
```

#### 132. Write a custom debounce function.
```javascript
function debounce(fn, delay) {
  let timerId;
  return function(...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}
```

#### 133. Write a custom throttle function.
```javascript
function throttle(fn, limit) {
  let inThrottle = false;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

#### 134. How do you deep clone an object with circular references?
By using a `WeakMap` to cache references and prevent infinite recursion loops:
```javascript
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null || typeof obj !== "object") return obj;
  if (hash.has(obj)) return hash.get(obj);
  
  const clone = Array.isArray(obj) ? [] : {};
  hash.set(obj, clone);
  
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key], hash);
    }
  }
  return clone;
}
```

#### 135. Write a function to flatten a nested array recursively.
```javascript
function flatten(arr) {
  return arr.reduce((acc, val) => 
    Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val), []
  );
}
```

#### 136. What is the output of `(function(){ var a = b = 3; })();`?
*   `a` is declared with `var`, making it local to the function.
*   `b` is assigned without declaration, making it a leak to the global scope.
*   Thus, `typeof a === 'undefined'` outside, but `b === 3` is accessible globally.

#### 137. Explain the console output order:
```javascript
console.log("Start");
setTimeout(() => console.log("Timeout"), 0);
Promise.resolve().then(() => console.log("Promise"));
console.log("End");
```
*Output order:* `Start` -> `End` -> `Promise` -> `Timeout`.
*Reason:* Synchronous blocks run first. Promises are microtasks and execute before macrotasks (timers).

#### 138. Implement a basic Event Emitter class.
```javascript
class EventEmitter {
  constructor() { this.events = {}; }
  on(name, cb) {
    (this.events[name] || (this.events[name] = [])).push(cb);
  }
  emit(name, ...args) {
    (this.events[name] || []).forEach(cb => cb(...args));
  }
}
```

#### 139. Implement a generic `curry` helper function.
```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return (...args2) => curried.apply(this, [...args, ...args2]);
  };
}
```

#### 140. Write a function checking if two objects are deeply equal.
```javascript
function deepEqual(a, b) {
  if (a === b) return true;
  if (typeof a !== 'object' || a === null || typeof b !== 'object' || b === null) return false;
  
  const keysA = Object.keys(a), keysB = Object.keys(b);
  if (keysA.length !== keysB.length) return false;
  
  for (let key of keysA) {
    if (!keysB.includes(key) || !deepEqual(a[key], b[key])) return false;
  }
  return true;
}
```

#### 141. How do you identify array vs. object vs. null types?
```javascript
function getType(val) {
  if (val === null) return "null";
  if (Array.isArray(val)) return "array";
  return typeof val; // "object", "string", "number", etc.
}
```

#### 142. What does `console.log(1 < 2 < 3)` and `console.log(3 > 2 > 1)` print?
*   `1 < 2 < 3` -> `true < 3` -> `1 < 3` -> `true`.
*   `3 > 2 > 1` -> `true > 1` -> `1 > 1` -> `false`.

#### 143. Implement a custom memoize/cache function.
```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}
```

#### 144. Compare `parseFloat('12.3px')` vs. `Number('12.3px')`.
*   `parseFloat('12.3px')` returns `12.3`.
*   `Number('12.3px')` returns `NaN`.

#### 145. Implement async serialization: run array of promises in sequence.
```javascript
async function runInSequence(tasks) {
  const results = [];
  for (let task of tasks) {
    results.push(await task());
  }
  return results;
}
```

#### 146. Implement a retry mechanism for async promise calls.
```javascript
async function retry(fn, retries = 3) {
  try {
    return await fn();
  } catch (err) {
    if (retries <= 0) throw err;
    return await retry(fn, retries - 1);
  }
}
```

#### 147. Why does `Array(5).map(...)` do nothing? How do you create an empty, map-able array?
`Array(5)` creates an array of empty slots with no indices. Array methods like `map` skip empty slots.
*Fix:*
```javascript
Array.from({ length: 5 }).map((_, i) => i); // [0, 1, 2, 3, 4]
```

#### 148. What occurs when assigning to a read-only property in strict mode?
It throws a `TypeError`. In non-strict mode, it fails silently.

#### 149. Create a custom prototype chain link.
```javascript
const parent = { greet() { return "hello"; } };
const child = Object.create(parent);
console.log(child.greet()); // "hello"
```

#### 150. FINAL CHEAT SHEET: One-sentence summaries of high-yield JS terms.
*   **Hoisting:** Variable declarations are parsed before execution, moving them to the top of scope.
*   **Closure:** Function retaining its birthplace scope reference.
*   **Dynamic binding (`this`):** Context determined solely by the invoker call site.
*   **Event Loop:** Engine loop syncing stack tasks, microtasks, and macrotasks.
*   **Prototype:** Underlying chain link objects use to delegate shared methods.
*   **Destructuring:** Multi-variable assignment patterns unpacking collections.
*   **Memoization:** Return value caching based on input parameters.
*   **Microtask Queue:** Execution pipeline with highest event loop priority.
*   **Temporal Dead Zone:** Period where declaring variable reference throws error.
*   **Polyfill:** Code emulating newer APIs inside older browser engines.

---

# PART 2 — Advanced JavaScript Deep Dives

---

## Proxy & Reflect

```javascript
// Proxy: intercepts operations on objects (get, set, apply, construct, etc.)
// Syntax: new Proxy(target, handler)

const handler = {
    get(target, prop, receiver) {
        console.log(`Getting: ${prop}`);
        return prop in target ? Reflect.get(target, prop, receiver) : `Property '${prop}' doesn't exist`;
    },
    set(target, prop, value, receiver) {
        if (typeof value !== 'number') throw new TypeError(`${prop} must be a number`);
        console.log(`Setting: ${prop} = ${value}`);
        return Reflect.set(target, prop, value, receiver);
    },
    has(target, prop) {
        console.log(`Checking: ${prop} in target`);
        return prop in target;
    },
    deleteProperty(target, prop) {
        console.log(`Deleting: ${prop}`);
        return Reflect.deleteProperty(target, prop);
    }
};

const user = new Proxy({ age: 25 }, handler);
user.age;                    // Getting: age → 25
user.name;                   // Getting: name → "Property 'name' doesn't exist"
user.age = 30;               // Setting: age = 30
user.age = "hello";          // TypeError: age must be a number
'age' in user;               // Checking: age in target → true

// Real-world use case: Validation proxy
function createValidated(schema) {
    return new Proxy({}, {
        set(target, prop, value) {
            if (schema[prop] && !schema[prop](value)) {
                throw new Error(`Validation failed for '${prop}'`);
            }
            target[prop] = value;
            return true;
        }
    });
}

const person = createValidated({
    age: v => Number.isInteger(v) && v >= 0 && v <= 150,
    email: v => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v)
});
person.age = 25;            // OK
person.age = -5;            // Error: Validation failed for 'age'

// Reflect: mirrors all Proxy traps as static methods
// Purpose: call default behavior from within proxy traps
Reflect.get(obj, 'prop')          // obj.prop
Reflect.set(obj, 'prop', value)   // obj.prop = value
Reflect.has(obj, 'prop')          // 'prop' in obj
Reflect.ownKeys(obj)              // Object.keys + symbols
Reflect.apply(fn, thisArg, args)  // fn.apply(thisArg, args)
Reflect.construct(Cls, args)      // new Cls(...args)
```

---

## WeakRef & FinalizationRegistry

```javascript
// WeakRef: holds a weak reference to an object (does not prevent GC)
let obj = { name: "Heavy Data" };
const ref = new WeakRef(obj);

ref.deref();                // { name: "Heavy Data" } — returns object if still alive
ref.deref()?.name;          // "Heavy Data"

obj = null;                 // Allow GC to collect
// At some point:
ref.deref();                // undefined (if GC ran) or still the object (GC not yet run)

// FinalizationRegistry: run cleanup when object is GC'd
const registry = new FinalizationRegistry((heldValue) => {
    console.log(`${heldValue} was garbage collected`);
});

let resource = { data: "large data" };
registry.register(resource, "my-resource");   // register with held value

resource = null;            // Eventually: "my-resource was garbage collected"

// Use case: Cache that doesn't prevent GC
class WeakCache {
    #cache = new Map();
    
    set(key, value) {
        this.#cache.set(key, new WeakRef(value));
    }
    
    get(key) {
        const ref = this.#cache.get(key);
        if (!ref) return undefined;
        const value = ref.deref();
        if (!value) this.#cache.delete(key);  // clean up dead ref
        return value;
    }
}
```

---

## Generator Functions (Deep Dive)

```javascript
// Generator: function that can pause (yield) and resume
function* fibonacci() {
    let a = 0, b = 1;
    while (true) {          // infinite sequence!
        yield a;            // pause and return a
        [a, b] = [b, a + b];
    }
}

const fib = fibonacci();
fib.next();                 // { value: 0, done: false }
fib.next();                 // { value: 1, done: false }
fib.next();                 // { value: 1, done: false }
fib.next();                 // { value: 2, done: false }

// Generator can receive values via next(value)
function* calculator() {
    let result = 0;
    while (true) {
        const input = yield result;     // yield returns current result, pauses, receives next input
        if (input === null) return result;
        result += input;
    }
}

const calc = calculator();
calc.next();                // { value: 0, done: false } — start
calc.next(10);              // { value: 10, done: false }
calc.next(5);               // { value: 15, done: false }
calc.next(null);            // { value: 15, done: true }

// Delegating generators with yield*
function* concat(...iterables) {
    for (const iterable of iterables) {
        yield* iterable;    // delegate to another iterable
    }
}
[...concat([1,2], [3,4], [5,6])]    // [1, 2, 3, 4, 5, 6]

// Generator as custom iterable
class Range {
    constructor(start, end, step = 1) {
        this.start = start; this.end = end; this.step = step;
    }
    [Symbol.iterator]() {
        return (function*(start, end, step) {
            for (let i = start; i <= end; i += step) yield i;
        })(this.start, this.end, this.step);
    }
}

[...new Range(1, 10, 2)]    // [1, 3, 5, 7, 9]
for (const n of new Range(0, 5)) console.log(n);

// Async generator: async function*
async function* fetchPages(baseUrl, maxPages = 5) {
    for (let page = 1; page <= maxPages; page++) {
        const res = await fetch(`${baseUrl}?page=${page}`);
        const data = await res.json();
        if (!data.length) return;
        yield data;         // yield one page at a time
    }
}

// Consume async generator
async function processAll() {
    for await (const page of fetchPages('/api/items')) {
        console.log(`Got ${page.length} items`);
    }
}
```

---

## Symbol — Deep Dive

```javascript
// Symbol: unique, immutable primitive — no two symbols are equal
const s1 = Symbol('description');
const s2 = Symbol('description');
s1 === s2;              // false — every Symbol() call creates unique value

// Global symbol registry (shared across files/modules)
const gs1 = Symbol.for('app.id');
const gs2 = Symbol.for('app.id');
gs1 === gs2;            // true — same key retrieves same symbol

Symbol.keyFor(gs1);     // 'app.id'
Symbol.keyFor(s1);      // undefined (not in global registry)

// Symbols as object keys — won't appear in for...in or Object.keys()
const ID = Symbol('id');
const user = {
    name: 'Alice',
    [ID]: 123          // Symbol-keyed property — hidden from enumeration
};
Object.keys(user);                  // ['name']
Object.getOwnPropertySymbols(user); // [Symbol(id)]
Reflect.ownKeys(user);              // ['name', Symbol(id)]
user[ID];                           // 123

// Well-known symbols — hooks into JS built-in behaviors
// Symbol.iterator — defines default iteration behavior
class EvenNumbers {
    constructor(max) { this.max = max; }
    [Symbol.iterator]() {
        let n = 0;
        return {
            next: () => ({
                value: n += 2,
                done: n > this.max
            })
        };
    }
}
[...new EvenNumbers(10)]    // [2, 4, 6, 8, 10]

// Symbol.toPrimitive — customize type coercion
class Temperature {
    constructor(celsius) { this.celsius = celsius; }
    [Symbol.toPrimitive](hint) {
        if (hint === 'number') return this.celsius;
        if (hint === 'string') return `${this.celsius}°C`;
        return this.celsius;  // 'default'
    }
}
const temp = new Temperature(100);
+temp;                  // 100 (number hint)
`${temp}`;              // "100°C" (string hint)
temp + 0;               // 100 (default hint)

// Symbol.hasInstance — customize instanceof
class EvenChecker {
    static [Symbol.hasInstance](num) {
        return Number.isInteger(num) && num % 2 === 0;
    }
}
2 instanceof EvenChecker;   // true
3 instanceof EvenChecker;   // false

// Symbol.asyncIterator — for async for-of
class Delayed {
    async *[Symbol.asyncIterator]() {
        for (let i = 1; i <= 3; i++) {
            await new Promise(res => setTimeout(res, 100));
            yield i;
        }
    }
}
for await (const val of new Delayed()) console.log(val);  // 1, 2, 3
```

---

## ES2020–2024 Features Reference

```javascript
// ===== ES2020 =====
// Optional chaining (?.)
user?.address?.city?.toLowerCase()

// Nullish coalescing (??)
const count = config.limit ?? 100;    // only null/undefined triggers right side

// Nullish assignment (??=)
user.name ??= 'Anonymous';            // only assigns if null/undefined

// Logical OR assignment (||=)
settings.theme ||= 'dark';            // assigns if falsy

// Logical AND assignment (&&=)
cache.data &&= transform(cache.data); // assigns if truthy

// BigInt
const big = 9007199254740991n;        // Number.MAX_SAFE_INTEGER as BigInt
big + 1n;                             // 9007199254740992n

// globalThis
console.log(globalThis);             // window in browser, global in Node

// Promise.allSettled
await Promise.allSettled([p1, p2, p3]);
// [{ status: 'fulfilled', value: v }, { status: 'rejected', reason: e }, ...]

// Dynamic import
const module = await import('./heavy.js');
module.doSomething();

// ===== ES2021 =====
// String.replaceAll
'aababc'.replaceAll('a', 'x');        // 'xxbxbc'

// Promise.any — resolves on FIRST success, rejects only if ALL fail
const first = await Promise.any([fetchA(), fetchB(), fetchC()]);

// WeakRef, FinalizationRegistry (see above)

// Numeric separators
const million = 1_000_000;
const bytes = 0xFF_00_FF;

// ===== ES2022 =====
// Object.hasOwn — safer than hasOwnProperty
Object.hasOwn(obj, 'prop');           // true if own property

// Array.at() — negative indexing
[1, 2, 3].at(-1);                    // 3 (last element)
'hello'.at(-1);                      // 'o'

// Error.cause — chain errors with context
throw new Error('Outer error', { cause: originalError });
error.cause;                          // originalError

// Class static blocks
class Config {
    static DB_URL;
    static {   // runs once when class is defined
        Config.DB_URL = process.env.DATABASE_URL ?? 'localhost:5432';
    }
}

// Private class methods and accessors
class BankAccount {
    #balance = 0;
    #validate(amount) { return amount > 0; }    // private method
    deposit(amount) {
        if (this.#validate(amount)) this.#balance += amount;
    }
    get #summary() { return `Balance: ${this.#balance}`; }  // private getter
}

// Top-level await (in ES modules)
const data = await fetch('/api/config').then(r => r.json());
export { data };   // works at top level in .mjs files

// ===== ES2023 =====
// Array.toSorted / toReversed / toSpliced — immutable array methods!
const arr = [3, 1, 2];
arr.toSorted();                       // [1, 2, 3] — new array, arr unchanged
arr.toReversed();                     // [2, 1, 3] — new array
arr.toSpliced(1, 1, 99);             // [3, 99, 2] — new array
arr.with(0, 99);                      // [99, 1, 2] — replace index, new array

// Array.findLast / findLastIndex
[1, 2, 3, 2, 1].findLast(x => x < 3);          // 2 (last match)
[1, 2, 3, 2, 1].findLastIndex(x => x < 3);     // 3 (last match index)

// Hashbang (#!) for executable scripts
// #!/usr/bin/env node   ← first line of .js file

// ===== ES2024 =====
// Promise.withResolvers() — expose resolve/reject outside Promise constructor
const { promise, resolve, reject } = Promise.withResolvers();
setTimeout(resolve, 1000, 'done');
await promise;   // 'done'

// Object.groupBy / Map.groupBy
const people = [{name:'Alice',age:25},{name:'Bob',age:30},{name:'Carol',age:25}];
Object.groupBy(people, p => p.age);
// { '25': [{name:'Alice',...}, {name:'Carol',...}], '30': [{name:'Bob',...}] }
```

---

## Web APIs Deep Dive

### BroadcastChannel (Cross-Tab Communication)

```javascript
// Broadcast messages across all tabs/windows on same origin
const channel = new BroadcastChannel('app-sync');

// Sender tab
channel.postMessage({ type: 'USER_LOGOUT', userId: 42 });

// Receiver tab(s)
channel.onmessage = (event) => {
    if (event.data.type === 'USER_LOGOUT') {
        clearUserSession();
        window.location.href = '/login';
    }
};

channel.close();   // Always close when done to free resources

// Use cases: logout propagation, theme syncing, cart updates
```

### IndexedDB (Client-Side Database)

```javascript
// IndexedDB: full structured database in the browser (much more than localStorage)
// Supports transactions, indexes, and large amounts of data

const DB_NAME = 'MyAppDB';
const DB_VERSION = 1;

function openDB() {
    return new Promise((resolve, reject) => {
        const request = indexedDB.open(DB_NAME, DB_VERSION);
        
        request.onupgradeneeded = (event) => {
            const db = event.target.result;
            // Create object store (like a table)
            if (!db.objectStoreNames.contains('users')) {
                const store = db.createObjectStore('users', { keyPath: 'id', autoIncrement: true });
                store.createIndex('email', 'email', { unique: true });
            }
        };
        
        request.onsuccess = () => resolve(request.result);
        request.onerror = () => reject(request.error);
    });
}

async function addUser(user) {
    const db = await openDB();
    return new Promise((resolve, reject) => {
        const tx = db.transaction('users', 'readwrite');
        const store = tx.objectStore('users');
        const req = store.add(user);
        req.onsuccess = () => resolve(req.result);
        req.onerror = () => reject(req.error);
    });
}

async function getUserByEmail(email) {
    const db = await openDB();
    return new Promise((resolve, reject) => {
        const tx = db.transaction('users', 'readonly');
        const index = tx.objectStore('users').index('email');
        const req = index.get(email);
        req.onsuccess = () => resolve(req.result);
        req.onerror = () => reject(req.error);
    });
}
```

### MutationObserver

```javascript
// Observe DOM changes without polling
const observer = new MutationObserver((mutations) => {
    for (const mutation of mutations) {
        if (mutation.type === 'childList') {
            mutation.addedNodes.forEach(node => console.log('Added:', node));
            mutation.removedNodes.forEach(node => console.log('Removed:', node));
        }
        if (mutation.type === 'attributes') {
            console.log(`${mutation.attributeName} changed to ${mutation.target.getAttribute(mutation.attributeName)}`);
        }
    }
});

observer.observe(document.querySelector('#container'), {
    childList: true,           // watch for added/removed children
    attributes: true,          // watch for attribute changes
    subtree: true,             // watch entire subtree, not just direct children
    attributeFilter: ['class', 'style']  // only watch specific attributes
});

observer.disconnect();  // stop observing
```

### ResizeObserver

```javascript
// Observe element size changes (more performant than window.resize events)
const resizeObserver = new ResizeObserver((entries) => {
    for (const entry of entries) {
        const { width, height } = entry.contentRect;
        console.log(`Element resized to ${width}x${height}`);
        
        // Change layout based on container size (container queries in JS)
        if (width < 400) entry.target.classList.add('compact');
        else entry.target.classList.remove('compact');
    }
});

resizeObserver.observe(document.querySelector('.card'));
resizeObserver.unobserve(element);  // stop observing specific element
resizeObserver.disconnect();         // stop all observations
```

---

## Design Patterns in JavaScript

```javascript
// ===== SINGLETON =====
class DatabaseConnection {
    static #instance = null;
    
    constructor(url) {
        if (DatabaseConnection.#instance) return DatabaseConnection.#instance;
        this.url = url;
        this.connected = false;
        DatabaseConnection.#instance = this;
    }
    
    connect() { this.connected = true; console.log(`Connected to ${this.url}`); }
}

const db1 = new DatabaseConnection('localhost:5432');
const db2 = new DatabaseConnection('remote:5432');
db1 === db2;   // true — same instance

// ===== OBSERVER / PUB-SUB =====
class EventBus {
    #subscribers = new Map();
    
    subscribe(event, callback) {
        if (!this.#subscribers.has(event)) this.#subscribers.set(event, []);
        this.#subscribers.get(event).push(callback);
        // Return unsubscribe function
        return () => {
            const subs = this.#subscribers.get(event);
            this.#subscribers.set(event, subs.filter(cb => cb !== callback));
        };
    }
    
    publish(event, data) {
        (this.#subscribers.get(event) ?? []).forEach(cb => cb(data));
    }
    
    once(event, callback) {
        const unsub = this.subscribe(event, (data) => {
            callback(data);
            unsub();
        });
    }
}

const bus = new EventBus();
const unsub = bus.subscribe('user:login', (user) => console.log(`${user.name} logged in`));
bus.publish('user:login', { name: 'Alice' });   // "Alice logged in"
unsub();   // unsubscribe

// ===== FACTORY =====
class ShapeFactory {
    static create(type, ...args) {
        const shapes = { circle: Circle, rectangle: Rectangle, triangle: Triangle };
        const Shape = shapes[type.toLowerCase()];
        if (!Shape) throw new Error(`Unknown shape: ${type}`);
        return new Shape(...args);
    }
}
const circle = ShapeFactory.create('circle', 5);

// ===== DECORATOR (ES PROPOSAL + MANUAL) =====
// Manual decorator pattern
function readonly(target, prop, descriptor) {
    descriptor.writable = false;
    return descriptor;
}
function measure(fn) {
    return function(...args) {
        const start = performance.now();
        const result = fn.apply(this, args);
        console.log(`${fn.name} took ${(performance.now() - start).toFixed(2)}ms`);
        return result;
    };
}
const measuredSort = measure(Array.prototype.sort.bind([]));

// ===== STRATEGY =====
const sortStrategies = {
    bubble: (arr) => { /* bubble sort */ },
    quick: (arr) => { /* quicksort */ },
    merge: (arr) => { /* mergesort */ }
};

class Sorter {
    constructor(strategy = 'quick') {
        this.strategy = sortStrategies[strategy];
    }
    sort(arr) { return this.strategy(arr); }
    setStrategy(name) { this.strategy = sortStrategies[name]; }
}

// ===== COMMAND =====
class CommandHistory {
    #history = [];
    
    execute(command) {
        command.execute();
        this.#history.push(command);
    }
    
    undo() {
        const command = this.#history.pop();
        if (command) command.undo();
    }
}

class AddTextCommand {
    constructor(editor, text) { this.editor = editor; this.text = text; }
    execute() { this.editor.content += this.text; }
    undo() { this.editor.content = this.editor.content.slice(0, -this.text.length); }
}
```

---

## JavaScript Performance Patterns

```javascript
// ===== 1. OBJECT POOL (avoid GC pressure) =====
class ObjectPool {
    #pool = [];
    #factory;
    #reset;
    
    constructor(factory, reset, initialSize = 10) {
        this.#factory = factory;
        this.#reset = reset;
        for (let i = 0; i < initialSize; i++) this.#pool.push(factory());
    }
    
    acquire() {
        return this.#pool.length > 0 ? this.#pool.pop() : this.#factory();
    }
    
    release(obj) {
        this.#reset(obj);
        this.#pool.push(obj);
    }
}

const particlePool = new ObjectPool(
    () => ({ x: 0, y: 0, vx: 0, vy: 0, life: 0 }),
    (p) => { p.x = p.y = p.vx = p.vy = p.life = 0; }
);

// ===== 2. THROTTLE vs. DEBOUNCE — when to use which =====
// Debounce: delay execution until user STOPS doing something
// Use for: search input, form validation, resize handler END
const searchInput = document.querySelector('#search');
searchInput.addEventListener('input', debounce(fetchResults, 300));

// Throttle: ensure function runs AT MOST once per interval
// Use for: scroll position tracking, mouse move, game loop ticks
window.addEventListener('scroll', throttle(updateScrollIndicator, 100));

// ===== 3. WEB WORKER for CPU-intensive tasks =====
// main.js
const worker = new Worker('./heavy-worker.js');
worker.postMessage({ data: largeArray });
worker.onmessage = (e) => console.log('Result:', e.data.result);
worker.onerror = (e) => console.error('Worker error:', e.message);

// heavy-worker.js
self.onmessage = (e) => {
    const result = performHeavyCalculation(e.data.data);
    self.postMessage({ result });
};

// ===== 4. VIRTUAL SCROLLING (render only visible items) =====
// Instead of rendering 10,000 DOM nodes, only render ~20 visible items
// Libraries: react-window, react-virtualized, @tanstack/virtual

// ===== 5. LAZY INITIALIZATION =====
class ExpensiveService {
    #client = null;
    get client() {
        // Only create when first accessed
        if (!this.#client) this.#client = createExpensiveClient();
        return this.#client;
    }
}

// ===== 6. REQUESTIDLECALLBACK (non-urgent background work) =====
// Run non-critical work when browser is idle (no user interaction)
requestIdleCallback((deadline) => {
    while (deadline.timeRemaining() > 0 && tasks.length > 0) {
        const task = tasks.shift();
        task();
    }
    if (tasks.length > 0) requestIdleCallback(processQueue);
}, { timeout: 2000 });  // timeout: force execution after 2s if still idle

// ===== 7. structuredClone vs JSON vs Spread =====
const original = {
    date: new Date(),
    map: new Map([['key', 'value']]),
    set: new Set([1, 2, 3]),
    nested: { arr: [1, 2, 3] }
};

// JSON.parse(JSON.stringify()) — FAILS for: Date, Map, Set, undefined, functions, circular refs
JSON.parse(JSON.stringify(original));   // Date becomes string, Map/Set become {}!

// spread/Object.assign — SHALLOW copy only
const shallow = { ...original };
shallow.nested.arr.push(99);   // MUTATES original!

// structuredClone — DEEP clone, supports: Date, Map, Set, ArrayBuffer, TypedArray
const deep = structuredClone(original);   // ✅ Everything works correctly
// Cannot clone: Functions, DOM nodes, WeakMap/WeakSet, Promises

// ===== FAANG JavaScript TRICK QUESTIONS =====
// These come up frequently — know them cold:

// Q: What is the output?
console.log(typeof undefined);     // "undefined"
console.log(typeof null);          // "object" ← famous bug
console.log(typeof NaN);           // "number" ← counterintuitive
console.log(typeof function(){});  // "function"
console.log(typeof []);            // "object"
console.log(typeof {});            // "object"
console.log(typeof class{});       // "function" ← classes are functions!
console.log(typeof Symbol());      // "symbol"

// Q: Explain output
console.log(0.1 + 0.2 === 0.3);   // false (floating point)
console.log([] == ![]);            // true (coercion hell)
// [] == ![] → [] == false → [] == 0 → "" == 0 → 0 == 0 → true

console.log(null == undefined);    // true (special rule)
console.log(null === undefined);   // false (different types)

console.log(+"");     // 0
console.log(+[]);     // 0
console.log(+{});     // NaN
console.log(+null);   // 0
console.log(+undefined);  // NaN
console.log(+true);   // 1
console.log(+false);  // 0

// Q: What logs?
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i), 0);  // 3 3 3 (var)
for (let i = 0; i < 3; i++) setTimeout(() => console.log(i), 0);  // 0 1 2 (let)
