# using the context apis
context apis give us a straightforward way to share data between all the components inside a given context without explicitly having to pass it via props from one component to another, even from children to a parent component

**we will store the selected products in the global state for simplicity's sake as a javascript object. each property is the id of a product and its value represents the number of products that the user has selected:**
- the following state shows the user has selected four carrots and two onions

```
{
   "8321-k532":4,
   "9126-b921":2
}
```

## creating the shopping cart context
**let's create the context for the shopping cart by creating a cartContext.js file in the components/context/ directory:**

```
import { createContext } from 'react';
const ShoppingCartContext = createContext({
  items: {},
  setItems: () => null,
});
export default ShoppingCartContext;
```

*we will want to wrap all the components that need to share the cart data under the same context

*we will also want our global state to be persistent when changing the page because we want to display the number of products selected by the user on the checkout page
- **we can customize the /pages/_app.js page to wrap the entire app under the same react context:**

```
import { useState } from 'react';
import Head from 'next/head';
import CartContext from '../components/context/cartContext';
import Navbar from '../components/Navbar';
function MyApp({ Component, pageProps }) {
  const [items, setItems] = useState({});
  return (
    <>
      <Head>
        <link
          href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css"
          rel="stylesheet"
        />
      </Head>
      <CartContext.Provider value={{ items, setItems }}>
      <Navbar />
        <div className="w-9/12 m-auto pt-10">
          <Component {...pageProps} />
        </div>
      </CartContext.Provider>
    </>
  );
}
export default MyApp;
```

## wrapping the app under the cartContext
we have wrapped our Navbar component and pageProps under the same context
- they both gain access to the same global state, creating a link between all the components rendered on every page and the nav bar

**here we have the contents of the index.js page:**

```
import ProductCard from '../components/ProductCard';
import products from '../data/items';
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

we import all the products from a local javascript file (or an api)
- for each product, we render the ProductCard component

**here is the ProductCard component:**

```
function ProductCard({ id, name, price, picture }) {
  return (
    <div className="bg-gray-200 p-6 rounded-md">
      <div className="relative">
        <img src={picture} alt={name} className="object-cover" />
      </div>
      <div className="flex justify-between mt-4">
        <div className="font-bold text-l"> {name} </div>
        <div className="font-bold text-l text-gray-500">
          {" "}
          ${price}
          per kg{" "}
        </div>
      </div>
      <div className="flex justify-between mt-4 w-2/4 m-auto">
        <button
          className="pl-2 pr-2 bg-red-400 text-white rounded-md"
          disabled={false /* To be implemented */}
          onClick={() => {} /* To be implemented */}
        >
          -
        </button>
        <div>{/* To be implemented */}</div>
        <button
          className="pl-2 pr-2 bg-green-400 text-white rounded-md"
          onClick={() => {} /* To be implemented */}
        >
          +
        </button>
      </div>
    </div>
  );
}
export default ProductCard;
```

## link ProductCard component to cartContext
we need to link the ProductCard component to the cartContext context and update the context state as soon as the user clicks one of the two buttons

```
import { useContext } from 'react';
import cartContext from '../components/context/cartContext';
function ProductCard({ id, name, price, picture }) {
  const { setItems, items } = useContext(cartContext);
  // ....
```

we link both setItems and items from _app.js to the ProductCard component using the useContext hook
- when we call setItems on the ProductComponent, we'll be updating the global items object 
  - this change will be propagated to ALL the components living under the same context and linked to the same global state
- we don't need to keep a local state for each ProductCard component
  - the info about the number of single products added to the shopping cat ALREADY EXISTS in our context state 
    - **if we want to know the number of products added to the shopping cart, we can do this:**

    ```
    import { useContext } from "react";
    import cartContext from "../components/context/cartContext";
    function ProductCard({ id, name, price, picture }) {
      const { setItems, items } = useContext(cartContext);
      const productAmount = id in items ? items[id] : 0;
      // ...
    ```

    every time the user clicks the increment button, the global items state will change, the ProductCard component will be rendered, and the productAmount constant will end up having a new value

we need to control the user clicks on the increment and decrement buttons
- the handleAmount function takes a single argument that can be either "increment" or "decrement"
  - depending on the parameter entered, the function will make sure that the global state is updated accordingly

```
import { useContext } from 'react';
import cartContext from '../components/context/cartContext';
function ProductCard({ id, name, price, picture }) {
  const { setItems, items } = useContext(cartContext);
  const productAmount = id in items ? items[id] : 0;
  const handleAmount = (action) => {
  if (action === 'increment') {
    const newItemAmount = id in items ? items[id] + 1 : 1;
    setItems({ ...items, [id]: newItemAmount });
    }
  if (action === 'decrement') {
    if (items?.[id] > 0) {
      setItems({ ...items, [id]: items[id] - 1 });
      }
    }
  };
  return (
    <div className="bg-gray-200 p-6 rounded-md">
      <div className="relative">
        <img src={picture} alt={name} className="object-cover" />
      </div>
      <div className="flex justify-between mt-4">
        <div className="font-bold text-l"> {name} </div>
        <div className="font-bold text-l text-gray-500"> ${price}
              per kg </div>
        </div>
    <div className="flex justify-between mt-4 w-2/4 m-auto">
    <button
        className="pl-2 pr-2 bg-red-400 text-white rounded-md"
        disabled={productAmount === 0}
        onClick={() => handleAmount('decrement')}>
        -
    </button>
        <div>{productAmount}</div>
    <button
        className="pl-2 pr-2 bg-green-400 text-white rounded-md"
        onClick={() => handleAmount('increment')}>
        +
    </button>
      </div>
    </div>
);
}
export default ProductCard;
```

if we try to increment and decrement our products' amount, the number inside of the ProductCard component will change after each button click

**however, we still need to link the global items state to the Navbar component:**

```
import { useContext } from 'react';
import Link from 'next/link';
import cartContext from '../components/context/cartContext';
function Navbar() {
  const { items } = useContext(cartContext);
  // ..
```

## update products in the Navbar
we don't need to update the global items state from our nav bar so we don't need to declare the setItems function
- we only want to display the TOTAL AMOUNT of products added to the shopping cart

```
import { useContext } from 'react';
import Link from 'next/link';
import cartContext from '../components/context/cartContext';
function Navbar() {
  const { items } = useContext(cartContext);; /* To be implemented */
  const totalItemsAmount = Object.values(items)
  .reduce((x, y) => x + y, 0);
  return (
    <div className="w-full bg-purple-600 p-4 text-white">
      <div className="w-9/12 m-auto flex justify-between">
        <div className="font-bold">
          <Link href="/" passHref>
            <a> My e-commerce </a>
          </Link>
        </div>
        <div className="font-bold underline">
          <Link href="/cart" passHref>
          <a>{totalItemsAmount} items in cart</a>
          </Link>
        </div>
      </div>
    </div>
  );
}

export default Navbar;
```

**let's fix the /pages/cart.js page to display the products on the checkout page:**

```
import { useContext } from 'react';
import cartContext from '../components/context/cartContext';
import data from '../data/items';
    function Cart() {
        const { items } = useContext(cartContext);
        // ...
```

## add context to the Cart
**we need to write a getFullItem function outside of our component that only takes an id and returns the entire product object:**

```
import { useContext } from 'react';
import cartContext from '../components/context/cartContext';
import data from '../data/items';
function getFullItem(id) {
  const idx = data.findIndex((item) => item.id === id);
  return data[idx];
}
function Cart() {
  const { items } = useContext(cartContext);
// ...
```

we now have access to the complete product object
- **let's get the total price of all our products inside of the shopping cart:**

```
// ...
function Cart() {
  const { items } = useContext(cartContext);
  const total = Object.keys(items)
    .map((id) => getFullItem(id).price * items[id])
    .reduce((x, y) => x + y, 0);


// ...
```

now we want to display a list of products inside of the shopping cart in the format of ```x2 Carrots ($7)```
- we can create a new array called amounts and fill it with all the products we've added to the cart, plus the amount for every single product

**let's update our cart.js page with these changes:**

```
import { useContext } from 'react';
import cartContext from '../components/context/cartContext';
import data from '../data/items';

function getFullItem(id) {
  const idx = data.findIndex((item) => item.id === id);
  return data[idx];
}

function Cart() {
  const { items } = useContext(cartContext);
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

now we can add as many products as we want to the shopping cart and see the total price going to the /cart page