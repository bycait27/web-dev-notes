# manage users in firebase
we need to be able to retrieve, manage, and update the user profiles in firebase

## get the currently signed-in user

### the onAuthStateChanged function
this function sets an observer on the firebase Auth object (the RECOMMENDED way to get current user)

ensures that the Auth object is no longer in an intermediate state when getting current user
- at any given time, the function waits until the user’s authentication state has been resolved before it triggers

```
import { getAuth, onAuthStateChanged } from "firebase/auth";

const auth = getAuth();

onAuthStateChanged(auth, (user) => {
  if (user) {
     // User is signed in
     console.log(user.email);
     console.log(user.uid);                   
     // ...
  } else {
    // User is signed out; returns null  
    // ...
  }
})
```

### the currentUser property
this exists on the Auth object

returns null whenever a user isn’t signed in

may also return null because the Auth object has not been completely initialized

```
import { getAuth } from "firebase/auth";

const auth = getAuth();
const user = auth.currentUser;

if (user) {
  // User is signed in
  // ...
} else {
  // No user is signed in.
}
```

## get a user's profile
contains properties we can use to retrieve users’ info

we can access this instance either from within the onAuthStateChanged observer or from the currentUser property

**ALL THE USER’S PROFILE INFO EXISTS WITHIN THE AUTH INSTANCE:**

```
import { getAuth, onAuthStateChanged } from "firebase/auth";

const auth = getAuth();

onAuthStateChanged(auth, (user) => {
  if (user) {
      console.log(user.email);
      console.log(user.uid);                   // Firebase identifier
      console.log(user.displayName);
      console.log(user.photoURL);
      console.log(user.emailVerified);
  } 
});
```