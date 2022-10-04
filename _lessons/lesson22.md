---
title: Lab 10 (ch10)
tag: Full Stack
layout: project
points: 100
---

## Overview

In this lab we will build on the previous lab and create a simple REST API that
is backed by an AWS Database. This will allow us to create a simple
[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
application.

## Task 1 - Using Lambda with API Gateway

You will use Amazon API Gateway to create a REST API and a resource
(`DynamoDBManager`). You will define one method (`POST`) on the resource, and
create a Lambda function (`LambdaFunctionOverHttps`) that backs the `POST`
method. That way, when you call the API through an HTTPS endpoint, API Gateway
invokes the Lambda function.

The `POST` method that you define on the `DynamoDBManager` resource supports the
following Amazon DynamoDB operations:

- Create, update, and delete an item.
- Read an item.
- Scan an item.
- Other operations (echo, ping), not related to DynamoDB, that you can use for
  testing.

### Create an execution policy

Create an execution role. This AWS Identity and Access Management (IAM) role
uses a custom policy to give your Lambda function permission to access the
required AWS resources. Note that you must first create the policy and then
create the execution role.

1. Open the [Policies page](https://console.aws.amazon.com/iam/home#/policies)
   of the IAM console.
2. Choose **Create Policy**.
3. Choose the **JSON** tab, and then paste the following custom policy into the JSON editor.
```json
{
    "Version": "2012-10-17",
    "Statement": [
    {
        "Sid": "Stmt1428341300017",
        "Action": [
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Query",
        "dynamodb:Scan",
        "dynamodb:UpdateItem"
        ],
        "Effect": "Allow",
        "Resource": "*"
    },
    {
        "Sid": "",
        "Resource": "*",
        "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
        ],
        "Effect": "Allow"
    }
    ]
}
```

1. Choose **Next: Tags**.
2. Choose **Next: Review**.
3. Under **Review policy**, for the policy **Name**, enter **lambda-apigateway-policy**.
4. Choose **Create policy**.

This policy includes permissions for your function to access DynamoDB and Amazon
CloudWatch Logs.

### Create an execution role

1. Open the [Roles page](https://console.aws.amazon.com/iam/home#/roles) of the
   IAM console.
2. Choose **Create role**.
3. For the type of trusted entity, choose **AWS service**.
4. For the use case, choose **Lambda**.
5. Choose **Next: Permissions**.
6. In the policy search box, enter **lambda-apigateway-policy**.
7. In the search results, select the policy that you created
   (`lambda-apigateway-policy`), and then choose **Next**.
8. Under **Role Details**, for the **Role name**, enter **lambda-apigateway-role**.
9. Choose **Create role**
10. On the **Roles** page, choose the name of your role
    (`lambda-apigateway-role`).

## Task 2 - Create the Function

The following code example receives an API Gateway event input and processes the
messages that this input contains. For illustration, the code writes some of the
incoming event data to CloudWatch Logs. Create a new lambda function named
`LambdaFunctionOverHttps` with the following code:

```javascript
console.log('Loading function');

var AWS = require('aws-sdk');
var dynamo = new AWS.DynamoDB.DocumentClient();

/**
 * Provide an event that contains the following keys:
 *
 *   - operation: one of the operations in the switch statement below
 *   - tableName: required for operations that interact with DynamoDB
 *   - payload: a parameter to pass to the operation being performed
 */
exports.handler = function(event, context, callback) {

    var operation = event.operation;

    if (event.tableName) {
        event.payload.TableName = event.tableName;
    }

    switch (operation) {
        case 'create':
            dynamo.put(event.payload, callback);
            break;
        case 'read':
            dynamo.get(event.payload, callback);
            break;
        case 'update':
            dynamo.update(event.payload, callback);
            break;
        case 'delete':
            dynamo.delete(event.payload, callback);
            break;
        case 'list':
            dynamo.scan(event.payload, callback);
            break;
        case 'echo':
            callback(null, "Success");
            break;
        case 'ping':
            callback(null, "pong");
            break;
        default:
            callback(`Unknown operation: ${operation}`);
    }
};
```

### Test the function

Create a new test event with sample event data to test your function.
You can invoke the function using the Lambda console.

```json
{
    "operation": "echo",
    "payload": {
        "somekey1": "somevalue1",
        "somekey2": "somevalue2"
    }
}
```

### Update the functions execution role

You will need to update the lambdas execution role to the
`lambda-apigateway-role` role.

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Choose the `LambdaFunctionOverHttps` function that you created
3. Select Configuration
4. **Edit** the Execution role
5. Set the function to use an **Existing role** `lambda-apigateway-role`
6. Click **Save**

## Task 3 - REST API

In this section, you create an API Gateway REST API (`DynamoDBOperations`) with
one resource (`DynamoDBManager`) and one method (`POST`). You associate the
`POST` method with your Lambda function. Then, you test the setup.

When your API method receives an HTTP request, API Gateway invokes your Lambda
function.

### Create the API

In the following steps, you create the `DynamoDBOperations` REST API using the
API Gateway console.

1. Open the [API Gateway console](https://console.aws.amazon.com/apigateway)\.
2. In the **REST API** box, choose **Build**\.
3. Under **Create new API**, choose **New API**\.
4. Under **Settings**, do the following:
   1. For **API name**, enter **DynamoDBOperations**\.
   2. For **Endpoint Type**, choose **Regional**\.
5. Choose **Create API**\.

### Create a resource in the API

In the following steps, you create a resource named `DynamoDBManager` in your
REST API.

1. In the [API Gateway console](https://console.aws.amazon.com/apigateway), in
   the **Resources** tree of your API, make sure that the root \(`/`\) level is
   highlighted\. Then, choose **Actions**, **Create Resource**\.
2. Under **New child resource**, do the following:
   1. For **Resource Name**, enter **DynamoDBManager**\.
   2. Keep **Resource Path** set to `/dynamodbmanager`\.
3. Choose **Create Resource**\.

### Create a POST method on the resource

In the following steps, you create a `POST` method on the `DynamoDBManager`
resource that you created in the previous section.

1. In the [API Gateway console](https://console.aws.amazon.com/apigateway), in
   the **Resources** tree of your API, make sure that `/dynamodbmanager` is
   highlighted. Then, choose **Actions**, **Create Method**.
2. In the small dropdown menu that appears under `/dynamodbmanager`, choose `POST`, and then choose the check mark icon\.
3. In the method's **Setup** pane, do the following:
   1. For **Integration type**, choose **Lambda Function**\.
   2. For **Lambda Region**, choose the same AWS Region as your Lambda function\.
   3. For **Lambda Function**, enter the name of your function \(**LambdaFunctionOverHttps**\)\.
   4. Select **Use Default Timeout**\.
   5. Choose **Save**\.
4. In the **Add Permission to Lambda Function** dialog box, choose **OK**\.

## Task 4 - Create a DynamoDB table

Create the DynamoDB table that your Lambda function uses.

1. Open the [Tables page](https://console.aws.amazon.com/dynamodbv2#tables) of the DynamoDB console.
2. Choose **Create table**.
3. Under **Table details**, do the following:
   1. For **Table name**, enter **lambda-apigateway**.
   2. For **Partition key**, enter **id**, and keep the data type set as **String**.
4. Under **Settings**, keep the **Default settings**.
5. Choose **Create table**.

## Task 5 - Test the setup

You're now ready to test the setup. You can send requests to your `POST` method
directly from the API Gateway console. In this step, you use a `create`
operation followed by an `update` operation.

Your Lambda function can use the `create` operation to create an item in your
DynamoDB table.

1. In the [API Gateway console](https://console.aws.amazon.com/apigateway),
   choose the name of your REST API (`DynamoDBOperations`).
2. In the **Resources** tree, under `/dynamodbmanager`, choose your `POST`
   method.
3. In the **Method Execution** pane, in the **Client** box, choose **Test**.
4. In the **Method Test** pane, keep **Query Strings** and **Headers** empty.
   For **Request Body**, paste the following JSON:
   ```json
   {
     "operation": "create",
     "tableName": "lambda-apigateway",
     "payload": {
       "Item": {
         "id": "1234ABCD",
         "number": 5
       }
     }
   }
   ```

5. Choose **Test**\.

The test results should show status `200`, indicating that the `create`
operation was successful. To confirm, you can check that your DynamoDB table
now contains an item with `"id": "1234ABCD"` and `"number": "5"`.

### Test the update

You can also update items in the table using the `update` operation.

1. In the [API Gateway console](https://console.aws.amazon.com/apigateway), return to your POST method's **Method Test** pane\.
2. In the **Method Test** pane, keep **Query Strings** and **Headers** empty\. In **Request Body**, paste the following JSON:
   ```json
   {
       "operation": "update",
       "tableName": "lambda-apigateway",
       "payload": {
           "Key": {
               "id": "1234ABCD"
           },
           "AttributeUpdates": {
               "number": {
                   "Value": 10
               }
           }
       }
   }
   ```
3. Choose **Test**\.

The test results should show status `200`, indicating that the `update`
operation was successful. To confirm, you can check that your DynamoDB table
now contains an updated item with `"id": "1234ABCD"` and `"number": "10"`.

## Clean up your resources

This section is optional. You may want to keep all the AWS resources active
to use in your next project.

### Delete the Lambda

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Select the function that you created.
3. Choose **Actions**, then choose **Delete**.
4. Choose **Delete**.

### Delete the execution Role

1. Open the [Roles page](https://console.aws.amazon.com/iam/home#/roles) of the IAM console.
2. Select the execution role that you created.
3. Choose **Delete role**.
4. Choose **Yes, delete**.

### Delete the API

1. Open the [APIs page](https://console.aws.amazon.com/apigateway/main/apis) of the API Gateway console.
2. Select the API you created.
3. Choose **Actions**, **Delete**.
4. Choose **Delete**.

### Delete the DynamoDB table

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home#tables:) of the DynamoDB console.
2. Select the table you created.
3. Choose **Delete**.
4. Enter **delete** in the text box.
5. Choose **Delete**.
