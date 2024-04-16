# intro to firebase hosting
**firebase hosting:** a production-grade web content hosting solution provided by firebase
- provides a fast, reliable, and secure way to host our app's static and dynamic content as well as microservices
- we can deploy web apps to a global CDN with a single CLI command

firebase provides subdomains with all firebase projects at NO COST
- devs can set up custom domains for their firebase-hosted sites as well
- all apps deployed with firebase hosting are automatically provisioned with SSL cerificates, making sure the content is always delivered securely

## implementation
we need to run the firebase CLI's hosting initialization command on a terminal to initialize firebase hosting on a project

**perform the following steps:**
1. run the command below in the root of the project's directory

```
firebase init hosting
```

2. we need to associate the directory with a firebase project
3. press the "enter" key to use an existing project
4. select the firebase project associated with the configuration values we are working with (or we can include the --project flag alongside the firebase project ID when running the initialization command)
5. choose a name for our public directory (holds the files for our app and is eventually deployed by firebase)
- default is public (rename it build)
    - react.js exports our app to the build folder after running the build command with npm
6. choose whether or not to configure it as a single-page app
- this option is for front-end libraries or frameworks like react, vue, or angular (serve only one HTML file)
7. press "Y" then "enter"
8. we get asked if we need to set up automatic deploys with GitHub
9. skip the overwriting of the index.html file in the build folder and wait for the initialization to complete

## enable firebase hosting
...