# file-based environment configuration in firebase
**environment variables:** variables made up of name-value pairs used to configure values inside an app's code from outside the code
- they can affect the behavior of running processes on a computer or operating system

when running clou functions, we will often need to include additional configuration required by functions such as:
- API keys
- client secrets
- etc.

the firebase SDK for cloud functions offers a built-in environment configuration to make it easy to store and retrieve this type of data for our projects
- we can choose between using a runtime environment configuration with the firebase CLI or a file-based configuration of these variables

## file-based configuration
the firebase SDK for cloud functions supports the .env file format for loading environment variables into the process.env interface, where our app reads them
- we need to create the .env file containing our desired variables and include it in the functions/ directory on our firebase project

```
PLATFORM=Educative
COURSE=Firebase
YEAR=2022
```

**the values declared in this file are loaded whenever we run the cloud functions deploy command and can be accessed on our cloud functions code with process.env:**

```
const functions = require("firebase-functions");

// function responds with "Firebase course on Educative"
exports.firebaseOnEducative = functions.https.onRequest((request, response) => {
  response.send(`${process.env.COURSE} course on ${process.env.PLATFORM}`);
});
```

**to successfully use file-based configuration in our cloud functions, we must have at least v3.18.0 of the cloud functions module and v10.2.0 of the firebase CLI**