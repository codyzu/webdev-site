# Firebase Cloud Functions

## 1 Add functions to your project

1. Enable Firebase functions:
   ```cmd
   npm run firebase -- init
   ```
   
   1. Choose "Functions"
   1. Select "JavaScript"
   1. "Do you want to use ESLint to catch probable bugs and enforce style?" **enter `n`**
   1. "Do you want to install dependencies with npm now?" **enter `n`**

1. Modify your functions to use the [node.js version 8 runtime](https://firebase.google.com/docs/functions/manage-functions#set_nodejs_version).

   In `functions/package.json` add:
   ```
   "engines": {
     "node": "8"
   },
   ```

1. Install the dependencies with yarn (or npm):
   ```cmd
   cd functions
   yarn --ignore-engines
   ```

   ⚠️ _Note: `yarn` or `yarn install` will enforce the engine that we added to `package.json`. Therefore, we should use the `--ignore-engines` flag with yarn or use `npm install`. You can make this permanent for your `functions` directory by running `echo "--ignore-engines true" > .yarnrc`_

1. In vscode, open `functions/index.js` and **un-comment** all of the lines of code.

1. Open the `firebase.json` file. Add a rewrite rule so that you function is accessible in the same domain as your hosting. Your rewrite rules should resemble the following:
   ```json
   "rewrites": [
     {
       "source": "/helloWorld",
       "function": "helloWorld"
     },
     {
       "source": "**",
       "destination": "/index.html"
     }
   ]
   ```

1. Re-deploy your project **⚠️ from the root of your project ⚠️**:
```cmd
cd ..
npm run firebase -- deploy
```

1. You just deployed your first serverless cloud function! You can access either directly in the URL listed in the console:
   
   `https://us-central1-{your project name}.cloudfunctions.net/helloWorld`

   Or at the path that we configured in the hosting:

   `https://{your project name}.firebaseapp.com/helloWorld`

   ☝️ Take a moment to appreciate what we now have. A website deployed, with javascript backend configured to exact routes. **Plus, we never configured a server, virtual machine, docker, or paid for hosting!**

💡 The function `onRequest()` accepts supports routers and apps from [Express.js](https://expressjs.com/fr/). In our example, we use a function `(request, response) => { ... }`. The `request` and `response` objects are the same `req` and `res` that we used in Express.

#### Exercise 1.1 Add a function to `functions/index.js` named `helloData` that accepts a name as a query parameter and returns it in JSON, i.e. `{name: 'cody'}`.

1. use `response.send()` to send and object in the response
1. query parameters, i.e. `/helloData?name=cody` are parsed into the `request.query`, i.e. `request.query.name`.
1. you can debug your function by adding logging statements, i.e. `console.log(request.query)` and reading them with `npm run firebase -- functions:log` (there will be some delay)
1. ⚠️ don't forget to add a rewrite rule for `helloData` to your `firebase.json` file!

#### Question 1.1: What is the content type of the response? (Hint: use the inspector)

#### Exercise 1.2: Add a function to `functions/index.js` named `helloHtml` that accepts a name as a query parameter and returns it in HTML, i.e. `<h1>hello cody</h1>`.

1. use `response.send()` to send an HTML document, i.e. `<html><body><h1>...</h1></body></html>`
1. use query parameters to parse the name
1. ⚠️ don't forget to add a rewrite rule for `helloData` to your `firebase.json` file!

#### Question 1.2: What is the content type of the response? (Hint: use the inspector)

#### Bonus 1.1 See [pug renderFile](https://pugjs.org/api/reference.html#pugrenderfilepath-options-callback) for how you might use pug to render a complete HTML template.

#### Bonus 1.2 The `onRequest` function handler can also be passed an express app object. This allows us to to serve multiple routes from the same function. Serve a request app from a new function that serves multiple routes.

# Why Functions?

Firebase Cloud Functions
- Functions as a Service (FaaS)
- Serverless

What is possible?

Types of triggers:

## Call them directly

- HTTPS request (our example above)
- From a Firebase application (automatic authentication)

Examples:
- Perform complex calculations (i.e. algorithms not suited for portable phone)
- Add dynamic pages to a static site
- Call external APIs (i.e. AI & ML)
- Authenticate and perform "admin" actions
- Perform routine maintenance with cron service (i.e. delete "old" data for GDPR)

## Triggered by events in Firestore

- New document
- Modified document
- Deleted document

Examples:
- Update aggregations (i.e. rating average and count)
- Send notifications (i.e. notify user when they have a new follower)
- Delete associated data (i.e. delete all ratings when a post is deleted)
- Sanitize text (i.e. remove profanity from comments)

## Triggered by events in Cloud Storage

- File saved
- File deleted

Examples:
- Generate thumbnail or lower quality version of an image

## Triggered by authentication

- New user
- User deleted

Examples:
- Send welcome email when user joins
- Create default data when user joins
- Delete associated data when user deletes their account

Refer to the [Firebase documentation](https://firebase.google.com/docs/functions/use-cases) for more use cases.

