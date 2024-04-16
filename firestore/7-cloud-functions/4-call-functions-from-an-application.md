# call functions from an application
**HTTPS callable functions:** the cloud functions SDK allows us to call functions directly from our app

**steps to trigger these functions:**
1. write and deploy the function
2. add client logic that allows us to trigger the functions from our app

**similar to typical HTTP functions**
- **major difference:** callable functions require us to use the cloud functions for firebase client SDK alongside the functions.https backend API (can't be triggered directly from the browser)

## write an HTTPS callable function
we need to use the onCall handler that exists on the functions.https object
- **takes a callback function with two parameters: data and context**
    - the data object represents any data we send to this cloud function from the frontend when it triggers
    - the context object is an OPTIONAL parameter that holds the current user's authentication info

to terminate an HTTP callable function, we must return a response to the client using the javascript return statement

```
const functions = require("firebase-functions");

// http callable function
exports.helloFromEducative = functions.https.onCall((data, context) => {
  const name = context.auth.token.name || "user";
  // const picture = context.auth.token.picture || null;
  // const email = context.auth.token.email || null;

  return `Hello ${name} from Educative`;
});
```

## initialize cloud functions
we need to initialize the cloud functions instance on the client SDK in order to call functions directly from our app

```
import { initializeApp } from 'firebase/app';
import { getFunctions } from "firebase/functions";


const firebaseConfig = {
  apiKey: "AIzaSyDNv52XXXXXXXXXXXXXXXXXXXXXX-krGF8E",
  authDomain: "educative-xxxxxxxxxxx-cfceb.firebaseapp.com",
  projectId: "educative-xxxxxxxxxxx-cfceb",
  messagingSenderId: "01234567890",
  appId: "1:01234567890:web:8e17ea99e18ac710e7934b",
};

const firebaseApp = initializeApp(firebaseConfig);

// Get a reference to the Cloud storage service
export const functions = getFunctions(firebaseApp);
```

## invoke a callable function
before we invoke a callable function, we need to get a reference to the cloud function written on the backend
- we use the httpsCallable function imported from the firebase/functions subpackage
    - takes a reference to the initialized cloud functions instance as its first parameter
    - takes the name of the deployed cloud function as its second parameter

the function returns a reference to the callable HTTPS function with the given name
- we can invoke the callable function once we save this returned reference in a javascript constant

the result parameter on the .then() method holds the data returned from the callable function

we access the returned data using the data property on the result object

```
import { getFunctions, httpsCallable } from "firebase/functions";
import { firebaseApp } from "../services/firebase";

// initialise functions
const functions = getFunctions(firebaseApp);


const helloFromEducative = httpsCallable(functions, "helloFromEducative");

helloFromEducative().then((result) => {
 alert(result.data);
});
```

we can also use a javascript object to pass along additional info while invoking the callable function

```
import { getFunctions, httpsCallable } from "firebase/functions";
import { firebaseApp } from "../services/firebase";

// initialise functions
const functions = getFunctions(firebaseApp);

const sanitizeMessage = httpsCallable(functions, "sanitizeMessage");

sanitizeMessage({ text: messageText, somethingFun: somethingFun }).then((result) => {
 // handle operation
});
```

the object inside the sanitizeMessage function call represents the data that we send to the cloud function whenever it is invoked

we access this object using the data parameter on the onCall method

the text and somethingFun properties are accessed from the data object on the cloud function

```
const functions = require("firebase-functions");

// http callable function
exports.sanitizeMessage = functions.https.onCall((data, context) => {
  const text = data.text;
  const somethingFun = data.somethingFun;
  
  // Cloud Function logic
  return `Sanitized ${text}`;
});
```