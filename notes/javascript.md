# JavaScript

- [JavaScript](#javascript)
  - [Intro](#intro)
    - [Numbers](#numbers)
    - [Comparison Operators](#comparison-operators)
    - [String](#string)
    - [Loops](#loops)
  - [Regular Expressions](#regular-expressions)
  - [Nullish coalescing operator](#nullish-coalescing-operator)
  - [Functions](#functions)
    - [Scope](#scope)
  - [Error](#error)
  - [Arrays](#arrays)
    - [Array: Basic Methods](#array-basic-methods)
    - [Array: Searching](#array-searching)
    - [Array: Sorting](#array-sorting)
    - [Array: Iteration](#array-iteration)
  - [Spread and Rest Operator](#spread-and-rest-operator)
  - [Object](#object)
    - [Object BluePrint](#object-blueprint)
    - [Extra Examples](#extra-examples)
  - [DOM Manipulation and Events](#dom-manipulation-and-events)
    - [DOM and Nodes](#dom-and-nodes)
    - [Events](#events)
    - [Event flow](#event-flow)
    - [Document](#document)
    - [Working with dimensions](#working-with-dimensions)
    - [Improving performance](#improving-performance)

## Intro

- To prevent initializing variables that haven't been declared use `"use strict"`
- Numbers are always stored as double precision floating point numbers
  - 0-51 => number or fraction
  - 52-62 => exponent
  - 63 => sign bit
- JS has ONLY two datatype for numbers: `Number`, `BigInt`
- Pass string to `Number()` to get NaN or the number
- `var` declared variables are either **function scoped** or **global scoped**. They are visible through blocks
  - It tolerates re-declarations
  - Declarations are hoisted (raised to top), but assignments are not
- False values:
  1. **false**
  2. **undefined**
  3. **null**
  4. **0**
  5. **NaN**
  6. an empty string
- We have two types of data types:
  1. **Primitives** can store only a single thing (number or string). Seven primitive data types:
     1. **number** for numbers of any kind: integer or floating-point, integers are limited by ±(253-1).
     2. **bigint** for integer numbers of arbitrary length.
     3. **string** for strings.
     4. **boolean** for true/false.
     5. **null** for unknown values – a standalone type that has a single value null.
     6. **undefined** for unassigned values – a standalone type that has a single value undefined.
     7. **symbol** for unique identifiers.
  2. **object** for more complex data structures.

> NOTE: `typeof null` returns **object** but it is an error in **typeof** and has been kept for backwards compatibility. Similarly there is no **function** type in js but **typeof** returns it which is also an error (such a freaking messy language!!)

- All javascript objects have a `toString()` method
- `!!variable` is equivalent to `Boolean(variable)` i.e both convert value to boolean
- `!` > `&&` > `||` is precedence order
- You can (but should not) use `||` and `&&` in place of if-else
- **Node** is a js runtime env that allows us to run js outside web browser

### Numbers

- **Number** type has three symbolic values: **Infinity**, **-Infinity**, **NaN**
- **BitInt** can represent an integer of arbitrary precision e.g `const bigInt = 1234n`
- `parseInt(string, radix)` and `Number(string)` parses a given string to integer
- `parseFloat(string)` parses a given string to float

### Comparison Operators

- `===` checks equality for both value and datatype
- `==` checks equality for value only

### String

- `at(pos)` & `charAt(pos)` are similar but the former supports negative indexing. `myString[pos]` property access can also be used
- `slice(start, end)`: end is exclusive. You can use negative indexing. End is optional
  - `substr(start, length)`: same as above except optional second parameter is length
  - `substring(start, end)`: same as above except -ves are treated as 0
- `padStart(targetLength, padString)` and `padEnd(targetLength, padString)` can be used to pad strings
- `repeat(count)`, `replace(regex, replacement)`, `replaceAll(regex, replacement)`, `split()`

### Loops

- `for...of` loops the values of any iterable
- `for...in` loops the properties of any object

## Regular Expressions

```js
const persons = `FirstName: John, LastName: Doe
FirstName: Jane, LastName: Smith`;

const re = /ab+c/; // compiled when script is loaded, can improve performance
const re = new RegExp("ab+c"); // compiled at runtime

const nameExtractor =
  /FirstName: (?<firstname>\w+), LastName: (?<lastname>\w+)/gm; // named capturing group
const matches = persons.matchAll(nameExtractor);
matches[0]["firstname"]; // matches are accessed by index and named captured groups by name
matches[1]["firstname"];

// back reference \k<Name>
/(?<title>\w+), yes \k<title>/; /* will match "Sir, yes Sir" */
```

## Nullish coalescing operator

- `a ?? b`: if `a` is **undefined** or **null** then `b` otherwise `a`
  - Equivalent to `(a != undefined && a != null) ? a : b`

## Functions

- Function declarations are always hoisted (raised to top).
  - On the contrary, **function expressions**, as in anonymous and arrow functions, are not hoisted
  - As a result, **function expression** is created when the execution reaches it and is usable only from that moment while as Function Declaration can be called earlier than it is defined.
- Default value of a function parameter is evaluated every time function is called and all arguments are not provided
- `arguments` is an array-like object accessible to functions and contains a list of passed arguments. It can be used to accept more than expected number of arguments. You should prefer [rest parameters](#spread-and-rest-operator)

### Scope

- **Scope** is the current context of execution in which values and expressions can be referenced
  - **Global Scope:** default scope
  - **Module Scope:** scope for code running in module mode
  - **Function Scope:** scope created with a function
  - **Block Scope:** scop created with a pair of curly braces

## Error

- It is type of object
- Common Error Types:
  - `ReferenceError`: thrown when we reference an undeclared/uninitialized variable
  - `SyntaxError`
  - `TypeError`: is thrown when **operand** or **argument of function** is incompatible with the expected type (like calling **charAt** on number) or when attempting to use a value in an inappropriate way (like change value of const variable or trying to call a variable)

## Arrays

- Arrays are special type of objects which use numbers to access its elements
- Array object has only one property: `length`
- `Array.isArray(obj)` or `arr instanceof Array` for array type confirmation
- `at(index)`: same as [] notation except we can use negative indices
- `delete arr[12]` will leave undefined hole at index 12 (not recommended)
- `Array.from(array-like, map-fn=e=>e)` will create an array from given array-like object

```js
[1, 2, 3]; // const recommended
Array(1, 2, 3); // not preferred
Array(100); // create an array with 100 undefined values
Array.from({ length: n }, (element, idx) => []); // create an 2d array of size n
Array.from("foobar", (e, i) => e.charousCodeAt(0)); // array of ascii codes of foobar
Array.from({ length: 20 }, () => Array(10)); // create a 2d 20*10 array
Array(10)
  .fill()
  .map((_) => Array(30)); // create a 2d 10*30 array
```

### Array: Basic Methods

- `fill(val, start, end)` fills array from `[start, end)` with `val`
- `push(obj)` adds new element to the end and returns the new array length
  - `arr[arr.length] = value` is alternative way
- `pop()` removes and returns the last element
- Shifting is equivalent to popping/pushing except it is for beginning
  - `shift(element)` adds element to the beginning of array
  - `unshift()` removes and returns element from the beginning of array
- `concat(...otherArrays)` joins the two arrays and returns new joined version
  - You can use [Spread operator](#spread-and-rest-operator) as well
- `copyWithin(target, start, end)` copy elements from **start** to **end** to index starting from **target**
  - `e.g [1,2,3,4,5].copyWithin(2,0)` will result in `[1,2,1,2,3]`
- `flat(depth)` reduces dimensionality of an array to the specified depth
- `splice(start, deleteCount, ...items)` add **items** (splice in) from **start** by removing **deleteCount** elements. It returns the array of removed elements
  - It can also be used to delete elements as `splice(2, 4)` will delete `i = 2,3,4`
  - `toSpliced(start, deleteCount, ...items)` same as above but instead of modifying the array it returns a new array
- `slice(start, end=length)` returns the slice from start to end - 1. It does not modify the array

### Array: Searching

- `indexOf(element, fromIndex=0)`, `lastIndexOf(element, fromIndex=0)`, `includes(element, fromIndex=0)`
- `find(function)`: returns the value of the first array element that passes a test function. `findIndex(function)`, `findLast(function)`, `findLastIndex(function)` are similar

### Array: Sorting

- `sort(function=def)`, `reverse(function=def)`, `toSorted(function=def)`, `toReversed(function=def)`

> Sort array in random order: sort((a, b) => 0.5 - Math.random())
> Above method will favor some elements (not much difference though)

### Array: Iteration

- Following functions accept callback function like `(val, idx, arr) => {}`
  - `forEach(function)`, `map(function)`, `flatMap(function)`, `filter(function)`
  - `some(function)`: does some values pass the test?
  - `every(function)`: do all values pass the test?
- `reduce((acc, curVal, curIdx, arr) => {}, initialValue=arr[0])`: initial value of accumulator is `arr[0]` unless specified
  - `reduceRIght(...)` is same but works from right-to-left
  - Notice arrow function is similar accept the first argument is accumulator
- `Array.from(array-like-object, mapFn=(e, i) => e)`: creates a new array from iterable
- `keys()` returns an index iterator (see example)
- `entries()` returns an iterator with index and value (see example)
- `with(idx, val)`: return new array with **val** at **idx**. It is same as bracket notation but does not modify the original array

```js
const nums = ["H", "E", "Y"];
nums.keys(); // [0, 1, 2] iterator
nums.entries(); // [[0, 'H'], [1,'E'], [2,'Y']]

for (let [i, e] of nums) console.log(`${e} is at ${i}`);
```

## Spread and Rest Operator

- Spread: Allows an iterable to be expanded where zero or more arguments (for function calls) or elements (for array literals) are expected.
- Rest: Rest parameter syntax allows a function to accept an indefinite number of arguments as an array
  - Unlike **arguments** object, **rest parameters** is a real array.

```js
const arr3 = [...arr1, ...arr2]; // combining arrays
const arr2 = [...arr1]; // copying arrays
sum(...arr3); // passing arguments to functions
function sum(...nums) {} // rest parameter syntax
function func(a, b, ...sees) {} // rest parameter syntax
```

## Object

- An Object has **properties** and **methods**

  - can be initialized using `Object()` or `Object.create()` or `{}` or `new` operator (see example way below)
  - All objects are instances of Object and inherit from its properties
  - Changes to **Object.prototype** can be seen by all objects unless they shadow/override
  - Object in js is essentially a collection of key-value of pairs

- OOPs concepts:

  - **Encapsulation**: Hide details and only expose essentials. Prevent direct access of properties
  - **Abstract**: Hide complexity
  - **Inheritance**: Reuse code
  - **Polymorphism**: (Many forms) for example each object has its own `render()` method.

- **this** keyword

  - reference to global object by default

    - in browser, global object is window object
    - in node, global object is global object

  - In js, functions are objects thus have following props and methods
    - `name`, `length`
    - The constructor for creating this object is `Function(args, code)` (see example)
    - `call(obj, arg1, arg2 ... argN)` or `apply(obj, ...arg)`: **this** in function references to `obj`

- Ways of declaring an Object:

  1. Object literal
  2. Factory function
  3. Constructor

- Some non-intuitive examples

```js
cat['sayName']()

// factory function
function createCat(name) {
  return {
    name,
    sayName: function() {
      console.log("meow");
    }
  }
}

// constructor function
function Cat(name) {
  this.name = name;
  this.sayName = function() {
    console.log("meow");
  }
  const age = 12; // private member
  // define getters and setters
  Object.defineProperty(this, 'age', {
    get: () => age,
    set: (newAge) => if(newAge >= 0) age = newAge
  });
}
const kitty = new Cat("kitty");

// delete property
delete cat.name
```

```js
// function is object with constructor Function()
function sayHi(greeting, name) {
  console.log(greeting + ", " + name);
  return 12;
}

// Other way to create
const sayHi = new Function(
  "name, greeting",
  `console.log(greeting + ', ' + name);
  return 12;`
);
```

### Object BluePrint

- If you want to create a template/blueprint of object then do these things otherwise object literal syntax is enough
- `this` keyword refers to the current object the code is being written inside
- `constructors` is just a function called using `new` keyword.

  - By convention they start with a capital letter

- When you do **new Func()** (**Func** is termed as **constructor**) following things happen when you call a constructor:
  - New **object** is created
  - **this** points to newly created object
  - constructor/Func code is executed which uses `this` keyword for work
  - **object** is returned (automatically, no explicit return statement is required)
- Every object has a property called constructor which points to the function used to create that object
  - e.g `kitty.constructor`
- When you use object literal syntax, js internally creates that object using `Object()` constructor function
  - Thus `{}` is eq to `new Object()`

```js
// method-1
function createUser(name) {
  const user = {};
  user.name = name;
  user.printName = () => console.log(user.name); // notice 'this' is not used as this will not be called with 'new'
  return user;
}
let user = createUser("junaid");

// method-2
function User(name) {
  this.name = name;
  this.printName = () => console.log(this.name);
}
let user = new User(); // notice that function does not return anything because 'new' takes care of all things
```

### Extra Examples

- Use `for...in` to loop over **keys**
- Use `Object.keys(obj)` to get array of **keys**
- Use `if...in` to check if property/key exists
- **Computed properties:** Compute the key first and then use it (see example)
- **Ordering of keys**: (see example)
  1. First, the keys that are integer strings (like '43', '0', '-12', etc), in ascending numeric order.
  2. Then, all other string keys and symbol keys, in the order in which they were added to the object. Note that keys like '+43', '43.12' etc are not integer keys. Integer keys are those whose string format and integer form is exactly same.

```js
let user = new Object(); // object constructor syntax
let user = {}; // object literal syntax

delete user.age; // delete a property


/****** How keys are handled ********/
let key = "apple";
let fruitColors = {
  // Computed property examples.
  // It is equivalent to fruitColors[key] = "red"
  [key]: "red",
  [key + "_" + "taste"]: "delicious",
  for: 12, // you can use reserved keywords as keys because it will be converted to "for": 12
  key, // property value shorthand value and is eq. to "key": key
  0: "zero", // automatically converted to '0': 'zero'
  test: undefined,
};

/****** Keys ordering in js ********/
let obj = {           //   ORDER
  cat: "kitty",           // 3 (INSERTION ORDER)
  12: "twelve",           // 2 (ASCENDING ORDER)
  -12: "minus twelve",    // 1 (ASCENDING ORDER)
  "+13": "plus thirteen", // 4 (INSERTION ORDER)
  sound: "meow",          // 5 (INSERTION ORDER)
  12.3: "some decimal",   // 6 (INSERTION ORDER)
};
```

## DOM Manipulation and Events

### DOM and Nodes

- DOM **(Document Object Model)** is an api for manipulating HTML document
- It represents html in tree like DS with nodes being elements, their attributes, etc. It is a live DS i.e any changes to it are reflected on page and any changes on page are reflected on DOM
  - Results of methods like `getElementsByTagName()` are alive but same is not true for `querySelectorAll()`
- In DOM `document.documentElement` is `html` tag and is the root of tree
- `window` object represents the browser window

---

- Every node in DOM has property `nodeType` (a number) which represents the type of node like **Node.ELEMENT_NODE**, **Node.TEXT_NODE**, etc
- Every node has a `parentNode`, `previousSibling`, `nextSibling`, `nodeValue`, `firstChild`, `lastChild`, `childNodes` (contains all child nodes), `children` (contains only Node.ELEMENT_TYPE nodes) etc. properties. Nodes like that of type Node.TEXT_NODE don't support some or all these operations so they may return `null` or throw error.

  - `nodeValue` contains **tag** name or **text** depending on node type
  - `remove()` remove this node from parent
  - `appendChild(child)`, `replaceChild(replacement, ref)`, `insertBefore(child, ref)` for adding new nodes

- A node can exist only at one place in document. So when you add an already existing node at another place, it is first removed from its original place and then added to new place
  - It includes all types of nodes so for example, if you add **text** node of one paragraph to another the first will completely lose its text
- Standard html attributes are present as properties and can be accessed by dot-notation.
  - Custom attributes can be set/get using `setAttribute`, `getAttribute` and `removeAttribute` methods
  - Prefer prefixing custom attributes with `data-`
- Learn more about Node API at [this MDN article](https://developer.mozilla.org/en-US/docs/Web/API/Node)
- Find more at [Javascript DOM in Depth](https://www.javascripttutorial.net/javascript-dom/)
- Script should be added at the end of **body** tag or in **head** tag as `<script src=... defer></script>`, otherwise DOM may not have parsed all html
- Nodes can be targeted with selectors
  - You can use any css-selector using `querySelector()` or `querySelectorAll()`
  - Relational selectors via properties like `firstElementChild`, `lastElementChild`
- Multiple nodes are returned as **NodeList** which is an array like object. Use `Array.from(nodelist)` or `[...nodelist]` to convert it to actual array
- `document.createElement(tagName, options=def)` creates a tag in memory which can be then added to DOM using `parentNode.appendChild(...)` or `parentNode.insertBefore(newNode, referenceNode)`
- `parentNode.removeChild(child)` is self explanatory
- CSS rule can be accessed in kebab-case like `div.style['background-color']` or in camel-case like `div.style.backgroundColor`
  - To get css from the cascade, `window.getComputedStyle(div)`. It is readonly
  - `classList.add`, `classList.remove`, `classList.toggle` to manipulate css classes. Later is preferred

### Events

- We can specify function attributes directly on HTML elements like `on[eventType]` as in `onclick`, `onmousedown`, etc
- There are three ways:
  1. `<button onclick="alert('')"></button>`
  2. `btn.onclick = () => alert('')`
  3. `btn.addEventListener('click', () => alert(''))`
- We can add multiple listeners using method-3 only
- In event handler, `this` keyword points to the element where event was fired
- Many events have default action, like `submit` button in forms, click on a link takes to the target etc. You can stop that from happening by using `event.preventDefault()`
- `input` elements fire **input** event when their content is changed
- When you press down mouse button `mousedown` event fires and when you release it `mouseup` event fires. `click` event follows `mouseup` event
- If you press mouse button on one element and move pointer to another element then the first will fire **mousedown** event and the later will fire **mouseup** event

### Event flow

- Even flow explains the order in which events are received on the page from the element where the even occurs and propagated through the DOM tree
- Events are processed in three phases in DOM:
  - Capturing Phase: document -> html -> body -> container
  - Target Phase: button (targets both capturing and bubbling handlers are called)
  - Bubbling phase: container -> body -> html -> document
- By default all events you add with `addEventListener` are in bubble phase. You can make it capturing handler like `addEventListener(action, function, true)`
- `event.stopPropagation()` stops bubbling/capturing (depending whether third parameter i.e capture is true or false) further in DOM

### Document

- Basics `document.title = 123`, `document.URL`, `document.domain` etc
- `document.all` is an array and nodes are in the order they were defined in html
- Use `document.forms`, `document.links`, `document.images`, etc to grab all things

### Working with dimensions

- [MDN article on dimensions](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model/Determining_the_dimensions_of_elements)
- All are integer values so rounding errors are expected
- **offsetWidth** and **offsetHeight** if you need to know the total amount of space an element occupies, including the width of the visible content, scrollbars (if any), padding, and border
- **clientWidth** and **clientHeight** if you need to know how much space the actual displayed content takes up, including padding but not including the border, margins, or scrollbars
- **scrollWidth** and **scrollHeight** if you need to know the actual size of the content, regardless of how much of it is currently visible
- The most effective way to find the precise position of an element on the screen is the **getBoundingClientRect** method. It returns an object with top, bottom, left, and right properties, indicating the pixel positions of the sides of the element relative to the top left of the screen

### Improving performance

- A program that repeatedly alternates between reading DOM layout information and changing the DOM forces a lot of layout computations to happen and will consequently run very slowly.
- So you should do all necessary calculations first and then modify DOM without reading further layout information
