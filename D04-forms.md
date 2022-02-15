### Forms in React

Forms are one of the few HTML elements that are interactive by default.

They were designed to allow the user to interact with a page.

Common uses of forms?

- Search
- Contact forms
- Shopping carts checkout
- Login and registration
- and more!

Using React we can make our forms much more interactive and less static.

There are two main ways of handling forms in React, which differ on a fundamental level: how data is managed.

- if the data is handled by the DOM, we call them **uncontrolled components**
- if the data is handled by the components we call them **controlled components**

As you can imagine, controlled components is what you will use most of the time. The component state is the single source of truth, rather than
the DOM. Some form fields are inherently uncontrolled because of their
behavior, like the `<input type="file">` field.

When an element state changes in a form field managed by a component, we track it using the `onChange` attribute.

```
class Form extends React.Component {
  constructor(props) {
    super(props)
    this.state = { username: '' }
  }
  
  handleChange(event) {}
  
  render() {
    return (
      <form>
        Username:
        <input
          type="text"value={this.state.username}onChange={this.handleChange}/>
      </form>)
  }
}
```

In order to set the new state, we must bind `this` to the `handleChange` method, otherwise `this` is not accessible from within that method:

```
class Form extends React.Component {
  constructor(props) {
    super(props)
    this.state = { username: '' }
    this.handleChange = this.handleChange.bind(this)
  }
  
  handleChange(event) {
    this.setState({ value: event.target.value })
  }
  
  render() {
    return (
      <form>
        <input
          type="text"
          value={this.state.username}
          onChange={this.handleChange}
        />
      </form>
    )
  }
}
```

Similarly, we use the `onSubmit` attribute on the form to call the `handleSubmit` method when the form is submitted:

```
class Form extends React.Component {
  constructor(props) {
    super(props)
    this.state = { username: '' }
    this.handleChange = this.handleChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }
  
  handleChange(event) {
    this.setState({ value: event.target.value })
  }
  
  handleSubmit(event) {
    alert(this.state.username)
    event.preventDefault()
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type="text"
          value={this.state.username}
          onChange={this.handleChange}
        />
        <input type="submit" value="Submit" />
      </form>
    )
  }
}
```

Validation in a form can be handled in the `handleChange` method: you have access to the old value of the state, and the new one. You can check the new value and if not valid reject the updated value
(and communicate it in some way to the user).

HTML Forms are
inconsistent. They have a long history, and it shows. React however
makes things more consistent for us, and you can get (and update) fields using its `value` attribute.

Here’s a `textarea`, for example:

```
<textarea value={this.state.address} onChange={this.handleChange} />
```

The same goes for the `select` tag:

```
<select value="{this.state.age}" onChange="{this.handleChange}">
  <option value="teen">Less than 18</option>
  <option value="adult">18+</option>
</select>
```

Previously we mentioned the `<input type="file">` field. That works a bit differently.

In this case you need to get a reference to the field by assigning the `ref` attribute to a property defined in the constructor with `React.createRef()`, and use that to get the value of it in the submit handler:

```
class FileInput extends React.Component {
  constructor(props) {
    super(props)
    this.curriculum = React.createRef()
    this.handleSubmit = this.handleSubmit.bind(this)
  }
  
  handleSubmit(event) {
    alert(this.curriculum.current.files[0].name)
    event.preventDefault()
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="file" ref={this.curriculum} />
        <input type="submit" value="Submit" />
      </form>)
  }
}
```

This is the **uncontrolled components** way. The state is stored in the DOM rather than in the component state (notice we used `this.curriculum` to access the uploaded file, and have not touched the `state`.

I know what you’re thinking — beyond those basics, there must be a
library that simplifies all this form handling stuff and automates
validation, error handling and more, right? There is a great one, [Formik](https://github.com/jaredpalmer/formik).
