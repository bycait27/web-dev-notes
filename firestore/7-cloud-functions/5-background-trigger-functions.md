# background trigger functions
**examples of background triggers:**
- when a user signs up with firebase auth
- a write, update, or delete operation on the realtime database or cloud firestore
- a conversion event in Google analytics

we can monitor these events and trigger a function in response to such events

**all background trigger functions must be terminated by returning a promise that is fulfilled after the function completes all the work to be done (if there is no work, return null to terminate the function)**

## authentication triggers
we can trigger cloud functions for firebase in response to a user creation or a deletion event on our firebase app
- we can use onCreate or onDelete with the functions.auth.user() function to do this
    - takes a callback function with a user parameter (this allows us to access the user's attributes)

the userSignup function will only respond to user creation events

```
const functions = require("firebase-functions");

// onCreate function
exports.userSignup = functions.auth.user().onCreate((user) => {
  const name = user.displayName
  const email = user.email

  console.log("user created", user.displayName, user.uid);
  // send welcome email
});

// onDelete function
exports.userDelete = functions.auth.user().onDelete((user) => {
  // send goodbye email
});
```

## realtime database triggers
we can trigger cloud functions for firebase in response to events in the realtime database

cloud functions wait for a change in a particular location on the database, as specified by the reference
- trigger when the specified event type occurs
- receives a data object that contains a snapshot of the data after (an sometimes before the trigger event)

**to create these functions, follow these steps:**
1. we use functions.database
2. specify a path to the database location using the ref function
3. attach an event handler that triggers in response to specific event types
    
    a. **onCreate:** this triggers when new data is created on the realtime database
    
    b. **onDelete:** this triggers when data is deleted from the realtime database

    c. **onUpdate:** this triggers when a change is made to existing data in the realtime database

    d. **onWrite:** this responds to create, update, and delete operations to the location in the realtime database

the snapshot parameter contains a snapshot of the data stored in the location

the context parameter contains additional info about the user and the triggered event

```
const functions = require("firebase-functions");

// onCreate handler
exports.databaseTriggerSample = functions.database
  .ref("/users/educative/tasks")
  .onCreate((snapshot, context) => {
    // do something
    const uid = context.auth.uid;

    // return something
    return snapshot.val();
  });
```

we can also use wildcards {} when defining the path reference
- allows us to match any child of the parent path in the location
- we can access the values of the wildcard components under the context object

the before and after properties can be used to read data before and after the triggered event occurs

we access the value of the wildcard path from the context object and assign it to a wildcard constant. -** lines 10, 11, 15, and 16:** we demonstrate how to check for the existence of the old and new data snapshots and access them if needed

**only the onUpdate and onWrite event handlers have access to the change object that contains a snapshot of the path before and after the trigger event**

**the snapshot object returned for the onCreate and onDelete event types is a snapshot of the data created or deleted**

```
const functions = require("firebase-functions");

// onUpdate handler
exports.databaseTriggerSample = functions.database
  .ref("/users/{userId}/tasks")
  .onUpdate((change, context) => {
    const wildcard = context.params.userId;

    // do something
    if (change.before.exists()) {
      const old = change.before.val()
      // do something
    }

    if (change.after.exists()) {
      const new = change.after.val()
      // do something
    }

    // return something
  });
```

## cloud firestore triggers
we can also trigger cloud functions for firebase in response to events in cloud firestore (similar but different to realtime database triggers)
- they are accessed with functions.firestore 
- paths are referenced using the document function (wait for changes on a particular document path and trigger in response to those event types)

**cloud firestore has 4 triggers:**
- onCreate
- onDelete
- onUpdate
- onWrite

    **these work the same as realtime database triggers**

**our trigger must always point to a document, even if we're using a wildcard**

```
const functions = require("firebase-functions");

// onUpdate handler
exports.firestoreTriggerSample = functions.firestore
    .document('users/{userId}')
    .onUpdate((change, context) => {
      const newValue = change.after.data();
      const previousValue = change.before.data();

      const wildcard =  context.params.userId;


      // perform desired operations ...

      // return something
      return change.after.ref.set({
       //  update field 
      }, {merge: true})
    });
```

we only need the admin SDK when we write to the same location that triggers the event
- making changes to other locations outside the trigger event or to other firebase services will require the admin SDK

```
const admin = require('firebase-admin');

admin.initializeApp();

exports.writeToFirestore = functions.firestore
  .document('some/doc')
  .onWrite((change, context) => {
    admin.firestore().doc('some/otherdoc').set({ ... });
  });
```