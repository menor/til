# Advanced React Patterns
- Try to use inversion of control when a component is handling too many cases.
- APIs are more complex when you do inversion of control, but you can replicate easily the old API using the new control inverted function
- Some examples of inverted control functions are `Array.prototype.map` or `Array.prototype.filter`

- Two ways of using this for components:
  - Compound components (i.e. <select><option/></select>) [see](https://kentcdodds.com/blog/compound-components-with-react-hooks)
  - State Reducer: Think of a state reducer as a function which gets called any time the state of a component changes and gives the app developer a chance to modify the state change that's about to take place. [see](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)
  - You can also count render props as a way of doing this but it's not recommended anymore.

## Context Module Functions
- This is a context and reducer combo:

```js
// src/context/counter.js
const CounterContext = React.createContext()

function CounterProvider({step = 1, initialCount = 0, ...props}) {
  const [state, dispatch] = React.useReducer(
    (state, action) => {
      const change = action.step ?? step
      switch (action.type) {
        case 'increment': {
          return {...state, count: state.count + change}
        }
        case 'decrement': {
          return {...state, count: state.count - change}
        }
        default: {
          throw new Error(`Unhandled action type: ${action.type}`)
        }
      }
    },
    {count: initialCount},
  )

  const value = [state, dispatch]
  return <CounterContext.Provider value={value} {...props} />
}

function useCounter() {
  const context = React.useContext(CounterContext)
  if (context === undefined) {
    throw new Error(`useCounter must be used within a CounterProvider`)
  }
  return context
}

export {CounterProvider, useCounter}

// src/screens/counter.js
import {useCounter} from 'context/counter'

function Counter() {
  const [state, dispatch] = useCounter()
  const increment = () => dispatch({type: 'increment'})
  const decrement = () => dispatch({type: 'decrement'})
  return (
    <div>
      <div>Current Count: {state.count}</div>
      <button onClick={decrement}>-</button>
      <button onClick={increment}>+</button>
    </div>
  )
}

// src/index.js
import {CounterProvider} from 'context/counter'

function App() {
  return (
    <CounterProvider>
      <Counter />
    </CounterProvider>
  )
}
```

- Notice how the consumer (`src/screens/counter`) has to create its own `increment` and `decrement` functions that just call dispatch, this is far from ideal.
- What they do at FB is to export these helpers functions from their custom hook component, so: They return the state and the dispatch from the custom hook, and then from the custom hook module they also export any helper function that are going to be needed as HOF (those functions take a dispatch as an argument)

```js
// src/context/counter.js
const CounterContext = React.createContext()

function CounterProvider({step = 1, initialCount = 0, ...props}) {
  const [state, dispatch] = React.useReducer(
    (state, action) => {
      const change = action.step ?? step
      switch (action.type) {
        case 'increment': {
          return {...state, count: state.count + change}
        }
        case 'decrement': {
          return {...state, count: state.count - change}
        }
        default: {
          throw new Error(`Unhandled action type: ${action.type}`)
        }
      }
    },
    {count: initialCount},
  )

  const value = [state, dispatch]

  return <CounterContext.Provider value={value} {...props} />
}

function useCounter() {
  const context = React.useContext(CounterContext)
  if (context === undefined) {
    throw new Error(`useCounter must be used within a CounterProvider`)
  }
  return context
}

const increment = dispatch => dispatch({type: 'increment'})
const decrement = dispatch => dispatch({type: 'decrement'})

export {CounterProvider, useCounter, increment, decrement}

// src/screens/counter.js
import {useCounter, increment, decrement} from 'context/counter'

function Counter() {
  const [state, dispatch] = useCounter()
  return (
    <div>
      <div>Current Count: {state.count}</div>
      <button onClick={() => decrement(dispatch)}>-</button>
      <button onClick={() => increment(dispatch)}>+</button>
    </div>
  )
}
```

- Note: You may notice that the context provider/consumers in React DevTools just display as `Context.Provider` and `Context.Consumer`. To avoid this set the display name like this.

```js
const MyContext = React.createContext()
MyContext.displayName = 'MyContext'
```
- One advantage of this case is if the user has to make several dispatches from one event, they can just call our exported functions (increment, or decrement in this case) and we handle the orchestration of dispatches there.

