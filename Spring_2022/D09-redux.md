### Redux

Redux is a state manager that’s usually used along with React, but it’s not tied to that library — it can be used
with other technologies as well, but we’ll stick to React for the sake
of the explanation..

Redux is a way to manage an application state, and move it to an **external global store**.

There are a few concepts to grasp, but once you do, Redux is a very simple approach to the problem.

Redux is very popular with React applications, but it’s in no way unique to
React: there are bindings for nearly any popular framework. That said,
I’ll make some examples using React as it is its primary use case.

### When should you use Redux?

Redux is ideal for medium to big apps, and you should only use it when you
have trouble managing the state with the default state management of
React, or the other library you use.

Simple apps should not need it at all (and there’s nothing wrong with simple apps).

### Immutable State Tree

In Redux, the whole state of the application is represented by **one** [JavaScript](https://flaviocopes.com/javascript/) object, called **State** or **State Tree**.

We call it **Immutable State Tree** because it is read only: it can’t be changed directly.

It can only be changed by dispatching an **Action**.

### Actions

An **Action** is **a JavaScript object that describes a change in a minimal way** (with just the information needed):

```
{
  type: 'CLICKED_SIDEBAR'
}

// e.g. with more data
{
  type: 'SELECTED_USER',
  userId: 232
}
```

The only requirement of an action object is having a `type` property, whose value is usually a string.

### Actions types should be constants

In a simple app an action type can be defined as a string, as I did in the example in the previous lesson.

When the app grows is best to use constants:

```
const ADD_ITEM = 'ADD_ITEM'
const action = { type: ADD_ITEM, title: 'Third item' }
```

and to separate actions in their own files, and import them

```
import { ADD_ITEM, REMOVE_ITEM } from './actions'
```

### Action creators

**Actions Creators** are functions that create actions.

```
function addItem(t) {
  return {
    type: ADD_ITEM,
    title: t
  }
}
```

You usually run action creators in combination with triggering the dispatcher:

```
dispatch(addItem('Milk'))
```

or by defining an action dispatcher function:

```
const dispatchAddItem = i => dispatch(addItem(i))
dispatchAddItem('Milk')
```

### Reducers

When an action is fired, something must happen, the state of the application must change.

This is the job of **reducers**.

A **reducer** is a **pure function** that calculates the next State Tree based on the previous State Tree, and the action dispatched.

```
;(currentState, action) => newState
```

A pure function takes an input and returns an output without changing the input or anything else. Thus, a reducer returns a completely new state
tree object that substitutes the previous one.

### What a reducer should not do

A reducer should be a pure function, so it should:

- never mutate its arguments
- never mutate the state, but instead create a new one with `Object.assign({}, ...)`
- never generate side-effects (no API calls changing anything)
- never call non-pure functions, functions that change their output based on factors other than their input (e.g. `Date.now()` or `Math.random()`)

There is no reinforcement, but you should stick to the rules.

### Multiple reducers

Since the state of a complex app could be really wide, there is not a single reducer, but many reducers for any kind of action.

### A simulation of a reducer

At its core, Redux can be simplified with this simple model:

### The state

```
{
  list: [
    { title: "First item" },
    { title: "Second item" },
  ],
  title: 'Groceries list'
}
```

### A list of actions

```
{ type: 'ADD_ITEM', title: 'Third item' }
{ type: 'REMOVE_ITEM', index: 1 }
{ type: 'CHANGE_LIST_TITLE', title: 'Road trip list' }
```

### A reducer for every part of the state

```
const title = (state = '', action) => {
    if (action.type === 'CHANGE_LIST_TITLE') {
      return action.title
    } else {
      return state
    }
}

const list = (state = [], action) => {
  switch (action.type) {
    case 'ADD_ITEM':
      return state.concat([{ title: action.title }])
    case 'REMOVE_ITEM':
      return state.map((item, index) =>
        action.index === index
          ? { title: item.title }
          : item
    default:
      return state
  }
}
```

### A reducer for the whole state

```
const listManager = (state = {}, action) => {
  return {
    title: title(state.title, action),
    list: list(state.list, action)
  }
}
```

### The Store

The **Store** is an object that:

- **holds the state** of the app
- **exposes the state** via `getState()`
- allows us to **update the state** via `dispatch()`
- allows us to (un)register a **state change listener** using `subscribe()`

A store is **unique** in the app.

Here is how a store for the listManager app is created:

```
import { createStore } from 'redux'
import listManager from './reducers'
let store = createStore(listManager)
```

### Can I initialize the store with server-side data?

Sure, **just pass a starting state**:

```
let store = createStore(listManager, preexistingState)
```

### Getting the state

```
store.getState()
```

### Update the state

```
store.dispatch(addItem('Something'))
```

### Listen to state changes

```
const unsubscribe = store.subscribe(() =>
  const newState = store.getState()
)

unsubscribe()
```

### Data Flow

Data flow in Redux is always **unidirectional**.

You call `dispatch()` on the Store, passing an Action.

The Store takes care of passing the Action to the Reducer, generating the next State.

The Store updates the State and alerts all the Listeners.
