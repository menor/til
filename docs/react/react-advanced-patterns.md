# Advanced React Patterns
- Try to use inversion of control when a component is handling too many cases.
- APIs are more complex when you do inversion of control, but you can replicate easily the old API using the new control inverted function
- Some examples of inverted control functions are `Array.prototype.map` or `Array.prototype.filter`

- Two ways of using this for components:
  - Compound components (i.e. `<select><option/></select>`) [see this article](https://kentcdodds.com/blog/compound-components-with-react-hooks)
  - State Reducer: Think of a state reducer as a function which gets called any time the state of a component changes and gives the app developer a chance to modify the state change that's about to take place. [see](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)
  - You can also count render props as a way of doing this but it's not recommended anymore.

## Patterns
- [Context Module Functions](./context-module-functions.md)
- [Compound Components](./compound-components.md)