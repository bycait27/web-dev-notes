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
by default, the cloud firestore retrieves all documents that satisfy a query's constraints in ascending order of the document's IDs

to sort the data, we use the orderBy function
- takes the document field to sort as first parameter
- OPTIONALLY takes the sorting direction (asc or desc) as second parameter
    - default sorting is ascending order

```
import { getFirestore, collection, query, orderBy, getDocs } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

// ordered alphabetically by the values of their task fields in descending order
const queryRef = query(colRef,  orderBy("task", "desc"));

// retrieves all documents in the references collection
export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(queryRef);

  colSnapshot.forEach((doc) => {
    console.log(doc.data());
  });
};
```

## limit firestore queries
by default, cloud firestore returns all documents in a collection that satisfy the query constraints

we can use the limit function when working with large data
- sets a limit to the number of documents returned from a single query
- takes an integer that sets the max number of documents returned from the query as a parameter

```
import { getFirestore, collection, query, limit, getDocs } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

// only retrieves 3 documents in the referenced collection
const queryRef = query(colRef,  limit(3));

export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(queryRef);

  colSnapshot.forEach((doc) => {
    console.log(doc.data());
  });
};
```

**we can also retrieve 5 documents in the referenced collection with a difficulty value if hard:**
- these will be **ordered in descending alphabetical order by the values of their task fields 

```
import { getFirestore, collection, query, limit, getDocs } from "firebase/firestore";
import { firebaseApp } from "../services/firebase";

const firestore = getFirestore(firebaseApp);

const colRef = collection(firestore, "users/test@educative.io/tasks");

const queryRef = query(
  colRef,
  where("difficulty", "==", "hard"),
  orderBy("task", "asc"),
  limit(5)
);

export const retrieveDocs = async () => {
  const colSnapshot = await getDocs(queryRef);

  colSnapshot.forEach((doc) => {
    console.log(doc.data());
  });
};
```

## create firestore indexes
firebase automatically provides an appropriate index to use on our app queries

1. open the browser console on which our app is opened
2. check for an error message from firebase like "the query requires an index. you can create it here: "hyperlink""
3. click the hyperlink provided in the error message. this link will take us to the "cloud firestore" section of our firebase project console. we'll need to also sign in to our Google account
4. click the "create the index" button. it takes a few minutes for firestore to build out the index created. after which you can make those queries successfully withot the error message from before

***only queries that combine both where and order functions require indexes!**