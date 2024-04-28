# organizing the folder structure

## next.js project structure
it is very important to organize our new project's folder structure neatly and clearly
- this keeps our code base scalable and maintainable

next.js forces us to place some files and folders in particular locations of our cade base (_app.js and _document.js files, pages/ and public/ directories, etc.)
- also provides a way to customize their placement inside our project repository

**quick recap on project folder structure:**

```
next-js-app
    - node_modules/
    - package.json
    - pages/
    - public/
    - styles/
```

**node_modules/:** the default folder for node.js project dependencies

**pages/:** the directory where we place our pages and build the routing system for our web app

**public/:** the directory where we place files to be served as static assets (compiled css and javascript files, images, and icons)

**styles/:** the directory where we place our styling modules, regardless of their format (css, sass, less)

we can then customize our repository structure to make it easier to navigate through
- we can move our pages/ directory inside a src/ folder
- we can also move all the other directories (except for the public/ one and node_modules) inside src/, making our root directory a bit cleaner

## organizing the components
we can start by separating components into three different categories and then putting styles and tests in the same folder for each component

**for example:** a new components/ folder is created inside the root directory
- we move inside this folder and create the following folders: atoms, molecules, organisms, templates

this is called the **atomic design principle**, where we divide our components into different levels to better organize our code base

**includes four categories:**
- **atoms:** these are the most basic components that we'll ever write in our code base. sometimes, they act as a wrapper for standard html elements, such as button, input, and p, but we can also add animations, color palettes, and so on, to this category of components
- **molecules:** these are small groups of atoms combined to create slightly more complex structures with a minimum of utility. the combination of the input atom and the label atom can serve as a simple and clear example of a molecule
- **organisms:** molecules and atoms combine to create  complex structures, such as  a registration form, a footer, and a carousel
- **templates:** we can think of templates as the skeleton of our pages. here, we decide where to put organisms, atoms, and molecules together to create the final page that the user will browse

### creating a button component
to create a new component, we often need at least **three different files:** the component itself, the style, and a test file
- we create those files by moving inside components/atoms/ and then creating a new folder called Button

organizing our components this way will help us a lot when we need to search, update, or fix a given component
- fixing a bug is made easier because we can easily find the component inside our cade base, find its test and styling files, and fix them

***this atomic design principle helps keep the project structure tidy and easy to maintain over time**

## organizing utilities
there are specific files that don't export any component (they're just modular scripts used for many different purposes)
- these are called **utility scripts**

if we want to use a specific function for multiple components, we can create a generic utility function and import it inside every component that needs that kind of feature
- all of these utility functions will go inside a utilities/ folder
- each utility function will have its own file according to their purpose
- we will name these files according to their function

we can also create test files for these utility functions (localStorage.test.js) to ensure that they work as expected

*we can also create a different folder for each utility, to hold its tests, styles, and other stuff to make our code base even more organized

## organizing static assets
next.js makes it easy to serve static files because we only need to put them inside the public/ folder, and the framework will do the rest

**the following are common static assets we may want to serve:**
- images
- compiled javascript files
- compiled css files
- icons (including favicon and web app icons)
- manifest.json, robot.txt, and other static files

inside our public/ folder, we will create an assets/ folder
- inside this folder, we will add a folder for each type of static asset (js, css, icons, images)
    - our compiled vendor javascript files are placed inside the js/ directory 
    - the compiled vendor css files are placed inside the css/ directory
    - the icons/ directory will contain our web app manifest icons
        - **web app manifest:** a json file that includes some useful info about the web app that we're building, such as the app name and the icons to use when installing it on a mobile device

**here is an example of a manifest.json file:**

```
{
  "name": "My Next.js App",
  "short_name": "Next.js App",
  "description": "A test app made with next.js",
  "background_color": "#a600ff",
  "display": "standalone",
  "theme_color": "#a600ff",
  "icons": [
    {
      "src": "/assets/icons/icon-192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/assets/icons/icon-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ]
}
```

**this file can be linked inside the html meta tag as follows:**

```
<link rel="manifest" href="/manifest.json">
```

this allows users to install the app on their smartphones or tablets!

## organizing styles
style organization depends on the stack we are using

a common approach is to create a specific styling file for each component
- this makes it easier for us to find a particular component style inside our code base when we need to make some changes

however, we may also need to create some common styles or utility files, such as color palettes, themes, and media queries
- it can be useful to reuse the default styles/ directory shipped with a default next.js installation
    - we can put our common styles inside that folder and import them inside other styling files only when we need them

## using lib files
**lib files:** refer to scripts that explicitly wrap third-party libraries as lib files
- lib files are specific for a certain library

**for example:** say we want to initialize a graphql client, save some graphql queries and mutations locally, etc.
- we can store them inside a new folder called graphql/, which lies inside inside a lib/ directory at the root of our project

**here is what the file structure would look like:**

```
next-js-app
    - lib/
        - graphql/
            - index.js
            - queries/
                - query1.js
                - query2.js
            - mutations/
                - mutation1.js
                - mutation2.js
```

other lib scripts can include all those files connecting and making queries to redis, rabbitmq, etc., or functions specific to any external library

an organized file structure can help us manage the app state

we want our components to be dynamic most of the time (they can render content and behave differently depending on the global app state or the data coming from external services)
- we will need to call external apis to retrieve our web app content dynamically in many cases