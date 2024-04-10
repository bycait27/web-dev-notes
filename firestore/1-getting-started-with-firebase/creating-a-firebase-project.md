# adding firebase to a project/creating a firebase project

creating a firebase project

    a. allows us to share app data and resources across platforms

    b. this also creates a Google Cloud project behind the scene (virtual containers for data and resources on Google Cloud Platform)

    c. we can set up and manage all versions of our app via the firebase console

## create a firebase project
we have to create a firebase project in order to add it to a javascript project

1. go to firebase website (link)
2. click “go to console”
3. click “create project” to create a new firebase project OR click “add project” to add an existing firebase project to the console
4. we can choose to add Google Analytics to our project
5. click “create project”
6. click “continue” to head to the project dashboard on the firebase console

## register an app
we need to obtain configuration details that connect our local front-end app to our firebase project and its resources
- the configuration contains parameters we can add to our front-end app to enable it to communicate wth the firebase server APIs

1. click “</>” on the project dashboard
2. give the front-en app a nickname
3. click “register app”
4. click “continue to console” to go back to project dashboard

we can now access the app we create by clicking “1 app” on the project dashboard
- this represents the front-end app that connects to our firebase project
- we still need to link our local app to the firebase project

1. click “1 app” on the project dashboard
2. click on the ⚙️ icon to go to project settings
3. scroll down to the “your apps” section
4. click on the “config” option under “SDK setup and configuration” to show the config object

    a. this object contains info about our firebase project that we need to link to the frontend

## install the firebase SDK and initialize firebase
1. install the firebase SDK via npm
2. import the initializeApp function from the firebase/app subpackage

    a. this helps us create a firebase app that stores our firebase config for our project
3. pass the config object to the initializeApp function

    a. this returns an instance that we can store in a constant (firebaseApp)

        i. this represents our firebase app which the firebase SDK uses to connect our app to our specific firebase project or backend

```
import { initializeApp } from 'firebase/app';

const firebaseConfig = {
  apiKey: "AIzaSyDNv52XXXXXXXXXXXXXXXXXXXXXX-krGF8E",
  authDomain: "educative-xxxxxxxxxxx-cfceb.firebaseapp.com",
  projectId: "educative-xxxxxxxxxxx-cfceb",
  storageBucket: "educative-xxxxxxxxxxx-cfceb.appspot.com",
  messagingSenderId: "01234567890",
  appId: "1:01234567890:web:8e17ea99e18ac710e7934b",
  measurementId: "G4-HBF7IU9NPX",
};

// firebase app instance
const firebaseApp = initializeApp(firebaseConfig);
```

## access firebase services in our app
we can import all firebase srvices from the firebase SDK via their individual subpackages

we import the getter function service from the service path “firebase/service”
- we need to initialize our firebase app BEFORE calling any getter function
- the getter function takes the firebase app as a parameter

```
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';


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

export const auth = getAuth(firebaseApp);
export const db = getFirestore(firebaseApp);
```