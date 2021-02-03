--------

The AWS Mobile SDK for Unity is now included in the AWS SDK for \.NET\. This guide references the archived version of the Mobile SDK for Unity\.

--------

# AWS Lambda<a name="lambda"></a>

AWS Lambda is a compute service that runs your code in response to requests or events and automatically manages the compute resources for you, making it easy to build applications that respond quickly to new information\. AWS Lambda functions can be called directly from mobile, IoT, and Web apps and sends a response back synchronously, making it easy to create scalable, secure, and highly available backends for your mobile apps without the need to provision or manage infrastructure\.

AWS Lambda can execute your Lambda functions in response to one of the following:
+ Events, such as discrete updates \(for example, object\-created events in Amazon S3 or CloudWatch alerts\), or streaming updates \(for example, website clickstreams or outputs from connected devices\)\.
+ JSON inputs or HTTPS commands from your custom applications\.

AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second\. With these capabilities, you can use Lambda to easily build triggers for AWS services like Amazon S3 and Amazon DynamoDB, process streaming data stored in Amazon Kinesis, or create your own back\-end that operates at AWS scale, performance, and security\.

To learn more about how AWS Lambda works, see [AWS Lambda: How It Works](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction.html)\.

## Permissions<a name="permissions"></a>

There are two types of permissions related to Lambda functions:
+  **Execution permissions** — The permissions that your Lambda function needs to access other AWS resources in your account\. You grant these permissions by creating an IAM role, known as an execution role\.
+  **Invocation permissions** — The permissions that the event source needs to communicate with your Lambda function\. Depending on the invocation model \(push or pull model\), you can grant these permissions using either the execution role or resource policies \(the access policy associated with your Lambda function\)\.

## Project Setup<a name="project-setup"></a>

### Set Permissions for AWS Lambda<a name="set-permissions-for-aws-lambda"></a>

1. Open the [AWS IAM Console](https://console.aws.amazon.com/iam/home)\.

1. Attach this custom policy to your roles, which allows your application to make calls to AWS Lambda\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "lambda:*"
      ],
      "Resource": "*"
    }
  ]
}
```

### Create a New Execution Role<a name="create-a-new-execution-role"></a>

This role applies to the Lambda function that you will create in the next step and determines which AWS resources that function can access\.

1. Open the [AWS IAM Console](https://console.aws.amazon.com/iam/home)\.

1. Click **Roles**\.

1. Click **Create New Roles**\.

1. Follow the on\-screen instructions to select the services and corresponding policies that your Lambda function will need access to\. For example, if you want your Lambda function to create an S3 bucket, your policy will need write access to S3\.

1. Click **Create Role**\.

### Creating a Function in AWS Lambda<a name="creating-a-function-in-aws-lambda"></a>

1. Open the [AWS Lambda Console](https://console.aws.amazon.com/lambda/home)\.

1. Click **Create a Lambda function**\.

1. Click **Skip** to skip creating a blueprint\.

1. Configure your own function on the next screen\. Enter the function name, a description, and choose your runtime\. Follow the on\-screen instructions based on your chosen runtime\. Specify your execution permissions by assigning your newly created execution role to your function\.

1. When finished, click **Next**\.

1. Click **Create Function**\.

## Create a Lambda Client<a name="create-a-lambda-client"></a>

```
var credentials = new CognitoAWSCredentials(IDENTITY_POOL_ID, RegionEndpoint.USEast1);
var Client = new AmazonLambdaClient(credentials, RegionEndpoint.USEast1);
```

## Create a Request Object<a name="create-a-request-object"></a>

Create a request object to specify the invocation type and the function name:

```
var request = new InvokeRequest()
{
    FunctionName = "hello-world",
    Payload = "{\"key1\" : \"Hello World!\"}",
    InvocationType = InvocationType.RequestResponse
};
```

## Invoke Your Lambda Function<a name="invoke-your-lambda-function"></a>

Call invoke, passing the request object:

```
Client.InvokeAsync(request, (result) =>
{
    if (result.Exception == null)
    {
        Debug.Log(Encoding.ASCII.GetString(result.Response.Payload.ToArray()));
    }
    else
    {
        Debug.LogError(result.Exception);
    }
});
```