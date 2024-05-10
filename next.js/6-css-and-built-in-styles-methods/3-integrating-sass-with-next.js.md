# integrating sass with next.js
**sass** is one of the most loved and used css preprocessors out there, and next.js did an excellent job making it easy to integrate it
- sass is supported out of the box
    - we just have to install the sass npm package inside our next.js project, and it's ready to use

## css modules with sass and scss syntax
**here is an example using the pages/index.js file:**

```
import styles from '../styles/Home.module.scss';
export default function Home() {
return (
    <div className={styles.homepage}>
      <h1> Welcome to the CSS Modules example </h1>
    </div>
  );
}
```

we just had to change the import to /styles/Home.module/scss

sass and scss makes our code even more modular and easy to maintain thanks to its syntax

***sass and scss are two different syntaxes for the same css preprocessor**
- they both provide enhanced ways for writing css styles, such as for variables, loops, mixins, and many other features
- the **main difference** is that scss (sassy css) extends the css syntax by adding those features available in every .scss file
    - any standard .css file can be renamed as .scss without any problem because the css syntax is valid in .scss files
- sass is an OLDER syntax that ISN"T COMPATIBLE with standard css
    - doesn't use curly brackets or semicolons
    - uses indentation and new lines to separate properties and nested selectors
- both syntaxes need to be transpiled into vanilla css in order to be used on regular web browsers

**here is an example of the use of the @extend keyword with scss (works just like the compose keyword from css modules):**

```
.button-default {
  padding: 5px;
  border: none;
  border-radius: 5px;
  background-color: grey;
  color: black;
}
.button-success {
  @extend .button-default;
  background-color: green;
  color: white;
}
.button-danger {
  @extend .button-default;
  background-color: red;
  color: white;
}
```

**alternatively, we can change our class names a bit and take advantage of the selector nesting feature:**

```
.button {
  padding: 5px;
  border: none;
  border-radius: 5px;
  background-color: grey;
  color: black;
  &.success {
  background-color: green;
  color: white;
  }
  &.danger {
  background-color: red;
  color: white;
  }
}
```

*scss ships with a large set of features, such as loops, mixins, functions, and more, allowing any dev to write complex uis with ease

## customizing sass configuration in next.js
there may be times when we need to enable or disable some specific feature or edit the default sass configuration in next.js

**we can do this by editing the next.config.js configuration file:**

```
module.exports = {
  sassOptions: {
  outputStyle: 'compressed'
  // ...add any SASS configuration here
  },
}
```

*to learn more about sass and scss, we can look at the [official documentation](https://sass-lang.com/)