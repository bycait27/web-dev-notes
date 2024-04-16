# cloud firestore an cloud storage security rules
begin with a **service declaration** that defines the scope of the security rules by identifying the firebase product to which they apply

then contains the **match declaration** using one or more match blocks
- used to specify the path to the document or file in the database or the storage bucket
- match blocks contain allow statements that determine the conditions to access the document or file in the path

## cloud firestore security rules
**the steps for writing cloud firestore security rules are as follows:**
1. indicate a syntax version using the rules_version statement (evaluated as v1 if none is provided)
2. the service cloud.firestore declaration defines the scope of these rules (cloud firestore)
3. define the match patterns to identify the path. all documents in cloud firestore fall into the /databases/{database}/documents path

    a. this path can be thought of as the root of the database (it's always the path defined in the first match block)

    b. the next match block must contain the actual path to the document
        - we can also declare possible wildcards in the paths using the curly braces
        - these will match all documents existing on the defined path
4. the match blocks contain allow expressions that determine the conditions to grant access to the defined path

    a. require methods like read or write to describe the nature of the database access

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userID} {         // matches any document in the path users/
      allow read: if <condition>;
      allow write: if <condition>;
    }
  }
}
```

ALL match statements MUST point to documents, NOT collections
- must either point to specific documents or match any document in the specific collection using wildcards

we can break down the read and write methods into more explicit methods
- get, list, create, update, and delete

the data request made by the cloud firestore client SDK is called the request object
- **two main properties:** auth and resource
    - the auth object contains info about the signed-in user that we can use to define access conditions
    - the resource object contains info about the document being accessed
    - we can access the fields on the document using the data property on the resource object and then check these fields for validity and consistency

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userID} {
      allow read: if request.auth.uid == userID;
      allow create: if request.resource.data.score is number &&
      		request.resource.data.score >= 5 &&
          request.resource.data.score <= 10;
    }
  }
}
```

## cloud storage security rules
we can control access to objects and files stored in our cloud storage buckets (similar to cloud firestore rules, follow same syntax)
- both begin with a declaration of the syntax version (also use v2)
- use the service firebase.storage declaration to define the scope of the rules
- all files in a cloud storage bucket fall into the /b/{bucket}/o path
    - considered the root of the storage bucket (always thee path defined in the first match block)
    - the next match block must point to a file in the bucket
        - can point to specific files or use wildcards to match all files in a path
- the match blocks contain allow expressions that determine the access conditions for the path

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{imageID} {
  	  allow read: if <condition>;
      allow write: if <condition>;
    }
  }
}
```

the data request made by the cloud storage client SDK is avaliable under the request object
- **have access to two properties:** auth and resource
    - the auth object contains info about the signed-in user that we can use to define access conditions
    - the resource object has properties that provide info about the file, such as the file name and other metadata that we can utilize in our security rules to ensure validity and consistency

```
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{imageId} {
      allow read: if request.auth != null
      // Only allow uploads of any image file that's less than 5MB
      allow write: if request.resource.contentType.matches('image/.*') && 
        request.resource.size < 5 * 1024 * 1024;
    }
  }
}
```

***we must always define rules for a specific document or file path in the database. we can use recursive wildcards to match all the documents, in a collection and its subcollection, and files in multiple Storage Bucket segments:**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
		match /users/{restOfPath=**} {
      allow read;
      allow write: if <condition>;
    }
  }
}
```

**recursive wildcards:** {path=**}, apply security rules to an arbitrarily deep hierarchy
- while matching documents in a Cloud Firestore collection, it will also match documents contained in deeply nested subcollections
- not only will the rules above match documents in the users/test@educative.io path, they will also match documents in the users/test@educative.io/tasks/{taskId} path
- we must be careful if we need to use the values of the recursive wildcard variable in our Security Rules since the paths can change