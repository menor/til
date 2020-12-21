# Prop Collections and Prop Getters
[Back to React Advanced Patterns](./react-advanced-patterns.md)

## Prop Collections
- Prop collection is a way of passing several props to various components at the same time like this `<MyComponent {...customProps} />`
- To do this we return all the types of props we would apply to the component using the hook from the hook itself. So the user can just reuse them.

```jsx
import * as React from 'react'
import {Switch} from '../switch'

function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  const togglerProps = {'aria-pressed': on, onClick: toggle}
  return {on, toggle, togglerProps}
}

function App() {
  const {on, togglerProps} = useToggle()
  return (
    <div>
      <Switch on={on} {...togglerProps} />
      <hr />
      <button
        aria-label="custom-button"
        {...togglerProps}
        onClick={() => console.info('onButtonClick')}
      >
        {on ? 'on' : 'off'}
      </button>
    </div>
  )
}

export default App

```

## Prop Getters
- Prop getters improves prop collections pattern, in case we want the user to be able to verride some props from the prop collection.
- In our previous example, what if we wanted the user to apply their own click handler?. It won't work the way it is right now, but we could use a prop getter.
- A prop getter is a function, instead of returning an object from our hook we return this function. This function takes as arguments props the user wants to override.

```jsx
import * as React from 'react'
import {Switch} from '../switch'

// returns a function that
// calls all of the passed functions if they exist
function callAll(...fns) {
  return (...args) => {
    fns.forEach(fn => fn?.(...args))
  }
}

function useToggle() {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)

  const getTogglerProps = ({onClick, ...props} = {}) => ({
    'aria-pressed': on,
    onClick: callAll(onClick, toggle),
    ...props,
  })

  return {on, toggle, getTogglerProps}
}

function App() {
  const {on, getTogglerProps} = useToggle()
  return (
    <div>
      <Switch {...getTogglerProps(on)} />
      <hr />
      <button
        {...getTogglerProps({
          'aria-label': 'custom-button',
          onClick: () => console.info('onButtonClick'),
        })}
      >
        {on ? 'on' : 'off'}
      </button>
    </div>
  )
}

export default App
```
