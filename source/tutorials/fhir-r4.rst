FHIR R4 Tutorial
****************

Overview
========
This tutorial provides a working example of a typical Linux for Health FHIR R4 route: data ingress via FHIR R4, storage via a Kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_
* `Install Node.js <https://nodejs.org/en/download/package-manager/#macos>`_
* `Install Postman <https://www.postman.com/downloads>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to see Linux for Health in action using FHIR R4 resources.

Start the NATS Subscriber
-------------------------
In a new console window, cd to the NATS test directory in the Linux for Health connect repo (cloned during the Developer Setup Prerequisite)::

   cd connect/src/test/resources/nats

Run the subscriber::

   node nats-subscriber

Send a FHIR R4 Message to Linux for Health 
------------------------------------------
Open Postman and import this collection by clicking 'Import' -> 'Import File' -> 'Choose Files'::

   connect/src/test/resources/messages/postman/Linux for Health FHIR R4 Tutorial.postman_collection.json

Click on the collection in the left navigation area and select 'Create a patient resource', then click 'Send'.

You should see the JSON result below in the Postman window::

   {
      "meta": {
         "routeId": "fhir-r4-rest",
         "uuid": "bc4e2040-4a33-44ff-ba5d-374787ceed47",
         "routeUrl": "http://0.0.0.0:8080/fhir/r4/Patient",
         "dataFormat": "fhir-r4",
         "timestamp": 1594053821,
         "dataStoreUri": "kafka:FHIR_R4_PATIENT?brokers=localhost:9092",
         "status": "success",
         "dataRecordLocation": [
               "FHIR_R4_PATIENT-0@22"
         ]
      }
   }

   You should also see a NATS notification in the nats-subscriber console window, indicating the message was stored in Kafka.  The message received by the NATS subscriber indicates the topic, partition and offset of the message in Kafka, which could be used for downstream application integration.

View the Message in the Kafdrop Console (Optional)
--------------------------------------------------
You can optionally view the message in Kafka, via the Kafdrop Kafka client.  In your browser, navigate to::

   http://localhost:9000/

Scoll down and click on the 'FHIR_R4_PATIENT' topic.

Click 'View Messages', then click 'View Messages' again on the next page.  This will take you to a list of all messages for the topic.  

Navigate to the message at the offset indicated in the query results.  For example, from 'FHIR_R4_PATIENT-0@22' in the result, we know the message is at offset '22' in the 'FHIR_R4_PATIENT' topic.  At your result offset, you should see the FHIR patient resource you just sent to Linux for Health.
