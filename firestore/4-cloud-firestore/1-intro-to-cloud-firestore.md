# intro to cloud firestore

## how it works
data is stored as documents

**document -** a unit of storage made of a set of key-value pairs
- supports various data types and subcollections

documents are stored in a collection

**collection -** a container for documents that organizes data and helps build efficent queries
- can also create subcollections within documents

**shallow querying** allows us to retrieve individual, specific documents, or all documents in a collection or subcollection
- can add, sort, filter, and limit our queries or cursors to paginate our results
- we can add real-time listeners to update the app by retrieving data wherever changes occur in the database

persists data to the local cache (optimizes it for offline usage)
- when a usr goes offline, their app is still able to read, write, and listen to data
- the data is synchronized when the user comes back online

## implementation
1. import the cloud firestore’s getter function from its subpackage
2. initialize it

    a. importing the getFirestore function from the firebase/firestore subpackage

        i. this function takes a reference to the firebase app instance as a parameter

```
import { initializeApp } from 'firebase/app';
import { getFirestore } from "firebase/firestore";


const firebaseConfig = {
  apiKey: "AIzaSyDNv52XXXXXXXXXXXXXXXXXXXXXX-krGF8E",
  authDomain: "educative-xxxxxxxxxxx-cfceb.firebaseapp.com",
  projectId: "educative-xxxxxxxxxxx-cfceb",
  storageBucket: "educative-xxxxxxxxxxx-cfceb.appspot.com",
  messagingSenderId: "01234567890",
  appId: "1:01234567890:web:8e17ea99e18ac710e7934b",
  measurementId: "G4-HBF7IU9NPX",
};

const firebaseApp = initializeApp(firebaseConfig);

// Get a reference to the firestore service
export const firestore = getFirestore(firebaseApp);
```

## enable cloud firestore
## 

1. head to the firebase project dashboard 
2. navigate to the “firestore database” section

    a. click the “create the database” button to get started
3. define the security rules for our database…

    a. for now, choose the “start in test mode” option to permit all reads and writes for the next 30 days

    b. click “next”
4. set a location for our cloud firestore database…

    a. default location (”nam5(us-central)”) suffices
5. click “enable”

    a. wait for firebase to provide database resources and set up our security rules

## temporary security rules
after the 30 days end, we will need to set new security rules for the cloud firestore database to enable us to read and write data to it

1. go to the “cloud firestore” section of your firebase console
2. head over to the “rules” tab to show the existing firestore database rules
3. copy the below lines of code 

    a. paste them onto the code editor provided

    b. click the “publish” button

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write;
    }
  }
}
```