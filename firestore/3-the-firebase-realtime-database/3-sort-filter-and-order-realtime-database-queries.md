# sort, filter, and order realtime database queries
we can use the realtime database query function to sort data by key, value, or the child value
- also provides a wide range of options to filter data
- takes the query of the instance as the first parameter
- takes a comma-separated list of query restraints as the second parameter

## sort data
1. specify an order method

    a. orderByChild - orders queries by the values of the specified child key or the nested path

```
import { getDatabase, orderByChild, query, ref } from "firebase/database";

const db = getDatabase();

const tasksRef = ref(db, "tasks");

const orderedTasksRef = query(tasksRef, orderByChild("difficulty"));
```

b. orderByKey - orders queries by their child key values

c. orderByValue - helps order scalar query results, such as strings, numbers, or boolean

## filter data
there are multiple functions to filter data

we can combine filter functions within the same query
- they can be grouped into two categories based on their functions
- category 1 - limit the number of results
- category 2 - filters results by key or value

### limit the number of results
limitToFirst and limitToLast functions
- set a maximum number of children to be synced for a given query

```
// retrieves the 20 most recent tasks written to our database

import { getDatabase, query, ref, limitToLast } from "firebase/database";

const db = getDatabase();

const tasksRef = ref(db, "tasks");

const filteredTasksRef = query(tasksRef, limitToLast(20));
```

### filter by key or value
startAt, startAfter, endAt, endBefore, equalTo functions
- set starting, ending, and equivalence points for queries
    - useful to paginate data
    - useful for finding items with children that have a specific value
    - ONLY ALLOWED AS ARGUMENTS when ordering by child or value

```
// retrieves a list of database entries with difficulty values that are
// alphabetically greater than or equal to hard

import { getDatabase, query, ref, orderByChild } from "firebase/database";

const db = getDatabase();

const tasksRef = ref(db, "tasks");

const filteredTasksRef = query(tasksRef, orderByChild("difficulty"), startAt("hard"));
```