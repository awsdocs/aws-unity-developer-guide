.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _react-native-access-apis:

################
Access Your APIs
################


.. list-table::
   :widths: 1 6

   * - **BEFORE YOU BEGIN**

     - The steps on this page assume you have already completed the steps on :ref:`Get Started <react-native-getting-started>`.


Set Up Your Backend
===================

The AWS Mobile :ref:`Cloud Logic <cloud-logic>` feature lets you call APIs in the cloud. API calls are handled by your serverless Lambda functions.

**To enable cloud APIs in your app**

.. code-block:: bash

   awsmobile cloud-api enable

   awsmobile push

Enabling Cloud Logic in your app adds a sample API, :code:`sampleCloudApi` to your project that can be used for testing.

You can find the sample handler function for the API by running :code:`awsmobile console` in your app root folder, and then choosing the :guilabel:`Cloud Logic` feature in your |AMH| project.

.. image:: images/web-view-cloud-api.png
   :scale: 100
   :alt: View your sample cloud API and its lambda function handler.

Quickly Test Your API From the CLI
----------------------------------

The :code:`sampleCloudApi` and its handler function allow you to make end to end API calls.

**To test invocation of your unsigned APIs in the development environment**

.. code-block:: bash

   awsmobile cloud-api invoke <apiname> <method> <path> [init]

For the :code:`sampleCloudApi` you may use the following examples to test the :code:`post` method

.. code-block:: bash

   awsmobile cloud-api invoke sampleCloudApi post /items '{"body": {"testKey":"testValue"}}'

This call will return a response similar to the following.

.. code-block:: bash

    { success: 'post call succeed!',
    url: '/items',
    body: { testKey: 'testValue' } }

**To test the :get method**

.. code-block:: bash

   awsmobile cloud-api invoke sampleCloudApi get /items


This will return a response as follows.

.. code-block:: bash

   { success: 'get call succeed!', url: '/items' }


Connect to Your Backend
=======================

Once you have created your own :ref:`Cloud Logic <cloud-logic>` APIs and Lambda functions, you can call them from your app.

**To call APIs from your app**

In :file:`App.js` (or  other code that runs at launch-time), add the following import.

.. code-block:: java

    import Amplify, { API } from 'aws-amplify';
    import aws_exports from './aws-exports';
    Amplify.configure(aws_exports);

Then add this to the component that calls your API.

.. code-block:: java

    state = { apiResponse: null };

    async getSample() {
     const path = "/items"; // you can specify the path
      const apiResponse = await API.get("sampleCloudApi" , path); //replace the API name
      console.log('response:' + apiResponse);
      this.setState({ apiResponse });
    }


To invoke your API from a UI element, add an API call from within your component's :code:`render()` method.

.. code-block:: html

      <View>
         <Button title="Send Request" onPress={this.getSample.bind(this)} />
         <Text>Response: {this.state.apiResponse && JSON.stringify(this.state.apiResponse)}</Text>
      </View>

To test, save the changes, run :code:`npm run android` or :code:`npm run ios`` to launch your app. Then try the UI element that calls your API.

Next Steps
==========

Learn more about the AWS Mobile :ref:`Cloud Logic <cloud-logic>` feature which uses `|ABP| <http://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html>`__ and `|LAM| <http://docs.aws.amazon.com/lambda/latest/dg/welcome.html>`__.

To be guided through creation of an API and it's handler, run :code:`awsmobile console` to open your app in the |AMH| console, and choose :guilabel:`Cloud Logic`.

Learn about :ref:`AWS Mobile CLI <aws-mobile-cli-reference>`.

Learn about `AWS Mobile Amplify <https://aws.github.io/aws-amplify/>`__.
