.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _react-native-host-frontend:

################
Access Your APIs
################


.. meta::
    :description:
        Learn how to use |AMHlong| (|AMH|) to create, build, test and monitor mobile apps that are
        integrated with AWS services.


Publishing Your Frontend to S3 / CloudFront Distribution
========================================================

You can publish your frontend to the S3 / CloudFront Distribution using a simple command

.. code-block:: java

      awsmobile publish

The Hosting and Streaming feature enables you to host code and content in the cloud for your React app.

Mobile Hub creates a container for your content using an `Amazon S3 <http://docs.aws.amazon.com/AmazonS3/latest/dev/>`__ bucket. The content is available publicly on the Internet and you can preview the content directly using a testing URL.

Your content is automatically distributed to a global content delivery network (CDN). `Amazon
CloudFront <https://aws.amazon.com/cloudfront/>`__ also supports media file streaming. To learn more, see `CloudFront Streaming
Tutorials <http://docs.aws.amazon.com/mobile-hub/latest/developerguide/url-cf-dev;Tutorials.html>`__.


Configure a Custom Domain for Your Web App
------------------------------------------

 To use your custom domain for linking to your Web app, use the |R53| service to configure DNS
 routing.

 For a web app hosted in a single location, see `Routing Traffic to a Website that Is Hosted in
 an Amazon S3 Bucket <http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/RoutingToS3Bucket.html>`__.

 For a web app distributed through a global CDN, see `Routing Traffic to an Amazon CloudFront
 Web Distribution by Using Your Domain Name <http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloud-fron-distribution.html>`__
