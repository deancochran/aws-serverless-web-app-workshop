# aws-serverless-web-app-workshop
This is a demo of the official aws serverless web app workshop
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
