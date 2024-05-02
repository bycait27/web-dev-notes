# local vs global state management

## local state management
*the app state that is component-scoped

**for example:**

```
import React, { useState } from "react";
function Counter({ initialCount = 0 }) {
  const [count, setCount] = useState(initialCount);
  return (
    <div>
      <b>Count is: {count}</b><br />
      <button onClick={() => setCount(count + 1)}>
        Increment +
      </button>
      <button onClick={() => setCount(count - 1)}>
        Decrement -
      </button>
    </div>
  )
}
export default Counter;
```

8it is easy for a parent component to pass an initialCount value as a prop for the Counter element, but it is difficult to pass the current count value to the parent component
- **react's useState hook (and useReducer hook) is an good way to manage just the local state. these include:**
    - **atom component:** atoms are the most essential react components we can encounter, and they're likely to manage little local states only. more complex states can be delegated to molecules or organisms in many cases
    - **loading states:** when fetching external data on the client side, we always have a moment when we have neither some data nor an error, as we're still waiting for the http request to complete. we can decide to handle that by setting a loading state to true until the fetch request is completed to display a nice loading spinner on the ui

things can change once we need to maintain a global app state across all of our components (e-commerce website: displaying the items in a user's cart with an icon inside the nav bar while shopping across the site)

## global state management
*a state shared between all the components for a given web app that is reachable and modifiable by any component

the react data flow is unidirectional (components can pass data to their children components but not to their parents)
- this makes our components less error prone, easier to debug, and more efficient
- **BUT it adds extra complexity:** BY DEFAULT there CANNOT BE A GLOBAL STATE

there are many external libraries for managing the global app state (such as redux). however, we can use the context apis in react for managing the global app state without the need to external libraries. we can also use the apollo client (and its in-memory cache)