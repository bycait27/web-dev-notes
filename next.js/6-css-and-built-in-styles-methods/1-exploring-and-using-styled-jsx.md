# exploring and using styled jsx
**styled jsx** is a built-in mechanism provided by default by next.js
- integrates a bit of javascript into our css rules
- a css-in-js library (we can use js to write css properties)

*allows us to write css rules and classes that are scoped to a specific component

**here we have a Button component styled with styled jsx:**

```
export default function Button(props) {
  return (
    <>
      <button className="button">{props.children}</button>
      <style jsx>{`
        .button {
          padding: 1em;
          border-radius: 1em;
          border: none;
          background: green;
          color: white;
        }
      `}</style>
    </>
  );
}
```

styled jsx allows us to write highly dynamic css thanks to javascript 
- it also makes sure that the rule we're declaring WON'T affect any component other than the one we're writing

**now let's create a new FancyButton component that doesn't override the Button component's styles when both are rendered on a page:**

```
export default function FancyButton(props) {
  return (
    <>
      <button className="button">{props.children}</button>
      <style jsx>{`
        .button {
          padding: 2em;
          border-radius: 2em;
          background: purple;
          color: white;
          font-size: bold;
          border: pink solid 2px;
        }
      `}</style>
    </>
  );
}

***the same happens with html sections:**

```
export default function Highlight(props) {
  return (
    <>
      <span>{props.text}</span>
      <style jsx>{`
        span {
          background: yellow;
          font-weight: bold;
        }
      `}</style>
    </>
  );
}
```

the ```<span>``` style will only affect the Highlight component, not any other ```<span>``` element inside of our pages

**if we want to create a css rule that will applied to ALL components, we can just use the global prop, and styled jsx will apply that rule to all the html elements matching our selector:**

```
export default function Highlight(props) {
  return (
    <>
      <span>{props.text}</span>
      <style jsx global>{`
        span {
          background: yellow;
          font-weight: bold;
        }
      `}</style>
    </>
  );
}
```

every time we use a ```<span>``` element, it will inherit the styles we declared inside of our Highlight component (can be risky)