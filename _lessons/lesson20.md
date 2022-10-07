---
title: Lab 9 - AWS Lambda
tag: Full Stack
layout: lab
points: 50
---

## Overview

We are going to live on the bleeding edge and learn how to use a serverless
architecture.
[Serverless](https://www.cloudflare.com/learning/serverless/what-is-serverless)
is just a marketing term that means you the developer will be using someone
else's server to run your code. Currently there are two big players in the
serverless market [AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) and
[Azure](https://en.wikipedia.org/wiki/Microsoft_Azure). We will be using AWS
because it is currently the biggest player in the market and has pretty killer
documentation to help us get started.

## Task 1 - AWS Signup

The department has created an AWS account for you with your BSU student email.
You don't have to pay any money or sign up with your personal account. You
should have received an email similar to what you see below with your AWS
credentials and a link to the [sign in
page](https://bsucompsci.signin.aws.amazon.com/console). If you encounter any
issues with logging in with your student email please email your instructor
ASAP!

![AWS email]({% link /assets/images/labs/lesson20-email.png %})

When you sign in the sign in screen should look similar to what is shown below.

![AWS sign in screen]({% link /assets/images/labs/lesson20.png %})

## Task 2 - Hello World

In this task, you will create a Lambda function using the console. The Lambda
console provides a [code
editor](https://github.com/awsdocs/aws-lambda-developer-guide/blob/main/doc_source/foundation-console.md#code-editor)
for non-compiled languages that lets you modify and test code quickly. 

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console
2. Choose **Create function**
3. Under **Basic information**, do the following:
   - For **Function name**, enter **my-function**.
   - For **Runtime**, confirm that **Node.js 16.x** is selected.
   - Keep all other setting in their default state
4. Choose **Create function**.

Lambda creates a Node.js function and an execution role that grants the function
permission to upload logs. The Lambda function assumes the execution role when
you invoke your function, and uses the execution role to create credentials for
the AWS SDK and to read data from event sources.

In the **Code source** section update code listed in **index.js** to what is
shown below and make sure and deploy the changes.

```javascript
const data = {
    "key1": "value1",
    "key2": "value2",
    "key3": "value3",
};

exports.handler = async (event) => {
    const response = {
        statusCode: 200,
        body: JSON.stringify(data),
    };
    return response;
};
```

![Deploy changes]({% link assets/images/labs/lesson20-deploy.png%})

### Test the Lambda function

You can test  your Lambda function using the sample event data provided in the
console. This can allow you to quickly verify that everything is working before
you proceed any further.

1. After selecting your function, choose the **Test** tab.
2. In the **Configure test event** section, choose **Create new event**.
3. Give your event a name in the **Even name** field. This can be whatever you
   want.
4. In **Template**, leave the default **hello-world** option and then save your
   changes.

![Test event]({% link assets/images/labs/lesson20-test.png %})

1. Choose **Test**. Each user can create up to 10 test events per function.
   Those test events are not available to other users. Lambda runs your function
   on your behalf. The function handler receives and then processes the sample
   event.
2. Upon successful completion, view the results in the console.

![Test event]({% link assets/images/labs/lesson20-test.png %})

## Task 3 - Add a Function URL

A function URL is a dedicated HTTP(S) endpoint for your Lambda function. You can
create and configure a function URL through the Lambda console or the Lambda
API. When you create a function URL, Lambda automatically generates a unique URL
endpoint for you. Once you create a function URL, its URL endpoint never
changes. Function URL endpoints have the following format:

`https://<url-id>.lambda-url.<region>.on.aws`

Function URLs are dual stack-enabled, supporting IPv4 and IPv6. After you
configure a function URL for your function, you can invoke your function through
its HTTP(S) endpoint via a web browser, curl, Postman, or any HTTP client.
Lambda function URLs use resource-based policies for security and access
control. Function URLs also support cross-origin resource sharing (CORS)
configuration options.

You can apply function URLs to any function alias, or to the $LATEST unpublished
function version. You can't add a function URL to any other function version.

### To create a function URL for an existing function (console)

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Choose the name of the function that you want to create the function URL for.
3. Choose the **Configuration** tab, and then choose **Function URL**.
4. Choose **Create function URL**
5. For **Auth type**, choose  **NONE**.
6. Check the box to allow **Configure cross-origin resource sharing (CORS)**

![Test event]({% link assets/images/labs/lesson20-url.png %})

For our simple hello world we are not going to worry about authentication at
this point, we can add it later if necessary.

![Test event]({% link assets/images/labs/lesson20-auth-type.png %})

At this point your AWS console should look like the following:

![AWS console]({% link /assets/images/labs/lesson20-aws.png %})

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

You now should be able to load data from your AWS Lambda function when using
the live server preview in VSCode. This will NOT work if you just open up the
file in your browser, you **MUST** use the live preview as shown below!

![AWS Load]({% link /assets/images/labs/lesson20-load.gif %})

## It didn't work

If you have completed everything and your code doesn't work you may have missed
a step. At this point instead of trying to figure out what went wrong it is
easier to just start over from scratch.

### Delete your Function

If you are done working with the example function, delete it. You can also
delete the log group that stores the function's logs, and the execution role
that the console created.

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Choose a function.
3. Choose **Actions**, **Delete**.
4. In the **Delete function** dialog box, choose **Delete**.

### Delete your Logs

1. Open the [Log groups page](https://console.aws.amazon.com/cloudwatch/home#logs:) of the CloudWatch console.
2. Select the function's log group `/aws/lambda/my-function`.
3. Choose **Actions**, **Delete log group\(s\)**.
4. In the **Delete log group\(s\)** dialog box, choose **Delete**.

## Deliverables (35pts)

For this lab you need to check in the completed index.html file that you
created that has the AWS lambda function URL updated.

## Task 5 - Flipgrid (10pts)

Show off your website to the class. Once you have everything ready create a video using flipgrid!

- [How to record your screen](https://help.flip.com/hc/en-us/articles/360045940833-Screen-Recording-How-to-record-your-screen-using-the-Flipgrid-camera)
- [Flipgrid topic]({{site.data.semester-info.flip[page.slug]}})

You need to demo the following:

- Show your completed Lambda function on AWS
- Show your simple web page that invokes the function

## Task 6 - Complete the Retrospective (5pts)

Once you have completed all the tasks open the file **Retrospective.md** and complete each section with a TODO comment.
