QuickStart
**********

Overview
========
This tutorial shows you how to send a FHIR message to LinuxForHealth, using a mock FHIR patient resource and the LinuxForHealth Open API UI.

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_

Tutorial Steps
==============
Once you have completed the prerequisites, follow these steps to send a message to LinuxForHealth.

Send a Message to LinuxForHealth
--------------------------------
In your browser, navigate to https://127.0.0.1:5000/docs and select :code:`POST /fhir/{resource_type}`, then click :code:`Try It Out`.

In the **resource_type** field, type :code:`Patient`.

In the **Request body** field, use::

   { "resourceType": "Patient",
     "identifier": [ {
        "system": "urn:oid:1.2.36.146.595.217.0.1",
        "value": "12345" } ],
     "name": [ {
        "family": "Duck",
        "given": [ "Donald", "D." ] } ],
     "gender": "male",
     "birthDate": "1974-12-25" }

Then click :code:`Execute`.

This posts a mock Patient resource to the LinuxForHealth FHIR R4 route.  Scroll down and you should see a 200 response code and a response body similar to::

   {"uuid":"fd09c54d-4e64-478c-93bb-456e0cfe67f4",
    "lfh_id":"My-Computer.net",
    "creation_date":"2021-04-07T21:08:45+00:00",
    "store_date":"2021-04-07T21:08:45+00:00",
    "consuming_endpoint_url":"/fhir/Patient",
    "data":"eyJiaXJ0aERhdGUiOiAiMTk3NC0xMi0yNSIsICJnZW5kZXIiOiAibWFsZSIsICJpZGVudGlmaWVyIjogW3sic3lzdGVtIjogInVybjpvaWQ6MS4yLjM2LjE0Ni41OTUuMjE3LjAuMSIsICJ2YWx1ZSI6ICIxMjM0NSJ9XSwgIm5hbWUiOiBbeyJmYW1pbHkiOiAiRHVjayIsICJnaXZlbiI6IFsiRG9uYWxkIiwgIkQuIl19XSwgInJlc291cmNlVHlwZSI6ICJQYXRpZW50In0=",
    "data_format":"PATIENT",
    "status":"success",
    "data_record_location":"PATIENT:0:17",
    "target_endpoint_url":null,
    "elapsed_storage_time":0.087262,
    "transmit_date":null,"elapsed_transmit_time":null
    "elapsed_total_time":0.154136}


The **data_record_location** value indicates the topic, partition and offset of the message in Kafka, shown above as :code:`PATIENT:0:17`.

View the Result in Kafka
------------------------
On the same page in your browser, scroll up and select :code:`GET /data`, then click :code:`Try It Out`.

Populate the fields with the data_record_location information returned in the previous FHIR R4 response:

In the **dataformat** field, type :code:`PATIENT`.

In the **partition** field, type :code:`0`.

In the **offset** field, type :code:`17`.

Then click :code:`Execute`.

This performs a GET from kafka for the topic, partition and offset you provided.  Scroll down and you should see a 200 response code and a response body similar to::

   {
      "uuid": "fd09c54d-4e64-478c-93bb-456e0cfe67f4",
      "lfh_id": "My-Computer.net",
      "creation_date": "2021-04-07T21:08:45+00:00",
      "store_date": "2021-04-07T21:08:45+00:00",
      "consuming_endpoint_url": "/fhir/Patient",
      "data": "eyJiaXJ0aERhdGUiOiAiMTk3NC0xMi0yNSIsICJnZW5kZXIiOiAibWFsZSIsICJpZGVudGlmaWVyIjogW3sic3lzdGVtIjogInVybjpvaWQ6MS4yLjM2LjE0Ni41OTUuMjE3LjAuMSIsICJ2YWx1ZSI6ICIxMjM0NSJ9XSwgIm5hbWUiOiBbeyJmYW1pbHkiOiAiRHVjayIsICJnaXZlbiI6IFsiRG9uYWxkIiwgIkQuIl19XSwgInJlc291cmNlVHlwZSI6ICJQYXRpZW50In0=",
      "data_format": "PATIENT",
      "status": null,
      "data_record_location": null,
      "target_endpoint_url": null,
      "elapsed_storage_time": null,
      "transmit_date": null,
      "elapsed_transmit_time": null,
      "elapsed_total_time": null
    }

The **data** field contains a base64-encoded version of the original data you posted to the /fhir endpoint.  Some fields, like **data_record_location**, are not available until after the storage in Kafka is complete and won't be included in the data stored in Kafka.

Next Steps
----------
Experiment further with the LinuxForHealth APIs, providing your own data.  There is also a :code:`GET /status` endpoint that requires no parameters and returns the status of the LinuxForHealth services.

Think about extending LinuxForHealth to add new APIs to transform and connect healthcare data.  What APIs can you provide?  Open a PR and contribute!