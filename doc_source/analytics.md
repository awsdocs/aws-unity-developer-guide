--------

The AWS Mobile SDK for Unity is now included in the AWS SDK for \.NET\. This guide references the archived version of the Mobile SDK for Unity\.

--------

# Amazon Mobile Analytics<a name="analytics"></a>

Using Amazon Mobile Analytics, you can track customer behaviors, aggregate metrics, generate data visualizations, and identify meaningful patterns\. For information about Mobile Analytics see [AWS Mobile Analytics](https://aws.amazon.com/mobileanalytics/)\.

## Integrating Amazon Mobile Analytics<a name="integrating-amazon-mobile-analytics"></a>

The sections below explain how to integrate Mobile Analytics with your app\.

### Create an App in the Mobile Analytics Console<a name="create-an-app-in-the-mobile-analytics-console"></a>

Go to the [Amazon Mobile Analytics console](https://docs.aws.amazon.com/mobileanalytics/latest/ug/migrate-console.html) and create an app\. Note the `appId` value, as you’ll need it later\.

**Note**  
To learn more about working in the console, see the [Amazon Mobile Analytics User Guide](https://docs.aws.amazon.com/mobileanalytics/latest/ug/)\.

When you are creating an App in the Mobile Analytics Console you will need to specify a Cognito Identity Pool ID\. To create a new Cognito Identity Pool and generate an ID, see [Cognito Identity Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html)\.

### Integrate Mobile Analytics into Your App<a name="integrate-mobile-analytics-into-your-app"></a>

To access Mobile Analytics from Unity you will need the following using statements:

```
using Amazon.MobileAnalytics.MobileAnalyticsManager;
using Amazon.CognitoIdentity;
```

Best practice is to use Amazon Cognito to provide temporary AWS credentials to your application\. These credentials let the app access your AWS resources\. To create a credentials provider, follow the instructions at [Amazon Cognito Identity](cognito-identity.md)\.

Instantiate a MobileAnalyticsManager instance with the following information:
+ cognitoIdentityPoolId \- The ID of the Cognito Identity Pool for your app
+ cognitoRegion \- The region for your Cognito Identity Pool, for example “RegionEndpoint\.USEast1”
+ region \- The region for the Mobile Analytics service, for example “RegionEndpoint\.USEast1”
+ appId \- The value generated by the Mobile Analytics Console when you add an app

Use the MobileAnalyticsClientContextConfig to initialize an ** `MobileAnalyticsManager` ** instance as shown in the following code snippet:

```
// Initialize the MobileAnalyticsManager
void Start()
{
    // ...
    analyticsManager = MobileAnalyticsManager.GetOrCreateInstance(
        new CognitoAWSCredentials(<cognitoIdentityPoolId>, <cognitoRegion>),
        <region>,
        <appId>);
    // ...
}
```

**Note**  
The app ID is generated for you during the app creation wizard\. Both of these values must match those in the Mobile Analytics Console\.

The `appId` is used to group your data in the Mobile Analytics console\. To find your app ID after creating the app in the Mobile Analytics console, browse to the Mobile Analytics Console, click the gear icon in the upper right\-hand corner of the screen\. This will display the App Management page which lists all registered apps and their app IDs\.

### Record Monetization Events<a name="record-monetization-events"></a>

The SDK for Unity provides the `MonetizationEvent` class, which enables you generate monetization events to track purchases made within mobile applications\. The following code snippet shows how to create a monetization event:

```
// Create the monetization event object
MonetizationEvent monetizationEvent = new MonetizationEvent();

// Set the details of the monetization event
monetizationEvent.Quantity = 3.0;
monetizationEvent.ItemPrice = 1.99;
monetizationEvent.ProductId = "ProductId123";
monetizationEvent.ItemPriceFormatted = "$1.99";
monetizationEvent.Store = "Your-App-Store";
monetizationEvent.TransactionId = "TransactionId123";
monetizationEvent.Currency = "USD";

// Record the monetiziation event
analyticsManager.RecordEvent(monetizationEvent);
```

### Record Custom Events<a name="record-custom-events"></a>

Mobile Analytics allows you to define custom events\. Custom events are defined entirely by you; they help you track user actions specific to your app or game\. For more information about Custom events see [Custom\-Events](https://aws.amazon.com/mobileanalytics/faqs/#custom-event-details)\. For this example, let’s say your app is a game, and you want to record an event when a user completes a level\. Create a “LevelComplete” event by creating a new `AmazonMobileAnalyticsEvent` instance:

```
CustomEvent customEvent = new CustomEvent("LevelComplete");

// Add attributes
customEvent.AddAttribute("LevelName","Level1");
customEvent.AddAttribute("CharacterClass","Warrior");
customEvent.AddAttribute("Successful","True");

// Add metrics
customEvent.AddMetric("Score",12345);
customEvent.AddMetric("TimeInLevel",64);

// Record the event
analyticsManager.RecordEvent(customEvent);
```

### Recording Sessions<a name="recording-sessions"></a>

When the application loses focus you can pause the session\. In `OnApplicationFocus` check to see if the app is being paused\. If so call `PauseSession` otherwise call `ResumeSession` as shown in the following code snippet:

```
void OnApplicationFocus(bool focus)
{
    if(focus)
    {
        analyticsManager.ResumeSession();
    }
    else
    {
        analyticsManager.PauseSession();
    }
}
```

By default, if the user switches focus away from the app for less than 5 seconds, and switches back to the app the session will be resumed\. If the user switches focus away from the app for 5 seconds or longer, a new session will be created\. This setting is configurable in the awsconfig\.xml file\. For more information, refer to the Configuring Mobile Analytics section of [Getting Started with the AWS Mobile SDK for Unity](getting-started-unity.md)\.