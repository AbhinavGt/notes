# Function As A Service (FAAS)
- It means being able to deploy functions on a cloud
- And run those function based on an event
- The difference between this and a server is that servers are long lived (ie they are always working on some port)
- FAAS are not long lived they work only when the subscribed event is triggered and then they shut down (ie stateless)
- Lambda is name for FAAS of AWS
- You can create you own using kubernetes
# Core Concepts
## Functions
- Each function is an independent unit of execution and deployment, like a microservice.
- A function is merely code, deployed in the cloud, that is most often written to perform a single job such as:
	- Saving a user to the database
	- Processing a file in a database
	- Performing a scheduled task
# Serverless vs Framework Serverless

- Serverless	
	- A cloud computing model
	 - A concept/architecture
	- Cloud provider manages servers
	- Examples: AWS Lambda, Azure Functions, Google Cloud Functions

- Serverless (Framework)
	- It is a build system and an environment around all the provider platforms that have FAAS
	- A tool for building and deploying serverless applications
	 - An open-source framework (CLI)
	- Helps you interact with cloud providers
	- It gives us a common API point to interact with those functions.
	- Deploys to AWS Lambda, Azure Functions, Google Cloud Functions, etc.
# Setting Up
- Globally install serverless
- `sls` is short hand for `serverless`
- `sls create -t {template} -n {name}`.
- Template has `{platform}-{language}`
```bash
➜  serverless-with-aws git:(lesson-1)
	npm i serverless -g
➜  aws 
	sls

Serverless ϟ Framework
Welcome to Serverless Framework V.4
Create a new project by selecting a Template to generate scaffolding for a specific use-case.
✔ Select A Template: · AWS / Node.js / HTTP API
✔ Name Your Project: · aws-nodejs
✔ Template Downloaded
Create or select an existing App below to associate with your Service, or skip.
✔ Create Or Select An Existing App: · Skip Adding An App
✔ AWS Credentials Set-Up Method: · Skip & Set Later (AWS SSO, ENV Vars)
➜  aws 
	cd aws-nodejs
➜  aws-nodejs 
	ls -a
.  ..  .gitignore  handler.js  README.md  serverless.yml
```
- `handler.js`
	- Handler is the term that platforms use to descripe the actual file
	- It is the file that contains the AWS Lambda function—the code that AWS executes when your Lambda is invoked.
	- Think of it as the entry point to your serverless application, similar to: `main()` in C/C++
- `serveless.yml`
	- This is the configuration file for a serverless itself
	- The Serverless Framework reads this file and creates the necessary AWS resources for you.
```js
// handler.js
exports.hello = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: "Go Serverless v4! Your function executed successfully!",
    }),
  };
};
// This is an handler for an API call
```

```yml
org: a8hi9t

service: aws-nodejs

provider:
  name: aws
  runtime: nodejs20.x
  profile: femasters
  region: us-west-1
  stage: dev

plugins: 
	- serveless-offline
functions:
  hello:
    handler: handler.hello
    events:
      - httpApi:
          path: /
          method: get
```

-  The `org` in the Serverless Framework Dashboard.
	- Used when you log in to Serverless Framework.
	- Helps manage multiple projects.
- `service` is the namespace for the application or group of lambdas that you're creating.
- You can put many lambdas in this project and deploy them together or separate it.
- `provider` tells Serverless:
	- `name` :- Deploy to `{platform}`
	- `runtime`:- Which runtime to use for Lambda
	- `profile`:- This corresponds to a named profile in your local configuration.
	- `region` :- Deploy everything into `{region}` server of the platform
	- `stage` :- State that your project is in. 
		- Stages allow multiple environments. Each stage gets its own resources.
		- `aws-nodejs-dev`,  `aws-nodejs-prod`
		- This prevents production from interfering with development.
- `plugins`:- Plugins add extra features to the Serverless Framework
	- `serverless-offline`:- plugin lets you run your API locally without deploying to AWS.`

- `functions`section lists all the Lambda functions in your project one or many
	- `hello` :- The name of the lambda function
	- `handler:handler.hello` 
		- The path to the file name of the source code for the function that we are calling.
		- This tells AWS: `handler.js -> export const hello = ...`
		- The format is always: `filename.exportedFunction`
		- The path is relative to `serverless.yml`
	- `events`:- Events decide what triggers the Lambda function.
		- `httpApi` :- This creates an HTTP API in API Gateway.
			- `path` :- This defines the URL path. So your endpoint is the root path
			- `method: get` :- Only HTTP GET requests will invoke this Lambda.
## To run lambda function locally
- Since we have not set an API and its just a function
- We can invoke the function locally 
- `sls invoke local -f {functionName}`
```bash
➜  aws-nodejs 
	sls invoke local -f hello

{
    "statusCode": 200,
    "body": "{\"message\":\"Go Serverless v4! Your function executed successfully!\"}"
}
```

# First Lambda Solution
## old version -- start

1. Config base
```YAML

service: serverless

provider:
  name: aws
  runtime: nodejs20.x
  profile: femasters
  region: us-west-1
  stage: dev

plugins: 
 - serverless-offline

```

2.  Install plugin
```bash
npm install serverless-offline --save-dev
```

```js
// helloworld.js
module.export.handler = (evt, ctx, done) => {
	
}
```
- `module.export.handler` :-  export a function on a handler
	- It has three arguments:
	- `evt` :- event; what was the payload; example: if you do request to API what will you pass to the body
	- `ctx` :-  context; meta information about the execution context of your lambda
	- `done` :- done; callback; Whenever you have finished the execution inside the handler. You call done and send back the response that you want.
```json
// event.json
{
	"message": "hello"
}
```
- The event.json is used as path to the `sls invoke` as `-p` flag
	- `-p` :-  Path to JSON or YAML file holding the input data
## Creating Lambda

```
serverless-with-aws/
│
├── .git/
├── node_modules/
├── src/
│   ├── event.json
│   └── helloworld.js
│
├── .gitignore
├── README.md
├── package-lock.json
├── package.json
├── serverless.yml
└── yarn.lock
```

1. Update serverless.yaml
```YAML
service: serverless

provider:
  name: aws
  runtime: nodejs20.x
  profile: femasters
  region: us-west-1
  stage: dev

plugins: 
 - serverless-offline

functions:
	helloworld:
	handler:
	 - src/helloworld.handler
```

2. Update helloworld.js
```Js
module.export.handler = (evt, ctx, done) =>{
	done(null, 'hello')
}
```

3. run invoke
``` bash

➜  serverless-with-aws git:(lesson-1) ✗ 
	sls invoke local -f helloworld
"hello"
```

4. Introducing an error
```js
module.export.handler = (evt, ctx, done) =>{
	done(new Error('yooo'), 'hello')
}
```

```bash
➜  serverless-with-aws git:(lesson-1) ✗ 
	sls invoke local -f helloworld

{
    "errorMessage": "yooo",
    "errorType": "Error",
}
```

5. Passing an event
```Js
module.export.handler = (evt, ctx, done) =>{
	done(null,evt)
}
```

```bash
➜  serverless-with-aws git:(lesson-1) ✗ 
	sls invoke local -f helloworld -p src/event.json


{
    "message": "hello"
}
```
- File is the payload which is also the event
- Lambda function can also be used as another lambda function's event

# Lambda Events
## Sync vs Async
- There are two different mode of running a lambda function 
	- Synchronous:-  It is when you call a lambda and you're waiting for the response to comeback. Like API event
	- Asynchronous:- Its that you fire and forget about it. Update S3 somewhere, send an email in the background or update a DynamoDB.
## Event Types

Commonly Used Event Template in AWS
- `Cloudfront A/B Test` 
	- Cloudfront is AWS CDN, It's an Edge Network. You can perform A/B testing on it using Lambda
- `API Gateway AWS Proxy`
	- Let lambdas respond to the API calls. Controller for the API
- `SES Email Recieving`
	- Simple Email Service with AWS; Upon subscribing lambda perform task when email is received
- `CloudWatch Logs`
	- Subscribing to logs from cloudwatch; Keep tracks of API calls; pick things off a stream and send them through ETL service or pipeline or data warehouse for processing later
- `S3 put and S3 delete`
	- You can respond to events inside of S3. It's basically like Amazon's file system in a cloud
## Cloud Formation and serverless framework

- Test templates are comprised of objects
```json
//CloudWatch logs
{
  "awslogs": {
    "data": "H4sIAAAAAAAAAHW..."
  }
}
```
- Bring the object of the event template to your local machine and write you lambda to respond to this type of event
- There is tradeoff. If you lambda is using resources that AWS needs, or that it needs AWS.
### Cloud Fromation
- Serverless (framework) uses Cloud Fromation
- Cloud Formation is an API that you can use ties the available services together
- You are basically creating a configration file and send that to {cloud provider}
- Services are an abstraction on top of AWS's abstraction
## API Gateway Routing
- It is like a proxy in front of anything that you want it to be. In this case lambda functions
- It is going to be responsible for routing our HTTP events to our lambdas.
- The event that we will subscribe to is `API Gateway AWS proxy`
- API Gateway sits between you client and lambda. Thus it is a long live process or a server 
```json
{
  "body": "eyJ0ZXN0...",
  "resource": "/{proxy+}",
  "path": "/path/to/resource",
  "httpMethod": "POST",
  "isBase64Encoded": true,
  "queryStringParameters": {
    "foo": "bar"
  },
  ...
  ...
}
```
- There is  a `body` for the `POST` request that you are going to pass
- `resource` is the non-formatted URL that someone made request to 
- The event is formatted from API gateway. 
- When someone issues an HTTP request to you API Gateway, the Gateway formats this object based on the incoming request and sends it to your lambda as an event.
- There are two different API Gateway events
	- Proxy
		- You can control what the response object looks like
	- Forwarding
- Velocity Template
	- Without API Gateway events we have to go to GUI of AWS API Gateway and determine what the response looks like
- Objects are not what users will be sending. This is just what the API Gateway formats for you 
	- They send the normal request and API Gateway will format it and send it to lambda in form of an object.
## Flow
- API Gateway Proxy Method
	1. Client interact with API Gateway
	2. API Gateway calls our lambda and then we would respond
	3. Response goes back through API Gateway 
	4. API gateway responds back to our client
- In order for API Gateway to respond back to our client in a predictable way, we have to respond back to API Gateway in a predictable way
- They give us a format in which we have to respond back to them 
## Setup API Gateway locally

```
.
└── serverless-with-aws/
    ├── .git/
    ├── node_modules/
    ├── src/
    │   └── api/
    │       ├── todo.js
    │       └── todos.js
    ├── .gitignore
    ├── README.md
    ├── package-lock.json
    ├── package.json
    ├── serverless.yml
    └── yarn.lock
```

1. Update serverless.yaml 
```YAML
service: serverless

provider: 
  name: aws
  runtime: nodejs20.x
  profile: femasters
  region: us-west-1
  stage: dev

plugins:
  - serverless-offline
 
function:
#--------------
# Later after we setup api.js in src/api/api.js
  
# --------------
  todos:
    handler: src/api/todos.handler
  todo:
    handler: src/api/todo.handler
```

2. Create src/api/todo.js
```Js
module.exports.handler = (event, stx, done) => {
	done(null, {
		statusCode: 200,
		body: JSON.stringify({data: {
			id: 1, name: 'clean up', status: 'open'
		}})
	})
}
```

3. Create src/api/todos.js
```JS
module.exports.handler = (event, ctx, done) => {
	const todos = [
		{id: 1, name: 'clean up', status: 'open'},
		{id: 1, name: 'cook', status: 'done'},
	]
	done(null,{
		statusCode: 200,
		body: JSON.stringify({data: todos})
	})
}
```

4. Create a new function, and hook this function up to an HTTP event. 
	- We are going to emulate an API Gateway server on our machine 
	- API Gateway, locally on our machine, is going to execute our lambdas for us, Just like it would do on AWS. 
	- We are now going to go through a local API Gateway, which formats the event like in object on AWS.
/src/api/api.js
```Js 
module.exports.handler = (evt, ctx, done) => {
	done(null, {
		statusCode: 200,
		headers: {},
		body.JSON.stringify({message: 'hello'})
	})
}
```
- These only works with API proxy method (default)
- If proxy method is off you have to go to AWS API Gateway GUI to set them there
	- `statusCode`: Put status code that you want to respond it with
	- `headers` are optional. You put objects of key value pair for headers here.
		- Tells API Gateway which headers you want to send out.
	- `body` This feild holds whatever payload that you want to send .
		- It must be stringified
5. Setting up the event inside serverless.yml
```YAML
...
functions:
  api:
    handler: src/api/api.handler
    events:
      - http: GET /api
...
```
 - `events`:  allows you to pick the type of events that your lambda can subscribe to
- `http: GET /api ` : http event can go a GET request to `/api`
	- If someone does a request to `/api`. It is going to execute this handler for `api` function.
	- It is going to pass the event that AWS gateway formatted as the first argument. Then it waits for response or error.
	- Finally it responds back to client
6. Run it locally
-  We cannot use `sls invoke local -f api` as it will just call the lambda
- OR you can copy the event object from AWS and send that in
- To test the whole flow we have to run it through gateway and send a GET request
- `npm run dev`

```json
//package.json
{
  "dependencies": {
    "express": "^4.16.3",
    "serverless-http": "^1.5.5",
    "serverless-offline": "^3.20.2"
  },
  "scripts": {
    "dev": "sls offline start --httpPort 4500",
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^22.4.3",
    "supertest": "^3.0.0"
  }
}
```

```bash
➜  serverless-with-aws git:(lesson-2) ✗ npm run dev

> dev
> sls offline start --httpPort 4500


Starting Offline at stage dev (us-west-1)

Offline [http for lambda] listening on http://localhost:3002
Function names exposed for local invocation by aws-sdk:
   * api: serverless-dev-api
   * todos: serverless-dev-todos
   * todo: serverless-dev-todo

┌────────────────────────────────────────────────────────────────┐
│                                                                │
│   GET  | http://localhost:4500/dev/api                         │
│   POST |                                                       │
│   http://localhost:4500/2015-03-31/functions/api/invocations   │
│                                                                │
└────────────────────────────────────────────────────────────────┘

Server ready: http://localhost:4500 🚀


GET /dev/api (λ: api)
(λ: api) RequestId: a820246b-58ee-46c5-bcf3-6780cea7dea8  Duration: 119.94 ms  Billed Duration: 120 ms

```

7. visit `http://localhost:4500/dev/api`
	- `{"message":"Hello"}`




