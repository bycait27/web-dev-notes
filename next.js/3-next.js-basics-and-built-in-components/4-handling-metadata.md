# handling metadata
in modern web development, it is crucial to correctly handle metadata

when sharing a website on social networks, a card pops up with an image
- we can use the open graph protocol: [Open Graph](https://ogp.me/)
- in order to give that info to any social network or website, we need to add some metadata to our page

*the user experience could be negatively affected without proper metadata, as these meta tags help the browser create an optimized experience for our users

## using the head component
**next.js provides a great way of solving these problems:** the built-in Head component
- this component allows us to update the ```<head>``` section of our html page from any component
    - we can dynamically change, add, or delete any metadata, link, or script at runtime depending on our user's navigation

the most common metadata is the ```<title>``` tag

**let's make an index.js file first:**

```
import Head from "next/head";
import Link from "next/link";

function IndexPage() {
  return (
    <>
      <Head>
        <title>Welcome to my Next.js website </title>
      </Head>
      <div>
        <Link href="/about" passHref>
          <a> About us </a>
        </Link>
      </div>
    </>
  );
}
export default IndexPage;
```

**next, let's create an about.js file:**

```
import Head from "next/head";
import Link from "next/link";

function AboutPage() {
  return (
    <>
      <Head>
        <title>About this website</title>
      </Head>
      <div>
        <Link href="/" passHref>
          Back to home
        </Link>
      </div>
    </>
  );
}

export default AboutPage;
```

when viewing these pages in a web browser, the ```<title>``` content changes depending on the route we're visiting

now let's create a new component that only displays a button. once we click it, our page title will change depending on the page we're currently on. we can also roll back to the original title by clicking the button again

**let's create a file named Widget.js in our components folder:**

```
import { useState } from 'react';
import Head from 'next/head';

function Widget({ pageName }) {
  const [active, setActive] = useState(false);

  if (active) {
    return (
      <>
        <Head>
          <title>You're browsing the {pageName} page</title>
        </Head>
        <div>
          <button onClick={() => setActive(false)}>
            Restore original title
          </button>
          Take a look at the title!
        </div>
      </>
    );
  }

  return (
    <>
      <button onClick={() => setActive(true)}>
        Change page title
      </button>
    </>
  );
}

export default Widget;
```

**now we need to include this component in our index.js and about.js pages:**

```
import Head from "next/head";
import Link from "next/link";
import Widget from "../components/Widget";

function IndexPage() {
  return (
    <>
      <Head>
        <title>Welcome to my Next.js website</title>
      </Head>
      <div>
        <Link href="/about" passHref>
          <a>About us</a>
        </Link>
      </div>
      <div>
        <Widget pageName="index" />
      </div>
    </>
  );
}
export default IndexPage;
```

```
import { useState } from "react";
import Head from "next/head";
function Widget({ pageName }) {
  const [active, setActive] = useState(false);
  if (active) {
    return (
      <>
        <Head>
          <title> You're browsing the {pageName} page</title>
        </Head>
        <div>
          <button onClick={() => setActive(false)}>
            Restore original title
          </button>
          Take a look at the title!
        </div>
      </>
    );
  }
  return (
    <>
      <button onClick={() => setActive(true)}>Change page title</button>
    </>
  );
}
export default Widget;
```

now every time we click "Change the page title," next.js will update the html ```<title>``` element

***if we are trying to update the same meta tag multiple times in different components, we can use the key prop so that next.js will only look for the tag with the specific key and update it instead of adding a new one**