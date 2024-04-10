# federated identity authentication providers
with email/password authentication, malicious actors can try to get a hold of users’ passwords with phishing

to mitigate this risk, we can use federated identity providers (social media sign-in)
- use OpenID Connect based on 0Auth 2.0 to authenticate users
- includes Google, Apple, Microsoft, Twitter, and GitHub
- sends user to identity provider’s login page and is then redirected back to the app with their provider token

## authenticate using Google sign-in
1. go to “authentication” section of firebase console
2. click “add new provider”
3. select “Google”
4. include a project support email
5. click “save”
    1. once saved, firebase automatically provides our web SDK config details and a Google sign-in is then available for use on our firebase project

we need to add domains that can access this 0Auth feature:

1. go to “authentication” section of firebase console
2. click “add domain” under the “authorized domains” section
3. retrieve the unique domain
4. copy and add the link to the list of the authorized domains on the firebase console

**create an instance of Google provider object:**

```
import { GoogleAuthProvider } from "firebase/auth";

const provider = new GoogleAuthProvider();
```

**prompt users to sign-in to their Google accounts either by opening a pop-up menu or a redirecting window to a sign-in page:**

```
import { getAuth, signInWithPopup, signInWithRedirect, GoogleAuthProvider } from "firebase/auth";

const auth = getAuth();
const provider = new GoogleAuthProvider();

// sign in with popup
signInWithPopup(auth, provider)
  .then((result) => {
    // ...
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // ...
  });

// sign in with redirect
signInWithRedirect(auth, provider)
  .then((result) => {
    // ...
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // ...
  });
```

## sign in with GitHub
steps are similar to integrating Google sign-in
- only difference: we need to register our app on the GitHub dev console
1. go to “authentication” section of firebase console
2. click “add new provider”
3. select “GitHub”
4. we need to add the client ID and Client Secret (go to GitHub’s New Auth app page to obtain these)
5. fill in required details to register the app 
6. set the “Homepage URL” to the local host
7. copy the “Authorization callback URL” from the GitHub Auth pop-up on the firebase console 
8. paste the URL on the GitHub registration page
9. we are redirected to the “Application settings” page
10. click “Generate a new client secret” to create a Client secret
11. go to the firebase console 
12. paste the Client secret where required

to sign up via GitHub, we need to create an instance of the GitHub provider object

users can sign in by opening a pop-up menu or a redirecting window to a sing-in page

```
import { getAuth, signInWithPopup, signInWithRedirect, GithubAuthProvider } from "firebase/auth";

const auth = getAuth();
const provider = new GithubAuthProvider();

// sign in with popup
signInWithPopup(auth, provider)
  .then((result) => {
    // ...
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // ...
  });

// sign in with redirect
signInWithRedirect(auth, provider)
  .then((result) => {
    // ...
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // ...
  });
```