# delete and re-authenticate a user

## re-authenticate users
we want to re-authenticate users when they perform security-sensitive actions
- deleting an account, changing their primary emaill, changing their passwords
    - the user can’t perform these actions if they signed in a long time ago (reauthenticate by requesting new sing-in credentials from either the user or the sing-in provider)

we do this with the reauthenticateWithCredential function
- receives the new user credentials to handle such cases

```
import { getAuth, reauthenticateWithCredential, EmailAuthProvider } from "firebase/auth";

const auth = getAuth();
const user = auth.currentUser;

// TODO: prompt the user to re-provide their sign-in credentials
const authCredential =  EmailAuthProvider.credential(email, password);

reauthenticateWithCredential(user, authCredential).then(() => {
  // User re-authenticated.
  console.log(user)
}).catch((error) => {
  // An error occurred
  console.log(error)
  // ...
});
```

## delete a user
we call the deleteUser function 
- pass a reference to the currently signed-in user

```
import { getAuth, deleteUser } from "firebase/auth";

const auth = getAuth();
const user = auth.currentUser;

deleteUser(user).then(() => {
  // User deleted.
  alert("User deleted successfully");
})
  .catch((error) => {
   // An error occurred
   console.log(error);
   // ...
});
```