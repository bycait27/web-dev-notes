# data fetching

## fetching techniques in next.js
**server-side data fetching can happen in two different moments:**
- at build time using getStaticProps for static pages
- at runtime using getServerSideProps for server-side rendered pages

**data can come from several resources:**
- databases
- search engines
- external apis
- filesystems
- etc.

*it is discouraged to access a database and query for specific data because next.js should only care about the frontend of our app

**let's say we're building a blog:** we want to display an author page showing their name, job, title, and biography
- the data is stored in a mysql database
    - we could easily access it using any mysql client for node.js

accessing data from next.js would make our app LESS SECURE
- a malicious user could find a way to exploit our data using an unknown framework vulnerability, injecting malicious code and using other techniques to steal our data

***suggested to delegate database connections and queries to external systems like cmses such as wordpress, strapi, and contentful, or back-end frameworks such as spring, laravel, and ruby on rails**
- this will make sure the data is coming from a trusted source 
- will sanitize the user input detecting malicious code
- will establish a secure connection between our next.js app and their apis

## fetching data on the server side
**node.js doesn't support javascript fetch apis like browsers do...we have two options for making http requests on the server:**
- **using node.js's built-in http library:** we can use this module without installing any external dependency, but even if its apis are really simple and well made, it would require a bit of extra work when compares to third-party http clients
- **using http client libraries:** there are several great http clients for next.js, making it really straightforward to make http requests from the server. popular libraries include isomorphic-unfetch (this renders the javascript fetch api available on node.js), undici (an official node.js http 1.1 client), and axios (a very popular http client that runs both on client and server with the same apis)

*axios is one of the most frequently used http clients for both client and server(rest requests)

## consuming rest apis on the server side
rest apis are divided into public and private apis:
- public apis are accessible by anyone without any kind of authorization
- private apis always need to be authorized to return some data

*an industry standard for securing apis under user authentication is a process called OAuth2 (used by google apis)

pexels apis allow us to consume their contents using an api key (an authorization token that is sent with our request)

*OAuth2, jwt, and api key are the most common ways that we'l encounter while developing our next.js apps

**let's edit the default next.js index page to include the following code:**

```
import { useEffect } from 'react';
import Link from 'next/link';
import axios from 'axios';

export async function getServerSideProps() {
  const usersReq = await axios.get('https://ed-6555414034513920.educative.run:3000/api/users');
  return {
    props: {
      users: usersReq.data,
    },
  };
}

function HomePage({ users }) {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>
          <Link href={`/users/${user.username}`} passHref>
            {user.username}
          </Link>
        </li>
      ))}
    </ul>
  );
}

export default HomePage;
```

in order to run this properly, we need to call a rest api from the built-in getServerSideProps and pass the request result as a prop to the HomePage component

we should see the list of users appear on the browser

if we try to click on one of the listed users, we will be redirected to a 404 page because we haven't created a single page user yet. **we can solve this issue by creating a new file: [username].js in the users folder**
- we will call another rest api to get a single user's data

**the [username].js file has the following code:**

```
import Link from "next/link";
import axios from "axios";

export async function getServerSideProps(ctx) {
  const { username } = ctx.query;
  try {
    const userReq = await axios.get(`https://ed-6555414034513920.educative.run:3000/api/users/${username}`,
      {
        headers: {
          token: process.env.NEXT_PUBLIC_API_TOKEN,
        },
      }
    );
    
    return {
      props: {
        user: userReq.data,
      },
    };
  } catch (error) {
    if (error.response && error.response.status === 404) {
      return {
        notFound: true,
      };
    }
    throw error; 
  }
}

function UserPage({ user }) {
  [user] = user;
  return (
    <div>
      {user.status && user.status === 404 ? (
        <h1>{user.message}</h1>
      ) : (
        <div>
          <div>
            <Link href="/" passHref>
              Back to home
            </Link>
          </div>
          <hr />
          <div style={{ display: "flex" }}>
            <img
              src="/Angryboss.png"
              alt={user.username}
              width={150}
              height={150}
            />
            <div>
              <div>
                <b>Username:</b> {user.username}
              </div>
              <div>
                <b>Full name:</b> {user.first_name} {user.fullName}
              </div>
              <div>
                <b>Email:</b> {user.email}
              </div>
              <div>
                <b>Company:</b> {user.company}
              </div>
              <div>
                <b>Job title:</b> {user.jobTitle}
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

export default UserPage;
```

axios makes it really easy to add an http header to the request because we only need to pass an object as the second argument of its get method, containing a property called headers (an object including all the http headers we want to send to the server within our request)

**process.env.NEXT_PUBLIC_API_TOKEN is bad practice for the following reasons:**
- when committing our code using git or any other version control system, everyone having access to that repository will be able to read private info, such as the authorization token (should be kept secret)
- **most of the time, api tokens change depending on the stage at which we're running our app:** running our app locally, we may want to access apis using a test token and use a production one when deploying it. using an environment variable will make it easier for us to use different tokens depending on the environment. the same is valid for api endpoints
- if an api token changes for any reason, we can easily edit using a shared enviroment file for the whole app instead of changing the token value in every http request

*instead of manually writing sensitive data inside our files, we can create a new file called .env inside the project root folder and add all the info we need for our app to run
- this file contains sensitive and private info and should never be committed using any version control software
- add this file to the .gitignore file before deploying or committing your code

next.js has built-in support for .env and .env.local files, so we don't have to install external libraries to access those environment variables

next.js will automatically redirect us to the default 404 page if we enter the wrong route