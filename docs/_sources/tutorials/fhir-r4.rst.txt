FHIR R4 Tutorial
****************

Overview
========
This tutorial provides a working example of a typical Linux for Health FHIR R4 route: data ingress via FHIR R4, storage via a Kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_
* `Install Postman <https://www.postman.com/downloads>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to see Linux for Health in action using FHIR R4 resources.

Send a FHIR R4 Message to Linux for Health 
------------------------------------------
Open Postman and import the Linux for Health Examples collection by clicking 'Import' -> 'Import File' -> 'Choose Files'::

   connect/src/test/resources/messages/postman/Linux for Health Examples.postman_collection.json

Click on the collection in the left navigation area and select 'Create a patient resource', then click 'Send'.

You should see a result similar to the JSON result below in the Postman window::

   {
      "meta": {
         "routeId": "fhir-r4-rest",
         "uuid": "bc4e2040-4a33-44ff-ba5d-374787ceed47",
         "routeUrl": "http://0.0.0.0:8080/fhir/r4/Patient",
         "dataFormat": "FHIR-R4",
         "messageType": "PATIENT",
         "timestamp": 1594053821,
         "dataStoreUri": "kafka:FHIR-R4_PATIENT?brokers=localhost:9092",
         "status": "success",
         "dataRecordLocation": [
               "FHIR-R4_PATIENT-0@22"
         ]
      }
   }

The dataRecordLocation value indicates the topic, partition and offset of the message in Kafka.

View the NATS Notification (Optional)
-------------------------------------
You should also see a NATS notification in the nats-subscriber service log.  The message received by the NATS subscriber also indicates the topic, partition and offset of the message in Kafka, which could be used for downstream application integration.

To view NATS notifications in a new console window::

   cd connect/container-support/compose
   docker-compose logs -f nats-subscriber

View the Message in the Kafdrop Console (Optional)
--------------------------------------------------
You can optionally view the message in Kafka, via the Kafdrop Kafka client.  In your browser, navigate to::

   http://localhost:9000/

Scoll down and click on the 'FHIR-R4_PATIENT' topic.

Click 'View Messages', then click 'View Messages' again on the next page.  This will take you to a list of all messages for the topic.  

Navigate to the message at the offset indicated in the query results.  For example, from 'FHIR-R4_PATIENT-0@22' in the result, we know the message is at offset '22' in the 'FHIR-R4_PATIENT' topic.  At your result offset, you should see the FHIR patient resource you just sent to Linux for Health.
