# Build a serverless Web Application

Services:

- lambda
- amazon api gateway
- aws amplify
- amazon dynamnodb
- amazon cognito

Summary:

The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

![Architecture](Serverless_Architecture.d930970c77b382db6e0395198aacccd8a27fefb7.png)

Modules
This tutorial is divided into five modules. Each module describes a scenario of what we're going to build and step-by-step directions to help you implement the architecture and verify your work. 

- Host a Static Website (15 minutes): Configure AWS Amplify to host the static resources for your web application with continuous deployment built in
- Manage Users (30 minutes): Create an Amazon Cognito user pool to manage your users' accounts
- Build a Serverless Backend (30 minutes): Build a backend process for handling requests for your web application
- Deploy a RESTful API (15 minutes): Use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API
- Terminate Resources (10 minutes): Terminate all the resources you created throughout this tutorial

# Setting up the enviornment... (git, amplify, cli tools, etc..)

- make a aws account (root account)
- make a admin account (best practice not to use root)
- provide admin access to your admin account through the IAM service on IAM
- generate the admin user access/secret keys
- install aws cli
- run 'aws configure'
- enter the admin credentials and configure the availability zone you want to deploy your application into
- Make a git repository
- clone it locally
- run 'aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ --recursive'
- launch amplify console from the internet
- opt in to host a web app from github
- authorize amplify to access your repository
- select the main/master branch
- leave the default build settings
- deploy

Now... Anytime you publish to github your app is automatically updated

# Setting up the user database (amazon cognito service)

- A user database will ensure users are authenticated, allow for tiered permission levels in your website
  - this can all be done with amazon cognito
- users will register themselves by providing their PII (email, password, username, etc...)
  - you can configure exactly what you want to require from users inside the cognito console.
  - cognito verifies their email remotely
  - once users are authenticated they can sign in

### Steps:

- in the aws console open cognito
- Go to Cognito Identity Pools
- Create a new pool (appropriately label the pool)
  - take note of the pool ID
- in the pool's settings open the Client App
  - make a new client app (appropriately label the client app)
  - take note of the client app id

Inside the code repository, and edit the js/config.js file

When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.
