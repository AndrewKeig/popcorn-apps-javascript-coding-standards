
# PopcornApps Javascript Coding Standards

This file contains Javascript Coding Standards for, Node.js based APIs/microservices and React applications.

## ESLint

Use eslint, airbnb has a good style guide, there are others, standard.js, for example. choose one and stick with it.

- https://github.com/airbnb/javascript
- https://standardjs.com/


## VSCode

The terminal in vscode, has a useful problems tab, it will display any eslint issues, after installing vscode extension ESLint.

Learn how to use the debugger, so you can debug and step through your code.

Useful vscode extensions

- ESLint
- Debugger for Chrome
- Prettier - Code formatter
- Path Intellisense
- npm Intellisense
- npm
- JavaScript (ES6) code snippets
- Markdown Preview Github Styling
- Depcheck



## API Development

### Node.js

Use NVM to install node.js

- https://github.com/nvm-sh/nvm


### Add scripts to your package.json

```
"scripts": {
  "start": "node index.js",
  "test:watch": "jest --watch",
  "lint": "eslint 'src/**/*.js'",
  "fix": "prettier --write '*.js'",
  "test": "jest --runInBand",
  "coverage": "jest --coverage --runInBand",
  "depcheck": "depcheck .",
},
```


### API Frameworks

- Fastify, if you need to use swagger
- Express, use this if you prefer, and dont neeed swagger


### Node Modules

Remove unused modules fron your `package.json` dependencies.  Configure and use depcheck.

https://github.com/depcheck/depcheck


### Modules to prefer

- `axios`, for http
- `date-fns`, for dates, `moment` is very big try to avoid, expecially in react.
- `pg-promise`, postgres, promise based
- `dotenv`, env files
- `mongodb` mongo client
- `pino` superfast logger
- `uuid` generating uuids


### Tool belts (lodash/ramda)

Notes: `lodash/ramda`

Please try to avoid using these libraries, for functions that already exist on the array prototype, examples:

- map
- foreach
- reduce
- some
- find
- filter

Please study the javascript array methods, and prefer these, only use lodash if it makes sense:

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

If using lodash, please import like so, do not import the entire lodash library.  This is really important in react.

```
const uniqWith = require('lodash/uniqWith');
```

### REST APIs

Your API should follow REST principles, think about the resource name. e.g. operations on a user would have a `user` api, which would have enpoints like:

- `GET /user/:id` - get a single user
- `POST /user` - create a user
- `DELETE /user` - delete a user
- `PUT /user` - update a user
- `POST /user/search` - search for users

Try to follow the above format, use the HTTP verbs, and do NOT for example do this, we do not want resources with verbs as resource names:

- `GET /getuser/:id` - get a single user
- `POST /createUser` - create a user
- `DELETE /deleteUser` - delete a user
- `PUT /updateUser` - update a user
- `POST /user-search` - search for users

#### API Responses

Please keep the response format of your API endpoints the same throughout the API.  Prefer something like:

```
{
  requestId: '12345',  // So we can track the request
  message, // A message to return to the sender
  responseTimeStamp, // a time stamp, how long the request took
  data: {} or [] // if data it returned
}

```

#### Status codes

Please use the correct status codes:

- `GET /user/:id` - 200
- `POST /user` - 201
- `DELETE /user` - 202
- `PUT /user` - 202
- `POST /user/search` - 200

- Validation error - 422
- Authentication error 401


## Testing

Please make an effort to write tests, use `Jest`, as it has:

- mocking capabilities
- code coerage
- expectations

Notes:

- Testing is easy when functions are small, and do a single thing; the single responsibility principle will help you
- You should mock calls to external APIs
- Create fixture files for intergration tests
- Tear down resouces after/before test runs
- Assert against `status codes`, `messages` and some part of the `http response`
- Tests act as documentation so use good naming conventions, so its obvious what the test is testing


https://codepen.io/allanpope/post/single-responsibility-principle

### Integration Testing

When testing APIs, use fastifys `inject` or for `express` use `supertest`, they are simple to use, you should have at least one integration test, for each API endpoint.

https://github.com/fastify/fastify/blob/master/docs/Testing.md

https://github.com/visionmedia/supertest


### Code Coverage

Jest has support for code coverage, try to keep coverage above 80%.

if you run `jest --coverage --runInBand`, a report is generated in your project folder under `coverage/lcov-report/index.html`.  Open it up and you can see what is, and is not, being tested.  You can then decide if its worth the effort of writing tests to cover lines of code not under test.


### Swagger/Joi validation

If you are required to use swagger, then swagger will be used to validate your API.  You will not need to write validation logic, checking the properties of a request to an API.

If you do not need swagger you should use `Joi` to validate a request on your API.  This should also be used in React.

https://github.com/hapijs/joi


### Load testing

When you have created an API endpoint, it makes sense to load test the endpoint. A simple way to do this locally, is to use `autocannon`.

https://github.com/mcollina/autocannon

Other useful tools for debuggging performance issues are `clinic.js` suit of tools, `autocannon` can be used with these tools to debug performance issues:

- https://clinicjs.org/doctor/
- https://clinicjs.org/flame/
- https://clinicjs.org/bubbleprof/


## Asynchronous Programming

- Use async await
- Prefer modules that supports promises
- If you must use a module that uses callbacks, use promisify, or wrap callback function in a promise.

- https://2ality.com/2017/05/util-promisify.html


## Asynchronous Iteration

When performing IO bound/async operations, on multiple items in an array, using `async await` use `Promise.all/map`, do not use `foreach`, it does not work correctly:

```
const downloadFile = async (media) => {
  // do something async
}

const getMedia = (media = []) => Promise.all(media
  .map(m => downloadFile(m)));

const result = await getMedia(media);
```

## Synchronous code

If your code does not make an IO bound call, no calls to DB/API etc, do not wrap this code in a ` new Promise`, and do not mark the function as `async`.

This simply creates promises for no reason, and can cause memory leaks.


## Frontend Development

### React Frameworks

If SEO is important to the application you are building, you should use Server Side Rendering (SSR),

Try to use one of these two frameworks.

- `next.js`, is already configured to support SSR out the box
- `create-react-app`, use this, if SEO not important, it can be configured with SSR, requires some work


- https://nextjs.org/
- https://github.com/facebook/create-react-app

### UI Frameworks

Please use a framework such as Material UI or Ant Design over bootstrap.

- https://material-ui.com
- https://ant.design

Do not use JQuery. Ever.

### CSS

Prefer styled components over sass or raw css.

### React State management


Prefer react hooks over redux.

Prefer stateless, pure components/functions.  Only containers/pages should manage global state.  State should be passed into components via props.

https://reactjs.org/docs/components-and-props.html#stateless-functions


### Proptypes/default props

Use Proptypes/default props, if you are not using flow or typescript.


## Continous Integration and Deployment,

Automate deployments to each environment, you may choose to manually deploy to production.

### Environment Variables

Use the npm package `dotenv`, to pull environment variables into your application, these will be available in `process.env`

Create a `.env` file for every project, do not check this in to github, distribute this amongst the team.

### Github

Create the following branches:

- develop
- uat or staging
- master - already exists

Do not push to Develop directly, create a feature branch, and a pull request and have it reviewed.  Do not merge your own pull requests.  have a tech lead merge this.

When you want to push a release to UAT or staging, create a pull request `staging <- develop` and merge.

When pushing to production, do the same, create a pull request `master <- staging` and merge.

Feature branches, should follow this format:

```
feature/<name>-<jira-number>
```

e.g.

```
feature/add-login-j1234
```

### Github actions

Github actions allow you to run lint and test scripts against pull reguests and branches, its free :).

Here is a build file, which is configured to run on each push, place this file here:

```
<project>/.github/workflows/ci.yml
```

```
name: Node.js CI

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x, 13.x]
        mongodb-version: [3.4, 3.6]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - name: Start MongoDB
      uses: supercharge/mongodb-github-action@1.1.0
      with:
        mongodb-version: ${{ matrix.mongodb-version }}
    - run: npm install
    - run: npm run lint
    - run: npm test
    - run: npm run depcheck
      env:
        CI: true

```
