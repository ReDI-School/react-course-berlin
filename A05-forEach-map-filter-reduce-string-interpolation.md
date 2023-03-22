Credit: [https://javascript.info/](https://javascript.info/), [https://www.freecodecamp.org/](https://www.freecodecamp.org/)

# Slides and Exercise

- 🖥 [Slides](https://docs.google.com/presentation/d/1SxWqPxy146xlfwiC_T1eJwssSmgoSGaudFY9zK85VOs)
- 📦 [forEach, map, reduce, filter exercises](https://codesandbox.io/s/js-for-react-foreach-map-reduce-filter-3pmzx)

# Concepts

## [Iterate: forEach](https://javascript.info/array-methods#iterate-foreach)

The [arr.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) method allows to run a function for every element of the array.

The syntax:

```js
forEach(function (element, index, array) { /* … */ }, thisArg)
```

where,

*element*
The current element being processed in the array.

*index*
The index of the current element being processed in the array.

*array*
The array forEach() was called upon.


**examples**

```js
// This shows each element of the array
["Bilbo", "Gandalf", "Nazgul"].forEach(element => console.log(element));

```

```js
// And this code is more elaborate about their positions in the target array
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
	console.log(`${item} is at index ${index} in ${array}`);
});
```

The result of the function (if it returns any) is thrown away and ignored.

## [Filter](https://javascript.info/array-methods#filter)

If there may be many, we can use [arr.filter(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

The syntax is similar to `find`, but `filter` returns an array of all matching elements:

```jsx
filter(function (element, index, array) { /* … */ }, thisArg)

let results = arr.filter(function(element, index, array) {
		// if true element is pushed to results and the iteration continue
	// returns empty array if nothing found
});
```

For instance:

```jsx
let users = [
	{id: 1, name: "John"},
	{id: 2, name: "Pete"},
	{id: 3, name: "Mary"}
];

// returns array of the first two users
let someUsers = users.filter(item => item.id < 3);

console.log(someUsers.length); // 2
```

## [Transform an array: `map`](https://javascript.info/array-methods#map)

The [arr.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method is one of the most useful and often used.

It calls the function for each element of the array and returns the array of results.

The syntax is:

```jsx
map(function (element, index, array) { /* … */ }, thisArg)
```

For instance, here we transform each element into its length:

```jsx
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
console.log(lengths); // 5,7,6
```

## [Reduction of an array: `reduce`](https://javascript.info/array-methods#reduce)

The [arr.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) method executes a user-supplied "reducer" callback function on each element of the array, in order, passing in the return value from the calculation on the preceding element. The final result of running the reducer across all elements of the array is a single value..

The syntax is:

```jsx
reduce((accumulator, currentValue, currentIndex, array) => { /* … */ }, initialValue)
```

Example to calculate sum of all elements of an array

```jsx
let sum = [1, 2, 3, 4].reduce((accumulator, currentValue) => accumulator + currentValue, 0);
console.log(sum); // 10
```

## All methods together

Credit: [https://kentcdodds.com/blog/javascript-to-know-for-react](https://kentcdodds.com/blog/javascript-to-know-for-react)

```jsx
const dogs = [
  {
    id: 'dog-1',
    name: 'Poodle',
    temperament: [
      'Intelligent',
      'Active',
      'Alert',
      'Faithful',
      'Trainable',
      'Instinctual',
    ],
  },
  {
    id: 'dog-2',
    name: 'Bernese Mountain Dog',
    temperament: ['Affectionate', 'Intelligent', 'Loyal', 'Faithful'],
  },
  {
    id: 'dog-3',
    name: 'Labrador Retriever',
    temperament: [
      'Intelligent',
      'Even Tempered',
      'Kind',
      'Agile',
      'Outgoing',
      'Trusting',
      'Gentle',
    ],
  },
]
dogs.find(dog => dog.name === 'Bernese Mountain Dog')
// {id: 'dog-2', name: 'Bernese Mountain Dog', ...etc}

dogs.map(dog => dog.name)
// ['Poodle', 'Bernese Mountain Dog', 'Labrador Retriever']

dogs.filter(dog => dog.temperament.includes('Faithful'))
// [{id: 'dog-1', ..etc}, {id: 'dog-2', ...etc}]

dogs.reduce((allTemperaments, dog) => {
  return [...allTemperaments, ...dog.temperaments]
}, [])
// [ 'Intelligent', 'Active', 'Alert', ...etc ]
```

# Exercises

`filter`:

- [https://javascript.info/task/filter-range](https://javascript.info/task/filter-range)
- [Use the filter Method to Extract Data from an Array](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/functional-programming/use-the-filter-method-to-extract-data-from-an-array)

`map`:

- [https://javascript.info/task/array-get-names](https://javascript.info/task/array-get-names)
- [https://javascript.info/task/map-objects](https://javascript.info/task/map-objects)

`reduce`:

- [https://javascript.info/task/reduce-object](https://javascript.info/task/reduce-object)
- [Use the reduce Method to Analyze Data](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/functional-programming/use-the-reduce-method-to-analyze-data)

All combined:

- [Use Higher-Order Functions map, filter or reduce to Solve a Complex Problem](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/functional-programming/use-higher-order-functions-map-filter-or-reduce-to-solve-a-complex-problem)
