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
