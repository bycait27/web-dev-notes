# read data from cloud firestore
there are multiple ways to read data from the cloud firestore database depending on the behavior we want for our app!

## use the getDoc function
retrieves the contents of a single document 
- takes a reference to the document to be fetched as parameter
- RETURNS A JAVASCRIPT PROMISE
    - RESOLVES WITH A SNAPSHOT OF THE DOCUMENT’S CURRENT CONTENT

```
import { doc, getFirestore, getDoc } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docRef = doc(firestore, "users/test@educative.io");

export const retrieveDoc = async () => {
  const docSnapshot = await getDoc(docRef);
	
	// we can use the exists method to check if there is a document
	// at the location specified in docRef variable
	// if no document exists, it returns false
	// if it does exist, the data method obtains the data in the document
  if (docSnapshot.exists()) {
    const docData = docSnapshot.data();
    console.log(docData);
  }
};
```

## use the getDocs function
retrieves all documents within a collection
- takes a reference to the collection location as parameter
- RETURNS A JAVASCRIPT PROMISE
    - RESOLVES WITH A SNAPSHOT OF THE COLLECTION

```
import { getFirestore, getDocs, collection } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(colRef);

	// call the forEach method to iterate over the snapshot received
	// this is executed on each document in the snapshot 
  colSnapshot.forEach((doc) => {
	  // we can slo use the data method to obtain the document's content
    console.log(doc.data());
  });
};
```

## listen for document changes in real-time
the onSnapshot function monitors real-time changes to our database
- triggers on the initial data read and whenever any document value changes
- takes a reference to the document to listen as first parameter
- takes a callback that triggers whenever a new snapshot is available as second parameter

```
import { doc, getFirestore, onSnapshot } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const docRef = doc(firestore, "users/test@educative.io");

export const docListener = () => {
  onSnapshot(docRef, (docSnapshot) => {
    if (docSnapshot.exists()) {
      console.log(docSnapshot.data());
    }
  });
};
```

***we can sloe detach a listener from our app so that we no longer subscribe to changes on the document**
- returns an unsubscribe function we can call to detach the listener