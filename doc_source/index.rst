.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. highlight:: csharp


########################
AWS Mobile SDK for Unity
########################

Welcome to the AWS Mobile SDK for Unity Developer Guide. `Unity <http://unity3d.com/>`_ is a popular
cross-platform game development environment. It allows you to write your code once and build your
game for many different devices including iPhone, iPad, and Android. The AWS Unity SDK contains a
set of classes that enables games written with Unity to call AWS services.

Supported AWS services include:

* `Amazon Cognito`_
* `Amazon DynamoDB`_
* `AWS Identity and Access Management`_
* `Amazon Kinesis Streams`_
* `AWS Lambda`_
* `Amazon Mobile Analytics`_
* `Amazon Simple Email Service`_
* `Amazon Simple Notification Service`_
* `Amazon Simple Queue Service`_
* `Amazon Simple Storage Service`_

For more information, see `GitHub <https://github.com/aws/aws-sdk-net>`_.

This guide will help you start developing Unity applications using Amazon Web Services. If you're
new to the AWS Mobile SDK, you'll probably want to look first at :doc:`what-is-unity-plugin` and
:doc:`setup-unity`. These topics explain what the SDK includes and how to set up the SDK in a Unity
development environment.

After you've set up the SDK, you can start working with the mobile clients for Amazon Web Services.
Additional topics in this guide include:

.. toctree::
   :maxdepth: 1

   what-is-unity-plugin
   setup-unity
   getting-started-unity
   cognito-identity
   cognito-sync
   analytics
   s3
   dynamodb
   sns
   lambda
   troubleshooting-unity

Download the AWS Mobile SDK for Unity
=====================================

`Download the AWS Mobile SDK for Unity v3
<http://sdk-for-net.amazonwebservices.com/latest/aws-sdk-unity.zip>`_

The AWS Mobile SDK for Unity v3 is compatible with Unity versions 4.6 and above, and 5.0 and above.

The AWS Mobile SDK for Unity is part of the AWS SDK for .NET. For more information, see the blog
post entitled: `AWS SDK for Unity Is Now Part of AWS SDK for .NET
<http://mobile.awsblog.com/post/Tx1K3R5975IFX8/AWS-SDK-for-Unity-Is-Now-Part-of-AWS-SDK-for-NET>`_.

If you still need v2 of the AWS Mobile SDK for Unity, you can check out the source from the
`aws-sdk-net-v2 branch <https://github.com/aws/aws-sdk-net/tree/aws-sdk-net-v2>`_.

Additional Resources
====================

- **API Reference**: The SDK reference documentation includes the ability to browse and search code
  included with the SDK. It provides thorough documentation and usage examples. You can find it at
  the `AWS SDK for Unity API Reference
  <http://docs.aws.amazon.com/sdkfornet/v3/apidocs/Index.html>`_.

- **Questions & Feedback**: Post questions and feedback at the `AWS Mobile Developer Forums
  <https://forums.aws.amazon.com/forum.jspa?forumID=88>`_.

- **Samples**: Samples can be found at the AWS Mobile SDK for Unity `Samples repository
  <https://github.com/awslabs/aws-sdk-unity-samples>`_.

- **Source Code**: Source code is available at the `AWS SDK for Unity repository
  <https://github.com/aws/aws-sdk-net/>`_. Source code for Amazon Cognito is available at the
  `Amazon Cognito repository <https://github.com/aws/amazon-cognito-unity>`_.

For more information about the AWS Mobile SDK, including a complete list of supported AWS products,
see the `AWS Mobile SDK product page <http://aws.amazon.com/mobile/sdk>`_.

.. _Amazon Cognito: http://aws.amazon.com/cognito
.. _Amazon DynamoDB: http://aws.amazon.com/dynamodb/
.. _AWS Identity and Access Management: http://aws.amazon.com/iam
.. _Amazon Kinesis Streams: https://aws.amazon.com/kinesis/streams/
.. _AWS Lambda: http://aws.amazon.com/lambda/
.. _Amazon Mobile Analytics: http://aws.amazon.com/mobileanalytics/
.. _Amazon Simple Email Service: https://aws.amazon.com/ses/
.. _Amazon Simple Notification Service: http://aws.amazon.com/sns/
.. _AWS Console: https://console.aws.amazon.com
.. _Amazon Simple Queue Service: http://aws.amazon.com/sqs/
.. _Amazon Simple Storage Service: http://aws.amazon.com/s3/
