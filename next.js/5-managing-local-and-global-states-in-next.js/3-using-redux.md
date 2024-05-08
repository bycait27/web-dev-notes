#using redux
redux is a popular tool used for handling large-scale app states
- is the de facto state manager for building large-scale apps in react
- gives devs access to an incredibly vast ecosystem of plug-ins, middleware, and debugging tools that make the dev experience much more effortless (for scaling and handling very complex business logic in our web app)

**to start, we will need two new dependencies to use redux in our app:**
- redux
- react-redux
- we can also use redux-devtools-extension (allows us to insepct and debug the app state from the browser)

1. we need to initialize the global store (part of our app containing the app state)
    a. create a folder inside of the root of our project called redux/
    - this is where we will write a new store.js file containing the logic for initializing our store on both the client and server side

    ```
    import { useMemo } from 'react';
    import { createStore, applyMiddleware } from 'redux';
    import { composeWithDevTools } from 'redux-devtools-extension';
    let store;
    const initialState = {};
    // ...
    ```

    - the store variable keeps our redux store
    - the initialState will be an empty object because we'll add more properties depending on which product our users selects on the store-front

2. we need to create our first and only reducer inside our store.js file (since we only have one)

```
const reducer = (state = initialState, action) => {
  const itemID = action.id;

  switch (action.type) {
    case "INCREMENT":
      const newItemAmount = itemID in state ? state[itemID] + 1 : 1;
      return {
        ...state,
        [itemID]: newItemAmount,
      };
    case "DECREMENT":
      if (state?.[itemID] > 0) {
        return {
          ...state,
          [itemID]: state[itemID] - 1,
        };
      }
      return state;
    default:
      return state;
  }
};
```

- the reducers logic is equal to that of the handleAmount function from our ProductCard component

3. we need to initialize our store 
    a. create a simple helper function called initStore

    ```
    // ...
    function initStore(preloadedState = initialState) {
        return createStore(
        reducer,
        preloadedState,
        composeWithDevTools(applyMiddleware())
    );
}
    ```

    b. create another helper function to properly initialize the store (populateStore)

    ```
    // ...
    export const populateStore = (preloadedState) => {
    let _store = store ?? initStore(preloadedState);
    if (preloadedState && store) {
         _store = initStore({
            ...store.getState(),
            ...preloadedState,
        });
        store = undefined;
    }
    //Return '_store' when initializing Redux on the server-side
    if (typeof window === "undefined") return _store;
    if (!store) store = _store;
    return _store;
    };
    ```

4. we need to create a useMemo function to cache complex initial states (avoids the system reparsing it on every useStore function call)

```
// ...
  export function useStore(initialState) {
  return useMemo(
  () => populateStore(initialState), [initialState]
  );
}
```

5. now we need to edit our _app.js file so that redux will be globally available for every component living inside of our next.js app

```
import Head from 'next/head';
import { Provider } from 'react-redux';
import { useStore } from '../redux/store';
import Navbar from '../components/Navbar';

function MyApp({ Component, pageProps }) {
  const store = useStore(pageProps.initialReduxState);
  return (
    <>
      <Head>
        <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet" />
      </Head>
      <Provider store={store}>
        <Navbar />
        <div className="w-9/12 m-auto pt-10">
          <Component {...pageProps} />
        </div>
      </Provider>
    </>
  );
}

export default MyApp;
```

6. we need to implement the increment/decrement logic for our ProductCard component using redux
    a. add dependencies to components/ProductCard.js file

    ```
    import { useDispatch, useSelector, shallowEqual } from 'react-redux';
    // ...
    ```

    b. create a hook to fetch all the products in our redux store

    ```
    import { useDispatch, useSelector, shallowEqual } from 'react-redux';
    function useGlobalItems() {
        return useSelector((state) => state, shallowEqual);
    }
    // ...
    ```

    c. edit the ProductCard component by integrating the redux hooks 

    ```
    // ...
    function ProductCard({ id, name, price, picture }) {
        const dispatch = useDispatch();
        const items = useGlobalItems();
        const productAmount = items?.[id] ?? 0;
            return (
    // ...
    ```

    d. trigger a dispatch when the user clicks on of our component's buttons (useDispatch hook). let's update the onClick callback for our html buttons inside the render function

    ```
    // ...
    <div className="flex justify-between mt-4 w-2/4 m-auto">
        <button
            className="pl-2 pr-2 bg-red-400 text-white rounded-md"
            disabled={productAmount === 0}
            onClick={() => dispatch({ type: 'DECREMENT', id })}>
            -
        </button>

        <div>{productAmount}</div>
  
        <button
            className="pl-2 pr-2 bg-green-400 text-white rounded-md"
            onClick={() => dispatch({ type: 'INCREMENT', id })}>
            +
        </button>
    </div>
    // ...
    ```

    *we can use our debugging tools with the redux devtools extension to see the action of incrementing or decrementing a product as it is dispatched directly inside our debugging tools

7. we need to update the nav bar when we add or remove a product from our cart 
    a. edit the components/NavBar.js component

    ```
    import ProductCard from "../components/ProductCard";
    import products from "../data/items";

    function Home() {
        return (
            <div className="grid grid-cols-4 gap-4">
                {products.map((product) => (
                    <ProductCard key={product.id} {...product} />
                ))}
            </div>
        );
    }

    export default Home;
    ```

    *now the state change will be visible in the nav bar

8. now let's update the cart.js file so that we can see a summary of the shopping cart before moving to the checkout page
    a. import the redux hook we used previously

    ```
    import { useSelector, shallowEqual } from 'react-redux';
    import data from '../data/items';

    function useGlobalItems() {
        return useSelector((state) => state, shallowEqual);
    }
    // ..
    ```

    b. replicate the getFullItem function

    ```
    // ...
    function getFullItem(id) {
        const idx = data.findIndex((item) => item.id === id);
        return data[idx];
    }
    // ...
    ```

    c. replicate what we did with the Cart component EXCEPT the items object will come from the redux store 

    ```
    import { useSelector, shallowEqual } from "react-redux";
    import data from "../data/items";

    function useGlobalItems() {
        return useSelector((state) => state, shallowEqual);
    }

    function getFullItem(id) {
        const idx = data.findIndex((item) => item.id === id);
        return data[idx];
    }

    function Cart() {
        const items = useGlobalItems();
        const total = Object.keys(items)
            .map((id) => getFullItem(id).price * items[id])
            .reduce((x, y) => x + y, 0);

        const amounts = Object.keys(items).map((id) => {
            const item = getFullItem(id);
            return { item, amount: items[id] };
        });

        return (
            <div>
                <h1 className="text-xl font-bold"> Total: ${total} </h1>
                <div>
                    {amounts.map(({ item, amount }) => (
                        <div key={item.id}>
                            x{amount} {item.name} (${amount * item.price})
                        </div>
                    ))}
                </div>
            </div>
        );
    }

    export default Cart;
    ```

    *we will now see a summary of our expenses before moving onto the /cart page