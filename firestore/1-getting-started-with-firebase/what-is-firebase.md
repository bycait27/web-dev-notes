# what is firebase?
a Google-backed <mark>Backend-as-a-Service</mark> (BaaS) solution that provides devs w/ a tool kit to <mark>build, improve, and grow their apps</mark>

- this <mark>eliminates the need for devs to build and manage various back-end services and focus solely on app development</mark> (authentication, file storage, databases, hosting, testing, performance monitoring, analytics, etc.)
- firebase apps <mark>communicate directly w/ the backend</mark> via custom APIs and SDKs provided by firebase

## firebase architecture
firebase provides <mark>support for multiple client apps</mark> 

- these can directly talk to the backend <mark>(serverless architecture)</mark>
- **some benefits of serverless architecture:**
    - reduced operational and developmental costs (no servers to manage)
    - increased speed (apps can receive real-time updates from the backend)

the <mark>firebase console</mark> allows devs to manage firebase apps, services, and project settings <mark>(management panel for all projects)</mark>

## firebase services
**three product categories:** build, release, and engage

### build products
products that help build apps

effortlessly scale and accelerate app development w/out managing the infrastructure

**authentication:**

- identifies and verifies app users
    - creates personalized experiences across devices
        - easy to use SDKs and UI libraries to authenticate users
- supports email/password and email/link combos
- supports social logins through popular identity providers (Google, GitHub, Twitter)
- supports phone number verification (authentication)

**cloud firestore:**

- a Google Cloud product that provides a flexible, scalable, and real-time NoSQL cloud database to store and sync data across all client apps during client-side and server-side development
- supports expressive queries

**real-time database:**

- a scalable, real-time NoSQL cloud database to store app data
- data is stored as a JSON tree
- data is synchronized in real-time across multiple client apps
- offers offline support for web and mobile

**cloud functions:**

- a Google Cloud product that lets us automatically run backend code in response to events triggered by other firebase products or HTTPS requests
- serverless infrastructure (requires no maintenance)

**cloud storage:**

- a powerful, scalable, and cost-effective object storage service that enables app devs to store and server user-generated content (photos or videos)
- enables us to upload and download files to and from our Cloud Storage bucket

**hosting:**

- a fast and secure hosting solution
- easily deploys web apps and serves both static and dynamic content to a global web hosting CDN
- free subdomain offers
- can set up custom domains
- automatically provides free SSL certificate (always delivered securely)

### release products
products used to improve and maintain app quality (testing, troubleshooting, performance monitoring)

- help monitor adoptions, carefully roll out features, and diagnose crashes
- makes it easier to fix performance and stability issues early on

**crashlytics:** 

- a powerful, lightweight, real-time crash reporting solution
    - helps devs track, prioritize, and troubleshootstability issues
- integrates w/ Analytics to make debugging processes easier
    - provides access to a list of events that lead to app crashes

**performance monitoring:**

- a tool that provides devs w/ insights about the performance charaacteristics of their apps in real-time
- measures and collects performance data (HTTP requests, page load, iOS, Android)
- presents this data for devs to review and analyze (firebase console)

**test lab:**

- a cloud-based service to test apps on a wide range of devices and configurations
- provides a variety of virtually-hosted iOS and Android devices to run tests on

**app distribution:**

- allows us to pre-release apps to a broad set of users and testers
- useful when devs need early feedback before official app release
- allows us to invite testers and organize them intoo groups for easier management

### engage products
products used to boost user engagement (analytcs, messaging campaigns, a/b testing)

- improve our understanding of how users interact w/ our apps
- uncover new insights on how to better support and retain them

**analytics:**

- a free app measurement solution (Google Analytics) that offers insights about the app users user engagement
- provides valuable metrics (user retention, demographics, engagement)
    - to better analyze user behavior

**remote config:**

- a cloud service that allows devs to make dynamic changes to the behavior and appearance of their apps w/out requiring users to download an update
- supports personalization that uses machine learning to automatically define config parameters and create optimal experiences for each user

**predictions:**

- applies machine learning to the data collected by Google Analytics to create dynamic user segments and make predictions on users’ expected behavior
- can be used w/ remote config and a/b testing (to create optimal experiences for users based on their “predicted” behavior)

**a/b testing:**

- a tool that makes it easy to conduct and analyze, product and market experiments (a/b tests)
- lets us target predicted. user segments for tests and analyze whether or not they have a measurable effect on key metrics like retention and revenue

**cloud messaging:**

- a cross-platform messaging solution solution that allows us to deliver push messages to our app users w/out cost
- integrated w/ Analytics and predictions
    - can be set up to target a particular predicted user segment (send push messages to users who are likely to disengage from our app to spark re-engagement or retention)

**in-app messaging:**

- lets us send targeted, contextual messages to active users of an app to prompt them to use the app’s key features
    - can encourage exploration (getting users to subscribe to a newsletter, watch a video, etc.)