# React

- [React Hooks](./react-hooks.md)
- [Error handling in React](./react-errors.md)

## Basic React
- Passing a key to an array (different than an index or a random thing) it’s really important cause it helps React keep track of what element of the array it snapped to what element in the DOM
- Use colocation to improve app performance (put the state as down in the tree as possible) [source](https://kentcdodds.com/blog/state-colocation-will-make-your-react-app-faster).
- Try to maintain a habit of recollecting state to improve performance.
- It's better to use a status enum (or better a state machine) instead of booleans. [source](https://kentcdodds.com/blog/stop-using-isloading-booleans).
