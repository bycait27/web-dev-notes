# next.js project structure

## default project structure
*need node.js and npm on our local machine

**the following command can be used to generate any next.js app:**
- this command installs all the required dependencies and creates a few default pages

```
npx create-next-app <app-name>
```

if we run npm run dev, a development server will start on port 3000, showing a default landing page

next.js will initialize our project using the npm package manager if installed on our machine

```
npx create-next-app <app-name> --use-npm
```

if you want an example of a next.js project with another technology, we can use the --example flag followed by with-technology to generate a boilerplate from the next.js github

```
npx create-next-app <app-name> --example with-docker
```

this will clone the example repository from github into our application folder and install the required dependencies

---------------------------------------------

next.js makes navigation even easier by using the pages/ folder
- every javascript file inside the pages/ directory will be a public page
- contains all the public and static assests used on our website (images, cdd stylesheets, compiled javascript files, fonts, etc.)

there is also a styles/ directory (not strictly required for a next.js project)
- useful for organizing our app stylesheets

***the public/ and pages/ are the only mandatory and reserved directories**

we can add other directories and files to the root of our project as needed and they won't interfere with our next.js build or development process
- we can have a components/ directory and a utilities/ directory to organize these files in our project