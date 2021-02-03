--------

The AWS Mobile SDK for Unity is now included in the AWS SDK for \.NET\. This guide references the archived version of the Mobile SDK for Unity\.

--------

# Set Up the AWS Mobile SDK for Unity<a name="setup-unity"></a>

To get started with the AWS Mobile SDK for Unity, you can set up the SDK and start building a new project, or you can integrate the SDK with an existing project\. You can also clone and run the [samples](https://github.com/awslabs/aws-sdk-unity-samples) to get a sense of how the SDK works\.

## Prerequisites<a name="prerequisites"></a>

Before you can use the AWS Mobile SDK for Unity, you will need the following:
+  [An AWS Account](https://aws.amazon.com/) 
+ Unity version 4\.x or 5\.x \(Unity 4\.6\.4p4 or Unity 5\.0\.1p3 is required if you want to write applications that run on iOS 64\-bit\)

After completing the prerequisites, you will need to do the following to get started:

1. Download the AWS Mobile SDK for Unity\.

1. Configure the AWS Mobile SDK for Unity\.

1. Obtain AWS credentials using Amazon Cognito\.

## Step 1: Download the AWS Mobile SDK for Unity<a name="step-1-download-the-aws-mobile-sdk-for-unity"></a>

First, [download the AWS Mobile SDK for Unity](http://sdk-for-net.amazonwebservices.com/latest/aws-sdk-unity.zip)\. Each package in the SDK is required to consume the corresponding AWS service based on the package’s name\. For example, the aws\-unity\-sdk\-dynamodb\-2\.1\.0\.0\.unitypackage package is used to call the AWS DynamoDB service\. You can import all of the packages or just the ones you intend to use\.

1. Open the Unity editor and create a new empty project, use the default settings\.

1. Select **Assets** > **Import Package** > **Custom Package**\.

1. In the **Import** package dialog, navigate to and select the \.unitypackage files you want to use\.

1. In the **Importing** package dialog, ensure all items are selected and click **Import**\.

## Step 2: Configure the AWS Mobile SDK for Unity<a name="step-2-configure-the-aws-mobile-sdk-for-unity"></a>

### Create a Scene<a name="create-a-scene"></a>

When working with the AWS Mobile SDK for Unity, you can get started by including the following line of code in the `Start` or `Awake` method of your mono behavior class:

```
UnityInitializer.AttachToGameObject(this.gameObject);
```

Create your scene by choosing **New Scene** from the **File** menu\.

The AWS SDK for Unity contains client classes for each AWS service it supports\. These clients are configured using a file named **awsconfig\.xml**\. The following section describes the most commonly used settings in the **awsconfig\.xml** file\. For more information about these settings, see the [Unity SDK API Reference](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/Index.html)\.

### Set the Default AWS Service Region<a name="set-the-default-aws-service-region"></a>

To configure the default region for all service clients:

```
<aws region="us-west-2" />
```

This sets the default region for all the service clients in the Unity SDK\. This setting can be overridden by explicitly specifying the region at the time of creating an instance of the service client, like so:

```
IAmazonS3 s3Client = new AmazonS3Client(<credentials>,RegionEndpoint.USEast1);
```

### Set Logging Information<a name="set-logging-information"></a>

Logging settings are specified as follows:

```
<logging logTo="UnityLogger"
         logResponses="Always"
         logMetrics="true"
         logMetricsFormat="JSON" />
```

This setting is used to configure logging in Unity\. When you log to `UnityLogger`, the framework internally prints the output to the Debug Logs\. If you want to log HTTP responses, set the logResponses flag \- the values can be Always, Never, or OnError\. You can also log performance metrics for HTTP requests using the logMetrics property, the log format can be specified using LogMetricsFormat property, valid values are JSON or standard\.

The following example shows the most commonly used settings in the awsconfig\.xml file\. For more information about specific service settings, see the service section below:

```
<?xml version="1.0" encoding="utf-8"?>
<aws region="us-west-2"
    <logging logTo="UnityLogger"
             logResponses="Always"
             logMetrics="true"
             logMetricsFormat="JSON" />
/>
```

### Working with the link\.xml file<a name="working-with-the-link-xml-file"></a>

The SDK uses reflection for platform\-specific components\. If you are using the IL2CPP scripting backend, `strip bytecode` is always enabled on iOS, so you need to have a `link.xml` file in your assembly root with the following entries:

```
<linker>
<!-- if you are using AWSConfigs.HttpClient.UnityWebRequest option-->
<assembly fullname="UnityEngine">
    <type fullname="UnityEngine.Networking.UnityWebRequest" preserve="all" />
    <type fullname="UnityEngine.Networking.UploadHandlerRaw" preserve="all" />
    <type fullname="UnityEngine.Networking.UploadHandler" preserve="all" />
    <type fullname="UnityEngine.Networking.DownloadHandler" preserve="all" />
    <type fullname="UnityEngine.Networking.DownloadHandlerBuffer" preserve="all" />
</assembly>
<assembly fullname="mscorlib">
    <namespace fullname="System.Security.Cryptography" preserve="all"/>
</assembly>
<assembly fullname="System">
    <namespace fullname="System.Security.Cryptography" preserve="all"/>
</assembly>
<assembly fullname="AWSSDK.Core" preserve="all"/>
<assembly fullname="AWSSDK.CognitoIdentity" preserve="all"/>
<assembly fullname="AWSSDK.SecurityToken" preserve="all"/>
add more services that you need here...
</linker>
```

## Step 3: Obtain the Identity Pool ID using Amazon Cognito<a name="step-3-obtain-the-identity-pool-id-using-amazon-cognito"></a>

To use AWS services in your mobile application, you must obtain the Identity Pool ID using Amazon Cognito Identity\. Using Amazon Cognito to obtain the Identity Pool ID allows your app to access AWS services without having to embed your private credentials in your application\. This also allows you to set permissions to control which AWS services your users have access to\.

To get started with Amazon Cognito, you must create an identity pool\. An identity pool is a store of user identity data specific to your account\. Every identity pool has configurable IAM roles that allow you to specify which AWS services your application’s users can access\. Typically, a developer will use one identity pool per application\. For more information on identity pools, see the [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html)\.

To create an identity pool for your application:

1. Log in to the [Amazon Cognito Console](https://console.aws.amazon.com/cognito/home) and click **Create new identity pool**\.

1. Enter a name for your Identity Pool and check the checkbox to enable access to unauthenticated identities\. Click **Create Pool** to create your identity pool\.

1. Click **Allow** to create the two default roles associated with your identity pool–one for unauthenticated users and one for authenticated users\. These default roles provide your identity pool access to Cognito Sync and Mobile Analytics\.

The next page displays code that creates a credentials provider so you can easily integrate Cognito Identity with your Unity application\. You pass the credentials provider object to the constructor of the AWS client you are using\. The code looks like this:

```
CognitoAWSCredentials credentials = new CognitoAWSCredentials (
    "IDENTITY_POOL_ID", // Identity Pool ID
    RegionEndpoint.USEast1 // Region
);
```

## Next Steps<a name="next-steps"></a>
+  **Get Started**: Read [Getting Started with the AWS Mobile SDK for Unity](getting-started-unity.md) to get a more detailed overview of the services included in the SDK\.
+  **Run the demos**: View our [sample Unity applications](https://github.com/awslabs/aws-sdk-unity-samples) that demonstrate common use cases\. To run the sample apps, set up the SDK for Unity as described above, and then follow the instructions contained in the README files of the individual samples\.
+  **Read the API Reference**: View the [API Reference](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/Index.html) for the AWS Mobile SDK for Unity\.
+  **Ask questions**: Post questions on the [AWS Mobile SDK Forums](https://forums.aws.amazon.com/forum.jspa?forumID=88) or [open an issue on Github](https://github.com/aws/aws-sdk-unity/issues)\.