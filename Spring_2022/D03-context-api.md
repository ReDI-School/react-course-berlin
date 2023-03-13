### The Context API

The Context API is a
neat way to pass state across the app without having to use props. It
was introduced to allow you to pass state (and enable the state to
update) across the app, without having to use props for it.

The
React team suggests to stick to props if you have just a few levels of
children to pass, because it’s still a much less complicated API than
the Context API.

In many cases, it enables us to avoid using Redux, simplifying our apps a lot, and also learning how to use React.

How does it work?

You create a context using `React.createContext()`, which returns a Context object:

```
const { Provider, Consumer } = React.createContext()
```

Then you create a wrapper component that returns a **Provider** component, and you add as children all the components from which you want to access the context:

```
class Container extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      something: 'hey'
    }
  }
  
  render() {
    return (
      <Provider value={{ state: this.state }}>{this.props.children}</Provider>
    )
  }
}

class HelloWorld extends React.Component {
  render() {
    return (
      <Container>
        <Button />
      </Container>
    )
  }
}
```

I used Container as the name of this component because this will be a global provider. You can also create smaller contexts.

Inside a component that’s wrapped in a Provider, you use a **Consumer** component to make use of the context:

```
class Button extends React.Component {
  render() {
    return (
      <Consumer>
        {context => <button>{context.state.something}</button>}
      </Consumer>
    )
  }
}
```

You can also pass functions into a Provider value, and those functions will be used by the Consumer to update the context state:

```
<Provider value={{
  state: this.state,
  updateSomething: () => this.setState({something: 'ho!'})
  {this.props.children}
</Provider>

/* ... */
<Consumer>
  {(context) => (
    <button onClick={context.updateSomething}>{context.state.something}</button>
  )}
</Consumer>
```

You can see this in action [in this Glitch](https://glitch.com/edit/#!/flavio-react-context-api-example?path=app/components/HelloWorld.jsx).

You can create multiple contexts, to make your state distributed across
components, yet expose it and make it reachable by any component you
want.

When using multiple files, you create the content in one file, and import it in all the places you use it:

```
//context.js
import React from 'react'
export default React.createContext()

//component1.js
import Context from './context'
//... use Context.Provider

//component2.js
import Context from './context'
//... use Context.Consumer
```
