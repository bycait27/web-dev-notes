# client-side navigation

## client-side navigation in next.js
next.js supports the html standard ```<a>``` tags for linking pages
- provides a **more optimized way for navigation between different routes:** the Link component
    - this is used for linking different pages or sections of our website

**for example, the file navbar.js inside the pages/ folder contains the following code:**

```
import Link from 'next/link';

function Navbar() {
    return (
        <div style={{display: "flex", justifyContent: "space-between",padding: "1em", backgroundColor: "#f0f0f0"}}>
            <Link href='/'>Home</Link>
            <Link href='/about'>About</Link>
            <Link href='/contacts'>Contacts</Link>
        </div>
    );
}

export default Navbar;
```

by default, next.js will preload every single Link found on the viewport
- once we click on one of the links, the browser will already have all the data needed to render the page
    - this feature **can be disabled** by passing the ```preload={false}``` prop to the Link component

*we are also able to link pages with dynamic route variables with ease

to link the following page using next.js 10, /blog/[date]/[slug].js, we can use the href prop for setting both the page we want to render and the url displayed in the browser's address bar:

```
<Link href='/blog/2021-01-01/happy-new-year'> Read post </Link>
<Link href='/blog/2021-03-05/match-update'> Read post </Link>
<Link href='/blog/2021-04-23/i-love-nextjs'> Read post </Link>
```

we can also pass an object to the href prop for more complex urls:

```
<Link
  href={{
    pathname: '/blog/[date]/[slug]',
    query: {
      date: '2020-01-01',
      slug: 'happy-new-year',
      foo: 'bar'
    }
  }}
>
  Read post
</Link>
```

when the user clicks that link, next.js will redirect the browser to the blog page

***note what the [slug].js file content looks like inside the pages/blog/[date] folder:**

```
import { useRouter } from "next/router";

function Post() {
  const router = useRouter();
  const { date, slug } = router.query;

  return (
    <div>
      <h1>Post: {slug}</h1>
      <p>
        This is blog post from {date} and the slug is {slug}.
      </p>
    </div>
  );
}

export default Post;
```

## using the router.push method
we can also use the useRouter hook to move between our next.js website pages

**for example, we can use the useRouter hook to dynamically redirect a user if, they're not logged in:**

```
import { useEffect } from "react";
import { useRouter } from "next/router";
import PrivateComponent from "../components/Private";
import useAuth from "../hooks/auth";

function MyPage() {
  const router = useRouter();
  const { loggedIn } = useAuth();
  useEffect(() => {
    if (!loggedIn) {
      router.push("/login");
    }
  }, [loggedIn]);
  return loggedIn ? <PrivateComponent /> : null;
}
export default MyPage;
```

we use the useEffect hook to run the code on the client side only
- if the user isn't logged in, we use the router.push method to redirect them to the login page

**like the Link component, we can create more complex page routes by passing an object to the push method:**

```
router.push({
  pathname: "/blog/[date]/[slug]",
  query: {
    date: "2021-01-01",
    slug: "happy-new-year",
    foo: "bar",
  },
});
```

once the router.push method has been called, the browser will be redirected to /blog/2021-01-01/happy-new-year?foo=bar

***using the router.push method is handy when we need to redirect a user on the client side after a certain action occurs**
- it is NOT RECOMMENDED to be used as a default way for handling client-side navigation