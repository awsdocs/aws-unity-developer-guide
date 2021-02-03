--------

The AWS Mobile SDK for Unity is now included in the AWS SDK for \.NET\. This guide references the archived version of the Mobile SDK for Unity\.

--------

# Amazon Simple Notification Service<a name="sns"></a>

Using Amazon Simple Notification Service \(SNS\) and the Unity SDK, you can write iOS and Android apps that can receive mobile push notifications\. For information about SNS, see [Amazon Simple Notification Service](https://aws.amazon.com/sns/)\.

This topic will walk you through configuring the AWS SDK for Unity sample app, SNSExample\.unity, to receive mobile push notifications through Amazon SNS\.

You can create both iOS and Android apps using the SNSExample\.unity sample\. The configuration steps are different between iOS and Android please read the appropriate section below for the platform you are targeting\.

## Prerequisites<a name="prerequisites"></a>

The following prerequisites are required for using this solution\.

### Set Permissions for SNS<a name="set-permissions-for-sns"></a>

When you create a Cognito Identity Pool two IAM roles are generated:
+ Cognito/\_<Identity\-Pool\-Name>Auth\_DefaultRole \- the default IAM role for authenticated users
+ Cognito/\_<Identity\-Pool\-Name>Unauth\_DefaultRole \- the default IAM role for unauthenticated users

You must add permissions to access the Amazon SNS service to these roles\. To do this:

1. Browse to the [IAM Console](https://console.aws.amazon.com/iam/home) and select the IAM role to configure\.

1. Click **Attach Policy**, select the AmazonSNSFullAccess policy and click **Attach Policy**\.

**Note**  
Using AmazonSNSFullAccess is not recommended in a production environment, we use it here to allow you to get up and running quickly\. For more information about specifying permissions for an IAM role, see [Overview of IAM Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies_permissions.html)\.

### iOS Prerequisites<a name="ios-prerequisites"></a>
+ Membership in the Apple iOS Developer Program
+ Generate a signing identity
+ Create a provisioning profile configured for push notifications

You will need to run your app on a physical device to receive push notifications\. To run your app on a device you must have a membership in the [Apple iOS Developer Program Membership](https://developer.apple.com/programs/ios/)\. Once you have a membership, you can use Xcode to generate a signing identity\. For more information, see Apple’s [App Distribution Quick Start](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013839) documentation\. Next you will need a provisioning profile configured for push notifications for more information, see Apple’s [Configuring Push Notifications](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW1) documentation\.

### Android Prerequisites<a name="android-prerequisites"></a>
+ Install the Android SDK
+ Install the JDK
+ android\-support\-v4\.jar
+ google\-play\-services\.jar

## Configuring the Unity Sample App for iOS<a name="configuring-the-unity-sample-app-for-ios"></a>

Open the Unity editor and create a new project\. Import the AWS SDK for Unity package by selecting **Assets**/**Import Package**/**Custom Package** and selecting aws\-unity\-sdk\-sns\-2\.0\.0\.1\.unitypackage\. Ensure all items in the **Importing Package** dialog are selected and click **Import**\.

### Unity Configuration<a name="unity-configuration"></a>

Perform the following steps to configure the Unity project:

1. In the **Project** pane, navigate to **Assets**/**AWSSDK**/**examples** and open the SNSExample scene\.

1. In the **Hierarchy** pane, select SNSExample\.

1. In the **Inspector** pane specify your Cognito Identity Pool ID\.

1. Notice there is a text box for **iOS Platform Application ARN**, you will generate that information later on\.

1. Select **File**/**Build Settings**, in the **Build Settings** dialog, click the **Add Current** button below the **Scenes in Build** list box to add the current scene to the build\.

1. Under **Platform** select **iOS** and click the **Player Settings…** button, in the **Inspector Pane** of the Unity editor, click the iPhone icon and scroll down to the **Identification** section and specify a **Bundle Identifier**\.

### iOS Configuration<a name="ios-configuration"></a>

Perform the following steps to configure the sample to configure iOS specific settings:

1. In a web browser, go to the [Apple Developer Member Center](https://developer.apple.com/membercenter/index.action), click **Certificates, Identifiers & Profiles**\.

1. Click **Identifiers** under **iOS Apps**, click the plus button in the upper right\-hand corner of the web page to add a new iOS App ID, and enter an App ID description\.

1. Scroll down to the **Add ID Suffix** section and select **Explicit App ID** and enter your bundle identifier\.

1. Scroll down to the **App Services** section and select **Push Notifications**\.

1. Click the **Continue** button\.

1. Click the **Submit** button\.

1. Click the **Done** button\.

1. Select the App ID you just created and then click the **Edit** button\.

1. Scroll down to the **Push Notifications** section\.

1. Click the **Create Certificate** button under **Development SSL Certificate**\.

1. Follow the instructions to create a Certificate Signing Request \(CSR\), upload the request, and download an SSL certificate that will be used to communicate with Apple Notification Service \(APNS\)\.

1. Back in the **Certificates, Identifiers & Profiles** web page, click **All** under **Provisioning Profiles**\.

1. Click the plus button in the upper right\-hand corner to add a new provisioning profile\.

1. Select **iOS App Development** and click the **Continue** button\.

1. Select your App ID and click the **Continue** button\.

1. Select your developer certificate and click the **Continue** button\.

1. Select your device and click the **Continue** button\.

1. Enter a profile name and click the **Generate** button\.

1. Download and double click the provision file to install the provisioning profile\.

You may need to refresh the Provisioning Profiles in Xcode after adding a new one\. In Xcode:

1. Select the **Xcode**/**Preferences** menu item\.

1. Select the **Accounts** tab, select your Apple ID and click **View Details**\.

1. Click the refresh button in the lower left\-hand corner of the dialog to refresh your provisioning profiles and make sure your new profile is displayed\.

### SNS Configuration<a name="sns-configuration"></a>

1. Run the KeyChain access app, select **My Certificates** on the lower left\-hand side of the screen, right click the SSL certificate you generated to connect to APNS and select **Export**, you will be prompted to specify a name for the file and a password to protect the certificate\. The certificate will be saved in a P12 file\.

1. In a web browser go to the [SNS Console](https://console.aws.amazon.com/sns/v2/home) and click **Applications** on the left\-hand side of the screen\.

1. Click **Create platform application** to create a new SNS platform application\.

1. Enter an **Application Name**\.

1. Select **Apple Push Notification Service Sandbox \(APNS\_SANDBOX\)** for **Push notification platform**\.

1. Click **Choose File** and select the P12 file you created when you exported your SSL certificate\.

1. Enter the password you specified when you exported the SSL certificate and click **Load Credentials From File**\.

1. Click **Create platform application**\.

1. Select the Platform Application you just created and copy the Application ARN\.

1. Go back to your project in the Unity Editor, select **SNSExample** in the **Hierarchy** pane, in the **Inspector** pane and paste the Platform Application ARN into the text box labeled **iOS Platform Application ARN**\.

1. Select **File**/**Build Settings** and click the **Build** button this will create an Xcode project\.

### Using Xcode<a name="using-xcode"></a>

1. Open the Xcode project, and select the project in the Project Navigator\.

1. Verify the bundle identifier is set correctly

1. Verify your Apple Developer Account is specified in the **Team** \- this is required for your Provisioning Profile to take effect\.

1. Build the project and run it on your device\.

1. Tap the **Register for Notification**, tap **OK** to allow notifications, the app will display your device token

In the [SNS Console](https://console.aws.amazon.com/sns/v2/home), click **Applications**, select your platform application and click **Create Platform Endpoint**, and enter the device token displayed by the app\.

At this point your app, APNS, and NSN are fully configured\. You can select your platform application, select your endpoint, and click **Publish to endpoint** to send a push notification to your device\.

### Unity Sample \(iOS\)<a name="unity-sample-ios"></a>

The sample creates an CognitoAWSCredentials instance to generate temporary limited\-scope credentials that allows the app to call AWS services\. It also creates an instance of AmazonSimpleNotificationServiceClient to communicate with SNS\. The app displays two buttons labeled **Register for Notification** and **Unregister**\.

When the **Register for Notifications** button is tapped, the `RegisterDevice()` method is called\. `RegisterDevice()` calls `UnityEngine.iOS.NotificationServices.RegisterForNotifications`, which specifies which notification types \(alert, sound, or badge\) will be used\. It also makes an asynchronous call to APNS to get a device token\. Because there is no callback defined, `CheckForDeviceToken` is called repeatedly \(up to 10 times\) to check for the device token\.

When a token is retrieved `AmazonSimpleNotificationServiceClient.CreatePlatformEndpointAsync()` is called to create an endpoint for the SNS platform application\.

The sample is now configured to receive push notifications\. You can browse to the [SNS Console](https://console.aws.amazon.com/sns/v2/home), click **Applications** on the left\-hand side of the page, select your platform application, select an endpoint, and click **Publish to endpoint**\. Select the endpoint to use and click **Publish to Endpoint**\. Type in a text message in the text box and click **Publish message** to publish a message\.

## Configuring the Unity Sample App for Android<a name="configuring-the-unity-sample-app-for-android"></a>

Open the Unity editor and create a new project\. Import the AWS SDK for Unity package by selecting **Assets**/**Import Package**/**Custom Package** and selecting aws\-unity\-sdk\-sns\-2\.0\.0\.1\.unitypackage\. Ensure all items in the **Importing Package** dialog are selected and click **Import**\.

### Unity Configuration<a name="id3"></a>

Perform the following steps to configure the Unity project:

1. In the **Project** pane, navigate to **Assets**/**AWSSDK**/**examples** and open the SNSExample scene\.

1. In the **Hierarchy** pane, select SNSExample\.

1. In the **Inspector** pane specify your Cognito Identity Pool ID\.

1. Notice there is a text box for **Android Platform Application ARN** and **Google Console Project ID**, you will generate that information later on\.

1. Select **File**/**Build Settings**, in the **Build Settings** dialog, click the **Add Current** button below the **Scenes in Build** list box to add the current scene to the build\.

1. Under **Platform** select **Android** and click the **Player Settings…** button, in the **Inspector Pane** of the Unity editor, click the Android icon and scroll down to the **Identification** section and specify a **Bundle Identifier**\.

1. Copy android\-support\-v4\.jar and google\-play\-services\.jar into the **Assets**/**Plugins**/**Android** directory in the **Project** pane\.

For more information about where to find android\-support\-v4\.jar, see [Android Support Library Setup](https://developer.android.com/tools/support-library/setup.html)\. For more information about how to find google\-play\-services\.jar, see [Google APIs for Android Setup](https://developers.google.com/android/guides/setup)\.

### Android Configuration<a name="android-configuration"></a>

First add a new Google API project:

1. In a web browser, go to the [Google Developers Console](https://console.developers.google.com), click **Create Project**\.

1. In the **New Project** box, enter a project name, take note of the project number \(you will need it later\) and click **Create**\.

Next, enable the Google Cloud Messaging \(GCM\) service for your project:

1. In the Google Developers Console, your new project should already be selected, if not, select it in the drop down at the top of the page\.

1. Select **APIs & auth** from the side bar on the left\-hand side of the page\.

1. In the search box, type “Google Cloud Messaging for Android” and click the **Google Cloud Messaging for Android** link below\.

1. Click **Enable API**\.

Finally obtain an API Key:

1. In the Google Developers Console, select **APIs & auth** > **Credentials**\.

1. Under **Public API access**, click **Create new key**\.

1. In the **Create a new key** dialog, click **Server key**\.

1. In the resulting dialog, click **Create** and copy the API key displayed\.

You will use the API key to perform authentication later on\.

### SNS Configuration<a name="id4"></a>

1. In a web browser go to the [SNS Console](https://console.aws.amazon.com/sns/v2/home) and click **Applications** on the left\-hand side of the screen\.

1. Click **Create platform application** to create a new SNS platform application\.

1. Enter an **Application Name** 

1. Select **Google Cloud Messaging \(GCM\)** for **Push notification platform** 

1. Paste the API key into the text box labeled **API key**\.

1. Click **Create platform application** 

1. Select the Platform Application you just created and copy the Application ARN\.

1. Go back to your project in the Unity Editor, select **SNSExample** in the **Hierarchy** pane, in the **Inspector** pane and paste the Platform Application ARN into the text box labeled **Android Platform Application ARN** and your project number into the text box labeled **Google Console Project ID**\.

1. Connect your Android device to your computer, select **File**/**Build Settings** and click the **Build and Run\.** 

### Unity Sample \(Android\)<a name="unity-sample-android"></a>

The sample creates an CognitoAWSCredentials instance to generate temporary limited\-scope credentials that allows the app to call AWS services\. It also creates an instance of AmazonSimpleNotificationServiceClient to communicate with SNS\.

The app displays two buttons labeled **Register for Notification** and **Unregister**\. When the **Register for Notifications** button is tapped, the `RegisterDevice()` method is called\. `RegisterDevice()` calls `GCM.Register`, which registers the app with GCM\. GCM is a class defined within the example code\. It makes an asynchronous call to register the app with GCM\.

When the callback is called `AmazonSimpleNotificationServiceClient.CreatePlatformEndpointAsync` is called to create a platform endpoint to receive SNS messages\.

The sample is now configured to receive push notifications\. You can browse to the [SNS Console](https://console.aws.amazon.com/sns/v2/home), click **Applications** on the left\-hand side of the page, select your platform application, select an endpoint, and click **Publish to endpoint**\. Select the endpoint to use and click **Publish to Endpoint**\. Type in a text message in the text box and click **Publish message** to publish a message\.