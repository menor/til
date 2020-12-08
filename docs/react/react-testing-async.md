# Testing Async Components

*Notes from [Reactathon 2020 talk](https://youtu.be/KgTW0xfyr9A?t=7524) by [Alex Krolick](https://alexkrolick.com/)

## Component Testing 101
Arrange, Act, Assert:
- 🎬 render()
- ☝️ fireEvent()
- ✅ expect()

```js
import { render, screen, fireEvent } from '@testing-library/react'

// 🎬 render()
const onSubmit = jest.fn()
render(<CommentBox onSubmit={onSubmit} />)

// ☝️ fireEvent()
const input = screen.getByRole("textbox")
fireEvent.change(input, { value: "I love it!" })
const button = screen.getByText("Submit")
fireEvent.click(button)

// ✅ expect() 
expect(onSubmit).toHaveBeenCalledWith("I love it!")
expect(screen.getByText("Submitted!")).toBeInTheDocument()
```
What about `act()`? `react-dom/test-utils/act`
```js
act(() => {
    fireEvent.change(input, { value: "I love it!" })
})
```
- `act()` hooks into React and makes sure effects generated by user events are processed and rendered before moving onto the next step of the test

- React Testing Library wraps `fireEvent` with `act` automatically
 
## Sources of Asynchronicity
We normally have to deal with three sources of asynchrony when testing:

### Timers
Normally seen on:
- Throttle/Debounce,
- Tooltips, Modals, Animations,
- Form Validation

I.E. an onChange that only fires after the user stops typing for 200ms
```js
handleChange = _.debounce(this.onChange, 200)
<input type="text" onChange={this.handleChange} />
```
 
**Strategy 1 - Remove the timer:**
- Replace timeout constants with 0

- Mock out functions that create timers (like lodash debounce)
```js
jest.mock("lodash/debounce")

__mocks__/
    lodash/
        debounce.js
        module.exports = jest.fn(fn => fn)
```

**Strategy 2 - Take control of time:**
    - `jest.useFakeTImers()`
    - `jest.advanceTimersByTime(200) // Option 1`
    - `jest.runAllTimers() // Option 2`

**Strategy 3 (Recommended) - Make your whole test async:**
- Use async await for your test functions
```js
test("should submit comment", async () => {
    // render
    const onSubmit = jest.fn()
    render(<CommentBox onSubmit={onSubmit} />)
    
    // fireEvent
    const input = screen.getByRole("textbox")
    const button = screen.getByText("Submit")
    fireEvent.change(input, { value: "I love it!" })
    // Retryable assertion that signals when we can resume the test
    await waitFor(() => expect(button.disabled).toBe(false)) // ⏱
    fireEvent.click(button)

    // expect
    expect(onSubmit).toHaveBeenCalledWith("I love it!")
    // Retry until element appears
    const successMsg = await screen.findByText("Submitted") // ⏱
    expect(successMsg).toBeInTheDocument()
})
 ```

### Promises
Normally seen on Browser APIs, library code, etc.
- If you have Promises, you need async tests
- Jest mock functions can resolve or reject Promises (you need to test both paths)
```js
test("should handle submission errors", async () => {
    // ...
    onSubmit.mockRejectedValueOnce({ status: 500 })
    fireEvent.click(button)
    const errorMsg = await screen.findByText("Server Error")
    expect(errorMsg).toBeInTheDocument()
})
```

### Network I/O
Seen on API calls
- You usually don't want to hit real servers (**important: we are talking unit tests here, not integration**) in tests because:
    - The server might be unreliable
    - The server code might not be deployed yet
    - The auth layer isn’t usable in unit tests
    - You want to change the response data and see how the frontend handles it
    - Network I/O is slow
 
 **Strategy 1**
 Most network APIs have a Promise interface that you can mock:
 ```js
let realFetch = window.fetch
beforeEach(() => window.fetch = jest.fn())
afterEach(() => window.fetch = realFetch)

window.fetch.mockResolvedValueOnce({ id: 123 })
// trigger network call
// respond to mocked result
```

- Problem with this approach is if you switch network library you need to rewrite all of your tests.

 **Strategy 2**
- Create a fake server.
- There are several libraries that create a service worker that can intercept API calls to certain routes and respond with mock data:
    - [Mock Service Worker](https://www.npmjs.com/package/msw)
    - [MirageJs](https://miragejs.com/)

```js
import {rest} from 'msw' // "mock service worker"
import {setupServer} from 'msw/node'

server.use(
    rest.get('/greeting', (req, res, ctx) => {
        return res(ctx.status(500))
    })
)
```

 **Strategy 3**
 - Create a module around your API to create an abstraction layer for tests to mock.
 
 ```js
 // api.js
export const getUser = async id => fetch("/users/" + id)

// test.js
const api = jest.createMockFromModule("../api")
test("handles authentication error, async () => {
    api.getUser.mockRejectedValue({ status: 401 })
    // ... })
 ```