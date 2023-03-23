Credit: [javascript.info](http://javascript.info), [freecodecamp.org](http://freecodecamp.org)

# Exercise
- 🖥  [Slides](https://docs.google.com/presentation/d/1SxWqPxy146xlfwiC_T1eJwssSmgoSGaudFY9zK85VOs)
- 📦 [Object destructuring exercise](https://codesandbox.io/s/object-destructuring-xlj5l?file=/src/App.js)
- 📦 [Array destructuring exercise](https://codesandbox.io/s/array-destructuring-335bk?file=/src/App.js)
- 📦 [Destructuring and spread exercises](https://codesandbox.io/s/js-for-react-destructuring-and-spread-d1of5)
- 📦 [Slides examples in sandbox](https://codesandbox.io/p/sandbox/priceless-sky-icjjit?file=%2FREADME.md)


# Spread & Rest Operator

The spread and rest operators actually use the same syntax: `...`

Yes, that is the operator - just three dots. It's usage determines whether you're using it as the spread or rest operator.

**Using the Spread Operator:**

The spread operator allows you to pull elements out of an array (=> split the array into a list of its elements) or pull the properties out of an object. Here are two examples:

```jsx
const oldArray = [1, 2, 3];
```

```jsx
const newArray = [...oldArray, 4, 5];
// This now is [1, 2, 3, 4, 5];
```

Here's the spread operator used on an object:

```jsx
const oldObject = {
	name: 'Max'
};
const newObject = {
	...oldObject,
	age: 28,
}
```

`newObject` would then be

```jsx
{
	name: 'Max',
	age: 28
}
```

The spread operator is extremely useful for cloning arrays and objects. Since both are [reference types (and not primitives)](https://www.youtube.com/watch?v=9ooYYRLdg_g&feature=youtu.be), copying them safely (i.e. preventing future mutation of the copied original) can be tricky. With the spread operator you have an easy way of creating a (shallow!) clone of the object or array.

# **Destructuring**

Destructuring allows you to easily access the values of arrays or objects and assign them to variables.

Here's an example for an array:

```jsx
const array = [1, 2, 3];
const [a,b] = array;
console.log(a); // prints 1
console.log(b); // prints 2
console.log(array); // prints [1, 2, 3]
```

And here for an object:

```jsx
const myObj = {
	name: 'Max',
	age: 28,
}
const { name } = myObj;
console.log(name); // prints 'Max'
console.log(age); // prints undefined
console.log(myObj); // prints {name: 'Max', age: 28}
```

Destructuring is very useful when working with function arguments. Consider this example:

```jsx
const printName = (personObj) => {
	console.log(personObj.name);
}
printName({name: 'Max', age: 28}); //prints 'Max'
```

Here, we only want to print the name in the function but we pass a complete person object to the function. Of course this is no issue but it forces us to call `personObj.name` inside of our function. We can condense this code with
destructuring:

```jsx
const printName = ({name}) => {
 console.log(name);
}
printName({name: 'Max', age: 28}); // prints 'Max'
```

We get the same result as above but we save some code. By destructuring, we simply pull out the `name` property and store it in a variable/ argument named `name` which we then can use in the function body.

# Resources:

- [Javascript.info: Rest parameters and spread syntax](https://javascript.info/rest-parameters-spread)
- [Javascript.info: Destructuring assignment](https://javascript.info/destructuring-assignment)

# Exercises

- [Use the Rest Parameter with Function Parameters](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-the-rest-parameter-with-function-parameters)
- [Use Destructuring Assignment to Extract Values from Objects](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-to-extract-values-from-objects)
- [Use Destructuring Assignment to Assign Variables from Objects](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-to-assign-variables-from-objects)
- [Use Destructuring Assignment to Assign Variables from Nested Objects](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-to-assign-variables-from-nested-objects)
- [Use Destructuring Assignment to Assign Variables from Arrays](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-to-assign-variables-from-arrays)
- [Use Destructuring Assignment with the Rest Parameter to Reassign Array Elements](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-with-the-rest-parameter-to-reassign-array-elements)
- [Use Destructuring Assignment to Pass an Object as a Function's Parameters](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-destructuring-assignment-to-pass-an-object-as-a-functions-parameters)
- [The maximal salary](https://javascript.info/task/max-salary)
