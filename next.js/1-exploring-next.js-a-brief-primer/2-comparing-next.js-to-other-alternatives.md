# comparing next.js to other alternatives
next.js is not the only option for server-side rendering

the option you choose will depend on the final purpose of the project

## gatsby
gatsby is a good option for building static websites
- only supports static site generation (does it incredibly well)
- every page is pre-rendered at build time
- can be served on any cdn as a static asset (competitive performance to dynamically server-side rendered alternatives)

gatsby loses the ability to have dynamic server-side rendering 
- this is an important feature for building more dynamically data-driven and complex websites

## razzle
tool for creating server-side rendered javascript apps
- aims to maintain the ease of use of create-react-app while abstracting all the complex configurations needed for rendering the app both on the server and client sides

razzle is framework agnostic 
- we can choose our favorite front-end framework or language (react, vue, angular, elm, etc.)

## nuxt.js
**offers support for:**
- server-side rendering
- static site generation
- progressive web app management

no significant differences in performance, seo, or developement speed

nuxt.js needs more configuration
- can define layouts, global plug-ins adn components, routes, etc. 

most significant difference is the library underneath

## angular universal
angular universal allows us to run Angular applications on the server side

supports static site generation and server-side rendering

developed by Google

good alternative for those who are already familiar with angular

## why next.js?
it has an incredible set of features (components, configurations, deployment options, etc.)

next.js has an incredibly welcoming and active community ready to support you at every step in building your app (vercel team is often involved in discussions and support requests on stackoverflow and github)

## moving from react to next.js
if you have experience with react, building your first next.js app will be very easy

has philosophy very close to react and provides a convention-over-configuration approach for most of its settings (no need for complex configurations)
- for example, we can specify which pages will be server-side rendered and which will be statically generated at build time without the need to write any configuration files 
- we just have to export a specific function from our page and let next.js do its thing

**most significant difference:** react is a javascript library, while next.js is a FRAMEWORK for building rich and complete user experiences both on the client and server sides (adds tons of incredibly useful features)

***we losse access to fetch, window, document, and canvas**
- next.js provides its own way of dealing with components that must use such global variables and html elements

next.js allows us to use node.js specific libraries or apis (fs or child_process) by running our server-side code on each request or at build time before sending the data to the client

a great alternative to the create-react-app

next.js can be used as a framework for writing progressive and offline-first web apps with ease, taking advantage of its incredible built-in components and optimizations