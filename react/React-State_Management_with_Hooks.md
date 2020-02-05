# State Management with Hooks

> **Reference:**
>
> - [State Management Using Only React Hooks](https://blog.logrocket.com/state-management-using-only-react-hooks/) &mdash; LogRocket blog post by Ovie Okeh
> - For React **v16.8** and up

## `useReducer`

`useReducer` allows you to handle complex state updates. We can use it to manage app state instead of using Redux or MobX.

- a new custom hook for React since **v16.8**
- similar to how Redux works
- allows you to update parts of your component's state when certain actions are dispatched
- arguments:
  - reducer function
  - initial state
- returns:
  - state variable
  - dispatch function (for updating state)

### How Does It Work

```javascript
// we have to define the initial state of the component's state
const initialState = { count: 0 }

// this function will determine how the state is updated
function reducer(state, action) {
  switch(action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 }
    case 'DECREMENT':
      return { count: state.count - 1 }
    case 'REPLACE':
      return { count: action.newCount }
    case 'RESET':
      return { count: 0 }
    default:
      return state
  }
}

// inside your component, initialize your state like so
const [state, dispatch] = useReducer(reducer, initialState);
```

- `initialState`: the default value for our component's state
- reducer function: takes `state` and `action` as arguments
  - `state`: current app state
  - `action`: object with details of the current action
    - needs a `type` property, eg. "INCREMENT"

## Dispatching Actions

`useReducer` returns the dispatcher, so we can use it to update state.

```javascript
dispatch({
    type: "REPLACE",
    newCount: 9001
})
```
