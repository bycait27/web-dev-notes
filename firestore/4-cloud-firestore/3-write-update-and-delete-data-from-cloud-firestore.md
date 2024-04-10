# write, update, and delete data from cloud firestore
***we need to create a reference to the specific path in our database**

## write to the database
there are multiple ways to do this depending on the behavior we want for our app!

### use the setDoc function
- imported from the firebase/firestore subpackage
- RETURNS A JAVASCRIPT PROMISE
    - resolves when data is successfully written to database
    - async/await or .then
- writes to the document specified by the path to which its reference points
    - takes a reference to the database document as the first perameter
    - takes an object with key-value pairs as the second parameter
    - takes a third OPTIONAL parameter to merge new data with old data
        - the merge parameter allows the setDoc function to create a new document if one doesn’t exist and **ONLY REPLACES/UPDATES THE SPECIFIED FIELDS**
    - THIS IS THE DATA WE WANT TO WRITE
    
    WITHOUT THIRD PARAMETER

```
import { doc, getFirestore, setDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docsRef = doc(firestore, "users/test@educative.io");

// creates a new document at the specified location (docsRef)
// overwrites any existing document
	// a optional third parameter merges new data with old data 
export const documentWrite = () => {
  setDoc(docsRef, {
    task: "Learn Firebase",
    difficulty: "easy",
  })
    .then(() => {
      console.log('Document written')
    })
    .catch((err) => {
      console.log(err.message);
    });
};
```

WITH THIRD PARAMETER

```
import { doc, getFirestore, setDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docsRef = doc(firestore, "users/test@educative.io");

export const documentWrite = () => {
  setDoc(docsRef, {
    task: "Learn on Educative",
    duration: "30 minutes",
  },
	  // third parameter merges this data with the existing data
    { merge: true }
  )
    .then(() => {
      console.log('Document merged')
    })
    .catch((err) => {
      console.log(err.message);
    });
};
```

### use the updateDoc function
an alternative to the setDoc function imported from the firebase/firestore subpackage

we can use this to write to the database

works like the setDoc function with merge 

***if unsure whether a document exists, use setDoc function with merge set to true**

```
import { doc, getFirestore, updateDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docsRef = doc(firestore, "users/test@educative.io");

export const documentWrite = () => {
  updateDoc(docsRef, {
     task: "Learn on Educative",
     duration: "30 minutes",
  })
    .then(() => {
      console.log('Document updated')
    })
    // throws an error if the document path in reference doesn't exist
    .catch((err) => {
      console.log(err.message);
    });
};
```

### use the addDoc function
creates a new document with a randomly-generated ID in the specified reference path location
- allows us not to set the document ID ourselves
- takes a collection reference as first parameter
- takes an object with key-value pairs representing the data to be written as second parameter

imported from firebase/firestore subpackage

```
import { collection, getFirestore, addDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

// Firestore collection reference
const tasksRef = collection(firestore, "users/test@educative.io/tasks");

export const documentWrite = () => {
  addDoc(tasksRef, {
    task: "Learn K8s",
    difficulty: "hard",
  })
    .then(() => {
      console.log('Document added')
    })
    .catch((err) => {
      console.log(err.message);
    });
};
```

**key differences from setDoc:**
- can generate unique IDs for documents created (setDoc cannot do this)
- function is called with a collection reference (not a document reference)

## delete a document
must use the deleteDoc function imported from the firebase/firestore subpackage
- takes a document reference to be deleted as parameter
- RETURNS A JAVASCRIPT PROMISE
    - RESOLVES WHEN THE DOCUMENT IS SUCCESSFULLY DELETED FROM THE DATABASE

*deleting a document **only deletes the key-value pairs** associated with the document (NOT THE SUBCOLLECTIONS WITHIN IT)

```
import { doc, getFirestore, deleteDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docsRef = doc(firestore, "users/test@educative.io");

export const deleteDocument = () => {
  deleteDoc(docsRef)
    .then(console.log("document deleted"))
    .catch((err) => {
      console.log(err.message);
    });
};
```

## delete fields in a document
we can also delete specific fields in a document with the deleteField function
- we call the function as a value to the unwanted fields
- we can also use the updateDoc function to update the fields

```
import { doc, getFirestore, deleteField } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docsRef = doc(firestore, "users/test@educative.io");

export const deleteField = () => {
  setDoc(docsRef, {
	  // this will only delete the duration field from the document
    duration: deleteField(),                           
  },
    { merge: true }
  )
    .then(() => {
      console.log('document updated')
    })
    .catch((err) => {
      console.log(err.message);
    });
};
```