# Module 3.12 Assignment
3.12 Continuous Deployment - Serverless

## Objective
The objective of this assignment is to set up a continuous integration and continuous deployment (CI/CD) pipeline for a Node.js application that is deployed on a serverless platform.

## Step 1: Create a code repository in GitHub
![image](https://github.com/liaucg/module_3.12_assignment/assets/22501900/df3b539c-e91e-469b-a057-00b7e64028df)

## Step 2: Clone the code repository into local machine
```
$ git clone git@github.com:liaucg/module_3.12_assignment.git
```

## Step 3: Create index.js file
![image](https://github.com/liaucg/module_3.12_assignment/assets/22501900/b69f8910-5fd1-43d6-80b0-b1bb01610604)

[index.js](index.js)
```js
module.exports.handler = async (event) => {
    return {
      statusCode: 200,
      body: JSON.stringify(
        {
          message: "Go Serverless v3.0! Your function executed successfully!",
          input: event,
        },
        null,
        2
      ),
    };
  };
```

## Step 4: Create serverless.yml
![image](https://github.com/liaucg/module_3.12_assignment/assets/22501900/3b2e966d-d56b-42f3-aefa-5e1c34dce406)

[serverless.yml](serverless.yml)
```yml
service: liau_module_3-12-assignment
frameworkVersion: '3'

provider:
  name: aws
  runtime: nodejs18.x
  region: ap-southeast-1

functions:
  api:
    handler: index.handler
    events:
      - httpApi:
          path: /
          method: get

plugins:
  - serverless-offline
```
