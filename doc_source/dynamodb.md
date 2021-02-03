--------

The AWS Mobile SDK for Unity is now included in the AWS SDK for \.NET\. This guide references the archived version of the Mobile SDK for Unity\.

--------

# Amazon DynamoDB<a name="dynamodb"></a>

 [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) is a fast, highly scalable, highly available, cost\-effective, non\-relational database service\. DynamoDB removes traditional scalability limitations on data storage while maintaining low latency and predictable performance\. For information about DynamoDB, see [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)\.

The AWS Mobile SDK for Unity provides a high\-level library for working with DynamoDB\. You can also make requests directly against the low\-level DynamoDB API, but for most use cases the high\-level library is recommended\. The AmazonDynamoDBClient is an especially useful part of the high\-level library\. Using this class, you can perform various create, read, update, and delete \(CRUD\) operations and execute queries\.

**Note**  
Some of the samples in this document assume the use of a text box variable called ResultText to display trace output\.

## Integrating Amazon DynamoDB<a name="integrating-amazon-dynamodb"></a>

To use DynamoDB in a Unity application, you’ll need to add the Unity SDK into your project\. If you haven’t already done so, [download the SDK for Unity](https://aws.amazon.com/mobile/sdk/) and follow the instructions in [Set Up the AWS Mobile SDK for Unity](setup-unity.md)\. We recommend using Amazon Cognito Identity to provide temporary AWS credentials for your applications\. These credentials allow your app to access AWS services and resources\.

To use DynamoDB in an application, you must set the correct permissions\. The following IAM policy allows the user to delete, get, put, scan, and update items in a specific DynamoDB table, which is identified by [ARN](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html):

```
{
"Statement": [{
    "Effect": "Allow",
    "Action": [
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Scan",
        "dynamodb:UpdateItem"
    ],
    "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/MyTable"
}]
}
```

This policy should be applied to roles assigned to the Cognito identity pool, but you will need to replace the ** `Resource` ** valuewith the correct ARN for your DynamoDB table\. Cognito automatically createsa role for your new identity pool, and you can apply policies to this role at the [IAM console](https://console.aws.amazon.com/iam/)\.

You should add or remove allowed actions based on the needs of your app\. To learn more about IAM policies, see [Using IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\. To learn more about DynamoDB\-specific policies, see [Using IAM to Control Access to DynamoDB Resources](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/UsingIAMWithDDB.html)\.

## Create a DynamoDB Table<a name="create-a-dynamodb-table"></a>

Now that we have our permissions and credentials set up, let’s create a DynamoDB table for our application\. To create a table, go to the [DynamoDB console](https://console.aws.amazon.com/dynamodb/home) and follow these steps:

1. Click **Create Table**\.

1. Enter `Bookstore` as the name of the table\.

1. Select **Hash** as the primary key type\.

1. Select **Number** and enter `id` for the hash attribute name\. Click **Continue**\.

1. Click **Continue** again to skip adding indexes\.

1. Set the read capacity to `10` and the write capacity to `5`\. Click **Continue**\.

1. Enter a notification email and click **Continue** to create throughput alarms\.

1. Click **Create**\. DynamoDB will create your database\.

## Create a DynamoDB Client<a name="create-a-dynamodb-client"></a>

For our app to interact with a DynamoDB table, we need a client\. We can create a default DynamodDB client as follows:

```
var credentials = new CognitoAWSCredentials(IDENTITY_POOL_ID, RegionEndpoint.USEast1);
AmazonDynamoDBClient client = new AmazonDynamoDBClient(credentials);
DynamoDBContext Context = new DynamoDBContext(client);
```

The AmazonDynamoDBClient class is the entry point for the DynamoDB API\. The class provides instance methods for creating, describing, updating, and deleting tables, among other operations\. Context adds a further layer of abstraction over the client and enables you to use additional functionality like the Object Persistance Model\.

## Describe a Table<a name="describe-a-table"></a>

To get a description of our DynamoDB table, we can use the following code:

```
resultText.text +=("\n*** Retrieving table information ***\n");
       var request = new DescribeTableRequest
       {
           TableName = @"ProductCatalog"
       };
       Client.DescribeTableAsync(request, (result) =>
       {
               if (result.Exception != null)
               {
                       resultText.text += result.Exception.Message;
                       Debug.Log(result.Exception);
                       return;
               }
               var response = result.Response;
               TableDescription description = response.Table;
               resultText.text += ("Name: " + description.TableName + "\n");
               resultText.text += ("# of items: " + description.ItemCount + "\n");
               resultText.text += ("Provision Throughput (reads/sec): " +
                   description.ProvisionedThroughput.ReadCapacityUnits + "\n");
               resultText.text += ("Provision Throughput (reads/sec): " +
                   description.ProvisionedThroughput.WriteCapacityUnits + "\n");

       }, null);
   }
```

In this example, we create a client and an DescribeTableRequest object, assign the name of our table to the ** `TableName` ** property, and then pass the request object to the DescribeTableAsync method on the AmazonDynamoDBClient object\. DescribeTableAsync also takes a delegate that will be called when the async operation completes\.

**Note**  
All of the async methods on the AmazonDynamoDBClient take delegates that are called when the async operation completes\.

## Save an Object<a name="save-an-object"></a>

To save an object to DynamoDB, use the SaveAsync<T> method of the AmazonDynamoDBClient object, where T is the type of object you are saving\.

We called our database “Bookstore”, and in keeping with that theme we’ll implement a data model that records book\-related attributes\. Here are the classes that define our data model\.

```
[DynamoDBTable("ProductCatalog")]
    public class Book
    {
        [DynamoDBHashKey]   // Hash key.
        public int Id { get; set; }
        [DynamoDBProperty]
        public string Title { get; set; }
        [DynamoDBProperty]
        public string ISBN { get; set; }
        [DynamoDBProperty("Authors")]    // Multi-valued (set type) attribute.
        public List<string> BookAuthors { get; set; }
    }
```

Of course, for a real bookstore application we’d need additional fields for things like author and price\. The Book class is decorated with the \[DynamoDBTable\] attribute, this defines the database table objects of type Book will be written to\. The key for each instance of the Book class is identified using the \[DynamoDBHashKey\] attribute\. Properties are identified with the \[DynamoDBProperty\] attribute, these specify the column in the database table to which the property will be written\. With the model in place, we can write some methods to create, retrieve, update, and delete Book objects\.

## Create a Book<a name="create-a-book"></a>

```
private void PerformCreateOperation()
{
    Book myBook = new Book
    {
        Id = bookID,
        Title = "object persistence-AWS SDK for.NET SDK-Book 1001",
        ISBN = "111-1111111001",
        BookAuthors = new List<string> { "Author 1", "Author 2" },
    };

    // Save the book.
    Context.SaveAsync(myBook,(result)=>{
        if(result.Exception == null)
            resultText.text += @"book saved";
    });
}
```

## Retrieve a Book<a name="retrieve-a-book"></a>

```
private void RetrieveBook()
{
    this.displayMessage += "\n*** Load book**\n";
    Context.LoadAsync<Book>(bookID,
                             (AmazonDynamoResult<Book> result) =>
    {
        if (result.Exception != null)
        {

            this.displayMessage += ("LoadAsync error" +result.Exception.Message);
            Debug.LogException(result.Exception);
            return;
        }
        _retrievedBook = result.Response;
        this.displayMessage += ("Retrieved Book: " +
                                "\nId=" + _retrievedBook.Id +
                                "\nTitle=" + _retrievedBook.Title +
                                "\nISBN=" + _retrievedBook.ISBN);
        string authors = "";
        foreach(string author in _retrievedBook.BookAuthors)
            authors += author + ",";
        this.displayMessage += "\nBookAuthor= "+ authors;
        this.displayMessage += ("\nDimensions= "+ _retrievedBook.Dimensions.Length + " X " +
                                _retrievedBook.Dimensions.Height + " X " +
                                _retrievedBook.Dimensions.Thickness);

    }, null);
}
```

## Update a Book<a name="update-a-book"></a>

```
private void PerformUpdateOperation()
{
    // Retrieve the book.
    Book bookRetrieved = null;
    Context.LoadAsync<Book>(bookID,(result)=>
    {
        if(result.Exception == null )
        {
            bookRetrieved = result.Result as Book;
            // Update few properties.
            bookRetrieved.ISBN = "222-2222221001";
            // Replace existing authors list with this
            bookRetrieved.BookAuthors = new List<string> { "Author 1", "Author x" };
            Context.SaveAsync<Book>(bookRetrieved,(res)=>
            {
                if(res.Exception == null)
                     resultText.text += ("\nBook updated");
            });
        }
   });
}
```

## Delete a Book<a name="delete-a-book"></a>

```
private void PerformDeleteOperation()
{
    // Delete the book.
    Context.DeleteAsync<Book>(bookID,(res)=>
    {
        if(res.Exception == null)
        {
            Context.LoadAsync<Book>(bookID,(result)=>
            {
                 Book deletedBook = result.Result;
                 if(deletedBook==null)
                     resultText.text += ("\nBook is deleted");
            });
        }
   });
}
```