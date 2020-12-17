# Compound Components
- [Back to React Advanced Patterns](./react-advanced-patterns.md)

- Compound components are components that work together to form a complete UI. The classic example of this is `<select>` and `<option>` in HTML.
- The `<select>` is the element responsible for managing the state of the UI, and the `<option>` elements are essentially more configuration for how the select should operate (specifically, which options are available and their values).

- Examples:
    - https://reach.tech/tooltip/
    - https://reach.tech

- One way of doing it is the `<select>` holds the state and then clones the children `<option>` using `React.cloneElement` to extend the elements with the props they need to work.
- By cloning we are extending every child with the props we are passing to the cloned components, we can check inside our `React.Children` map function to decide if we pass the props to the child or not

```jsx
import * as React from 'react'
import {Switch} from '../switch'

function Toggle({children}) {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return React.Children.map(children, child =>
    React.cloneElement(child, {on, toggle}),
  )
}

function ToggleOn({on, children}) {
  return on ? children : null
}

function ToggleOff({on, children}) {
  return on ? null : children
}

function ToggleButton({on, toggle, ...props}) {
  return <Switch on={on} onClick={toggle} {...props} />
}

function App() {
  return (
    <div>
      <Toggle>
        <ToggleOn>The button is on</ToggleOn>
        <ToggleOff>The button is off</ToggleOff>
        <ToggleButton />
      </Toggle>
    </div>
  )
}

export default App
```
