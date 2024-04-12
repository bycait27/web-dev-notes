# intro to cloud functions
**cloud functions:** a Google cloud product that lets us automatically run back-end code in response to events triggered either by other firebase services or by HTTPS requests
- allows us to easily run server-side code without the need to set up a server
    - i.e. adding roles to users
    - i.e. reacting to changes in a database
    - i.e. notifying the user when something interesting happens
        - i.e. sending a welcome email when a user signs up
- admin SDK allows us to seamlessly interact with other firebase services from trusted environments via cloud functions
    - i.e. creating a new document in a cloud firestore users collection when a user signs up

## how it works
cloud functions for firebase are written in a node.js environment using either javascript or typescript
- all functions must be deployed to the cloud functions runtime before usage
    - when deploying a function, the firebase CLI takes all the code associated with that function and puts each function in separate containers
        - these are then converted according to the requirements of Google servers so that the servers can manage the function

cloud functions can either be fired directly with an HTTP request or triggered in response to an event 
- when this happens, cloud functions spins up a server instance for the function in the cloud that activates and runs the function in response to these triggers
- they then monitor this function and ensures that the instance created remains active until its promise resolves
- as our app usage increases or decreases, Google responds by automatically scaling the server instances needed to run these functions

**to enable billing on our project, perform the following steps:**
1. go to the firebase project dashboard
2. click the "spark plan" button to show the billing plans
3. click the "select plan" buttn under the "blaze" plan section
4. set up a billing budget for the project by entering a value in the provided field
- this allows firebase to alert the project admins when the project costs approach or exceed this amount

***free tier allows up to 2 million function invocations per month**

## implementation
1. run the command below in the terminal
```
firebase init functions
```
2. select a firebase project where the functions get deployed
3. click the "enter" key to use an existing project
4. select the firebase project associated with the configuratation values you are working with
5. select a preferred language for writing cloud functions

    a. select javascript 

    b. click "enter"

6. click the "y" key and then the "enter" key to enable ESlint to help catch bugs
7. overwrite the existing files that the dialogue asks to overwrite
8. click the "y" key and then the "enter" key to install the dependencies with npm

***note that the files and modules -- firebase-functions and firebase-admin -- need to be updated to their latest versions t access their latest features**

## enable cloud functions
use your authentication token from the firebase console to enable cloud functions