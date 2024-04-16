# environment configuration with the firebase CLI
functions.config() was used for environment configuration before firebase-functions v3.18.0
- made it possible to store, retrive, and access data for each firebase project

## set environment configuration with the firebase CLI
we need to use the firebase functions:config:set command
- uses a key-value pair syntax similar to regular javascript objects (makes it easy to group related configurations)

this stores the variables with their assigned keys for use in any of our cloud functions at any time

**only lowercase characters are accepted in keys (uppercase are NOT ALLOWED)**

```
firebase functions:config:set user.name="AWESOME NAME" user.email="awesome@educative.io"
```

## retrieve runtime configuration
**to inspect or check the currently stored configuration associated with a firebase project, we need to use the following CLI command:**

this command returns a JSON output containing the current environment configuration associated with a project ir an empty javascript object if there is no environment configuration

```
firebase functions:config:get
```

## access the environment configuration
stored enviroonment configuration is available via the functions.config() object
- **stored variables can be accessed like javascript object keys:**

*we use destructuring of the functions.config().user object

```
const functions = require("firebase-functions");

exports.helloUser = functions.https.onRequest((request, response) => {
  const { name, email } = functions.config().user;

  response.send(`Hello ${name}! Your email is ${email}`);
});
```