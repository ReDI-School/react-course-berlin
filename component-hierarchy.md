# Example: Tweet Component

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a905a4f5-7759-400f-be5b-2be0813a6157/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a905a4f5-7759-400f-be5b-2be0813a6157/Untitled.png)

It’s rough on purpose: you don’t need a beautiful mockup. A simple pen-and-paper sketch works fine.

**2. Carve into Components**

The next step is to break this sketch into components. To do this, draw boxes around the “parts,” and think about reusability.

Imagine if you wanted to display 3 tweets, each with a different message and user. What would change, and what would stay the same? The parts that change would make good components.

Another strategy is to make every “thing” a component. Things like buttons, chunks of related text, images, and so on.

Try it yourself, then compare with this:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc66c100-6c76-4249-8bd6-c619d843cba6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc66c100-6c76-4249-8bd6-c619d843cba6/Untitled.png)

**3. Name the Components**

Now that we’ve broken the sketch into pieces, we can give them names.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af0d5399-f892-421f-9bea-27b510c7288e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af0d5399-f892-421f-9bea-27b510c7288e/Untitled.png)

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cbd65f6a-b1fd-4c3e-a825-f765a767e7c4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cbd65f6a-b1fd-4c3e-a825-f765a767e7c4/Untitled.png)

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79d1ee34-cee2-4aa8-b5e0-c5599e278371/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79d1ee34-cee2-4aa8-b5e0-c5599e278371/Untitled.png)

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4865c9e2-d6fc-4f1a-bdbc-751ed4ba3322/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4865c9e2-d6fc-4f1a-bdbc-751ed4ba3322/Untitled.png)

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e459055-b653-49df-af45-4bcef5735a63/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e459055-b653-49df-af45-4bcef5735a63/Untitled.png)

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

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3b39534-2e32-44ff-b436-61ecc197fc34/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3b39534-2e32-44ff-b436-61ecc197fc34/Untitled.png)

You can customize it to your heart’s content: change the name, the handle, the message, even the Gravatar icon. Pretty sweet, right?

“But… wait,” I hear you saying. “I thought we were going to build *reusable* components!” This tweet is pretty and all, but it’s only good for showing *one* message from *one* person…

Well don’t worry, because that’s what we’re going to do next: learn to parameterize components with props.
