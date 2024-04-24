# routing system

## next.js routing
libraries such as react router allow us to create client-side routes only
- all the pages will be created and rendered on the client side (no ssr involved)

**next.js uses a different approach:** file system-based pages and routes
- a default next.js project ships with a pages/ directory
    - every file inside this folder represents a new page or route for our app

a next.js page is a react component exported from any .js, .jsx, .ts, or .tsx files inside the pages/ folder

**simple example:** 
in order to create a simple website with two pages, the first one will be the home page and the second page will be a simply contact page
- we would just create an index.js file and a contacts.js file in the pages/ folder
    - both of these files will need to export a function returning some jsx content, which will be rendered on the server side and sent to the browser as standard html
    - if we wanted to rename the contact.js file to contact-us.js, next.js would automatically rebuild the page using the new route name for us

**more complicated example:**
in order to create a blog website, we would need to have dynamic routes that represent each blog post

we also want to create a /posts page that will show every post present on the website

**the dynamic route will look like this:**

```
pages/
    - index.js
    - contact-us.js
    - posts/
        - index.js
        - [slug].js
```

we have created a **nested route** by adding a /posts route inside the pages/ folder
- this contains the new index.js file which exports a function containing some jsx code to make the new **posts route**

### creating dynamic routes for blog posts
we want to create a dynamic route for every blog post so that we don't have to manually create a new page every time we want to publish an article on our website
- we create a new file inside the pages/posts/ folder called [slug].js
    - [slug] identifies a route variable that can contain any value, depending on what the user types in the browser's address bar
    - we are creating a new route contianing a variable called slug, which can vary for every blog post
    - we can export a simple function returning some jsx code from that file and then browse to /posts/my-firstpost, / posts/foo-bar-baz, or any other /posts/* route 
        - whatever route we browse to, it will always render the same jsx code
    

we can also nest multiple dynamic routes inside the pages/ folder
- we can add a new folder called [date] inside our pages/ directory and move the slug.js file inside it

```
pages/
    - index.js
    - contact-us.js
    - posts/
        - index.js
        - [date]/
            - [slug].js
```

both the [date] and [slug] variables can represent whatever we want (/posts/2021-01-01/my-first-post)

the variables are mainly meant for creating highly dynamic pages with different content depending on the route ariables we're using

### using route variables inside pages
suppose we want to create a greeting page, inside the pages/ directory we will create a greet/ folder that will contain the file [name].js

**the contents of [name].js look like this:**

```
export async function getServerSideProps({ params }) {
  const { name } = params;
  return {
    props: {
      name,
    },
  };
}

function Greet(props) {
  return <h1> Hello, {props.name}! </h1>;
}

export default Greet;
```

we use the next.js built-in getServerSideProps function to dynamically get the [name] variable from the URL and greet the user!

***the getServerSideProps and getStaticProps functions must return an object and the props must be passed inside the returning object's props property**

we could have also used the url to get user data from a database to show their profile

there are times when we need to fetch route variables from our components rather than our pages 
- we use a **react hook** for this

### using route variables inside components
*we cannot use getServerSideProps and getStaticProps outside of our page

**we can use the useRouter hook and instantiate it inside any component:**

```
import { useRouter } from 'next/router';

function Greet() {
    const { query } = useRouter();
    return <h1>Hello {query.name}!</h1>;
}

export default Greet;
```

we extract the query parameter from the useRouter hook
- this contains our route variables (name) and the parsed query string parameters

we can observe how next.js passes the route variables and query strings via the useRouter hook by trying to append any query parameter to our url and log the wuery variable inside our component

**the following object will be logged inside our console of the web page (/greet/Mitch?learning_nextjs=true):**

```
  {learning_nextjs: "true", name: "Mitch"}
```

*next.js doesn’t throw any error if we try to append a query parameter with the same key as our routing variable. we can easily try that by **calling the following url: URL/greet/Mitch?name=Christine.** next.js will give precedence to our route variable

