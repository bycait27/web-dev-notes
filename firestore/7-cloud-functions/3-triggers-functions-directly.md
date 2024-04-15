# trigger functions directly
**there are three ways we can call firebase cloud functions directly:**
- we can scedhule a cloud function to run at specific times or intervals
- trigger a function in response to an HTTP request
- calling a cloud function directly from the firebase app (calling it from the client)

## trigger via HTTP requests
we can use the onRequest handler that exists on the functions.https object
- takes two object parameters: request and response
    - the request object represents the properties of the request sent by the client
    - the response object provides a way to send a response back to the client

**we can terminate HTTP functions with either of the three following methods on the response object:** .send(), .redirect(), .end()

the firebase-functions module is required for all cloud functions to run

the firebase-admin module is required to interact with the firebase realtime database

we use the .ref() method to pass the /tasks path location in the database into it

then we can call the .get() method (returns a promise thar contains a snapshot of the queried location)

we then terminate the function after a successful query by sending a snapshot back to the client as a response

if the query fails, we terminate the function by sending an HTTP error response back to the client

***the .ref() and .get() methods work the same as the ref and get functions associated with the realtime database**

```
const functions = require("firebase-functions");
const admin = require("firebase-admin");

admin.initializeApp();

exports.getTasks = functions.https.onRequest((request, response) => {
  admin.database().ref("/tasks").get().then((snapshot) => {
    response.send(snapshot.val());
  }).catch((error) => {
    console.log(error.message);
    response.status(500).send(error.message);
  });
});
```

## invoke an HTTP function
after deploying an HTTP function, each function is assigned a unique URL similar to an HTTP endpoint

we can invoke a function through its unique URL
- we can view this on the terminal after a successful deployment or on the firebase console

***we can enter the URL on a browser to view the output**