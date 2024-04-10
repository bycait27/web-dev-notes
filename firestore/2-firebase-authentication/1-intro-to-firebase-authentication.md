# intro to firebase authentication
uses JSON Web Tokens (JWTs) to authenticate users

seamlessly integrates with other firebase services

supports INDUSTRY STANDARDS (0Auth 2.0, OpenID Connect)
- allows effortless integration with an existing auth system or a custom backend

## how it works
**two steps:**

1. identification

    a. by using an email address
2. verification

    a. by providing a password or a 0Auth token from a federated identity provider

these credentials are securely sent to the server via a sign-in method provided by the firebase authentication SDK
- the credentials are validated and a response is sent back to the browser

after a successful sign-in, firebase creates an Auth instance for the usr and provides a unique auth token to identify each user
- with every user request, an auth token is sent with it
    - these tokens verify access according to use permissions
- the auth instance persists the current user’s state
    - prevents the user from logging out if they refresh the browser or page

## implementation
we need to import and initialize the firebase auth service
- call the getAuth function from the firebase/auth subpackage
- initialize the firebase auth service by calling getAuth and passing the firebase app instance as a parameter

```
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';


const firebaseConfig = {
  apiKey: "AIzaSyDNv52XXXXXXXXXXXXXXXXXXXXXX-krGF8E",
  authDomain: "educative-xxxxxxxxxxx-cfceb.firebaseapp.com",
  projectId: "educative-xxxxxxxxxxx-cfceb",
  storageBucket: "educative-xxxxxxxxxxx-cfceb.appspot.com",
  messagingSenderId: "01234567890",
  appId: "1:01234567890:web:8e17ea99e18ac710e7934b",
  measurementId: "G4-HBF7IU9NPX",
};

const firebaseApp = initializeApp(firebaseConfig);

export const auth = getAuth(firebaseApp);
```

## create users
there are numerous methods to create and sign-in users

we have to enable the sign-in method in the firebase console

for some federated providers, we need to provide some extra info (Client ID, Client secret)
- we get this from the provider’s dev console