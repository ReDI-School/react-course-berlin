Credit: [https://javascript.info/](https://javascript.info/), [https://www.freecodecamp.org/](https://www.freecodecamp.org/)

# Concepts

## [Iterate: forEach](https://javascript.info/array-methods#iterate-foreach)

The [arr.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) method allows to run a function for every element of the array.

The syntax:

```jsx
arr.forEach(function(item, index, array) {
	// ... do something with item
});
```

For instance, this shows each element of the array:

```jsx
// for each element call alert
["Bilbo", "Gandalf", "Nazgul"].forEach(alert);
```

And this code is more elaborate about their positions in the target array:

```jsx
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
	alert(`${item} is at index ${index} in ${array}`);
});
```

The result of the function (if it returns any) is thrown away and ignored.

## [Filter](https://javascript.info/array-methods#filter)

The `[find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)` method looks for a single (first) element that makes the function return `true`.

If there may be many, we can use [arr.filter(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

The syntax is similar to `find`, but `filter` returns an array of all matching elements:

```jsx
let results = arr.filter(function(item, index, array) {
		// if true item is pushed to results and the iteration continue
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

alert(someUsers.length); // 2
```

## [Transform an array: `map`](https://javascript.info/array-methods#map)

The [arr.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method is one of the most useful and often used.

It calls the function for each element of the array and returns the array of results.

The syntax is:

```jsx
let result = arr.map(function(item, index, array) {
	// returns the new value instead of item
});
```

For instance, here we transform each element into its length:

```jsx
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
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
