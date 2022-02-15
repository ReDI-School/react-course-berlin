# Arrow functions

Read more: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/)

Arrow functions are a different way of creating functions in JavaScript. Besides a shorter syntax, they offer advantages when it comes to keeping the scope of the this keyword (see here).

Arrow function syntax may look strange but it's actually simple.

```jsx
function callMe(name) {
	console.log(name)
}
```

which you could write as:

```jsx
const callMe = function(name) {
	console.log(name)
}
```

becomes:

```jsx
const callMe = (name) => {
	console.log(name)
}
```

**Important:x**

When having **no arguments**, you have to use empty parentheses in the function declaration:

```jsx
const callMe = () => {
	console.log('Max!')
}
```

When having **exactly one argument**, you may omit the parentheses:

```jsx
const callMe = name => {
	console.log(name);
}
```

When **just returning a value**, you can use the following shortcut:

```jsx
const returnMe = name => name
```

That's equal to:

```jsx
const returnMe = name => {
	return name;
}
```

# Resources

- Javascript.info: Arrow functions, the basics: [https://javascript.info/arrow-functions-basics](https://javascript.info/arrow-functions-basics)

# Exercises

- Rewrite with arrow functions: [https://javascript.info/task/rewrite-arrow](https://javascript.info/task/rewrite-arrow)
- [Use Arrow Functions to Write Concise Anonymous Functions](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-arrow-functions-to-write-concise-anonymous-functions)
- [Write Arrow Functions with Parameters](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/write-arrow-functions-with-parameters)
