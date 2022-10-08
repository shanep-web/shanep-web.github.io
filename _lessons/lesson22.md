---
title: Lab 10 (ch10) - Database
tag: Full Stack
layout: lab
points: 50
---

## Overview

In this lab we will build on the previous lab and create a simple REST API using
AWS Lambda that is backed by an AWS Database. This will allow us to create a
simple [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
application.

## Task 1 - Create an execution policy

Create an execution role. This AWS Identity and Access Management (IAM) role
uses a custom policy to give your Lambda function permission to access the
required AWS resources. Note that you must first create the policy and then
create the execution role. This policy includes permissions for your function to
access DynamoDB and Amazon CloudWatch Logs.

1. Open the [Policies page](https://console.aws.amazon.com/iam/home#/policies)
   of the IAM console.
2. Choose **Create Policy**.
3. Choose the **JSON** tab, and then paste the following custom policy shown
   below into the JSON editor.
4. Choose **Next: Tags**.
5. Choose **Next: Review**.
6. Under **Review policy**, for the policy **Name**, enter
   **lambda-database-policy**.
7. Choose **Create policy**.
8. You can then verify that the policy was created by filtering by policy name.

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

![AWS email]({% link /assets/images/labs/lesson22-policy-search.png %})

## Task 2 - Create an execution role

Once you have create a new policy you need to assign that policy to a role.

1. Open the [Roles page](https://console.aws.amazon.com/iam/home#/roles) of the
   IAM console.
2. Choose **Create role**.
3. For the type of trusted entity, choose **AWS service**.
4. For the use case, choose **Lambda**.
5. Choose **Next: Permissions**.
6. In the policy search box, enter **lambda-database-policy**.
7. In the search results, select that policy and then choose **Next**.
8. Under **Role Details**, for the **Role name**, enter **lambda-database-role**.
9. Choose **Create role**

## Task 3 - Create a Lambda

You need to create a new lambda function with the proper execution role so you
can access your database.  

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Create a new lambda function named `lambda-database-function`.
3. Change the default execution role to **lambda-database-role**
4. Select **Create Function**
5. Add a **Function URL** with Auth type set to **NONE** and **Configure cross-origin resource sharing (CORS)**

**NOTE:** We are opening up our database without authentication which is
generally a bad thing! In industry you would want to have some sort of
authentication implemented to protect your data. For learning purposes,
authentication is something that we can add after everything is already working
😊.

![AWS email]({% link /assets/images/labs/lesson22-lambda-with-role.png %})

## Task 4 - Create a DynamoDB table

Read about AWS DynamoDB before you continue.

- [DynamoDB Core Components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
- [Create SQL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.WriteData.html)
- [Read SQL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.ReadData.SingleItem.html)
- [Update SQL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.UpdateData.html)
- [Delete SQL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.DeleteData.html)

Create the DynamoDB table that your Lambda function uses.

1. Open the [Tables page](https://console.aws.amazon.com/dynamodbv2#tables) of the DynamoDB console.
2. Choose **Create table**.
3. Under **Table details**, do the following:
   1. For **Table name**, enter **lambda-database**.
   2. For **Partition key**, enter **id**, and keep the data type set as **String**.
4. Under **Settings**, keep the **Default settings**.
5. Choose **Create table**.

Wait for the status of the table to change to **Active** before you move
forward.

![AWS email]({% link /assets/images/labs/lesson22-database.png %})

Now lets write and test our SQL queries in the built in editor so we can have
them all ready for when we write our Lambda function.

1. Open the [PartiQL editor](https://us-east-1.console.aws.amazon.com/dynamodbv2/home?region=us-east-1#partiql-editor)
2. Insert a test object so we can see things working!
3. Play around with the PartiQL editor testing various SQL statements before you continue.

```sql
INSERT INTO "lambda-database" value {'id': 'blog-id', 'body':'blog body'};
```

![AWS dynamo insert]({% link /assets/images/labs/lesson22-dynamo-insert.png %})

Run a simple `SELECT` Query to see your results.

`SELECT * FROM "lambda-database"`

![AWS dynamo select]({% link /assets/images/labs/lesson22-dynamo-select.png %})

## Task 5 - Write your Lambda

Before you start writing code you should read the articles listed below so
you are familiar with the terms we will introduce. We will be using the AWS SDK
to access our DynamoDB instance

- [Invoking Lambda function URLs](https://docs.aws.amazon.com/lambda/latest/dg/urls-invocation.html)
- [Lambda handler](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-handler.html)
- [Write an Item](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.WriteItem.html)
- [Read an Item](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.ReadItem.html)
- [Update an Item](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.UpdateItem.html)
- [Delete an Item](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.UpdateItem.html)

Using the **Code source** editor on AWS update your Lambda to the following:

```javascript

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

## Task 6 - Test the Lambda

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

## Task 4 - Invoke Function from your localhost

In this task you will load the JSON object that your Lambda function is sending
and display it in a web page. Create an `index.html` page with the following
content:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Test AWS Lambda</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="">
</head>

<body>
    <h1>Get Information</h1>
    <p id="lambda-info">
        <!-- loaded with AJAX -->
    </p>
    <button id="load-data">Load Data</button>

    <script>
        document.getElementById("load-data").onclick = function () {
            let lambda = document.getElementById("lambda-info");
            let xhr = new XMLHttpRequest();
            xhr.addEventListener("load", function () {
                lambda.innerHTML = xhr.response;
            });
            xhr.open("GET", "YOUR LAMBDA URL HERE!");
            xhr.send();
        }

    </script>
</body>

</html>
```

## It didn't work

If you have completed everything and your code doesn't work you may have missed
a step. At this point instead of trying to figure out what went wrong it is
easier to just start over from scratch.

This section is optional and only necessary if you are having issues getting
things to work. If everything works you are welcome to keep all your work alive
and use it for your next project or as a reference.

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

### Delete the policy

1. Open the [Policies page](https://console.aws.amazon.com/iam/home#/policies) of the IAM console.
2. Select the policy that you created.
3. Choose **Actions: Delete**
4. Choose **Yes, delete**

### Delete the DynamoDB table

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home#tables:) of the DynamoDB console.
2. Select the table you created.
3. Choose **Delete**.
4. Enter **delete** in the text box.
5. Choose **Delete**.
