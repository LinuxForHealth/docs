Message Structure
*****************
This section describes the message structure used by LinuxForHealth connect, and shows typical examples.


Structure
=========
The following JSON message structure is used throughout LinuxForHealth::

    {
        uuid: uuid,
        lfh_id: string,
        creation_date: datetime,
        store_date: datetime,
        consuming_endpoint_url: string,
        data: string,
        data_format: string,
        status: string,
        data_record_location: string,
        target_endpoint_url: string,
        elapsed_storage_time: float,
        transmit_date: datetime,
        elapsed_transmit_time: float,
        elapsed_total_time: float
    }    

Description
===========

The LinuxForHealth connect message format captures metadata pertaining to the data transaction. Data is received via an endpoint, as noted by the consuming_endpoint_url, and then persisted to the connect database. Data is transmitted to an external service if a transmission URL is configured.

+------------------------+-----------+---------------------------------------------------------------------+
| Key                    | Type      | Description                                                         |
+========================+===========+=====================================================================+
| uuid                   | string    | The message uuid assigned during route invocation.                  |
+------------------------+-----------+---------------------------------------------------------------------+
| lfh_id                 | string    | The LinuxForHealth connect node identifier.                         |
+------------------------+-----------+---------------------------------------------------------------------+
| creation_date          | datetime  | The message creation timestamp.                                     |
+------------------------+-----------+---------------------------------------------------------------------+
| store_date             | datetime  | The message storage timestamp.                                      |
+------------------------+-----------+---------------------------------------------------------------------+
| consuming_endpoint_url | string    | The LinuxForHealth connect endpoint which received the message.     |
+------------------------+-----------+---------------------------------------------------------------------+
| data                   | string    | Data content received as a base-64 encoded string.                  |
+------------------------+-----------+---------------------------------------------------------------------+
| status                 | string    | Indicates if the message was persisted. Either "success" or "error".|
+------------------------+-----------+---------------------------------------------------------------------+
| data_record_location   | string    | The data record's URI within the LinuxForHealth connect datastore.  |
+------------------------+-----------+---------------------------------------------------------------------+
| target_endpoint_url    | string    | The external URL where the data is transmitted.                     |
+------------------------+-----------+---------------------------------------------------------------------+
| elapsed_storage_time   | float     | Elapsed data storage time in seconds.                               |
+------------------------+-----------+---------------------------------------------------------------------+
| transmit_date          | datetime  | The data transmission timestamp.                                    |
+------------------------+-----------+---------------------------------------------------------------------+
| elapsed_transmit_time  | datetime  | Elapsed external transmission time in seconds.                      |
+------------------------+-----------+---------------------------------------------------------------------+
| elapsed_total_time     | datetime  | Elapsed total transmission time including storage and transmission  |
+------------------------+-----------+---------------------------------------------------------------------+

Examples
========

The LinuxForHealth message for a FHIR-R4 Patient resource which is persisted to the connect db::

    {
       "uuid": "fd09c54d-4e64-478c-93bb-456e0cfe67f4",
       "lfh_id": "My-Computer.net",
       "creation_date": "2021-04-07T21:08:45+00:00",
       "store_date": "2021-04-07T21:08:45+00:00",
       "consuming_endpoint_url": "/fhir/Patient",
       "data": "eyJiaXJ0aERhdGUiOiAiMTk3NC0xMi0yNSIsICJnZW5kZXIiOiAibWFsZSIsICJpZGVudGlmaWVyIjogW3sic3lzdGVtIjogInVybjpvaWQ6MS4yLjM2LjE0Ni41OTUuMjE3LjAuMSIsICJ2YWx1ZSI6ICIxMjM0NSJ9XSwgIm5hbWUiOiBbeyJmYW1pbHkiOiAiRHVjayIsICJnaXZlbiI6IFsiRG9uYWxkIiwgIkQuIl19XSwgInJlc291cmNlVHlwZSI6ICJQYXRpZW50In0=",
       "data_format": "FHIR-R4",
       "status": null,
       "data_record_location": null,
       "target_endpoint_url": null,
       "elapsed_storage_time": null,
       "transmit_date": null,
       "elapsed_transmit_time": null,
       "elapsed_total_time": null
    }

The **data** field contains a base64-encoded version of the original data sent to LinuxForHealth.  Some fields, like **data_record_location**, are not available until after the storage is complete.

When a transaction is complete, the resulting LinuxForHealth message contains the **data_record_location**, as well as the storage **status** and metrics such as **elapsed_storage_time**::

    {
        "uuid": "290487e0-2864-445b-acc7-907c227ba2d3",
        "lfh_id": "3b935d2b1441",
        "creation_date": "2021-04-09T20:34:36+00:00",
        "store_date": "2021-04-09T20:34:36+00:00",
        "consuming_endpoint_url": "/fhir/Patient",
        "data": "eyJpZCI6ICIwMDEiLCAiYWN0aXZlIjogdHJ1ZSwgInJlc291cmNlVHlwZSI6ICJQYXRpZW50In0=",
        "data_format": "FHIR-R4",
        "status": "success",
        "data_record_location": "FHIR-R4:0:0",
        "target_endpoint_url": null,
        "elapsed_storage_time": 1.64379,
        "transmit_date": null,
        "elapsed_transmit_time": null,
        "elapsed_total_time": 1.680011
    }

You may also define an external server to which LinuxForHealth will send messages, such as a FHIR server.  In this case, the message stored in the connect DB will include a value for **target_endpoint_url**::

    {
       "uuid": "fd09c54d-4e64-478c-93bb-456e0cfe67f4",
       "lfh_id": "My-Computer.net",
       "creation_date": "2021-04-07T21:08:45+00:00",
       "store_date": "2021-04-07T21:08:45+00:00",
       "consuming_endpoint_url": "/fhir/Patient",
       "data": "eyJiaXJ0aERhdGUiOiAiMTk3NC0xMi0yNSIsICJnZW5kZXIiOiAibWFsZSIsICJpZGVudGlmaWVyIjogW3sic3lzdGVtIjogInVybjpvaWQ6MS4yLjM2LjE0Ni41OTUuMjE3LjAuMSIsICJ2YWx1ZSI6ICIxMjM0NSJ9XSwgIm5hbWUiOiBbeyJmYW1pbHkiOiAiRHVjayIsICJnaXZlbiI6IFsiRG9uYWxkIiwgIkQuIl19XSwgInJlc291cmNlVHlwZSI6ICJQYXRpZW50In0=",
       "data_format": "FHIR-R4",
       "status": null,
       "data_record_location": null,
       "target_endpoint_url": "https://fhiruser:change-password@localhost:9443/fhir-server/api/v4",
       "elapsed_storage_time": null,
       "transmit_date": null,
       "elapsed_transmit_time": null,
       "elapsed_total_time": null
    }

When an external server is defined, the return value is the actual response from that external server (such as an HTTP 201 - created response). In this case, the LinuxForHealth message UUID is included in the **LinuxForHealth-MessageId** header of the response.