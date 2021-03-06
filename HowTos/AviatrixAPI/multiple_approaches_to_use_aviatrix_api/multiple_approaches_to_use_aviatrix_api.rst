.. meta::
   :description: Multiple Approaches to Use Aviatrix API
   :keywords: REST, API, CID, login, cloud account

=======================================
Multiple Approaches to Use Aviatrix API
=======================================


Introduction
------------

Aviatrix provides a REST/RESTful (Representational State Transfer) API to help customers to integrate Aviatrix products or to automate some routine tasks, such as backups for the Aviatrix controller, checking the status of active/live VPN users for management purposes, etc.

|

Tools
-----

In this document, we demonstrate Aviatrix REST API invocation with the following tools.
  1. **Postman**
  2. Linux **"curl"** command
  3. Python **"requests"** module/library/package

|

Value Format (URL Encoding)
---------------------------

If the input value contains certain special characters, such as '#' or '/' you may need to convert them to conform to a valid URL:


Tip:
"""""

Use '%23' instead of '#'; use '%2F' instead of '/'


For example:
""""""""""""

If my Azure ARM Subscription ID is "abc#efg", instead of using...

    "arm_subscription_id=abc#efg"

you need to use the following format instead...

    "arm_subscription_id=abc%23efg"

|

Tools to convert the value format
---------------------------------

There are many tools online that can do the job. Just simply google **"URL Encoder"**, and you can encode/convert the special character to the correct format.

|

How the Aviatrix REST API Works
-------------------------------

In order to invoke most of the Aviatrix API(s), the user must have a valid **"CID"** (session ID) for security purpose. Moreover, a valid CID can be acquired through Aviatrix **"login"** API. The examples are provided below.
Please refer to the `Aviatrix-REST-API Documentation. <https://s3-us-west-2.amazonaws.com/avx-apidoc/index.html>`__ for the completed Aviatrix REST API list.

|

Examples: Invoke Aviatrix "login" API to get a valid CID
--------------------------------------------------------

Postman
"""""""

    |image1|


Linux "curl" command
""""""""""""""""""""

**Syntax:**

::

    curl  -k  "https://AVIATRIX_CONTROLLER_IP/v1/api?action=login&username=admin&password=MY_PASSWORD"


**Example:**

    |image2|


Python "requests" module
""""""""""""""""""""""""

**Example Code:**

.. code-block:: python

    import requests

    # Controller configuration
    base_url = "https://10.67.0.2/v1/api"
    username = "admin"
    password = "MyPassword"
    action = "login"
    CID = ""

    # Configuration for "login" API
    payload = {
        "action": action,
        "username": username,
        "password": password
    }

    # Use "requests" module to invoke REST API
    response = requests.get(url=base_url, params=payload, verify=False)

    # If login successfully
    if True == response.json()["return"]:
        CID = response.json()["CID"]
        print("Successfully login to Aviatrix Controller. The valid CID is: " + CID)



**Execution Result:**

    |image3|


Examples: Invoke Other Aviatrix API with a valid CID
----------------------------------------------------

.. Note::
The following examples demonstrate using the Aviatrix API **"setup_account_profile"** to create Aviatrix **"Cloud Account"**.


Postman
"""""""

    |image4|


Linux "curl" command
""""""""""""""""""""

    |image5|


Python
""""""

**Example Code:**

.. code-block:: python

    import requests

    # Configuration for "setup_account_profile" API to create AWS IAM Role based account
    payload = {
        "action": "setup_account_profile",
        "CID": "B4XvxZYJUTHNaMcK2Nf2",
        "account_name": "my-AWS-operation-account",
        "account_password": "!MyPassword",
        "account_email": "test@aviatrix.com",
        "cloud_type": "1",
        "aws_account_number": "123456789999",
        "aws_iam": "true",
        "aws_access_key": "XXXXXXXXXXXXXXXXXXXXXX",
        "aws_secret_key": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }

    # Use "requests" module to invoke REST API
    response = requests.post(url="https://10.67.0.2/v1/api", data=payload, verify=False)

    # Display return message
    print(response.json())


**Execution Result:**

    |image6|


Conclusion:
-----------
At Aviatrix, we believe that networking is a foundational element of cloud computing which should be as dynamic, scalable, and elastic as compute and storage. Please do not hesitate to contact us if you have any feedback.



-----------------------------------------------------------------


.. |image1| image:: ./img_01_postman_login_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in
.. |image2| image:: ./img_02_linux_curl_login_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in
.. |image3| image:: ./img_03_python_login_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in
.. |image4| image:: ./img_04_postman_create_account_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in
.. |image5| image:: ./img_05_linux_curl_create_account_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in
.. |image6| image:: ./img_06_python_create_account_execution_results.png
    :width: 2.00000 in
    :height: 2.00000 in



.. disqus::
