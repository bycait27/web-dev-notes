# intro to cloud storage
a powerful, highly scalable, and cost effective object storage service
- enables app devs to store and serve user-generated content (photos or videos)
- users can upload, share, and download these files for use on our app

## how it works
we can store users' files (images, audio, videos, and user-generated content) associated with our firebase app
- stored in a Google Cloud storage bucket
    - we can access these files from both firebase and Google Cloud

users can share large files over the network any time, despite network quality

firebase SDKs adds Google security to this data to ensure uploads and downloads are done over a secure connection
- files are always protected and in safe hands :) (firebase auth, firebase security rules, SDKs)

## implementation
1. import the service's getter function from its subpackage
2. initialize it

    a. takes a reference to the firebase app instance as a parameter

```
import { initializeApp } from 'firebase/app';
import { getStorage } from "firebase/storage";


const firebaseConfig = {
  apiKey: "AIzaSyDNv52XXXXXXXXXXXXXXXXXXXXXX-krGF8E",
  authDomain: "educative-xxxxxxxxxxx-cfceb.firebaseapp.com",
  projectId: "educative-xxxxxxxxxxx-cfceb",
  storageBucket: "educative-xxxxxxxxxxx-cfceb.appspot.com",       // storage bucket URL
  messagingSenderId: "01234567890",
  appId: "1:01234567890:web:8e17ea99e18ac710e7934b",
  measurementId: "G4-HBF7IU9NPX",
};

const firebaseApp = initializeApp(firebaseConfig);

// Get a reference to the Cloud storage service
export const storage = getStorage(firebaseApp);
```

## enable firebase cloud storage
1. go to the firebase project dashboard
2. go to the "storage" section
3. click the "get started" button
4. we need to define the security rules for our bucket...

    a. choose the "start in test mode" option

        i. this allows all reads and writes to the bucket for the next 30 days
5. we need to set a location for our cloud storage bucket...

    a. default is "nam5 (us-central)"

        i. this works

6. click "done" and wait for firebase to create the storage bucket

## set cloud storage security rules
cloud storage data is private by default
- this also sets the default security rules for the cloud storage bucket
    - restricts all reads and writes to the bucket
        - we need to give the bucket new rules in order to use it

1. go to the "storage" section of the firebase console
2. go to the "rules" tab to show the existing cloud storage rules
3. **copy the eight lines of code in the widget below** and paste them into the code editor provided:
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write;
    }
  }
}
```

4. then click the "publish" button to finalize the process

***authenticated users can now perform read and write operations with cloud storage on our app**