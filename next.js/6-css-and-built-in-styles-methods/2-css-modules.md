# css modules 

## selection of styling method in next.js
the css-in-js approach has some drawbacks to consider when choosing the styling method for a new next.js app
- many css-in-js libraries don't provide good ide/code editor support, making things way harder for devs
    - no syntax highlighting
    - no autocomplete
    - no linting
    - etc.
- using css-in-js forces us to adopt more and more dependencies, making our app bundle bigger and slower
- even if we pre-generate css rules on the server side, we would need to re-generate them after react hydration on the client side
    - this adds a high runtime cost, making the web app slower and slower (it will only worsen with more added features)

an alternative to css-in-js are **css modules**
- css modules are able to create css classes with the same names but different purposes by writing plain css classes and then importing them to our react components without any runtime cost

**let's look at an example landing page with a blue background and welcome text. our pages/index.js file will look as follows:**

```
import styles from "../styles/Home.module.css";
export default function Home() {
  return (
    <div className={styles.homepage}>
      <h1 className={styles.title}> Welcome to the CSS Modules example </h1>
    </div>
  );
}
```

we import our css classes from a plain css file ending with .module.css
- the css modules transform its content into a javascript object, where every key is a class name

**here is the contents of the Home.module.css file:**

```
.homepage {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  background-color: #2196f3;
}
.title {
  color: #f5f5f5;
}
```

**if we inspect the generated html page, our landing page will contain a div with a class that looks like this:**

```
<div class="Home_homepage__Qlefa">
   <h1 class="Home_title__38XO6">
      Welcome to the CSS Modules example
   </h1>
</div>
```

css modules generated unique class names for our rules
- even if we were to create new classes using the same generic title and homepage names in other css files, there won't be any naming collision thanks to that strategy

**for global styles, we can create a new styles/globals.css file with the following content:**

```
html,body 
{
   padding: 0;
   margin: 0;
   font-family: sans-serif;
}
```

**we can also use the :global keyword to create globally available css rules:**

```
.button :global {
    padding: 5px;
    background-color: blue;
    color: white;
    border: none;
    border-radius: 5px;
}
```

we can also use **selector composition** which allows us to create a very generic rule and then override some of its properties by using the composes property:

```
.button-default {
  padding: 5px;
  border: none;
  border-radius: 5px;
  background-color: grey;
  color: black;
}
.button-success {
  composes: button-default;
  background-color: green;
  color: white;
}
.button-danger {
  composes: button-default;
  background-color: red;
  color: white;
}
```

*the main idea of css modules is to provide a straightforward way to write modular css classes with ZERO runtime costs in EVERY language

the [official repository](https://github.com/css-modules/css-modules) has more info on how to use css modules

css modules are also available out of the box in every next.js installation (we can use them immediately after bootstrapping our project)
- we may need to tweak the default configuration to add, remove, or edit some features

*next.js compiles css modules using **postcss**, a popular tool for compiling css at build time

**the default configurations by next.js include the following features:**
- **autoprefixer:** it adds vendor prefixes to our css rules using values from [can i use](https://caniuse.com/). for instance, if we're writing a rule for the ::placeholder selector, it will compile it to make it compatible with all the browsers where the selector is slightly different, such as :-ms-input- placeholder, ::-moz-placeholder, etc. we can [learn more about that feature on github](https://github.com/postcss/autoprefixer)
- **cross-browser flexbox bug fixes:** postcss follows a [community-curated list of flexbox issues](https://github.com/philipwalton/flexbugs) and adds some workarounds for making it work correctly on every browser
- **ie11 compatibility:** postcss compiles new css features, making them available on older browsers, such as ie11. still, there's an exception: css variables are not compiled, as it isn't safe to do so. if you really need to support older browsers and still want to use them, you can jump to the next lesson and uss sass/scss variables

**we can edit the postcss default configuration by creating a postcss.config.json file  inside the root of our project and adding the default next.js configuration:**

```
{
    "plugins": [
    "postcss-flexbugs-fixes",
        [
            "postcss-preset-env",
            {
                "autoprefixer": {
                "flexbox": "no-2009"
            },
                "stage": 3,
                "features": {
                "custom-properties": false
                }
            }
        ]
    ]
}
```

now, we can further edit the configuration as we prefer, adding, removing, or changing any property