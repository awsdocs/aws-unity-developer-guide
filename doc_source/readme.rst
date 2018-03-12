.. _readme:

.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _readme:

##########################
aws-mobile-developer-guide
##########################

This repository contains source content for the official :ref:`what-is-aws-mobile`. The
source code for the AWS Mobile SDKs is available at:

* Android: `https://github.com/aws/aws-sdk-android <https://github.com/aws/aws-sdk-android>`__

* iOS: `https://github.com/aws/aws-sdk-ios <https://github.com/aws/aws-sdk-ios>`__

The guide content is written in reStructuredText_ and built using Sphinx_. It relies upon content
which is provided in the AWS documentation team's `shared content`_ repository.


Reporting issues
================

You can use the Issues_ section of this repository to report problems in the documentation. *When
submitting an issue, please indicate*:

* What page (a URL or filename is best) the issue occurs on.

* What the issue is, using as much detail as you can provide. For many issues, this might be as
  simple as "The page has a typo; the word 'complie' in the third paragraph shoud be 'compile'." If
  the issue is more complex, please describe it with enough detail that it's clear to the AWS
  documentation team what the problem is.


Contributing fixes and updates
==============================

To contribute your own documentation fixes or updates, please use the Github-standard procedures for
`forking the repository`_ and submitting a `pull request`_.

Note that many common substitutions_ and extlinks_ found in these docs are sourced from the `shared
content`_ repository--if you see a substitution used that is not declared at the top of the source
file or in the ``_includes.txt`` file, then it is probably defined in the shared content.


Building the documentation
--------------------------

If you are planning to contribute to the docs, you should build your changes and review them before
submitting your pull request.

**To build the docs:**

1. Make sure that you have downloaded and installed Sphinx_.
2. Run the ``build_docs.py`` script in the repository's root directory.

The build process will automatically download a snapshot of the `shared content`_, combine it in the
``build`` directory and will generate output into the ``output`` directory.

``build_docs.py`` can take any of the `available Sphinx builders`_ as its argument. For example, to
build the docs into a single HTML page, you can use the ``htmlsingle`` target, like so::

 python build_docs.py htmlsingle


Copyright and license
=====================

All content in this repository, unless otherwise stated, is Copyright © 2010-2016, Amazon Web
Services, Inc. or its affiliates. All rights reserved.

Except where otherwise noted, this work is licensed under a `Creative Commons
Attribution-NonCommercial-ShareAlike 4.0 International License
<http://creativecommons.org/licenses/by-nc-sa/4.0/>`__ (the "License"). Use the preceding link for a
human-readable summary of the license terms. The full license text is available at:
http://creativecommons.org/licenses/by-nc-sa/4.0/legalcode and in the LICENSE file accompanying this
repository.

.. =================================================================================
.. Links used in the README. For sanity's sake, keep this list sorted alphabetically
.. =================================================================================

.. _`available sphinx builders`: http://www.sphinx-doc.org/en/stable/builders.html
.. _`aws sdk for ios developer guide`: http://docs.aws.amazon.com/mobile/sdkforios/developerguide/
.. _`aws sdk for ios`: http://aws.amazon.com/mobile/sdk/
.. _`forking the repository`: https://help.github.com/articles/fork-a-repo/
.. _`pull request`: https://help.github.com/articles/using-pull-requests/
.. _`shared content`: https://github.com/awsdocs/aws-doc-shared-content
.. _extlinks: http://www.sphinx-doc.org/en/stable/ext/extlinks.html
.. _issues: https://github.com/awsdocs/aws-ios-developer-guide/issues
.. _restructuredtext: http://docutils.sourceforge.net/rst.html
.. _sphinx: http://www.sphinx-doc.org/en/stable/
.. _substitutions: http://www.sphinx-doc.org/en/stable/rest.html#substitutions

