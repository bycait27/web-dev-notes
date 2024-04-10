# persistence in firebase
when a user logs into a firebase app, their authentication state is persisted by default UNTIL the user signs out of the app
- eliminates the need for the user to sign back in when the browser reloads OR when they re-visit the app

if the app holds sensitive info or the user uses the app on a publicly shared device and forgets to sign out, this may not be ideal
- with the firebase authentication SDK, we can specify how users’ authentication states persist and what conditions should trigger another sign-in from the users
    - reloading the page, closing the browser window, etc.

## supported persistence types
**three types:**

**local:**

- indicates that users’ authentication states persist even when the browser window is closed
    - explicit sign-out is required
    - done with the browserLocalPersistence function from the firebase/auth subpackage

**session:**

- the authentication state will only persist in the current tab or the browser session
    - cleared once tab is closed
    - ONLY APPLIES TO WEB APPS
    - done with the browserSessionPersistence function from the firebase/auth subpackage

**none:**

- the authentication state clears whenever the window or activity refreshes
    - done with the inMemoryPersistence function from the firebase/auth subpackage

## modify persistence in firebase
we use the setPersistence function to modify authentication state persistence
- **three arguments:** the auth instance, the type persistence to use, a promise that resolves once the persistence change completes

```
import { getAuth, setPersistence, signInWithEmailAndPassword, browserSessionPersistence } from "firebase/auth";

const auth = getAuth();

setPersistence(auth, browserSessionPersistence)
  .then(() => {
    return signInWithEmailAndPassword(auth, email, password);
  })
  .catch((error) => {
   // Handle Errors here.
   const errorCode = error.code;
   const errorMessage = error.message;
});
```