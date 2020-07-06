Blue Button 2.0 Tutorial
************************

Overview
========
This tutorial provides instructions for using Linux for Health with `Blue Button 2.0 <https://bluebutton.cms.gov/developers/#blue-button-implementation-guide>`_, the CMS API that gives 53 million Medicare beneficiaries access to four years of Medicare Part A, B and D information.  The Blue Button 2.0 route in Linux for Health uses the Blue Button 2.0 Sandbox environment, which includes sample Medicare information for mock beneficiaries.  The route converts the results from FHIR R3 to R4, plus it includes standard steps of storage via a Kafka topic and notification via NATS. 

Note: FHIR R3 to R4 conversion is performed for Patient and Coverage resources, but R3->R4 conversion is not yet possible for the ExplanationOfBenefit resource (not implemented in org.hl7.fhir.convertors).

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_
* `Install Node.js <https://nodejs.org/en/download/package-manager/#macos>`_
* `Install Postman <https://www.postman.com/downloads>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to use Linux for Health to work with Medicare FHIR resources.

Start the NATS Subscriber
-------------------------
In a new console window, cd to the NATS test directory in the Linux for Health connect repo (cloned during the Developer Setup Prerequisite)::

   cd connect/src/test/resources/nats

Run the subscriber::

   node nats-subscriber

Authorize Linux for Health to use the Blue Button 2.0 API
---------------------------------------------------------
Open Postman and import this collection by clicking 'Import' -> 'Import File' -> 'Choose Files'::

   connect/src/test/resources/postman/Linux for Health Blue Button Tutorial.postman_collection.json

Click on the collection in the left navigation area and select 'Authorize Linux for Health', then click 'Send'.

This operation opens your default browser and takes you to the Blue Button 2.0 Sandbox environment login.  Use these credentials to follow this tutorial, although there are 30K possible sample users to choose from::

   Username: BBUser02153
   Password: PW02153!

Select 'Share all of your data' and click 'Allow'.  You can also elect to not share personal information, but this will give you a 403 Unauthorized result when you query for the Patient FHIR resources for this account.

From your browser window, copy the resulting 'access_token' and save it for the next step.

Configure Postman for Blue Button Access
----------------------------------------
In Postman, navigate to your Blue Button tutorial collection and click '...' next to the collection name, then click 'Edit'.

Click the 'Authorization' tab, then paste your new access_token from the previous section in the 'Token' field and click 'Update'.

You are now ready to query for Medicare data using Linux for Health and the Blue Button 2.0 API.  

Send a Blue Button Query to Linux for Health 
--------------------------------------------
From the Blue Button collection in Postman, select 'Get patient details' and click 'Send'.

You should see a result similar to::

   {
    "meta": {
        "routeId": "bluebutton-20-rest",
        "uuid": "6735c7ef-3640-434e-9563-2ed38958b21a",
        "routeUrl": "http://0.0.0.0:8080/bluebutton/v1/Patient?-19990000002154",
        "dataFormat": "fhir-r4",
        "timestamp": 1594050020,
        "dataStoreUri": "kafka:FHIR_R4_PATIENT?brokers=localhost:9092",
        "status": "success",
        "dataRecordLocation": [
            "FHIR_R4_PATIENT-0@21"
        ]
      }
   }

The result indicates the topic, partition and offset of the message in Kafka.  'Get EOB details' and 'Get coverage details' queries from the Postman collection provide similar results for ExplanationofBenefit and Coverage resources.

You should also see a NATS notification in the nats-subscriber console window.  The message received by the NATS subscriber indicates the topic, partition and offset of the message in Kafka, which could be used for downstream application integration.

View the Message in the Kafdrop Console (Optional)
--------------------------------------------------
You can optionally view the message contents in Kafka, via the Kafdrop Kafka client.  In your browser, navigate to::

   http://localhost:9000/

Scoll down and click on the 'FHIR_R4_PATIENT' topic.

Click 'View Messages', then click 'View Messages' again on the next page.  This will take you to a list of all messages for the topic.  

Navigate to the message at the offset indicated in the query results.  For example, from 'FHIR_R4_PATIENT-0@21' in the result, we know the message is at offset '21' in the 'FHIR_R4_PATIENT' topic.  At your result offset, you should see the bundle of Blue Button FHIR patient resources you just requested from Linux for Health.
