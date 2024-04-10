# track and handle authentication
we need to set up a real-time listener to track users’ authentication status
- useful when we want to perform certain actions based on whether the user is signed in or not

## authentication state observers
we can attach an observer that is called whenever the user’s sing-in state changes

the onAuthStateChanged fuction is attached to the Auth object to listen for any changes in the authentication status
- imported from the firebase/auth subpackage
- takes the initialized auth instance as its first parameter
- takes a callback function that represents the user instance as its second parameter
- monitors users’ authentication state in real time and provides info about the current user

```
import { getAuth, onAuthStateChanged } from "firebase/auth";

const auth = getAuth();

onAuthStateChanged(auth, (user) => {
  if (user) {
    // User is signed in
    console.log(user.email);
    // ...
  } else {
    // User is signed out
    // ...
  }
});
```