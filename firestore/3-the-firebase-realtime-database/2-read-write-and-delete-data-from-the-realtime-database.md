# read, write, and delete data from the realtime database
we need to create a reference to the specific path within our database to carry out operations
- we use the ref function from the firebase/database subpackage
    - returns a reference to the location in the datbase that corresponds to the provided path
    - takes the database instance as its first argument
    - can take the path that the returned reference points to as its second argument (OPTIONAL)
        - if this isn’t provided, the function returns a reference to the root of the database

```
import { getDatabase, ref } from "firebase/database";

// initialise database
const db = getDatabase();

// reference to database root 
const dbRef = ref(db);

// reference to the users/tasks/* path
const tasksRef = ref(db, "users/tasks/" + user.uid);
```

## basic write operations
we use the set function to write data to the database
- this overwrites all existing data at the specified location
    - replaces child nodes with the new values
- takes a reference to the location as its first argument
- takes the values to be written as its second argument

```
import { getDatabase, ref, set } from "firebase/database";

// initialise database
const db = getDatabase();

const tasksRef = ref(db, "tasks");

const addNewTask = (task, difficulty) => {
  set(tasksRef, {
    tasks: task,
    difficulty,
  })
}
```

we can also use the push function to write new data to the same database location WITHOUT overwriting the existing data
- generates a new child node every time data is written to the location
    - assigns it a unique key

```
import { getDatabase, ref, push } from "firebase/database";

// initialise database
const db = getDatabase();

const tasksRef = ref(db, "tasks");

const addNewTask = (task, difficulty) => {
  push(tasksRef, {
    tasks: task,
    difficulty,
  })
}
```

## basic read operations
we can use the get function
- returns a promise that resolves to give us a snapshot of the queried database location
- IF USED EXCESSIVELY, we might encounter performance loss and increased bandwith usage
    - recommended to use a real-time observer to read data

```
import { getDatabase, ref, get } from "firebase/database";

// initialise database
const db = getDatabase();

const tasksRef = ref(db, "tasks");

get(tasksRef)
  .then((snapshot) => {
    const data = snapshot.val();
    console.log(data);
  })
  .catch((err) => {
    console.error(err);
});
```

## listen for changes with an observer
to listen for real-time changes in a particular location on our database, we use the onValue function
- it is triggered by the intial data read
- it is triggered again whenever the data changes

- takes a reference to the location to be queried as its first argument
- takes a callback that triggers when the event occurs as its second argument
    - we pass a snapshot to this callback
        - contains data in the queried location
        - the val() method on the snapshot returns null if there is NO DATA at that location

```
import { getDatabase, ref, onValue } from "firebase/database";

// initialise database
const db = getDatabase();

const tasksRef = ref(db, "tasks");

onValue(tasksRef, (snapshot) => {
  const data = snapshot.val();
  console.log(data);
});
```

## delete data from the database
we can call the remove function (MOST DIRECT WAY)
- called on the reference to the location that contains the data

we can also delete data by specifying null as the value to be written when calling the set function on the location

```
import { getDatabase, ref, remove } from "firebase/database";

// initialise database
const db = getDatabase();

const tasksRef = ref(db, "tasks");

remove(tasksRef).then(() => {
  console.log("location removed");
});
```