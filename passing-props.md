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
