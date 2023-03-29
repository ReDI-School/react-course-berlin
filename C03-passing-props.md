# Component Hierarchy
### Slides and Exercises 
- 🖥 [Slides](https://docs.google.com/presentation/d/1DvmaVRvEOMjpUeyK0T4ppLAMAfPjek8lmdE8C6j1Hqc/edit#slide=id.g9d90a069b6_0_349)
- 📦  [More ToDo app exercises](https://codesandbox.io/s/redi-week-05-exercises-forked-lb6bd?file=/src/ToDoApp.js)


### Example: Tweet Component

To learn to “think in components” you’ll need to build a few, and we’re going to start with a nice simple one. We’ll follow a 4-step process:

1. Make a sketch of the end result
2. Carve up the sketch into components
3. Give the components names
4. Write the code!

**1. Sketch**

Spending a few seconds putting pen (or pencil) to paper can save you time later. Even if you can’t draw (it doesn’t need to be pretty).

We’ll start with a humble sketch because it’s *concrete* and gives us something to aim for. In a larger project, this might come from a designer (as wireframes or mockups) but here, I recommend sketching it out yourself before writing any code.

Even when you can already visualize the end result, spend the 30 seconds and sketch it out on real dead-tree paper. It will help tremendously, especially for the complicated components.

There’s something satisfying about building a component from a drawing: you can tell when you’re “done.” Without a concrete picture of the end result, you’ll compare the on-screen component to the grand vision in your head, and it will never be good enough. It’s easy to waste a lot of time when you don’t know what “done” looks like. A sketch gives you a target to aim for.

Here is the sketch we’ll be building from:

![image](https://user-images.githubusercontent.com/51905839/154100134-ec5f1d9b-ad3b-40ab-abbf-ba9a65cb1db6.png)

It’s rough on purpose: you don’t need a beautiful mockup. A simple pen-and-paper sketch works fine.

**2. Carve into Components**

The next step is to break this sketch into components. To do this, draw boxes around the “parts,” and think about reusability.

Imagine if you wanted to display 3 tweets, each with a different message and user. What would change, and what would stay the same? The parts that change would make good components.

Another strategy is to make every “thing” a component. Things like buttons, chunks of related text, images, and so on.

Try it yourself, then compare with this:

![image](https://user-images.githubusercontent.com/51905839/154100166-7ce10b84-d130-4e50-893e-c0f6968becf0.png)

**3. Name the Components**

Now that we’ve broken the sketch into pieces, we can give them names.

![image](https://user-images.githubusercontent.com/51905839/154100203-9be05e44-7b9d-470d-98ed-e8e0a17008c4.png)

Each of these named items will become a component, with `Tweet` being the “parent” that groups them all together. The hierarchy looks like this:

- Tweet
    - Avatar
    - Author
    - Time
    - Message
    - ReplyButton
    - LikeButton
    - RetweetButton
    - MoreOptionsButton

**4. Build**

Now that we know what the component tree looks like, let’s get to building it! There are two ways to approach this.

### Top-Down, or Bottom-Up?

Option 1: Start at the top. Build the Tweet component first, then build its children. Build Avatar, then Author, and so on.

Option 2: Start at the bottom (the “leaves” of the tree). Build Avatar, then Author, then the rest of the child components. Verify that they work in isolation. Once they’re all done, assemble them into the Tweet component.

So what’s the best way? Well, it depends (doesn’t it *always*?).

For a simple hierarchy like this one, it doesn’t matter much. It’s easiest to start at the top, so that’s what we’ll do here.

For a more complex hierarchy, start at the bottom. Build small pieces, test that they work in isolation, and combine them as you go. This way you can be confident that the small pieces work, and, by induction, the combination of them should also work (in theory, anyway).

Writing small components in isolation makes them easier to unit test, too. Though we won’t cover unit testing in this book (learning how to use React is hard enough by itself!), when you’re ready, look into Jest and Enzyme for testing your components.

You’ll likely combine the top-down and bottom-up approaches as you build larger apps.

For example, if we were building the whole Twitter site, we might build the Tweet component top-down, then incorporate it into a list of tweets, then embed that list in a page, then embed that page into the larger application. The Tweet could be built top-down while the larger application is built bottom-up.

### Rewriting an Existing App

Another situation where building from the bottom is preferable is when you are converting an app to React. If you have an existing app written in another framework like AngularJS or Backbone and you want to rewrite in it React, starting at the top makes little sense, because it’ll have a ripple effect across your entire code base. You’d have to finish *all* of it before anything would work.

Starting at the bottom is manageable and controlled. You can build the “leaf nodes” of your app – the small, isolated pieces. Get those working, then build the next level up, and so on, until you reach the top. At that point you have the option to replace your current framework with React if you choose to.

The other advantage of bottom-up development in a rewrite is that it fits nicely with React’s one-way data flow paradigm. Since the React components occupy the bottom of the tree, and you’re guaranteed that React components only contain other React components, it’s easier to reason about how to pass your data to the components that need it.

### Build the Tweet Component

Here’s our blueprint again:

![image](https://user-images.githubusercontent.com/51905839/154100246-7a5fd155-97f9-48a9-a138-4b1963ccc7f1.png)

We’ll be building a plain static tweet in this section, starting with the top-level component, `Tweet`.

Create a new project with Create React App by running this command:

```
$ create-react-app static-tweet
$ cd static-tweet
```

Similar to before, we’ll delete some of the generated files and create our own empty `index.js`. Since we’ll need some style too, we’ll also create an empty `index.css`.

```
$ rm src/*
$ touch src/index.js src/index.css
```

The newly-generated project comes with an `index.html` file in the `public` directory. Add Font Awesome by putting this line inside the `<head>` tag (put it all on one line):

```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css">
```

Then open up our blank `src/index.css` file and replace its contents with this:

```css
.tweet {
  border: 1px solid #ccc;
	width: 564px;
	min-height: 68px;
	padding: 10px;
	display: flex;
	font-family: 'Helvetica', arial, sans-serif;
	font-size: 14px;
	line-height: 18px;
}
```

The `index.js` file will be very similar to the one from Hello World. It’s basically the same thing, with “Tweet” instead of “Hello World”. We’ll make it better soon, I promise. Replace the contents of `src/index.js` with this (don’t forget to type it out! Repetition is your friend, here):

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';

function Tweet() {
  return (
    <div className="tweet">
      Tweet
    </div>
  );
}

ReactDOM.render(<Tweet/>,
  document.querySelector('#root'));
```

That should do it. Start up the server, same as before, by opening up a command line terminal and running:

```
$ npm start
```

And the page should render something like this:

![image](https://user-images.githubusercontent.com/51905839/154100334-c1c431cd-7586-499a-a5cf-b9f31b9794b0.png)

This is nothing you haven’t seen before. It’s a simple component, with the addition of a special `className` attribute (which React calls a “prop”, short for property).

We’ll learn more about props in the next section, but for now, just think of them like HTML attributes. Most of them are named identically to the attributes you already know, but `className` is special in that its value becomes the `class` attribute on the DOM node.

One other new thing you might’ve noticed is the `import './index.css'` which is importing… a CSS file into a JavaScript file? Weird?

What’s happening is that behind the scenes, when Webpack builds our app, it sees this CSS import and learns that `index.js` depends on `index.css`. Webpack reads the CSS file and includes it in the bundled JavaScript (as a string) to be sent to the browser.

You can actually see this, too – open the browser console, look at the Elements tab, and notice under `<head>` there’s a `<style>` tag that we didn’t put there. It contains the contents of `index.css`.

Back to our outline. Let’s build the `Avatar` component.

Later on we’ll look at extracting components into files and importing them via `import`, but to keep things simple for now, we’ll just put all the components in `index.js`. Add the `Avatar` component to `index.js`:

```jsx
function Avatar() {
  return (
    <img
      src="https://www.gravatar.com/avatar/nothing"
      className="avatar"
      alt="avatar"
    />
  );
}
```

If you want to use your own Gravatar, go to [daveceddia.com/gravatar](https://daveceddia.com/gravatar) to figure out its URL.

Next we need to include `Avatar` in the `Tweet` component:

```jsx
function Tweet() {
  return (
    <div className="tweet">
      <Avatar/>
      Tweet
    </div>
  );
}
```

Now just give `Avatar` some style, in `index.css`:

```css
.avatar {
  width: 48px;
  height: 48px;
  border-radius: 5px;
  margin-right: 10px;
}
```

It’s getting better:

![image](https://user-images.githubusercontent.com/51905839/154100388-8c522d3c-e93e-4b98-ae78-c130533cf8d8.png)

Next we’ll create two more components, the `Message` and `Author`:

```jsx
function Message() {
  return (
    <div className="message">
      This is less than 140 characters.
    </div>
  );
}

function Author() {
  return (
    <span className="author">
      <span className="name">Your Name</span>
      <span className="handle">@yourhandle</span>
    </span>
  );
}
```

If you refresh after adding these, nothing will have changed because we still need to update `Tweet`to use these new components, so do that next:

```jsx
function Tweet() {
  return (
    <div className="tweet">
      <Avatar/>
      <div className="content">
        <Author/>
        <Message/>
      </div>
    </div>
  );
}
```

It’s rendering now, but it’s ugly. Spruce it up with some CSS for the name and handle:

```css
.name {
  font-weight: bold;
	margin-bottom: 0.5em;
  margin-right: 0.3em;
}
.handle {
  color: #8899a6;
  font-size: 13px;
}
```

![image](https://user-images.githubusercontent.com/51905839/154100458-3a0de717-07dc-4f32-9e6f-69d07754db6f.png)

It’s looking more like a real tweet now!

Next up, add the `Time` and the buttons (we’ll talk about the new syntax in a second – just type these in as shown):

```jsx
const Time = () => <span className="time"> 3h ago</span>;
const ReplyButton = () => <i className="fa fa-reply reply-button" />;
const RetweetButton = () => <i className="fa fa-retweet retweet-button" />;
const LikeButton = () => <i className="fa fa-heart like-button" />;
const MoreOptionsButton = () => (
  <i className="fa fa-ellipsis-h more-options-button" />
);
```

These components don’t look like the `function`s you’ve been writing up to this point, but they are in fact still functions. They’re *arrow functions*. Here’s a progression from a regular `function` to an arrow function so you can see what’s happening:

```jsx
// Here's a normal function component:
function LikeButton() {
  return <i className="fa fa-heart like-button" />;
}

// It can be rewritten as an
// anonymous function and stored in a
// (constant) variable:
const LikeButton = function () {
  return <i className="fa fa-heart like-button" />;
};

// The anonymous function can be
// turned into an arrow function:
const LikeButton = () => {
  return <i className="fa fa-heart like-button" />;
};

// It can be simplified by removing
// the braces and the `return`:
const LikeButton = () => <i className="fa fa-heart like-button" />;

// And if it's really simple,
// you can even write it on one line:
const Hi = () => <span>Hi</span>;
```

Arrow functions are a nice concise way of writing components.

Notice how there’s no `return` in the last couple versions: when an arrow function only contains one expression, it can be written without braces. And *when it’s written without braces*, the single expression is implicitly returned.

We’ll continue to use arrow functions throughout the book, so you’ll get lots of practice. Don’t worry if they look foreign now. Your eyes will adapt after writing them a few times. You’ll get used to them, I promise.

Also: arrow functions don’t “replace” regular functions, and aren’t strictly “better than” functions. They’re just different. For function components, they’re effectively interchageable. Personally, I tend to write `function` when the component is a bit larger, and use a `const () => (...)` when it’s only a couple lines. Some people prefer to write arrow functions everywhere. Use what you like.

### The let and const Keywords

If you’re familiar with languages like C or Java, you’re familiar with block scoping. As you may know, JavaScript’s `var` is actually *function-scoped*, not block-scoped. This has long been an annoyance (especially in `for` loops). But no more!

ES6 added `let` and `const` as two new ways of declaring block-scoped variables.

The `let` keyword defines a mutable (changeable) variable. You can use it instead of `var` almost everywhere.

The `const` defines a constant. It will throw an error if you try to reassign the variable, but it’s worth noting that it does not prevent you from modifying the data *within* that variable. Here’s an example:

```jsx
const answer = 42;
answer = 43; // error!

const numbers = [1, 2, 3];
numbers[0] = "this is fine"; // no error
```

Using `const` is more of a signal of intent than a bulletproof protection scheme, but it is still worthwhile.

### Add the Buttons and the Time

Now that we’ve got all those new components, update `Tweet` again to incorporate them:

```jsx
function Tweet() {
  return (
    <div className="tweet">
      <Avatar />
      <div className="content">
        <Author />
        <Time />
        <Message />
        <div className="buttons">
          <ReplyButton />
          <RetweetButton />
          <LikeButton />
          <MoreOptionsButton />
        </div>
      </div>
    </div>
  );
}
```

Finally, add a few more styles to cover the time and buttons:

```css
.time {
     padding-left: 0.3em;
     color: #8899a6;
}
 .time::before {
     content: "\00b7";
     padding-right: 0.3em;
}
 .buttons {
     margin-top: 10px;
     margin-left: 2px;
     font-size: 1.4em;
     color: #aab8c2;
}
 .buttons i {
     width: 80px;
}
```

And here we have a fairly respectable-looking tweet!

![image](https://user-images.githubusercontent.com/51905839/154100542-1c5630fa-d27c-492d-9271-76ae7967d7c0.png)

You can customize it to your heart’s content: change the name, the handle, the message, even the Gravatar icon. Pretty sweet, right?

“But… wait,” I hear you saying. “I thought we were going to build *reusable* components!” This tweet is pretty and all, but it’s only good for showing *one* message from *one* person…

Well don’t worry, because that’s what we’re going to do next: learn to parameterize components with props.


# Props

Where HTML elements have “attributes,” React components have “props” (short for “properties”). It’s a different name for essentially the same thing.

Think about how you would customize a plain function. This might seem a bit basic but bear with me. Let’s say you have this function:

```jsx
function greet() {
  return "Hi Dave";
}
```

It works great, but it’s pretty limited since it will always return “Hi Dave”. What if you want to greet someone else? You’d pass in their name as an argument:

```jsx
function greet(name) {
  return "Hi " + name;
}
```

Where functions have arguments, components have *props*. Props let you pass data to your components.

## Passing Props

Here is a bit of JSX passing a prop called `name` with a string value of `"Dave"` into the `Person`component:

```jsx
<Person name='Dave'/>
```

Here’s another example, passing a `className` prop with the value `"person"`:

```jsx
<div className='person'/>
```

**JSX uses `className` instead of `class` to specify CSS classes.** You will forget this over and over. You will type “class” out of habit, and React will warn you about it. That’s fine. Just change it to `className`.

Notice how the `div` is self-closing? This ability isn’t just for `<input/>` anymore: in JSX, *every*component can be self-closing. In fact, if the component has no children (no contents), the convention is to write it like this, instead of using a closing tag like `<div></div>`.

In this next component, we’re passing a string for `className`, a number for the `age` prop, and an actual JavaScript expression as the `name`:

```jsx
function Dave() {
  const firstName = "Dave";
  const lastName = "Ceddia";
  return (
    <Person className="person" age={33} name={firstName + " " + lastName} />
  );
}
```

Remember that in JSX, singles braces must surround JavaScript expressions. The code in the braces is real JavaScript, and it follows all the same scoping rules as normal JavaScript.

It’s important to understand that the JS inside the braces must be an *expression*, not a statement. Here are a few things you can do inside JSX expressions:

- Math, concatenation: `{7 + 5}` or `{'Your' + 'Name'}`
- Function calls: `{this.getFullName(person)}`
- Ternary (?) operator: `{name === 'Dave' ? 'me' : 'not me'}`
- Boolean expressions: `{isEnabled && 'enabled'}`

Here are some things you cannot do:

- Define new variables with `let`, `const`, and `var`
- Use `if`, `for`, `while`, etc.
- Define functions with `function`

Remember that JSX evaluates to JavaScript, which means the props become keys and values in an object. Here’s that example from above, transformed into JavaScript:

```jsx
function Dave() {
  const firstName = "Dave";
  const lastName = "Ceddia";
  return React.createElement(
    Person,
    { age: 33, name: firstName + " " + lastName, className: "person" },
    null
  );
}
```

All of the rules that apply to function arguments apply to JSX expressions. Could you call a function like this?

```jsx
myFunc(const x = true; x && 'is true');
```

Of course not! That looks completely wrong. If you tried to pass that argument to a JSX expression, this is what you’d get:

```jsx
<Broken value={  const x = true; x && 'is true'}/>
// gets compiled to:
React.createElement(Broken, {
  value: const x = true; x && 'is true'
}, null);
```

So when you’re trying to decide what to put in a JSX expression, ask yourself, “Could I pass this as a function argument?”

## Receiving Props

Props are passed as an object, and are the first argument to a component function, like this:

```jsx
function Hello(props) {
  return <span>Hello, {props.name}</span>;
}

// Used like:
<Hello name="Dave" />;
```

It works the same way for arrow functions:

```jsx
const Hello = (props) => <span>Hello, {props.name}</span>;
```

ES6 has a new syntax called *destructuring* which makes props easier to work with. It looks like this:

```jsx
const Hello = ({ name }) => <span>Hello, {name}</span>;
```

You can read `{ name }` as “extract the ‘name’ key from the object passed as the first argument, and put it in a variable called `name`”. You can extract multiple keys at the same time, too:

```jsx
const Hello = ({ firstName, lastName }) => (
  <span>
    Hello, {firstName} {lastName}
  </span>
);
```

In practice, props are very often written using this destructuring syntax. Just so you know, it works outside of function arguments as well. You can destructure props this way, for instance:

```jsx
const Hello = (props) => {
  const { name } = props;
  return <span>Hello, {name}</span>;
};
```

Remember, arrow functions need a `return` if the body is surrounded by braces, and it needs braces if the body contains multiple lines.

### Modifying Props

One important thing to know is that props are *read-only*. Components that receive props must not change them.

If you come from a framework that has two-way binding this is a change.

In React, data flows *one way*. Props are read-only, and can only be passed *down* to children.

## Communicating With Parent Components

If you can’t change props, but you need to communicate something up to a parent component, how does that work?

When a child needs to send data to its parent, the parent can send down a *function* as a prop, like this:

```jsx
function handleAction(event) {
  console.log("Child did:", event);
}
function Parent() {
  return <Child onAction={handleAction} />;
}
```

The `Child` component receives the `onAction` prop, which it can call whenever it needs to send up data or notify the parent that something happened.

One place where it’s common to pass functions as props is for handling events. For instance, the standard `button` element accepts an `onClick` prop, which it’ll call when the button is clicked.

```jsx
function Child({ onAction }) {
  return <button onClick={onAction} />;
}
```

We’ll learn more about event handling later on.

# Example: Tweet With Props

Now that you have a basic understanding of props, let’s see how they work in practice.

We’ll take the static `Tweet` example from Chapter 5 and rework it to display dynamic data by using props.

For this example, make a copy of the `static-tweet` project folder so that we can work without fear of breaking the old code. We’ll also start up a development server (stop the old one if it’s still running).

```
$ cp -a static-tweet props-tweet
$ cd props-tweet
$ npm start
```

**Note: Don’t use `cp -r` since it does not preserve symlinks, and can break `npm start`.**

If, after copying the project, the `npm start` command fails, delete the `node_modules` folder and run `npm install`. Then try `npm start` again.

Open up `src/index.js`. To begin with, update the `Tweet` component to accept a `tweet` prop as shown below. Then add the `testTweet` object, which will serve as our fake data, and update the call to `ReactDOM.render` to pass the `testTweet` object as the `tweet` prop.

Refresh the page after making these changes and make sure everything looks the same as before (nothing should be different yet).

```jsx
// add the { tweet } prop, destructured
function Tweet({ tweet }) {
  return (
    <div className="tweet">
      <Avatar />
      <div className="content">
        <Author /> <Time /> <Message />
        <div className="buttons">
          <ReplyButton />
					<RetweetButton />
					<LikeButton />
					<MoreOptionsButton />{" "}
        </div>
      </div>
    </div>
  );
}
// ...
const testTweet = {
  message: "Something about cats.",
  gravatar: "xyz",
  author: { handle: "catperson", name: "IAMA Cat Person" },
  likes: 2,
  retweets: 0,
  timestamp: "2016-07-30 21:24:37",
};
ReactDOM.render(<Tweet tweet={testTweet} />, document.querySelector("#root"));
```

### Avatar

Let’s start converting the static components to accept props, starting with `Avatar`. In the render method of `Tweet`, replace this line:

```jsx
<Avatar/>
```

With this line:

```jsx
<Avatar hash={tweet.gravatar}/>
```

This passes the tweet’s `gravatar` property into the `hash` prop. Now update `Avatar` to use this new prop:

```jsx
function Avatar({ hash }) {
  const url = `https://www.gravatar.com/avatar/${hash}`;
  return <img src={url} className="avatar" alt="avatar" />;
}
```
