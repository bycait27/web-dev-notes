# intro to the firebase realtime database
a scalable, real-time NoSQL cloud database provided by firebase for storing app data
- data is stored as a JSON tree
- data is synchronized in real time across multiple client apps

## how it works
lets us store and sync data between users in real time
- makes it easy for users to access their data on any device
- helps users collaborate

when data is updated in the realtime database, it stores the data in the cloud and updates all connected devices INSTANTLY
- persists data to the local cache
    - makes it optimized for offline usage

when a user goes offline, it uses the device’s local cache to store data and synchronizes both the cloud data and the local data when the user is online

security rules allow us to control users’ access to each database 
- useful in securing the database and authorizing user operations
- makes sure that users only have access to their data

## implementation
we need to import and initialize the firebase database service 
- we use the getDatabase function from the firebase/auth subpackage
    - we initialize the firebase database by calling the getDatabase function
    - we can also pass the firebase app instance to it

```
import { initializeApp } from 'firebase/app';
import { getDatabase } from 'firebase/database';


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

// Get a reference to the database service
export const db = getDatabase(firebaseApp);
```

## enable the firebase realtime database
1. go to the firebase project dashboard
2. go to the “realtime database” section 
3. click the “create the database”
4. choose a location for the database

    a. default is “United States(us-central1)”
5. click “next” to go to the next step
6. to define the security rules, choose the “start in test mode” option for now

    a. allows all reads and writes to the database for the next 30 days
7. click “enable” to get started

## temporary security rules
if the 30-day period elapses and we lose access to the database, we’ll need to set new security rules to enable us to read and write data to it

1. go to the “realtime database” section of the firebase console
2. go to the “rules” tab to show existing realtime database rules
3. copy the six lines of code in the widget below and **paste them into the editor provided:**

```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

4. click the “publish” button to finalize the process