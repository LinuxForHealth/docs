Quick Start
***********

Overview
========
This tutorial shows you how to send a FHIR message to LinuxForHealth, using a simple FHIR patient resource and the curl command, which can be found on most operating systems.  You can optionally view your data via the Kafdrop Kafka console or NATS subscriber.

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to send a message to LinuxForHealth.

Send a Message to LinuxForHealth
----------------------------------
Use the `curl` utility to send a mock patient record to LinuxForHealth.  In a new console window, copy and paste the following curl command and hit `return`::

   curl --insecure --header "Content-Type: application/json" \
   --request POST \
   --data '{ "resourceType": "Patient", "identifier": [ { "system": "urn:oid:1.2.36.146.595.217.0.1", "value": "12345" } ], "name": [ { "family": "Duck", "given": [ "Donald", "D." ] } ], "gender": "male", "birthDate": "1974-12-25" }' \
   https://localhost:8443/fhir/r4/Patient

This command posts a basic Patient resource to the LinuxForHealth FHIR R4 route.  You should see a message echoed in the console window, similar to::

   {"meta":{"routeId":"fhir-r4","uuid":"8bebaaae-a30b-4d8e-8424-d38836bf1d14","routeUri":"jetty:http://0.0.0.0:8080/fhir/r4/Patient?httpMethodRestrict=POST","dataFormat":"FHIR-R4","messageType":"PATIENT","timestamp":1597868068,"dataStoreUri":"kafka:FHIR-R4_PATIENT?brokers=localhost:9092","status":"success","dataRecordLocation":["FHIR-R4_PATIENT-0@0"]}}

The dataRecordLocation value indicates the topic, partition and offset of the message in Kafka.

View the NATS Notification (Optional)
-------------------------------------
You should also see a NATS notification in the LinuxForHealth connect log.  If you are running the LinuxForHealth connect application locally, you should see a message in the connect console window similar to::

   15:14:29.474 [nats:3] INFO  c.l.connect.support.NATSSubscriber - nats-subscriber-localhost:4222-lfh-events received message: {"meta":{"routeId":"fhir-r4-rest","uuid":"8bebaaae-a30b-4d8e-8424-d38836bf1d14","routeUri":"jetty:http://0.0.0.0:8080/fhir/r4/Patient?httpMethodRestrict=POST","dataFormat":"FHIR-R4","messageType":"PATIENT","timestamp":1597868068,"dataStoreUri":"kafka:FHIR-R4_PATIENT?brokers=localhost:9092","status":"success","dataRecordLocation":["FHIR-R4_PATIENT-0@0"]}}

If you are running a LinuxForHealth container within docker-compose, in a console window, navigate to the connect compose directory and view the logs::

   cd connect/container-support/compose
   docker-compose logs -f lfh

The message received by the NATS subscriber also indicates the topic, partition and offset of the message in Kafka, which could be used for downstream application integration.

View the Message in the Kafdrop Console (Optional)
--------------------------------------------------
You can optionally view the message in Kafka, via the Kafdrop Kafka client.  In your browser, navigate to::

   http://localhost:9000/

Scroll down and click on the 'FHIR-R4_PATIENT' topic.

Click 'View Messages', then click 'View Messages' again on the next page. This will take you to a list of all messages for the topic.  

Navigate to the message at the offset indicated in the result.  For example, from 'FHIR-R4_PATIENT-0@0' in the result, we know the message is at offset '0' in the 'FHIR-R4_PATIENT' topic.  At your result offset, you should see the FHIR R4 message sent to LinuxForHealth.
