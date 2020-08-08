Quick Start Tutorial
********************

Overview
========
This tutorial provides a working example of a typical Linux for Health route: data ingress via HL7 mllp, storage via a Kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to see Linux for Health in action.

Install the HL7 Client
----------------------
In a new console window, clone the Python HL7 client::

   git clone https://github.com/johnpaulett/python-hl7.git

Install the Python HL7 client::

   cd python-hl7
   python3 -m venv venv
   source venv/bin/activate
   python3 setup.py install

Send a Message to Linux for Health
----------------------------------
In the same console window as the previous step, cd to the test messages directory in the Linux for Health connect repo::

   cd connect/src/test/resources/messages/hl7

Send an HL7 message to Linux for Health::

   mllp_send --file ADT_A01.txt --loose --port 2575 localhost

You should see a message echoed in the HL7 client console window, similar to::

   {"meta":{"routeId":"hl7-v2-mllp","uuid":"4a49c1b8-c132-40e5-8175-c07dc90637f6","routeUrl":"netty:tcp://0.0.0.0:2575?sync=true&encoders=#hl7encoder&decoders=#hl7decoder","dataFormat":"HL7-V2","messageType":"ADT","timestamp":1596032326,"dataStoreUri":"kafka:HL7-V2_ADT?brokers=localhost:9092","status":"success","dataRecordLocation":["HL7-V2_ADT-0@7"]}}

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

Scoll down and click on the 'HL7-V2_ADT' topic.

Click 'View Messages', then click 'View Messages' again on the next page. This will take you to a list of all messages for the topic.  

Navigate to the message at the offset indicated in the result.  For example, from 'HL7-V2_ADT-0@7' in the result, we know the message is at offset '7' in the 'HL7-V2_ADT' topic.  At your result offset, you should see the HL7v2 ADT message sent to Linux for Health.
