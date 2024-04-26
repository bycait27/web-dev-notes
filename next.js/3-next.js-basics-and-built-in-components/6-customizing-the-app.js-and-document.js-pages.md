# customizing the _app.js and _document.js pages
there are certain cases where we need to take control over page initialization so that every time we render a page, next.js will need to run certain operations before sending the resulting html to the client
- we will use the _app.js and _document.js files inside our pages directory to do this

## the _app.js page
**by default, next.js sjips with the following pages/_app.js file:**

```
import "../styles/globals.css";
function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
export default MyApp;
```
the function is just returning the next.js page component (the Component prop) and its props (pageProps)

**let's create a Navbar.js component:**

```
import Link from "next/link";
function Navbar() {
  return (
    <div
      style={{
        display: "flex",
        flexDirection: "row",
        justifyContent: "space-between",
        marginBottom: 25,
      }}
    >
      <div>My Website</div>
      <div>
        <Link href="/">Home </Link>
        <Link href="/about">About </Link>
        <Link href="/contacts">Contacts </Link>
      </div>
    </div>
  );
}
export default Navbar;
```

**now let's import our Navbar.js component inside our _app.js page:**

```
import "../styles/globals.css";
import Navbar from "../components/Navbar";
function MyApp({ Component, pageProps }) {
  <Navbar />;
  return <Component {...pageProps} />;
}
export default MyApp;
```

if we create two more pages named about.js and contacts.js, we'll see that the Navbar component will be rendered on any page

we can also add support for both dark and light themes
- we can use react context and wrap the Component component inside our _app.js file

**let's create a context in components/themeContext.js:**

```
import { createContext } from 'react';
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => null
});
export default ThemeContext;
```

**if we go back to our _app.js file, we can also create the theme state and inline css styles and wrap the page component in a context provider:**

```
import { useState } from "react";
import ThemeContext from "../components/themeContext";
import Navbar from "../components/Navbar";
const themes = {
  dark: {
    background: "black",
    color: "white",
  },
  light: {
    background: "white",
    color: "black",
  },
};
function MyApp({ Component, pageProps }) {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => {
    setTheme(theme === "dark" ? "light" : "dark");
  };
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <div
        style={{
          width: "100%",
          minHeight: "100vh",
          ...themes[theme],
        }}
      >
        <Navbar />
        <Component {...pageProps} />
      </div>
    </ThemeContext.Provider>
  );
}
export default MyApp;
```

**then, we need to add a button for toggling dark/light themes. we can add this to our Navbar.js component:**

```
import { useContext } from "react";
import Link from "next/link";
import ThemeContext from "./themeContext";

function Navbar() {
  const { toggleTheme, theme } = useContext(ThemeContext);
  const newThemeName = theme === "dark" ? "light" : "dark";

  return (
    <nav
      style={{
        display: "flex",
        alignItems: "center",
        justifyContent: "space-between",
        padding: "15px 20px",
        background: "#f0f0f0",
      }}
    >
      <div style={{ fontWeight: "bold" }}>My Website</div>

      <div style={{ display: "flex", alignItems: "center" }}>
        <Link href="/" style={linkStyle}>
          <a>Home</a>
        </Link>

        <span style={{ margin: "0 10px" }}>/</span>
        <Link href="/about" style={linkStyle}>
          <a>About</a>
        </Link>

        <span style={{ margin: "0 10px" }}>/</span>
        <Link href="/contacts" style={linkStyle}>
          <a>Contacts</a>
        </Link>

        <button onClick={toggleTheme} style={buttonStyle}>
          Set {newThemeName} theme
        </button>
      </div>
    </nav>
  );
}

const linkStyle = {
  textDecoration: "none",
  color: "black",
  fontWeight: "bold",
};

const buttonStyle = {
  background: "#007bff",
  color: "white",
  border: "none",
  borderRadius: "4px",
  cursor: "pointer",
  padding: "8px 16px",
  fontWeight: "bold",
};

export default Navbar;
```

next.js will keep the theme state consistent between every route change

*the _app.js isn't meant for running data fetching using getServerSideProps or getStaticProps
- it's main use cases are maintaining state between pages during navigation, adding global styles, handling page layouts, or adding additional data to the page props

IF we HAVE TO fetch data on the server side every time we render a page, we can still use the built-in getInitialProps function, but it has a cost
- we will lose automatic static optimization in dynamic pages because next.js will need to perform ssr for every single page

**we can do this by using that built-in method:**

```
import App from 'next/app'
function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
};
MyApp.getInitialProps = async (appContext) => {
  const appProps = await App.getInitialProps(appContext);
  const additionalProps = await fetch(...)
  return {
    ...appProps,
    ...additionalProps
  }
};
export default MyApp;
```

*there might be cases where we need to customize html tags, not just react components

## the _document.js page
when writing next.js components, we don't need to define fundamental html tags

in order to render the two essential tags, ```<html>``` and ```<body>```, next.js uses a built-in class called Document
- **this class allows us to extend it by creating a new file called _document.js inside our pages/ directory:**

```
import Document, {
  Html,
  Head,
  Main,
  NextScript
} from 'next/document';

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx);
    return { ...initialProps };
  }

  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

1. we import the Document class, which we're going to extend to add our custom scripts
2. **we import Html:** this is the ```<html>``` tag for our next.js app. we can pass any standard html property to it as a prop
3. **we import Head:** thid component is used for all the tags common to all the app pages. this is not the Head component we've seen previously. they behave similarly, but we should use it only for code that is common to all we pages
4. **we import Main:** this will replace where next.js renders our page components. the browser won't initialize every component oustide ```<Main>```, so if we need to share common components between our pages, we should place them inside the _app.js file
5. **we import NextScript:** inside the custom javascripts generated by next.js, we can find all the code required to run client-side logic, react hydration, and so on

*REMOVING ANY OF THESE COMPONENTS WILL BREAK OUR NEXT.JS APP

_document.js DOES NOT support server-side data fetching methods, such as getServerSideProps and getStaticProps
- we do still get access to getInitialProps, but we should avoid putting data fetching functions inside it because this would disable automatic site optimization, forcing the server to server-side render the page on each request