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
service: liau-module-3-12-assignment
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

## Step 5: Deploy and verify that the serverless application is working
Install dependencies with:
```
$ npm install
```

and then deploy with:
```
$ serverless deploy
```

Following is the output from deploy command:
```
Deploying liau-module-3-12-assignment to stage dev (ap-southeast-1)

✔ Service deployed to stack liau-module-3-12-assignment-dev (107s)

endpoint: GET - https://56wd0tljt8.execute-api.ap-southeast-1.amazonaws.com/
functions:
  api: liau-module-3-12-assignment-dev-api (1.1 kB)
```

After succesful deployment the created serverless application can be invoked with curl command:
```
$ curl https://56wd0tljt8.execute-api.ap-southeast-1.amazonaws.com/
```

which resulted in the following response:
```
{
  "message": "Go Serverless v3.0! Your function executed successfully!",
  "input": {
    "version": "2.0",
    "routeKey": "GET /",
    "rawPath": "/",
    "rawQueryString": "",
    "headers": {
      "accept": "*/*",
      "content-length": "0",
      "host": "56wd0tljt8.execute-api.ap-southeast-1.amazonaws.com",
      "user-agent": "curl/7.81.0",
      "x-amzn-trace-id": "Root=1-646c6501-49d8bc074cbec337410a63d0",
      "x-forwarded-for": "118.200.182.45",
      "x-forwarded-port": "443",
      "x-forwarded-proto": "https"
    },
    "requestContext": {
      "accountId": "255945442255",
      "apiId": "56wd0tljt8",
      "domainName": "56wd0tljt8.execute-api.ap-southeast-1.amazonaws.com",
      "domainPrefix": "56wd0tljt8",
      "http": {
        "method": "GET",
        "path": "/",
        "protocol": "HTTP/1.1",
        "sourceIp": "118.200.182.45",
        "userAgent": "curl/7.81.0"
      },
      "requestId": "FXS4OhHjyQ0EP2g=",
      "routeKey": "GET /",
      "stage": "$default",
      "time": "23/May/2023:07:02:25 +0000",
      "timeEpoch": 1684825345180
    },
    "isBase64Encoded": false
  }
}
```

## Step 6: Create CI/CD pipeline with GitHub Actions
Create main.yml in .github/workflows folder
[main.yml](.github/workflows/main.yml)
```yml
name: CICD for Serverless Application
run-name: ${{ github.actor }} is doing CICD for Serverless Application

on:
  push:
    branches:  [ main, "*"]

jobs:
  pre-deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."

  install-dependencies:
    runs-on: ubuntu-latest
    needs: pre-deploy
    steps:
      - name: Check out repository code
        uses: actions/checkout@v3
      - name: Run Installation of Dependencies Commands
        run: npm install

  deploy:
    name: deploy
    runs-on: ubuntu-latest
    needs: install-dependencies
    strategy:
      matrix:
        node-version: [18.x]
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - name: serverless deploy
      uses: serverless/github-action@v3.2
      with:
        args: deploy
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

### Workflow Syntax
**name**: The name of the workflow.

**on**: The type of event that can run the workflow. This workflow will only run when there is a git push to either the main or other branch.

**jobs**: A workflow consists of one or more jobs. Jobs run in parallel unless a ***needs*** keyword is used. Each job runs in a runner environment specified by ***runs-on***.

**steps**: A sequence of tasks to be carried out.

**uses**: Selects an action to run as part of a step in your job. An action is a reusable unit of code.

**with**: A map of input parameters.

**run**: Runs command line programs.

**env**: Set the environment variables.

## Step 7: Add AWS_ACCESS_KEY_ID and ASW_SECRET_ACCESS_KEY to GitHub Secrets