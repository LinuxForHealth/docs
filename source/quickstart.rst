Quick Start Tutorial
********************

Overview
========
This tutorial provides a working example of a typical iDAAS route: data ingress via HL7 mllp, storage via a kafka topic and notification via NATS.

Prerequisites
=============
* `Developer Setup <./developer-setup.html>`_
* `Install Node.js <https://nodejs.org/en/download/package-manager/#macos>`_
* `Install Docker <https://docs.docker.com/get-docker/>`_

Tutorial Steps
==============
Once you have completed the Prerequisites, follow these steps to see iDAAS in action.

Install and run the NATS Server
-------------------------------
In a new console window, cd to the iDAAS NATS directory in the idaas-connect repo (cloned during the Prerequisite Developer Setup)::

   cd idaas-connect/src/test/resources/nats

Start the NATS Server::

   docker-compose up -d

This will run the NATS Server in the background.

Install and run the NATS Subscriber
-----------------------------------
In the same directory, install the modules needed by nats-subscriber.js::

   npm i
   
Run the subscriber::

   node nats-subscriber

Install the HL7 Client and send a Message to iDAAS
--------------------------------------------------
Clone the Python HL7 client::

   git clone https://github.com/johnpaulett/python-hl7.git

Install the Python HL7 client::

   cd python-hl7
   python3 -m venv venv
   source venv/bin/activate
   python3 setup.py install

cd to the iDAAS test resources directory in the idaas-connect repo::

   cd idaas-connect/src/test/resources/nats

Send an HL7 Message to iDAAS::

   mllp_send --file ADT_A01.txt --loose --port 2575 localhost

You should see the message echoed in the hl7 client terminal.  You should also see the message in the NATS subscriber window, indicating the message was received and processed by iDAAS before sending to NATS.
