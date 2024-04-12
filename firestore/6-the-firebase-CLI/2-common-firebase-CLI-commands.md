# common firebase CLI commands

## configuration and project management commands
**the following commands are used to configure the firebase CLI and manage projects with the firebase CLI:**

**login:** authenticates the firebase CLI with your firebase account

**logout:** this logs out of the CLI

**init:** this initializes firebase and sets up a new firebase project in the directory. it can be used to initialize specific features when called with them. it creates a configuration file, firebase.json, in the current directory

**--help:** this returns helpful info on the firebase CLI or specific CLI commands

**projects:list:** this lists all firebase projects to which an account has access

**projects:create:** this creates a new firebase project

## deployment and local emulator commands
**the following commands are used to deploy firebase apps and allow us to interact with firebase services locally:**

**deploy:** this deploys codes and assets associated with a firebase project. it relies on the firebase.json file created during initialization -- firebase init
- by using the --only flag, we can deploy specific firebase features or services on the deploy command
    - this is followed by a comma-separated list of the services to be deployed

    ```
    firebase deploy --only hosting,functions
    ```

**serve:** this starts a local server for the project's static assets using the hosting configuration found in firebase.json

**emulators:start:** this starts the local firebase emulators

## cloud functions commands
**the following commands are associated with the firebase cloud functions service:**

**functions:log:** this reads logs from all the deployed functions

**functions:config:set:** this stores configuration values for cloud function projects

**functions:config:get:** this retirieves a project's existing configuration values

**functions:config:unset:** this removes a project's existing configuration values

**functions:delete:** this deletes a project's functions by name