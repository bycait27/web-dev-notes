# realtime database security rules
it is dangerous to have security rules on read and write operations set to permit open access
- if the app is deployed, anyone who guesses or has access to our project ID can easily steal, modify, or delete our data

**firebase security rules:** secure and control access to data in these firebase products making sure that users in a firebase project can only read and write data they're allowed
- these rules sit between the client app and the data
    - act as protection against malicious users and ensure that all reads and writes performed by the client SDK are allowed for the end user

## how it works
firebase security rules provide an expression language that lets us choose how the end user is allowed to access data in firebase products
- match a pattern against database or bucket paths and apply custom conditions to either allow or deny access to these paths

the realtime database security rules are formatted using the JSON syntax

the cloud firestore security rules follow the common expression language syntax

## write a realtime database security rules
these security rules live on firebase servers and are enforced automatically on each request
- only approved read and write requests will be allowed to complete

**writing realtime database security rules:** 
1. we must start by identifying a node in the database
2. we can use wildcards to match possible child nodes to create a path
3. we define rules for those paths using conditional statements

```
{
  "rules": {
    "<<path>>": {
      // Allow the request if the condition for each method is true.
      ".read": <<condition>>,
      ".write": <<condition>>
    }
  }
}
```

all realtime database security rules begin with the rules key
- we need to specify a path as well as read and write conditions for the path

```
{
  "rules": {
    "tasks": {
      ".read": true,
      ".write": false
    }
  }
}
```

the realtime database security rules include built-in variables that allow us to refer to other paths, auth info, etc.

```
{
  "rules": {
    "users": {
      "$uid": {
        ".read": true,
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```