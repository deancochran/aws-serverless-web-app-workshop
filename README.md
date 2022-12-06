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

### Steps

- in the aws console open cognito
- Go to Cognito Identity Pools
- Create a new pool (appropriately label the pool)
  - take note of the pool ID
- in the pool's settings open the Client App
  - make a new client app (appropriately label the client app)
  - take note of the client app id

Inside the code repository, and edit the js/config.js file

When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.

# Setting up the application backend

AWS Lambda and Amazon DynamoDB allow us to build a backend process for handling requests for any web application.The already deployed app that was made earlier allows users to 'request a unicorn' to be sent to a location of their choice.To fulfill those requests, and to implement the logic of the program, the JavaScript running in the browser will need to invoke a service running in the cloud.

### Steps

- open amazon dynamodb
- make a dynamodb table (rides)
  - add a primary key (rideid)

- open IAM
- make a new IAM role to allow Lambda to use other services
  - For this IAM Role attach these policies:
    - AWSLambdaBasicExecutionRole
  - Also, create a custom inline policy for your role that allows the ddb:PutItem action for the table you created in the previous section.

- open Lambda
- add the role to the lambda function
- replace any default code with the requestUnicorns.js code and confirm the box

Every Lambda function has an IAM role associated with it. This role defines what other AWS services the function is allowed to interact with. For the purposes of this workshop, you'll need to create an IAM role that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.

Use the IAM console to create a new role. Name it WildRydesLambda and select AWS Lambda for the role type. You'll need to attach policies that grant your function permissions to write to Amazon CloudWatch Logs and put items to your DynamoDB table.

- create a test for the lambda function
- replace any default config with the test.json file
- test the lambda function and verify the results look like

    ```
    {
        "statusCode": 201,
        "body": "{\"RideId\":\"SvLnijIAtg6inAFUBRT+Fg==\",\"Unicorn\":{\"Name\":\"Rocinante\",\"Color\":\"Yellow\",\"Gender\":\"Female\"},\"Eta\":\"30 seconds\"}",
        "headers": {
            "Access-Control-Allow-Origin": "*"
        }
    }

# Deploy a RESTful API 

- allows users to invoke the lambda function from the browser
- use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API. This API will be accessible on the public Internet.
- It will be secured using the Amazon Cognito user pool you created in the previous module.

### Steps 

- go to Amazon API Gateway and make a new public REST API 
- name it WildRydes and configure it to be EdgeOptimized
- go to the API Authorizer settings and add a new authorizer (name it appropriately)
- add 'Authorization' to the Authorizer entry box
- Create the authorizer
- test the authorizer by attempting to register
- test api gateway authorizer by inputting the api token received on the browser
- if the status code is 200 you can continue

- now given that our authorization scheme and lambda function works we need to add resources to the api
- in the resources for the api add a new resource called 'ride'
- use the lambda settings and enable CORS hear  the lambda proxy
