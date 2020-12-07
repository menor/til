# React Hooks
- Back to [react](./index.md)

- To understand hooks better think of a component as having 3 phases: Mount, render and unmount. [source](https://epicreact.dev/modules/react-hooks/hooks-flow).

- What makes a custom hook a hook is that it uses other hooks, either build-in or other custom hooks

## useState
- If you pass a function to `useState` instead of a value, this function will only be ran on the initial mount on the component, this is handy to avoid expensive functions running on every render (I.e. running from localStorage)

## useEffect
- The array passed as a second argument to `useEffect` only does a shallow comparison, so if you pass one object in that array the effect will run on every render (same as passing nothing as a second argument to `useEffect`).


- You use `useRef()` and then pass that value as ref prop to the component you want to access later. Before mounting this ref will be undefined, so you normally want to access that inside `useEffect`. Then the component is already mounted, so we can access `ref.current` and do whatever we want to the component.


- The function passed as a first argument to` useEffect`, can return a callback that will be called when the component unmounts. This is useful normally to remove event listeners.


- One important thing to note about the `useEffect` hook is that you cannot return anything other than the cleanup function. This has interesting implications with regard to async/await syntax:

```js
// this does not work, don't do this:
React.useEffect(async () => {
  const result = await doSomeAsyncThing()
  // do something with the result
})
```

- The reason this doesn’t work is because when you make a function async, it automatically returns a promise. So if you want to use async/await, the best way to do that is like so:

```js
React.useEffect(() => {
  async function effect() {
    const result = await doSomeAsyncThing()
	// do something with the result
  }

  effect()
})
```
- This ensures that you don’t return anything but a cleanup function.

- It’s typically just easier to extract all the async code into a utility function which I call and then use the promise-based `.then` method instead of using async/await syntax:

```js
React.useEffect(() => {
  doSomeAsyncThing().then(result => {
  // do something with the result
  })
})
```

## useReducer
- `useReducer` is a very flexible pattern to manage state, you can make it work so it takes an object, a function or both. You can also make it work like a redux reducer by passing an action object with or without payload.

- I.E. this reducer will mimic `setState`:

```js
function setStateReducer(state, action) {
  return {
	...state,
	...(typeof action === 'function' ? action(state) : action),
  }
}
```

## useCallback
- When we pass a function to `useEffect`, we can include the function in the dependency list (the array that we pass as a second argument to `useEffect`). The problem is that it will cause the function to run on every render if the function is defined inside the component function body. Every render the function will be instantiated so we run the `useEffect` on every render.

- To avoid this we define the function using `React.useCallback,` the function will still be created on every render, but React will only give us the new one if the dependency list of the function registers changes (we also pass a dependency list to `useCallback`).

## useContext
- `useContext` is typically better suited for libraries than for application code (for app code is better to use composition)

- `useContext` avoids doing prop drilling, you can create a provider in a higher level of the tree, and use that later down that tree. Think global variables without the disadvantages.

Here's an example:

```js
import React from 'react'

const RickContext = React.createContext()

function RickDisplay() {
  const rick = React.useContext(Rick)
  return <div>Rick is: {rick}</div>
}

ReactDOM.render(
  <RickContext.Provider value="Pickle Rick">
    <RickDisplay />
  </RickContext.Provider>,
  document.getElementById('root'),
)
// renders <div>Rick is: Pickle Rick</div>
```

- `<RickDisplay />` could appear anywhere in the render tree, and it will have access to the value which is passed by the `RickContext.Provider `component.

- As the first argument to `createContext` you could pass a default value that will be shown in case anyone tries to use context outside a Provider, but it's not recommended cause this (accessing context outside a provider) is probably due to the user making a mistake.

- Context is primarily used when some data needs to be accessible by many components at different nesting levels. Apply it sparingly because it makes component reuse more difficult. If you only want to avoid passing props through many levels, use  component composition instead (component composition is when a component renders its props.children)

- Context does not need to be globally available. Keeping a context value scoped to the area that needs it most has improved performance and maintainability characteristics.

## useLayoutEffect
- `UseLayoutEffect` runs before React renders (think `componentWillMount`).
- `UseEffect` runs after React has rendered (think `componentDidMount`).

- If your effect is mutating the DOM (via a DOM node ref) and the DOM mutation will change the appearance of the DOM node between the time that it is rendered and your effect mutates it, then you don't want to use `useEffect`. You'll want to use `useLayoutEffect`. Otherwise the user could see a flicker when your DOM mutations take effect. This is pretty much the only time you want to avoid `useEffect` and use `useLayoutEffect` instead. 

- This runs synchronously immediately after React has performed all DOM mutations. This can be useful if you need to make DOM measurements (like getting the scroll position or other styles for an element) and then make DOM mutations or trigger a synchronous re-render by updating state
 
- When to choose one:
  - `useLayoutEffect`: If you need to mutate the DOM with notable effects to the user and/or do need to perform measurements
  - `useEffect`: If you don't need to interact with the DOM at all or your DOM changes are unobservable (seriously, most of the time you should use this).


