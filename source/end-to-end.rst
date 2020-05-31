End-to-End Tutorial
*******************

Overview
========
This End-to-End tutorial provides a working example of a typical iDAAS route: data ingress via HL7 mllp, transformation to json, storage via a kafka topic and notification of the storage via NATS.

Prerequisites
==============
* `Developer Setup <./developer-setup.html>`_
* `Node.js <https://nodejs.org/en/download/package-manager/#macos>`_

Steps
=====
Once you have completed the Developer Setup listed in the Prerequisites, follow these steps to see iDAAS in action.

Install the HL7 Client
----------------------
# Clone the Python HL7 client::

   git clone git@github.com:johnpaulett/python-hl7.git

# Install the Python HL7 client::

   cd python-hl7
   python3 -m venv venv
   source venv/bin/activate
   python3 setup.py install
   pip list

# Confirm the install of the Python HL7 client::

   pip list

Install the NATS Subscriber
---------------------------
# In a new console window, cd to the iDAAS NATS directory in the idaas-connect repo::

   cd idaas-connect/src/test/resources/nats

# Install the modules needed by nats-subscriber.js::

   npm i
   
# Run the subscriber::

   node nats-subscriber

The NATS subscriber will run indefinitely. Ctrl-Cn to exit.

Send an HL7 Message to iDAAS
----------------------------
# cd to the iDAAS tutorial directory::

   cd idaas-connect/docs/tutorials/end-to-end

# Send an HL7 Message to iDAAS::

   mllp_send --file ADT_A01.txt --loose --port 2575 localhost

You should see the message echoed back out in the hl7 client terminal.  You should also see the message in the NATS subscriber window, indicating the message was received by iDAAS and stored in the kafka topic.