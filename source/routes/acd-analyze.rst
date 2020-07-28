Annotator for Clinical Data (ACD) Analyze Route
***********************************************

Purpose
========
The Linux for Health (LFH) Annotator for Clinical Data (ACD) route enables the extraction of clinical insights from unstructured data. The ACD response is persisted as a Kafka topic and listeners are notified via NATS for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Parameter               | Description                                                         |
+=========================+=====================================================================+
| URL Property            | NA                                                                  |
+-------------------------+---------------------------------------------------------------------+
| Default URL             | NA                                                                  |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | NA                                                                  |
+-------------------------+---------------------------------------------------------------------+
| Protocol                | NA                                                                  |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | NA                                                                  |
+-------------------------+---------------------------------------------------------------------+

Configuration
=============

Path Parameters
---------------
+--------------------+-----------+--------------------------------------------------------------+
| Parameter          | Type      | Description                                                  |
+====================+===========+==============================================================+
| NA                 | --        | --                                                           |
+--------------------+-----------+--------------------------------------------------------------+

Query Parameters
----------------
None

Required Headers
----------------
The Content-Type header within the route message must be set to either ``text/plain`` or ``application/json``

Configuration Properties
------------------------
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Property                     | Description                                                                                                                                                                                                           |
+==============================+=======================================================================================================================================================================================================================+
| lfh.connect.acd_rest.auth    | camel-http authentication query params for ACD. Example: ``authMethod=Basic&authUsername=apikey&authPassword=<JASYPT ENCRYPTED ACD API KEY>``                                                                         |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.acd_rest.version | The required **version** query param for ACD in date format: YYYY-MM-DD                                                                                                                                               |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.acd_rest.flow    | The ACD annotator flow ID to employ in analyzing unstructured data.                                                                                                                                                   |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.acd_rest.host    | The ACD endpoint service hostname, which varies by region. Example: ``us-east.wh-acd.cloud.ibm.com``                                                                                                                  |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.acd_rest.baseUri | The http(s) protocol prefixing the ACD hostname. Example: ``https://{{lfh.connect.acd_rest.host}}``                                                                                                                   |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.acd_rest.uri     | The full camel-http endpoint to use for ACD. Example: ``{{lfh.connect.acd_rest.baseUri}}/wh-acd/api/v1/analyze/{{lfh.connect.acd_rest.flow}}?version={{lfh.connect.acd_rest.version}}&{{lfh.connect.acd_rest.auth}}`` |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Calling the Route
=================
Using a Camel ProducerTemplate or a ``to()`` operation within a route, set the message body and ``Content-Type`` header to either ``text/plain`` or ``application/json`` corresponding to the content of the message body, and route the message to **"direct:acd-analyze"**.

Results
=======
The json response from the ACD service analyze request is persisted as an **ACD_INSIGHTS** Kafka topic

See Also
========
