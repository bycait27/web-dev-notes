# consuming graphql apis
graphql has been a game changer in the api world (easy to use, modularity, and felxibility)
- it is a query language for apis
- improves many key aspects of data fetching and manipulation compared to other web service architectures, such as rest or soap
- allows us to avoid data overfetching (we can simply query the data fields we need), get multiple resources within a single request, obtain a strongly and statically typed interface for our data, avoid api versioningg, etc.

## setting up apollo client with next.js
**let's create an apollo client for our next.js app. we'll make a new file called index.js in our lib/apollo/ directory:**

```
import { useMemo } from "react";
import { ApolloClient, HttpLink, InMemoryCache } from "@apollo/client";

let uri = "https://ed-6555414034513920.educative.run/v1/graphql";
let apolloClient;

function createApolloClient() {
  return new ApolloClient({
    ssrMode: typeof window === "undefined",
    link: new HttpLink({ uri }),
    cache: new InMemoryCache(),
  });
}
```

by setting ssrMode: typeof window === "undefined" on **line 10**, we'll use the same apollo instance for both client and server
- ApolloClient uses the browser fetch api to make http requests, so we'll need to import a polyfill to make it work on the server side (isomorphic-unfetch)

**let's add a new function to initialize the apollo client:**

```
export function initApollo(initialState = null) {
  const client = apolloClient || createApolloClient();

  if (initialState) {
    const existingCache = client.extract();
    client.cache.restore({ ...existingCache, ...initialState });
  }

  if (typeof window === "undefined") {
    return client;
  }

  if (!apolloClient) {
    apolloClient = client;
  }

  return client;
}
```

*this function will allow us to avoid recreating a new apollo client for each page
- we'll store a client instance on the server (inside the previously written apolloClient variable) where we can pass an initial state as an argument
    - if we pass that parameter to the initApollo function, it will be merged witht he local cache to restore a full representation of the state once we move to another page

**we need to add another import statement to our index.js file using the react useMemo hook to speed up the process:**

```
  import { useMemo } from "react";

  export function useApollo(initialState) {
  return useMemo(
    () => initApollo(initialState),
    [initialState]
  );
}
```

**in our pages/ directory, we can now create a new _app.js file where we'll wrap the whole app using the official apollo context provider:**

```
import { ApolloProvider } from "@apollo/client";
import { useApollo } from "../lib/apollo";

export default function App({ Component, pageProps }) {
  const apolloClient = useApollo(pageProps.initialApolloState);
  return (
    <ApolloProvider client={apolloClient}>
      <Component {...pageProps} />
    </ApolloProvider>
  );
}
```

## writing graphql queries
we will organize our grqphql queries inside a folder called lib/apollo/queries/

**let's create a new file, lib/apollo/queries/getLatestSigns.js, exposing the following graphql query:**

```
import { gql } from "@apollo/client";

const GET_LATEST_SIGNS = gql`
  query GetLatestSigns($limit: Int! = 10, $skip: Int! = 0) {
    sign(offset: $skip, limit: $limit, order_by: { created_at: desc }) {
      uuid
      created_at
      content
      nickname
      country
    }
  }
`;

export default GET_LATEST_SIGNS;
```

**we can import this query inside our pages/index.js file and try to make our first graphql request using apollo and next.js:**

```
import { useQuery } from "@apollo/client";
import GET_LATEST_SIGNS from '../lib/apollo/queries/getLatestSigns';

function HomePage() {
  const { loading, data } = useQuery(GET_LATEST_SIGNS, {
    fetchPolicy: 'no-cache',
  });

  return <div></div>;
}

export default HomePage;
```

**the apollo client is very easy to use. thanks to the useQuery hook, we'll have access to three different states:**
- **loading:** only returns true or false when a request is fulfilled or is still pending
- **error:** if the request fails for any reason, we'll be able to catch the error and send a nice message to the user
- **data:** this contains the data we asked for with our query

## displaying data on the home page
we'll now add a remote tailwindcss dependency for styling our demo app. 

**let's make some changes to the index.js file in the pages/ folder:**

```
import Head from 'next/head';
import { ApolloProvider } from '@apollo/client';
import { useApollo } from '../lib/apollo';

export default function App({ Component, pageProps }) {
  const apolloClient = useApollo(pageProps.initialApolloState || {});

  return (
    <ApolloProvider client={apolloClient}>
      <Head>
        <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet" />
      </Head>
      <Component {...pageProps} />
    </ApolloProvider>
  );
}
```

**now we create a new file called Loading.js in our components/ folder where we'll render it while fetching the signs from graphcms:**

```
function Loading() {
  return (
    <div className="min-h-screen w-screen flex justify-center items-center">
      Loading signs from Hasura...
    </div>
  );
}
export default Loading;
```

**now we need to display the data on the home page. we'll create a new component called Sign.js:**

```
function Sign({ content, nickname, country }) {
  return (
    <div className="max-w-7xl rounded-md border-2 border-purple-800 shadow-xl bg-purple-50 p-7 mb-10">
      <p className="text-gray-700"> {content} </p>
      <hr className="mt-3 mb-3 border-t-0 border-b-2 border-purple-800" />
      <div>
        <div className="text-purple-900">
          Written by <b>{nickname}</b>
          {country && <span> from {country}</span>}
        </div>
      </div>
    </div>
  );
}

export default Sign;
```

**now we integrate these into our home page:**

```
import Link from 'next/link';
import { useQuery } from '@apollo/client';
import GET_LATEST_SIGNS from '../lib/apollo/queries/getLatestSigns';
import Sign from '../components/Sign';
import Loading from '../components/Loading';

function HomePage() {
  const { loading, data } = useQuery(GET_LATEST_SIGNS, {
    fetchPolicy: 'no-cache',
  });

  if (loading) {
    return <Loading />;
  }

  return (
    <div className="flex justify-center items-center flex-col mt-20">
      <h1 className="text-3xl mb-5">Real-World Next.js signbook</h1>
      <Link href="/new-sign">
        <button className="mb-8 border-2 border-purple-800 text-purple-900 p-2 rounded-lg text-gray-50 m-auto mt-4">
          Add new sign
        </button>
      </Link>
      <div>
        {data.sign.map((sign) => (
          <Sign key={sign.uuid} {...sign} />
        ))}
      </div>
    </div>
  );
}

export default HomePage;
```

## creating a new sign page
**we could also create a simple route for adding a new sign by creating a new page called new-sign.js:**
- we use the useState react hook to keep track of the changes in our form for submitting the sign
- we use next.js's useRouter hook for redirecting the user to the home page once they have created a new sign
- we use apollo's useMutation hook for creating a new sign on graphcms
    - we need to import ADD_SIGN to use the mutation

```
import { useState } from "react";
import Link from "next/link";
import { useRouter } from "next/router";
import { useMutation } from "@apollo/client";
import ADD_SIGN from "../lib/apollo/queries/addSign";

function NewSign() {
  const router = useRouter();
  const [formState, setFormState] = useState({});
  const [addSign] = useMutation(ADD_SIGN, {
    onCompleted() {
    router.push("/");
    }
  });
  const handleInput = ({ e, name }) => {
    setFormState({
      ...formState,
      [name]: e.target.value
    });
  };
}
export default NewSign;
```

we're taking the entire state stored inside the formState variable coming from the useState hook and passing it as a value for the variables property used by the addSign function

**now we need to render the actual html containf a form with three inputs: user's nickname, a message, and the user's country:**

```
return (
  <div className="flex justify-center items-center flex-col mt-20">
    <h1 className="text-3xl mb-10">Sign the Real-World Next.js signbook!</h1>
    <div className="max-w-7xl shadow-xl bg-purple-50 p-7 mb-10 grid grid-rows-1 gap-4 rounded-md border-2 border-purple-800">
      <div>
        <label htmlFor="nickname" className="text-purple-900 mb-2">Nickname</label>
        <input
          id="nickname"
          type="text"
          onChange={(e) => handleInput({ e, name: "nickname" })}
          placeholder="Your name"
          className="p-2 rounded-lg w-full"
        />
      </div>
      <div>
        <label htmlFor="content" className="text-purple-900 mb-2">Leave a message! </label>
        <textarea
          id="content"
          placeholder="Leave a message here!"
          onChange={(e) => handleInput({ e, name: "content" })}
          className="p-2 rounded-lg w-full"
        />
      </div>
      <div>
        <label htmlFor="country" className="text-purple-900 mb-2">If you want, write your country name and its emoji flag </label>
        <input
          id="country"
          type="text"
          onChange={(e) => handleInput({ e, name: "country" })}
          placeholder="Country"
          className="p-2 rounded-lg w-full"
        />
        <button
          className="bg-purple-600 p-4 rounded-lg text-gray-50 m-auto mt-4"
          onClick={() => addSign({ variables: formState })}
        >
          Submit
        </button>
      </div>
    </div>
    <Link href="/" passHref>
      <a className="mt-5 underline"> Back to the homepage</a>
    </Link>
  </div>
);
```

**the addSign function represents the mutation that will add a new sign to graphcms. we need to add dynamic data by passing an object matching the mutation variables written inside the addSign.js file:**

```
import { gql } from '@apollo/client';

const ADD_SIGN = gql`
  mutation InsertNewSign($nickname: String!, $content: String!, $country: String) {
    insert_sign(objects: { nickname: $nickname, country: $country, content: $content }) {
      returning {
        uuid
      }
    }
  }
`;

export default ADD_SIGN;
```

**the ADD_SIGN mutation takes three argument variables:**
- $nickname
- $content
- $country

using form field names that reflect the naming of the mutation variables, we can simply pass the whole form state as a value to our mutation