# deploying to firebase hosting 
now, firebase has automatically creates, or updates the firebase.json file to include our project's hosting configuration (REQUIRED to deploy assets with the firebase CLI) 
- lets Firebase know what files, folders, and settings on our directory are to be deployed to our Firebase project

## deploy our app
**we can deploy our application by running the Firebase deploy command below:**

```
firebase deploy --only hosting
```

after a successful deployment, we can access our application on the two subdomains provided by Firebase—https://project-id.web.app and https://project-id.firebaseapp.com 

**while deploying, we can also include comments using the -m flag, as shown below:**

```
firebase deploy --only hosting -m 'Awesome new version'
```

with this, we can track different versions of our application on the Firebase console, making it easier to roll back or delete older deploys