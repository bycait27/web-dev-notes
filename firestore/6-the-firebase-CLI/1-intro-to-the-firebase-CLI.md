# intro to the firebase CLI
**CLI:** command line interface, an interactive text-based interface that processes commands to a computer program or operating system as lines of text
- allows users to input text commands into a terminal or a command prompt

**command-line interpreter/ processor:** handles these commands and returns an appropriate result to the users' commands

## firebase CLI
set of tools used to administer firebase projects
- devs can view, test, manage, and deploy code and assets to their firebase projects from the CLI

## set up the firebase CLI

### firebase CLI installation
we can install the firebase CLI via npm 

**run this command via the terminal:**
- this enables the firebase command globally on our device

```
npm install -g firebase-tools
```

### log into the firebase CLI
we need to authenticate our Google account to log into the CLI
- from here we can access, manage, and deploy firebase projects linked to our account

**to login using your google account run the following command in the terminal:**
- this command opens a web page on our machine and connects to the localhost

```
firebase login 
```

**once we are taken to the web page, we will complete the following steps:**

1. sign in to your Google account or authenticate a Google account to access the firebase CLI
2. click the "allow" button to finalize the permissions
3. confirm the terminal command and session ID in the provided prompts
4. copy the authprization code from the authorization code section 
5. head back to the terminal
6. paste the authorization code in the terminal 
7. click the "enter" key
8. this returns an authentication token to be used for subsequent firebase CLI commands
9. copy the token and save it 

***the --token flag is not required on a local machine**
```
firebase <command> --token $CLI_TOKEN
```