# Javascript

- You can transpile babel in the browser (not for production) but great to test things quickly. By requiring it from unpkg and then putting your script between a `<script type=“text/babel”>` tag.


- You can pass a `defaultValue` attr to an input field and it will only render it at the first render.


- You can pass a second function as an argument to a `fetch.then()` that fires in case of error (it will get the error as an argument)


- If you need only one value from an array you can put commas to get to that value without getting the unused variable warning: `const [ , SetCount] = React.useState(0)`
