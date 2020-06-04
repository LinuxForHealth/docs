Quick Start Tutorial
********************

Note: This tutorial is in progress.  It works as is, but does not yet include Kafka in the route.  Further, we will be changing the way we use NATS, to be automatically triggered upon storage of the message in the Kafka topic.

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

Install and Run the NATS Server
-------------------------------
In a new console window, cd to the iDAAS NATS directory in the idaas-connect repo (cloned during the Prerequisite Developer Setup)::

   cd idaas-connect/src/test/resources/nats

Start the NATS Server::

   docker-compose up -d

This will run the NATS Server in the background.

Configure iDAAS to Use NATS
---------------------------
From the same directory, update the iDAAS config to use NATS::

   ./copy-cfg.sh

In the console window where you are running iDAAS from the Developer Setup step, stop iDAAS via Ctrl-C, then restart iDAAS::

   ./gradlew run

Install and Run the NATS Subscriber
-----------------------------------
In the same directory, install the modules needed by nats-subscriber.js::

   npm i
   
Run the subscriber::

   node nats-subscriber

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
cd to the iDAAS test resources directory in the idaas-connect repo::

   cd idaas-connect/src/test/resources/nats

Send an HL7 Message to iDAAS::

   mllp_send --file ADT_A01.txt --loose --port 2575 localhost

You should see the message echoed in the hl7 client terminal.  You should also see the message in the NATS subscriber console window, indicating the message was received and processed by iDAAS before sending to NATS.

Clean Up
--------
From the same directory, restore the original iDAAS configuration::

   ./restore-cfg.sh

In the NATS subscriber console window, stop the subscriber via Ctrl-C, then stop the NATS server::

   docker-compose down

In the iDAAS console window, stop iDAAS via Ctrl-C, then restart iDAAS::

   ./gradlew run

Note: Failure to run restore-cfg.sh will cause iDAAS to not start when NATS is not running.
