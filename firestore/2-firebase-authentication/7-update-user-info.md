# update user info
we can use the firebase authentication SDK 

## update a user's profile
we can use the updateProfile function from the firebase/auth subpackage
- takes a reference to the current user as the first argument
- takes an object containing the updated display name and photo URL as the second argument

```
import { getAuth, updateProfile } from "firebase/auth";

const auth = getAuth();

updateProfile(auth.currentUser, {
  displayName: "Educative Test",
  photoURL: "https://educative.io/test-user/profile.jpg",
 })
 .then(() => {
    // Profile updated!
   // ...
 })
 .catch((error) => {
  // An error occurred
   // ...
});
```

## set a user's email address
we can use the updateEmail function from the firebase/auth subpackage (works similar to updateProfile)

**difference:** the user must have signed in recently for this to work OR we need to reauthenticate the user\

```
import { getAuth, updateEmail } from "firebase/auth";

const auth = getAuth();

updateEmail(auth.currentUser, "user@educative.io").then(() => {
  // Email updated!
  // ...
}).catch((error) => {
  // An error occurred
  // ...
});
```

## send a verification email
we can use the sendEmailVerification function from the firebase/auth subpackage to send emails and verify users

we can also customize the email verification template in the “authentication” section of the firebase console

```
import { getAuth, sendEmailVerification } from "firebase/auth";

const auth = getAuth();

sendEmailVerification(auth.currentUser)
  .then(() => {
   // Email verification sent!
   alert("Email sent successfully");
   // ...
  })
  .catch((err) => {
    console.log(err);
});
```

## set a user's password
we can use the updatePassword function from the firebase/auth subpackage
- takes a reference to the current user as its first argument
- takes the new password as its second argument

THE USER MUST HAVE SIGNED IN RECENTLY FOR THIS TO WORK

```
import { getAuth, updatePassword } from "firebase/auth";

const auth = getAuth();

updatePassword(auth.currentUser, "testPassword!23*")
  .then(() => {
    // Update successful.
    alert("Password changed");
  })
  .catch((error) => {
    // An error ocurred
    console.log(error);
    // ...
});
```

## password reset email
we can use the sendPasswordResetEmail function from the firebase/auth subpackage
- sends emails to users who need to reset their passwords
- we can customize the “password reset” email template in the “authentication” section of the firebase console

```
import { getAuth, sendPasswordResetEmail } from "firebase/auth";

const auth = getAuth();

sendPasswordResetEmail(auth, "user@educative.io")
 .then(() => {
   // Password reset email sent!
   alert("Email sent");
   // ..
 })
 .catch((error) => {
    console.log(error);
    const errorCode = error.code;
   const errorMessage = error.message;       
   // .. 
 })
```