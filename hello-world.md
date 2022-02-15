At this point you have `node` and `npm` installed. All of the tools are ready. Let’s write some code!

### Step 1

Use `create-react-app` (“CRA”) to generate a new project. CRA will create a directory and install all the necessary packages, and then we’ll move into that new directory.

```
$ create-react-app react-hello
$ cd react-hello
```

The generated project comes with a prebuilt demo app. We’re going to delete that and start fresh. Delete the files under the `src` directory, and create an empty new `index.js` file.

```
$ rm src/*
$ touch src/index.js
```

### Step 2

Open up the brand new `src/index.js` file, and type this code in:

> Type it out by hand? Like a savage? Typing it drills it into your brain much better than simply copying and pasting it. You’re forming new neuron pathways. Those pathways are going to understand React one day. Help ’em out.
> 

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
function HelloWorld() {
	return (
    <div>Hello World!</div>  
	);
}

ReactDOM.render(
  <HelloWorld/>,  document.querySelector('#root')
);
```

The `import` statements at the top are an ES6 feature. These lines will be at the top of every `index.js` file that we see in this book.

Unlike with ES5, we can’t simply include a `<script>` tag and get `React` as a global object. So, the statement `import React from 'react'` creates a new variable called `React` with the contents of the `react` module.

The strings ‘react’ and ‘react-dom’ are important: they correspond to the names of modules installed by npm. If you’re familiar with Node.js, `import React from 'react'` is equivalent to `const React = require('react')`.

### Step 3

From inside the `react-hello` directory, start the app by running this command:

```
$ npm start
```

A browser will open up automatically and display “Hello World!”

### How the Code Works

Let’s start at the bottom, with the call to `ReactDOM.render`. That’s what actually makes this work. This bit of code is regular JavaScript, despite the HTML-looking `<HelloWorld/>` thing there. Try commenting out that line and watch how Hello World disappears.

React uses the concept of a *virtual DOM*. It creates a representation of your component hierarchy and then *renders* those components by creating real DOM elements and inserting them where you tell it. In this case, that’s inside the element with an id of `root`.

`ReactDOM.render` takes 2 arguments: what you want to render (your component, or any other React Element) and where you want to render it into (a real DOM element that already exists).

```jsx
ReactDOM.render(
  [React Element],  [DOM element]
);
```

Above that, we have a *component* named `HelloWorld`. The primary way of writing React components is as plain functions like this. Most people call them “function components” but you might also see them called “functional components” or “stateless function components” (SFC for short).

There are 2 other ways to create components: ES6 classes, and the now-deprecated `React.createClass`. You may still see the `createClass` style in old projects or Stack Overflow answers, but it’s not in common use anymore. Primarily we’ll be writing components as functions.

The HTML-like syntax inside the `render` function is called *JSX*, and we’ll cover that next.
