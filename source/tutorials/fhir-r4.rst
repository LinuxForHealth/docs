FHIR R4 Tutorial
****************

Overview
========
This tutorial provides a working example of a typical Linux for Health FHIR R4 route: data ingress via FHIR R4, storage via a Kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <./developer-setup.html>`_
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

   connect/src/test/resources/tutorials/Linux for Health FHIR R4 Tutorial.postma_collection.json

Click on the collection in the left navigation area and select the 'Create a patient resource', then click 'Send'.

You should see the JSON result below in the Postman window.  You should also see a NATS notification in the nats-subscriber console window, indicating the message was stored in Kafka.  The message received by the NATS subscriber indicates the topic, partition and offset of the message in Kafka, which could be used for downstream application integration::

   {"metadata":["FHIR_R4_PATIENT-0@5"],"results":[{"partition":0,"offset":5,"topic":"FHIR_R4_PATIENT","timestamp":1591818224767}]}

View the Message in the Kafdrop Console (Optional)
--------------------------------------------------
You can optionally view the message in Kafka, via the Kafdrop Kafka client.  In your browser, navigate to::

   http://localhost:9000/

Scoll down and click on the 'FHIR_R4_PATIENT' topic.

Click 'View Messages', then click 'View Messages' again on the next page.  You should see the body of the FHIR R4 message you just sent to Linux for Health.
