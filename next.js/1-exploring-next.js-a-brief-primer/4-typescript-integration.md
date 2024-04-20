# typescript integration

## typescript configuration in next.js
the next.js source code is written in typescript and natively provides high-quality type definitions to make our dev experience better
- to use typescript in our next.js project, we need to create a typescript configuration file inside the root of our project (tsconfig.json)
- once all the dependencies are installed and our javascript files are converted to typescript, we can run our project

even if we created an empty tsconfig.json file, after installing the required dependencies and rerunning the project, next.js fills it with its default configurations
- we can always customize the typeScript options inside that file, but next.js uses babel to handle typeScript files (via the @babel/plugin-transform-typescript)
    - **this has some caveats, including the following:**

        - the @babel/plugin-transform-typescript plug-in does not support const enum, often used in typeScript (need to add babel-plugin-const-enum to the babel configuration)

        - neither export = nor import = are supported because they can't be compiled to valid ECMAScript code
            - we should either install babel-plugin-replace-ts-export-assignment or convert our imports and exports to valid ECMAScript directives, such as import x, {y} from 'some-package' and export default x

        - *read about the other caveats too before going further with using typeScript with our next.js project

some compiler options might be different from the default typescript ones (make sure to read the babel documentation)

there is also a next-env.d.ts file in the root of our project
- we can edit this if needed, but DO NOT delete itß

## custom babel and webpack configuration
**babel:** a javascript transcompiler mainly used for transforming modern javascript code into a backward-compatible script, which will run without a problem on any browser

babel is good for when we are writing a web app that must support older web browsers (internet explorer 10 or 11)
- allows us to use modern javascript features and will transform them into ie-compatible code at build time

babel also transpiles modern code into a compatible script for today's environments

we can customize our default next.js babel configuration by creating a new file called .babelrc in the root of our project
- **DO NOT leave this empty, add the following code:**

```
{
    "presets": ["next/babel"]
}
```

## webpack configuration
**we can update our custom .babelrc file as follows:**

```
{
  "presets": ["next/babel"],
  "plugins": [
    [
      "@babel/plugin-proposal-pipeline-operator",
      { "proposal": "fsharp" }
    ]
  ]
}
```

webpack creates the bundles containing the compiled code for a specific library, page, or feature
- webpack is like an infrastructure for orchestrating different compilation, bundle, and minification tasks for every web asset (javascript files, css, svg, etc.)

if we want to use sass or less to create our app styles, we need to customize the default webpack configuration to parse sass/less files and  produce plain css as output (same as javascript code using babel as a transpiler)

***next.js provides a convention-over-configuration approach**
- we don't need to customize most of its settings for building a real-world app
    - we just need to follow some code conventions

## understanding next.config.js
if we do want to build something custom, we can edit the default settings via the next.config.js file
- this file goes inside the root of the project
- exports an object by default where its properties will override the default next.js configurations

**for example, we can create a new property inside this object called webpack (we can add a new webpack loader called my-custom-loader)**

```
module.exports = {
  webpack: (config, options) => {
    config.module.rules.push({
      test: /\.js/,
      use: [
        options.defaultLoaders.babel,
        // This is just an example
        // don't try to run this because it won't work
        {
          loader: "my-custom-loader", // Set our loader
          options: loaderOptions,
          options
        },
      ],
    });
    return config;
  },
};
```

these custom settings will be merged later with next.js's default settings
- we can extend, override, or delete any setting from the default configuration (not a good idea, but there may be times where we need it)