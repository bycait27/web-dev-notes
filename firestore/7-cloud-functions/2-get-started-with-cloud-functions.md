# get started with cloud functions
**most important files and folders in our editor created by initialization:**

**firebase.json:** this file is created in the root of the project directory. it defines the functions/hosting configurations of the project

**functions/:** this is the new directory created in the root of the project containing all cloud functions code

**package.json:** this is an npm package file used by node to describe the project. it contains dependencies required to run cloud functions. all other modules and packages to be used in our functions will also go here

**index.js:** this is the main source file where we write/define all cloud functions. it comes with a sample helloWorld function

**node_modules/:** this is the folder contained in the functions directory created when we install the dependencies from the initialization command

## the firebase admin SDK
**admin SDK:** a set of libraries available on firebase that allows devs to interact with firebase and firebase services from privileged or trusted environments
- devs can perform user management operations
    - i.e. read and write data to a database or storage with full admin privileges

**we need the firebase-admin module in our node.js environment in order to use the admin SDK:**

```
const admin = require("firebase-admin");

admin.initializeApp();

// use Firestore with Admin SDK
const firestore = admin.firestore()

// use Realtime database with Admin SDK
const database = admin.database()
```

## write our first cloud function
**we can use the helloWorld sample function created in the index.js file to write our first cloud function:**
- "firebase-functions" module is one of the two dependencies created when initializing our project (REQUIRED to build ALL cloud functions)
- helloWorld is an HTTP-type function
    - this is a function that handles HTTP events
    - the onRequest handler listens for events
        - **there are two parameters:** request and response
            - the request parameter gives access to properties of the request sent by the client
            - the response parameter provides a way to send a response back to the client (like express)
- the function is terminated with a "Hello from firebase" response message to the client

***it is important to terminate all cloud functions correctly to prevent charges that may occur from unnecessarily prolonged functions**

```
const functions = require("firebase-functions");

// Create and Deploy Your First Cloud Functions
// https://firebase.google.com/docs/functions/write-firebase-functions

exports.helloWorld = functions.https.onRequest((request, response) => {
  console.log("Function started");
  response.send("Hello from Firebase!");
});
```

## deploy our first cloud function
we need to use the deploy CLI command

**the following command only deploys the functions contained in the index.js file:**

```
firebase deploy --only functions
```

**we can also do this to deploy specific functions:**
- the --only flag is followed by a comma separated list of the functions to be deployed

*this is especially useful if we want to make changes to specific functions in an index.js file that contains multiple functions

```
firebase deploy --only functions:helloWorld,functions:sendEmailInvite
```

## delete a cloud function
**there are multiple ways to delete a cloud function:**
- we can use the functions:delete command on the firebase CLI
- we can use the function's context menu on the "cloud functions" section of the firebase project dashboard
- we can remove the function from the index.js file before deployment
    - this way, the CLI only parses the functions that exist in the index.js file during deployment and deletes previously contained functions from production

**the following command deletes a single function:**

```
firebase functions:delete functionName
```

**follow the following steps to delete a function from the project dashboard:**

1. go to the "functions" section of the firebase project dashboard
2. hover over the function we intend to delete (this reveals the "menu" icon at the far-right corner of the function)
3. click the "menu icon" and then click the "delete the function" option
4. click the "delete" button on the pop-up menu to confirm the deletion