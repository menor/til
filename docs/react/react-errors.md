# Dealing with React Errors

## Error Boundaries
- Error Boundary components are the only ones that need to be a class component as of now (React 17).

- A class component becomes an error boundary if it defines either (or both) of the lifecycle methods static `getDerivedStateFromError()` or `componentDidCatch()`. Use static `getDerivedStateFromError()` to render a fallback UI after an error has been thrown. Use `componentDidCatch()` to log error information.

- Since the error will bubble up until it is handled, try to put ErrorBoundaries as close to the component as possible, so you can rerender just the component with the error and not a bigger part of the app.

- You can make a generic ErrorBoundary component that will take a FallbackComponent as a prop, in case there is an error, the Error boundary will render the FallbackComponent passed instead of its children.

- There's a problem with ErrorBoundaries, once you have an error how do you reset it so it can recover? You can force the ErrorBoundary to unmount and mount again by passing a key to it. If they key changes react will unmount and mount the error component in its initial state (with no error). Imagine you have an ErrorBoundary that catches rejected requests, sou could use the search term as a key for the ErrorBoundary, so when the user changes the value of the search term the ErrorBoundary will remount.
