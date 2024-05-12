# Arrays

## Basics

- Array-like DS: Collections of data which are ordered by an index value
- No explicit array data type but `Array` object can be used to work with arrays
  - Array elements are properties (see example 3)
  - `arr['length']` will also return length of array
- Use **array literal** or `Array.of(...elements)` for creating single element array of arbitary data type (see example 2)

```js
// 1. Both are equivalent
const arr1 = new Array(10);

const arr1 = [];
arr1.length = 10;

// 2. Creating array of single element
Array(10); // creates an empty array with length 10
Array.of(10); // creates an array with element 10
[10]; // creates an array with element 10

// 3. Array elements are properties
arr["1"] === arr[1]; // always true
arr["length"] === arr.length; // always true
arr[12.2] = "Help"; // valid statement

// 4. Different way of looping If all values in array evaluate to true
for (let i = 0, div; (div = divs[i]); i++) {}
```

## Length

- JS: Stores elements as std object properties with array index as the property name
- `arr.length`
  - Always positive
  - Always greater than the index of last element
- `length` property can be modified
  - Setting it to zero empties the array
  - Setting it to shorter values, truncates the array

## Sparse Arrays

- 'empty slots' are not same as 'undefined'
- Unassigned values or empty slots are not iterated in a `array iteration methods` (like forEach, map, filter, some, etc) loop.
- Other than in above case, most of the time they behave as `undefined` values like in `for...of`, `spreading`, `indexing`

```js
Array(5) // [ <5 empty items> ]
[1, 2, , , 5] // [ 1, 2, <2 empty items>, 5 ]

const c = [1, 2]
c[4] = 6 // [1, 2, <2 empty items>, 6]
c.length = 6 // [1, 2, <2 empty items>, 6, <1 empty item>]
delete c[0] // [<1 empty item>, 2, <2 empty items>, 6, <1 empty item>]
```
