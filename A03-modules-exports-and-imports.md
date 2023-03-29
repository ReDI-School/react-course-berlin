# Modules Exports and Imports

In React projects (and actually in all modern JavaScript projects), you split your code across multiple JavaScript files - so-called modules. You do this, to keep each file / module focused and manageable.

To still access functionality in another file, you need `export` (to make it available) and `import` (to get access) statements.

You got two different types of exports: **default** (unnamed) and **named** exports:

**default** => `export default ...;`

**named** => `export const someData = ...;`

You can import **default exports** like this:

`import someNameOfYourChoice from './path/to/file.js`

Surprisingly, `someNameOfYourChoice` is totally up to you.

**Named exports** have to be imported by their name:

`import { someData } from './path/to/file.js';`

A file can only contain one default and an unlimited amount of named exports. You can also mix the one default with any amount of named exports in one and the same file.

When importing **named exports**, you can also import all named exports at once with the following syntax:

`upToYou` is - well - up to you and simply bundles all exported variables/functions in one JavaScript object. For example, if you export `export const someData = ...` (`/path/to/file.js`) you can access `upToYou` like this: `upToYou.someData`.

### Resources

- [Javascript.info: Export and Import](https://javascript.info/import-export)

### Exercises

- [Create a Module Script](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/create-a-module-script)
- [Use export to Share a Code Block](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use-export-to-share-a-code-block)
- [Reuse JavaScript Code Using import](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/reuse-javascript-code-using-import)
- [Use * to Import Everything from a File](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/use--to-import-everything-from-a-file)
- [Create an Export Fallback with export default](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/create-an-export-fallback-with-export-default)
- [Import a Default Export](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/es6/import-a-default-export)
