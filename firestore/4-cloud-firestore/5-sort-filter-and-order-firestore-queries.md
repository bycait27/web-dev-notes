# sort, filter, and order firestore queries
cloud firestore allows us to create a database query to specify the documents we want to retrieve in a collection

we can use the query function
- takes a collection reference as first parameter
- takes a comma-separated list of query constraints to be applied as second parameter

## use the where function
creates simple cloud firestore queries

**- takes three parameters:**

    - the path to compare
    - the operation string or a comparison operator
    - the value for comparison
- parameters enforce that the documents returned in the query snapshot contain the compared path as a field
    - the value must satisfy the operation string provided

```
import { getFirestore, collection, query, where, getDocs } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

const queryRef = query(colRef, where("difficulty", "==", "hard"));

export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(queryRef);

	// iterate using forEach method
  colSnapshot.forEach((doc) => {
    console.log(doc.data());
  });
};
```

## order cloud firestore queries

```
import { getFirestore, collection, query, orderBy, getDocs } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

const queryRef = query(colRef,  orderBy("task", "desc"));

export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(queryRef);

  colSnapshot.forEach((doc) => {
    console.log(doc.data());
  });
};
```

## limit firestore queries

## create firestore indexes