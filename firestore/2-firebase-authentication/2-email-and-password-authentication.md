# email and password authentication 
the most popular form of authentication used today

the firebase authentication SDK offers auth methods to create, sign in, and manage users that use their email addresses and passwords to sing into our app

## enable email/password authentication
1. click dropdown menu on project dashboard to show all products
2. click “build”
3. click “authentication” and “get started”
4. click “email/password” option under “native providers” section of “sing-in method” page
5. click togle button to enable email/password authentication
6. click “save”

## create users
**two ways:**

- manually create password-authenticated users by clicking the “add user” button on the “authentication” section of our project dashboard
- use the firebase authentication SDK to write functions that automatically create usrs on our app

## sign up users with the Auth SDK
we need to pass the user’s credentials to the createUserWithEmailAndPassword (async) function from the firebase/auth subpackage
- this creates a new user account in our firebase project and sets th user’s initial password

**three arguments:**

- the auth object (represents the initialized auth instance)
- email value (obtained from client-side input field)
- password value (obtained from client-side input field)

need a promise (async function)

```
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth(firebaseApp);

createUserWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    // Signed up 
    const user = userCredential.user;
    // ...
  })
  .catch((err) => {
    console.log(err.code);
    console.log(err.message);
  });
```

## sign in users
we need to pass the user’s credentials to the signInWithEmailAndPassword function

```
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const auth = getAuth(firebaseApp);

signInWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    // Signed in 
    const user = userCredential.user;
    // ...
  })
  .catch((err) => {
    console.log(err.code);
    console.log(err.message);
  });
```

## handling errors
the list of errors exists on the AuthErrorCodes object from the firebase/auth subpackage
- allows us to display better, user-readable error messages on our app

```
import { AuthErrorCodes, createUserWithEmailAndPassword, getAuth } from "firebase/auth";

const auth = getAuth();

createUserWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
      // Signed up
     // ...
  })
  .catch((err) => {
    if (err.code === AuthErrorCodes.WEAK_PASSWORD) {
      alert("The password is too weak");
     }
});
```

[possible error codes:] (https://firebase.google.com/docs/reference/js/auth#autherrorcodes)