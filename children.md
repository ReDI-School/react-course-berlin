# Children

We’ve seen how JSX supports nesting components, just like HTML. And we’ve seen that custom components can accept *props* as arguments, and use those props to render content or pass along to child components.

There’s a special prop we haven’t talked about yet: it’s called `children`.

Let’s say you wanted to make a reusable `IconButton` component that looked something like this:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac4bc225-101f-4126-8d0a-3e685b605e5f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac4bc225-101f-4126-8d0a-3e685b605e5f/Untitled.png)

You might imagine using it like this:

```jsx
<IconButton>Do The Thing</IconButton>
```

What happens to that inner text, “Do The Thing”? Well, that’s where the `children` prop comes in. When React renders `IconButton`, it will pass all of its sub-elements (in this case, the text “Do The Thing”) into `IconButton` as a prop called `children`.

The children are automatically passed in, but they’re not automatically displayed anywhere. You have to explicitly place the children somewhere in your component. If you don’t, they’ll be ignored. For instance, if the `IconButton` component looked like this:

```jsx
function IconButton() {
  return (
    <button>
      <i className="target-icon" />
    </button>
  );
}
```

The rendered component would be a button with an icon, and *no text*. It would look like this:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12d3d00c-5bdf-46af-8683-9f7cfbd49d3b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12d3d00c-5bdf-46af-8683-9f7cfbd49d3b/Untitled.png)

With a small tweak – accepting the `children` prop and then inserting it after the icon – the passed-in children will be rendered where we want them:

```jsx
function IconButton({ children }) {
  return (
    <button>
      <i className="target-icon" />
			{children}
    </button>
  );
}
```

Notice how `children` is available as a prop in `IconButton` even though we didn’t explictly pass a prop named `children`. That’s because React automatically passes in the content of a component as the `children` prop. You could achieve the same result by using `IconButton` by explictly passing the `children` prop, instead:

```jsx
<IconButton children={"Do The Thing"}></IconButton>
```

It looks a bit unnatural, but it works.

The convention is to put children *inside* the tag, and not pass them explicitly as a prop, but I wanted to show you that `children` can be used like any other prop.

By the way, you can also pass JSX into a prop. If we wanted to make the button text italic, we could write it either of these ways:

```jsx
<IconButton children={<em>Just Do It</em>} />
<IconButton>
  <em>Just Do It</em>
</IconButton>
```

You can also pass JSX into *any* prop, not just the one named “children”.

As an example, you might write a `Confirmation` component that displays a dialog with OK and Cancel buttons. If you wanted to be able to customize the title and message of the dialog, you could write it to accept `title` and `message` as separate props and then use it like this:

```jsx
<Confirmation
  title={<h1>Are you sure?</h1>}
  message={<strong> Really really sure?</strong>}
/>;
```

## Different Types of Children

The `children` prop is always pluralized as `children` no matter whether there’s a single child or multiple children. This means that `children` can be a single element *or* an array, depending on what was passed in.

When there are multiple children, `children` will be an *array* of ReactElements.

When there is only one child, it is a *ReactElement object*.

This might seem a little weird: wouldn’t it be easier to work with if `children` was always an array?

Well, yes, it would. But `children` is used so often and the single-child use case is so common that the React team decided to optimize it by not allocating an array when there’s only one child.

## Dealing with the Children

React provides utility functions for dealing with this opaque data structure.

- `React.Children.map(children, function)`
- `React.Children.forEach(children, function)`
- `React.Children.count(children)`
- `React.Children.only(children)`
- `React.Children.toArray(children)`

The first two, `map` and `forEach`, work the same as the methods on JavaScript’s built-in `Array`. They accept `children`, whether it’s a single element or an array, and a function that’ll be called for each element. `forEach` iterates over the children and returns nothing, whereas `map` returns an array made up of the values you return from the function you provide.

`count` is pretty self-explanatory: it returns the number of items in `children`.

`toArray` is similarly intuitive: it converts `children` into a flat array, whether it was an array or not.

`only` returns the single child, or throws an error if there is more than one child.

You have access to every child element individually, so you can reorder them, remove some, insert new ones, pass the children down to further children, and so on.

## Customizing Children Before Rendering

In the example above, we only inserted some text. What if we wanted to do something more expressive, like creating our own custom component hierarchy?

Imagine that we constructed our own “API” of sorts for expressing a navigation header:

```jsx
<Nav>
  <NavItem url="/">Home</NavItem>
	<NavItem url="/about">About</NavItem>
  <NavItem url="/contact">Contact</NavItem>
</Nav>
```

Using the `children` prop, the `Nav` component can do things like insert a separator between each item:

```jsx
function Nav({ children }) {
  let items = React.Children.toArray(children);
  for (let i = items.length - 1; i >= 1; i--) {
    items.splice(
      i,
      0,
      <span key={i} className="separator">|</span>
    );
  }
  return <div>{items}</div>;
}
```

The code converts children into an array, then walks backward from the end as it inserts a new element between every existing element.

You will implement `NavItem` in the exercises coming up. It could render a simple link, or an icon next to a link, or anything you’d like.

## Exercises

Now that you’ve seen how the `children` prop works, here are some exercises to improve your understanding.

1. Make a component to display an “error box” that looks like this:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee3f911e-1000-4625-9567-40f61d1828e1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee3f911e-1000-4625-9567-40f61d1828e1/Untitled.png)

Invoking the component should look like this:

```jsx
<ErrorBox>
	Something has gone wrong
</ErrorBox>
```

Use the `children` prop to place the text properly. The image above uses [Bootstrap](http://getbootstrap.com/getting-started/#download-cdn) for styling and [Font Awesome](https://www.bootstrapcdn.com/fontawesome/) for the icon. You can add these libraries to your `public/index.html` file for the styling icon if you like, or just make a plain-looking box. It’s more important to get practice with the `children` prop than to get the style perfect.

1. Practice using the `React.Children.toArray` function with these next few exercises. You can put them all in a single app.
    1. Write a component called `FirstChildOnly` that accepts any number of children but only renders the first.
    2. Write a component called `LastChildOnly` that only renders its last child.
    3. Create a component named `Head` that takes a `number` prop, and renders the first [number] children. e.g. If you pass number=3, and 7 child elements, it will render the first 3.
    4. Create a component named `Tail` that takes a `number` and renders the last N children.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efcbb0dc-facf-4038-8779-630612a18542/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efcbb0dc-facf-4038-8779-630612a18542/Untitled.png)

Feel free to use Bootstrap’s modal styles, or create your own.

# Example: GitHub File List

You have a few components under your belt now. You’ve seen props and propTypes, and written some JSX.

Before we move into state and interactivity, I want to go through an example that incorporates everything we’ve seen so far. There’ll be a few new techniques covered here too.

So with that in mind, let’s create a new mini-app that replicates GitHub’s file list.

Just as we did with the Tweet example, we’ll follow a 4-step process:

1. Start with a sketch/mockup/screenshot
2. Break it into components
3. Name the components
4. Build it!

Here’s a screenshot of the GitHub UI which we’ll be modeling our app after:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2715ba76-56dd-4e29-a98f-d10816261cff/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2715ba76-56dd-4e29-a98f-d10816261cff/Untitled.png)

### Break It Into Components

The first step is to outline or highlight all the components in this screenshot. Basically what we’re doing here is drawing boxes around the elements in the design, be they `div`s or some other HTML building block.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2eee07b-4099-4c64-a842-1dcde1ffd893/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2eee07b-4099-4c64-a842-1dcde1ffd893/Untitled.png)

### Name the Components

Once we’ve decided which parts will be components, we can assign names to them. For the highlighted areas above, I came up with these names (indented to show the parent/child relationships):

- FileList
    - FileListItem
        - FileName
            - FileIcon
        - CommitMessage
        - Time

### Can You Reuse Anything?

Look through the list of components and see if you can save yourself work anywhere.

Hey, look! That `Time` component looks very similar to the one we wrote for `Tweet`. Luckily we wrote that `Time` component generically, to just accept a time instead of a tweet. We can reuse it here.

### What Data Does Each Component Need?

Next, let’s figure out what data each component needs to render. This will give us the props and propTypes for each one.

See if you can figure out the `propTypes` definition for each of these without looking ahead at the code.

The `FileList` should take one prop, `files`, which is an array of file objects.

The `FileListItem` will just take a single file object as the `file` prop. That object should have a name, type, a commit with a message, and a last-modified time.

`FileName` will take a `file` object and expect it to have a `name` property.

`FileIcon` will take a `file` object and use its `type` property to decide which kind of icon to show.

`CommitMessage` will take a `commit` object and expect it to have a `message` property.

Finally, `Time` will take an absolute `time` string. We’re going to reuse the `Time` component we made for `Tweet`, propTypes and all.

### Top-Down or Bottom-Up?

We’ll start from the top and work down for this one.

### Keep it Working

Have you ever had the experience of coding, head down, for a long chunk of time without ever running the code? Inevitably, there’s something wrong when you run it the first time.

It’s disappointing, and it takes the wind out of your sails. You end up tracking down a bunch of syntax errors, logic errors, and whatever else before you can see your hard work come to life.

So as you write, try to make small changes, and refresh often. Make sure the code always works.

### FileList

We’ve got enough direction to start working. Create a new project the same way we’ve done a few times now:

```
$ create-react-app github-file-list
$ cd github-file-list
$ rm src/*
$ touch src/index.js src/index.css
```

Copy the `index.html` file from the `tweet` project, from the “public” directory. This will set us up with the Font Awesome icons already loaded in. Change the `<title>` if you’d like, but aside from that, `index.html` is fine as it is.

Open up `src/index.js`.

Do you remember how to start off the file with the imports, and how to set up the initial render call with `ReactDOM.render`? Try to do it from memory. Look back at the Tweet example if you have trouble.

Create the `FileList` component. In the interest of doing the simplest thing that can possibly work, we’ll render a plain unordered list of file names. Once it works we’ll extract the list items into a `FileListItem` component.

```jsx
// put the imports here
const FileList = ({ files }) => (
  <table className="file-list">
    <tbody>
      {files.map((file) => (
        <tr className="file-list-item" key={file.id}>
          <td className="file-name"> {file.name} </td>
        </tr>
      ))}
    </tbody>
  </table>
);

const testFiles = [
  {
    id: 1,
    name: "src",
    type: "folder",
    updated_at: "2016-07-11 21:24:00",
    latestCommit: { message: "Initial commit" },
  },
  {
    id: 2,
    name: "tests",
    type: "folder",
    updated_at: "2016-07-11 21:24:00",
    latestCommit: { message: "Initial commit" },
  },
  {
    id: 3,
    name: "README",
    type: "file",
    updated_at: "2016-07-18 21:24:00",
    latestCommit: { message: "Added a readme" },
  },
];

// put the ReactDOM.render call here
// pass testFiles as FileList's file prop
```

It should look like this, nice and ugly:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be6bcf0e-8125-4859-a319-d77409b3ad9c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be6bcf0e-8125-4859-a319-d77409b3ad9c/Untitled.png)

Mapping over an array like this is how you render lists of things in React.

If you haven’t seen it before, Array’s `map` function returns a new array which is the same size as your existing array, but where each item is replaced by a new value. The function you provide to `map` decides, by returning a value, how to transform each item into a new value.

As the name “map” implies, it creates a “mapping” from your existing array to a new array. In our case, it returns a new array where each file has been turned into a table row with one cell showing the file name.

## The “key” Prop

The `key` prop on the `<tr>` is a special one, and it’s required any time you render an array of elements. React uses it to tell components apart when reconciling differences during a re-render. If you’d like more details about why keys are important, read the official docs on the [reconciliation algorithm](https://reactjs.org/docs/reconciliation.html).

Any time you use `map` to render an array, you’ll also need `key` on the topmost element. React consumes the `key` prop before rendering, so the component you pass `key` to will not actually receive that prop.

You as the developer need to decide what to pass in as the `key`. The important thing to keep in mind is that keys should be *stable*, *permanent*, and *unique* for each element in the array.

- **Stable**: An element should always have the same key, regardless of its position in the array. This means `key={index}` is a bad choice.
- **Permanent**: An element’s key must not change between renders. This means `key={Math.random()}` is a bad choice.
- **Unique**: No two elements should have the same key.

The item’s array index is not a good choice because if the index changes, for instance when an element is added to the front of the array, React’s mapping of indexes will become outdated. The item that was previously index “0” will now be “1”, but React doesn’t know that, and it can cause tough-to-diagnose rendering bugs. Duplicate items can appear, or items can appear out of order. So it’s important to choose keys wisely.

If an item has a unique ID attached to it, that’s a great choice for the key. If the items don’t have IDs, try combining several fields to create a unique identifier.

### FileListItem

It’s generally a good idea to create a standalone component to render the individual items in a list, so we’ll do that now. Even though this example is small, and it could be left alone, we’ll pull it out to demonstrate the process.

Notice that we still need to pass the `key` prop. The `key` needs to be decided at the time of the “map” and passed in right there. You can’t push that detail into the `FileListItem` component.

```jsx
const FileList = ({ files }) => (
  <table className="file-list">
    <tbody>
      {files.map((file) => (
        /* now we use FileListItem here */
				<FileListItem
          key={file.id}
          file={file}
        />
      ))}
    </tbody>
  </table>
);

const FileListItem = ({ file }) => (
  /* this code has been extracted from FileList */
	<tr className="file-list-item">
    <td className="file-name"> {file.name} </td>
  </tr>
);
```

We’ll also add some CSS to make it more presentable. Open up `src/index.css` and replace its contents with this code:

```css
.file-list {
     font-family: Helvetica, sans-serif;
     width: 100%;
     max-width: 980px;
     color: #333;
     margin: 0 auto;
     border: 1px solid #ccc;
     border-collapse: collapse;
}
 .file-list td {
     border-top: 1px solid #ccc;
}
 .file-name {
     padding: 4px;
     max-width: 180px;
}
```

Don’t forget to add the line to import the CSS file if you haven’t already (`import './index.css'`).

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2204c931-b5e2-46fa-b9d0-dcbc5d231ecf/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2204c931-b5e2-46fa-b9d0-dcbc5d231ecf/Untitled.png)

Now that we’ve got some basic structure in place, we can add the remaining components to the row component: `FileIcon`, `FileName`, `CommitMessage`, and `Time`.

Let’s take care of `FileIcon` and `FileName` first, since they’re nested together. According to the mockup, `FileName` is the parent of `FileIcon`. And since we’re putting them into a table, it would be nice to keep them in separate cells. Try to write the code yourself before looking ahead. There’s more than one right way to do it, so don’t worry if your code doesn’t look exactly like mine!

Here’s `FileIcon`. This one is straightforward – a stateless component written as a plain function.

```jsx
function FileIcon({ file }) {
  let icon = "fa-file-text-o";
  if (file.type === "folder") {
    icon = "fa-folder";
  }
  return (
    <td className="file-icon">
      {" "}
      <i className={`fa ${icon}`} />{" "}
    </td>
  );
}
```

Here is `FileName`, which uses the `<>` fragment syntax to wrap the two elements it returns. Remember that table cells (`<td>`) need to be direct children of table rows (`<tr>`) without any wrapper elements in between, which is why we need a fragment instead of a `div` here. (This rule comes from HTML, not React)

```jsx
function FileName({ file }) {
  return (
    <>
      <FileIcon file={file} /> <td className="file-name"> {file.name} </td>
    </>
  );
}
```

Then here’s the updated `FileListItem` that uses the `FileName` component:

```jsx
const FileListItem = ({ file }) => (
  <tr className="file-list-item">
    <FileName file={file} />
  </tr>
);
FileListItem.propTypes = { file: PropTypes.object.isRequired };
```

Now let’s add some CSS and see how it looks.

```css
.file-icon {
     width: 17px;
     padding-left: 4px;
}
 .file-icon .fa-folder {
     color: #508FCA;
}
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/418130c6-a57d-4f6a-9dda-1eed436e14e8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/418130c6-a57d-4f6a-9dda-1eed436e14e8/Untitled.png)

### CommitMessage

Let’s create the `CommitMessage` component next, and then we can render a `CommitMessage` inside `FileListItem`.

```jsx
const FileListItem = ({ file }) => (
  <tr className="file-list-item">
    <FileName file={file} /> <CommitMessage commit={file.latestCommit} />
  </tr>
);
FileListItem.propTypes = { file: PropTypes.object.isRequired };
const CommitMessage = ({ commit }) => (
  <td className="commit-message"> {commit.message} </td>
);
CommitMessage.propTypes = { commit: PropTypes.object.isRequired };
```

And spruce it up with some style:

```css
.commit-message {
     max-width: 442px;
     padding-left: 10px;
     overflow: hidden;
}
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6502e2e2-0c03-418b-b9e0-1d44b15b9da3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6502e2e2-0c03-418b-b9e0-1d44b15b9da3/Untitled.png)

Notice that we’re passing in the commit itself instead of the whole file object. `CommitMessage`doesn’t need to know anything about files, and the fewer components that have knowledge of data structures, the better.

### Time

We’ll add the time now. Remember that we can reuse the `Time` component from the Tweet exercise. Rather than just copy-and-paste the `Time` component into this file, we’ll extract it into its own file so other components can use it too.

We’re going to need Moment.js again, so install that now:

```
$ npm install moment
```

Then create the file `src/Time.js` and paste in the `Time` component from earlier. We also need to add `import`s at the top, and an `export` at the bottom.

```jsx
import React from "react";
import PropTypes from "prop-types";
import moment from "moment";
const Time = ({ time }) => {
  const timeString = moment(time).fromNow();
  return <span className="time"> {timeString} </span>;
};
Time.propTypes = { time: PropTypes.string.isRequired };
export default Time;
```

You might not recognize the `export default Time` syntax at the bottom. This is the ES6 way of making a component available so it can be `import`ed into other files. The “default” means that this is the component we’ll get when we use `import Time from './Time'`.

The alternative is to make this a *named export*, which would look like `export { Time }`, with the braces. Then the corresponding import would look like `import { Time } from './Time'`.

Imports are all about the braces. No braces? You’re importing the default. With braces? You’re importing a named export. You can even mix them:

```jsx
import React, { Component } from "react";
```

Think of it like destructuring, where the module is the “object” and you’re extracting named items from it.

Now let’s use `Time` inside `FileListItem`. Import it first by adding this line to the top of `index.js`:

```jsx
import Time from './Time';
```

Since this is our own file instead of something from `node_modules`, the path needs to be relative (`./Time`), instead of merely the module name (`Time`). It’s common to name files with components in PascalCase (with the leading capital letter) but you can name them however you prefer.

Then, we can update `FileListItem`, and `Time` doesn’t render a `<td>` so we have to wrap it in one:

```jsx
const FileListItem = ({ file }) => (
  <tr className="file-list-item">
    <FileName file={file} /> <CommitMessage commit={file.latestCommit} />
    <td className="age">
      <Time time={file.updated_at} />
    </td>
  </tr>
);
FileListItem.propTypes = { file: PropTypes.object.isRequired };
```

Add a little bit of styling…

```css
.age {
     width: 125px;
     text-align: right;
     padding-right: 4px;
}
```

And it works!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04fa38ac-7554-4142-8c71-aedb0140ce03/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04fa38ac-7554-4142-8c71-aedb0140ce03/Untitled.png)

I must say though, the code does not look very nice. We’ve got a mix where some `<td>`’s are inside components and some aren’t. It just doesn’t look consistent, and moreover, the components that contain table cells can only be used inside table rows – not very reusable at all.

A better way to organize it would be to leave the table “stuff” inside the table, and let the components worry about *just* their own data. Something like this:

```jsx
const FileListItem = ({ file }) => (
  <tr className="file-list-item">
    <td>
      <FileIcon file={file} />
    </td>
    <td>
      <FileName file={file} />
    </td>
    <td>
      <CommitMessage commit={file.latestCommit} />
    </td>
    <td>
      <Time time={file.updated_at} />
    </td>
  </tr>
);
FileListItem.propTypes = { file: PropTypes.object.isRequired };
```

At this point I could’ve gone back and edited the examples so that the code came out better. But I left it this way as an example: sometimes, maybe even often, the code won’t come out quite as clean as you imagined it. You might not fully realize the impact of a decision until you’ve lived with it for a while.

Reality often gets in the way. Sometimes you adhere too closely to the mockup, and take yourself down a path that results in awkward code. Or you try three different approaches in the same component and it becomes inconsistent and confusing.

But nothing is permanent! Now would be a perfect time to refactor this code (and in fact, you’ll do that in the exercises).

As you’re plugging away, writing your code… when that thought pops into your head that says, “Wait! These components won’t be reusable at all because every one of them contains a table cell!”… well, listen to that voice. Refactor early and often.

## Exercises

1. Refactor the GitHub file listing example so that none of the components return a table cell (`<td>`). Every component should return a `<span>` or `<div>` instead. This makes them more reusable, and should also improve the code inside `FileList`. Change the CSS if necessary.
2. Sometimes a file will contain a few related components when those components are always used together, and when they’re small (as in the `Nav`/`NavItem` example from earlier). But most of the time, in real applications, you’ll want to have only one component per file. Refactor the code from Exercise 1 to pull out components into separate files, using `import` and `export`.
3. Now you know how to create lists, using Array’s `.map` function. Reuse the `Tweet` component from earlier and create a list of Tweets.

Lists are all over the place. It’s been said that most web applications are basically just a bunch of lists. Implement these interfaces from sites around the web. Follow the same process we’ve done a few times now – highlight which pieces of the screen will be components, give them names, then build them.

**4. Trello**

Work on rendering a single list of cards. For more practice, render multiple lists of cards side-by-side.

(screenshot from [https://trello.com](https://trello.com/))

**5. Hacker News**

Implement the list of stories. For more practice, implement the header too.

(screenshot from [https://news.ycombinator.com/news](https://news.ycombinator.com/news))

**6. Pinterest**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/686eda47-df30-45a7-a9d2-86fae53f0180/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/686eda47-df30-45a7-a9d2-86fae53f0180/Untitled.png)

**7. InternetRadio genre cloud**

Can you come up with a nice way of sizing the buttons so they get progressively larger?

(screenshot from [https://www.internet-radio.com/](https://www.internet-radio.com/))
