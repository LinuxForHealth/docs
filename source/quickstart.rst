Quick Start Tutorial
********************

Note: This tutorial is in progress.  It works as is, but does not yet include notification via NATS, which will be automatically triggered upon storage of the message in the Kafka topic.

Overview
========
This tutorial provides a working example of a typical iDAAS route: data ingress via HL7 mllp, storage via a Kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <./developer-setup.html>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to see iDAAS in action.

Install the HL7 Client
----------------------
In a new console window, clone the Python HL7 client::

   git clone https://github.com/johnpaulett/python-hl7.git

Install the Python HL7 client::

   cd python-hl7
   python3 -m venv venv
   source venv/bin/activate
   python3 setup.py install

Send a Message to iDAAS
-----------------------
cd to the iDAAS test resources directory in the idaas-connect repo (cloned during the Developer Setup Prerequisite)::

   cd idaas-connect/src/test/resources

Send an HL7 Message to iDAAS::

   mllp_send --file ADT_A01.txt --loose --port 2575 localhost

You should see the message echoed in the HL7 client terminal.  

View the Message in the Kafdrop Console
---------------------------------------
In your browser, navigate to the Kafdrop Kafka client::

   http://localhost:9000/

Scoll down and click on the 'HL7v2_ADT' topic.

Click 'View Messages', then click 'View Messages' again on the next page.  You should see the body of the HL7v2 message you just sent to iDAAS, indicating that the message was sent to the correct Kafka topic.
