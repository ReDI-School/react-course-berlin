# Setup Computer and Environment

Before we dive in, we’ll need to set up an environment.

Don’t worry, there’s no boilerplate to clone from GitHub. No Webpack config, either.

Instead, we’re using Create React App, a tool Facebook made. It provides a starter project and built-in build tools so you can skip to the fun part – creating your app!

### Prerequisites

### Tools

- Node.js (at least 8.10.0)
- NPM (ideally version >= 5.2)
- Google Chrome, Firefox, or some other modern browser
- React Developer Tools
- Your text editor or IDE of choice

You can use [nvm](https://github.com/nvm-sh/nvm#installation-and-update) to install Node and NPM on macOS & Linux, or [nvm-windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) on Windows. You can also download an installer from [nodejs.org](https://nodejs.org/), but the advantage of nvm is that it makes it very easy to upgrade your version of Node in the future.

Any modern browser should suffice. This book was developed against Chrome, but if you prefer another browser, it will probably work fine.

### Create React App

Throughout this book, you’ll be creating a lot of little projects with Create React App. Because the `create-react-app` command very rarely needs to be updated, I suggest installing the tool permanently, by running:

```
npm install -g create-react-app
```

If you don’t want to install an extra tool, you can use the `npx` command to create your projects, as in `npm create-react-app my-project-name`. The upside of that is that you don’t need to install a tool. The downside is that every command will take extra time because it needs to install Create React App from scratch every time.

Even without ever updating the global `create-react-app` command, it is designed to always pull down the latest version of React when it creates a new project. You don’t need to worry about it becoming outdated.

### Yarn

Yarn is an alternative package manager for JavaScript, released in June of 2016. It has all the same packages and it’s often faster than NPM. Througout the book I’ll show the npm commands for installing packages, but feel free to use Yarn if you like.

### React Developer Tools

The React Developer Tools can be installed from here:

[https://github.com/facebook/react-devtools](https://github.com/facebook/react-devtools)

Follow the instructions to install the tools for your browser. The React dev tools allow you to inspect the React component tree (as opposed to the regular DOM elements tree) and view the props and state assigned to each component. Being able to see how React is rendering your app is extremely useful for debugging.

### Knowledge

You should already know JavaScript (at least ES5), HTML, and CSS. I’ll explain the newer JavaScript features as they come up (you don’t need to already know ES6 and beyond).

If you aren’t very comfortable with CSS, don’t worry too much – you can use the code provided in the book. A few exercises might be challenging without knowledge of CSS but you can feel free to skip those or modify the designs into something you can implement.

I don’t recommend learning JavaScript and React at the same time. When everything looks new, it can be hard to tell where “JavaScript” ends and “React” begins. If you need to brush up on JS, here are some good (and free!) resources:

- Speaking JavaScript (book): http://speakingjs.com/
- Exercism (exercises): http://exercism.io/languages/javascript
- You Don’t Know JS (book series): https://github.com/getify/You-Dont-Know-JS

A passing familiarity with the command line will be helpful as well. We’ll mostly just be using it to install packages.
