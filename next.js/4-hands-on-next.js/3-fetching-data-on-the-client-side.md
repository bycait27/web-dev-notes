# fetching data on the client side
client-side data fetching is a crucial part of any dynamic web app
- fetching data on the browser can add some extra complexities and vulnerabilities
    - making http requests from the browser can reveal private info, making it easy for malicious users to perform a plethora of possible attacks that exploit our data

* fetching data on the server side is generally more secure (when done with caution)

**here are some rules for making http requests on browsers:**
- make http requests to trusted sources ONLY. do some research about who is developing the apis we're using and their security standards
- call http apis only when secured with an ssl certificate. if a remote api isn't secured under https, we're exposing ourself and our users to many attacks, such as man-in-the-middle, where a malicious user could sniff all the data passing from the client and the server using a simple proxy
- never connect to a remote database from the browser. it may seem obvious, but it's technically possible for javascript to access remote databases. this exposes us and our users to high risk because anyone could potentially exploit a vulnerability and gain access to our database

## consuming rest apis on the client side
fetching data on the client side is relatively easy with previous experience with react and javascript frameworks/libraries

if we make a fetch request inside a given component, it will be executed on the client side by default

**we want our client-side requests to run in two cases:**
- right after the component has mounted
- after a particular event occurs

next.js doesn't force us to execute those requests differently than react, so we can basically make an http request using the browser's built-in fetch api or an external library (axios) just like we saw previously

**let's create a Users.js component inside our components/ folder:**

```
import { useEffect, useState } from 'react';
import Link from 'next/link';

function List({ users }) {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>
          <Link href={`/users/${user.username}`} passHref>
            <a> {user.username} </a>
          </Link>
        </li>
      ))}
    </ul>
  );
}

function Users() {
  const [loading, setLoading] = useState(true);
  const [data, setData] = useState(null);

  useEffect(async () => {
    const req = await fetch('https://ed-6555414034513920.educative.run:3000/api/users');
    const users = await req.json();
    setLoading(false);
    setData(users);
  }, []);

  return (
    <div>
      {loading && <div>Loading users...</div>}
      {data && <List users={data} />}
    </div>
  );
}

export default Users;
```

- the html generated on the server side contains the Loading users... text because it's the initial state of our HomePage component
- we'll be able to see a list of users only after react hydration occurs. we'll need to wait for the component to mount on the client side and the http request to be spawned using the browser's fetch api

**let's implement a single user page:**
1. create a new file pages/users/[username].js with a getServerSideProps function, where we fetch the [username] variable from the route and the authorization token from the .env file

```
import { useEffect, useState } from 'react';
import Link from 'next/link';

export async function getServerSideProps({ query }) {
  const { username } = query;
  return {
    props: {
      username,
      token: `${process.env.NEXT_PUBLIC_API_TOKEN}`
    },
  };
}
```

2. now we create a UserPage component, where we'll execute the client-side data fetching function

```
function UserPage({ username, authorization }) {
  const [loading, setLoading] = useState(true);
  const [data, setData] = useState(null);

  useEffect(async () => {
    const req = await fetch(`https://ed-6555414034513920.educative.run/api/singleUser/?username=${username}`,
      { headers: { token: authorization } }
    );

    const reqData = await req.json();
    setLoading(false);
    setData(reqData);
  }, []);

  return (
    <div>
      <div>
        <Link href="/" passHref>
          Back to home
        </Link>
      </div>
      <hr />
      {loading && <div>Loading user data...</div>}
      {data && <UserData user={data} />}
    </div>
  );
}

export default UserPage;
```

3. we create the last component

```
function UserData({ user }) {
  return (
    <div style={{ display: "flex" }}>
      <img src="/Angryboss.png" alt={user.username} width={150} height={150} />
      <div>
        <div>
          <b>Username:</b> {user.username}
        </div>
        <div>
          <b>Full name:</b> {user.fullName}
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
  );
}
```

### vulnerabilities in client-side requests
**the preceding code will run with at least two problems:**
- the first issue is related to cors
    - **cross-origin resource sharing (cors):** a security mechanism implemented by browsers that aims to control the requests made from domains different from those of the api domain 
    - in some cases, the browser might make some restrictions on the /users/[username] endpoint, and we won't be able to call this api directly from the client because we might get blocked by cors policy
- the second issue relates to exposing the authorization token to the client
    - if we open the google chrome developer tools and go to "network," we can select the http request for the endpoint and see the authorization token in plain text in the "request headers" section
        - a malicious user could take this authorization token and use it for their own app (their http requests will count towards ours...making us pay more for their services)
    - to solve this issue using the next.js api pages, which allow us to quickly create a rest api, making the http request for use on the server side and returning the result to the client
        - **create a new folder inside pages/ called api/ and a new file called singleUser.js inside that folder:**

        ```
        import axios from 'axios';

        export default async function handler(req, res) {
            const username = req.query.username;
            const API_ENDPOINT = `https://ed-6555414034513920.educative.run`
            const API_TOKEN = process.env.NEXT_PUBLIC_API_TOKEN;

            const userReq = await axios.get(`${API_ENDPOINT}:3000/users/${username}`, {
                headers: { authorization: API_TOKEN },
            });

            res.status(200).json(userReq.data);
        }
        ```

        **code explanation:**
        - **req:** an instance of node.js’s http.IncomingMessage merged with some prebuilt middleware, such as req.cookies, req.query, and req.body.
        - **res:** an instance of node.js’s http.serverResponse, merged with some prebuilt middleware such as:
            - **res.status** for setting the http status code
            - **res.json** for returning a valid json
            - **res.send** for sending an http response containing a string, an object, or a buffer
            - **res.redirect([status,] path)** for redirecting to a specific page with a given (and optional) status code
        
        - every file inside the pages/api/ directory will be considered by next.js as an api route
        - **now let's refactor the UserPage component by changing the api endpoint to the newly created one:**

        ```
        import axios from 'axios';

        export default async function handler(req, res) {
            if (!req.query.username) {
                res.status(403).json({
                    error: 'Missing "username" query parameter',
                });
                return;
            }

            const username = req.query.username;
            const API_ENDPOINT = `https://ed-6555414034513920.educative.run`
            const API_TOKEN = process.env.NEXT_PUBLIC_API_TOKEN;

            const userReq = await axios.get(`${API_ENDPOINT}:3000/api/users/${username}`, {
                headers: { token: API_TOKEN },
            });

            res.status(200).json(userReq.data);
        }
        ```

*now, our issues are solved, but keep in mind that a malicious user could still be able to use the /api/singleUser route to access the private data with ease

**to solve this specific problem we can do the following:**
- render the component list exclusively on the server. that way, a malicious user won't call a private api or steal a secret api token. however, there are cases where we can't run those kinds of api calls on the server only; if we need to make a rest request after the user clicks a button, we're forced to make it on the client side
- use an authentication method to let authenticated users only access a specific api (jwt, api key, etc.)
- use a back-end framework, such as ruby on rails, spring, next.js, and strapi. they all provide different ways of securing our api calls from the client, making it way more comfortable for us to secure next.js apps
