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

## Task 2 - Lambda Console

In this task, you will create a Lambda function using the console. The Lambda
console provides a [code
editor](https://github.com/awsdocs/aws-lambda-developer-guide/blob/main/doc_source/foundation-console.md#code-editor)
for non-compiled languages that lets you modify and test code quickly. We will
start off by using the console and then will switch to using the command line
interface (CLI) for more complex functions.

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console
2. Switch to the US West (Oregon) data center ![AWS region]({% link /assets/images/labs/lesson20-region.png %})
3. Choose **Create function**
4. Under **Basic information**, do the following:
   - For **Function name**, use your student email, for example **student01-u-boisestate-edu**
   - For **Runtime**, confirm that **Node.js 16.x** is selected.
   - Change the default execution role
     - Select **Use an existing role**
     - Select **lambda-database-role**
   - Keep all other setting in their default state
5. Choose **Create function**.

![AWS create function]({% link /assets/images/labs/lesson20-create-function.png %})

Lambda creates a Node.js function and an execution role that grants the function
permission to upload logs. The Lambda function assumes the execution role when
you invoke your function, and uses the execution role to create credentials for
the AWS SDK and to read data from event sources.

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

## Task 4 - Developing with Lambda

While a lot of what we need to do can be accomplished with the web interface
that AWS provides we will still need to install the AWS CLI so we can push and
manage code from the command line. Luckily AWS provides detailed instructions
for all the major operating systems. If you can't get AWS installed and working
on your personal machine you will need to do your work in a [Linux virtual
machine](https://docs.google.com/document/d/1qmOEfgJ0d_-mVFotFbRgInRVfINe98rU-Z6TJiiTMck/edit?usp=sharing).
You are not required to use a Linux VM it is only provided if you can't
configure your own machine.

1. [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
2. [Install AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
3. [AWS Toolkit for VSCode](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)
4. [Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)
5. [Generate Your Access Key](https://us-east-1.console.aws.amazon.com/iam/home#/users/student01)

![AWS access key]({% link /assets/images/labs/lesson20-key.png %})

Use the us-west-2 (Oregon) for the default region.

```bash
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

### Update the function

You are now going to update the function that you created in the first task from
the command line. When you start using other AWS services you will need to
upload **lots** of files and dependencies. This is not something that you want
to do by hand as the GUI doesn't provide a good way to mass upload files.

1. Open the `update-lambda.sh` shell script in the starter code. It is located
   in the lambda folder.
2. Replace `PUT_THE_FUNCTION_NAME_HERE` with your function name that you create
   in the previous task.
3. Run the `update-lambda.sh` function.
4. Open the [Functions
   page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda
   console and confirm that your function was update. Make sure and switch to
   to Oregon region. ![AWS region]({% link /assets/images/labs/lesson20-region.png %})

### Test the function

You can test  your Lambda function using the sample event data provided in the
console. This can allow you to quickly verify that everything is working before
you proceed any further.

1. After selecting your function, choose the **Test** tab.
2. In the **Configure test event** section, choose **Create new event**.
3. Give your event a name in the **Even name** field. This can be whatever you
   want.
4. Update the Event JSON to match what is shown below.
5. Save your test event

```json
{
  "requestContext": {
    "http": {
      "method": "GET",
      "path": "/",
      "protocol": "HTTP/1.1",
      "sourceIp": "1.2.3.4",
      "userAgent": "testing"
    },
    "requestId": "asdf",
    "routeKey": "$default",
    "stage": "$default",
    "time": "09/Oct/2022:17:29:58 +0000",
    "timeEpoch": 1665336598126
  },
  "isBase64Encoded": false
}
```

![Test event]({% link assets/images/labs/lesson20-test.png %})

1. Choose **Test**. Each user can create up to 10 test events per function.
   Those test events are not available to other users. Lambda runs your function
   on your behalf. The function handler receives and then processes the sample
   event.
2. Upon successful completion, view the results in the console.

![Test event]({% link assets/images/labs/lesson20-test-results.png %})

### Errors

If you see the output below it is because you didn't update the function name
as required in the `update-lambda.sh` file.

>An error occurred (ResourceNotFoundException) when calling the UpdateFunctionCode operation: Function not found: arn:aws:lambda:us-west-2:064794820934:function:PUT_THE_FUNCTION_NAME_HERE

## Task 5 - Invoke Function from your localhost

We now have a function that we can invoke from our local host! In your starter
code in the client directory there is an `index.html` file. Replace the
**YOUR LAMBDA URL HERE!** with the URL that you create in the previous task.

You now should be able to load data from your AWS Lambda function when using
the live server preview in VSCode. This will NOT work if you just open up the
file in your browser, you **MUST** use the live preview as shown below!

![AWS Load]({% link /assets/images/labs/lesson20-load.gif %})

## It didn't work

If you can't find what you created look through all the AWS regions to see if
you maybe used the wrong region. The AWS console defaults to US East (N.
Virginia) `us-east-1`, make sure and select US West (Oregon).

![AWS region]({% link /assets/images/labs/lesson20-region.png %})

If you have completed everything and your code doesn't work you may have missed
a step. At this point instead of trying to figure out what went wrong it is
easier to just start over from scratch. Follow the steps below and then try
again.

### Delete your Function

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Choose YOUR function.
3. Choose **Actions**, **Delete**.
4. In the **Delete function** dialog box, choose **Delete**.
5. Got back to step one and try again 😊

## Task 6 -  Add Files (5pts)

For this lab you need to check in the completed index.html file that you created
that has the AWS lambda function URL and all the other files that you updated
when configuring your code.

## Task 7 - Flipgrid (35pts)

Most of this lab was just configuring everything, so the majority of your grade
will be you showing off everything working in a video. Once you have everything
ready create a video using flipgrid!

- [How to record your screen](https://help.flip.com/hc/en-us/articles/360045940833-Screen-Recording-How-to-record-your-screen-using-the-Flipgrid-camera)
- [Flipgrid topic]({{site.data.semester-info.flip[page.slug]}})

You need to demo the following:

- Show your completed Lambda function on AWS
- Show your simple web page that invokes the function

## Task 8 - Complete the Retrospective (10pts)

Once you have completed all the tasks open the file **Retrospective.md** and
complete each section with a TODO comment.
