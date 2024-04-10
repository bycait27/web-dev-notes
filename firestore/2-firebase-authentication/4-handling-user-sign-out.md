# handling user sign-out
once the user is authenticated, they are automatically signed in to the app

the firebase authentication SDK provides functions for handling user sign-out

we need to call the signOut function from the firebase/auth subpackage
- takes the initialized authentication instance as a parameter
- asynchronous (returns a javascript promise that we can use to handle the function’s success value or failure reason

```
import { getAuth, signOut } from "firebase/auth";

const auth = getAuth();

signOut(auth).then(() => {
  // Sign-out successful.
}).catch((error) => {
  // An error happened.
});
```