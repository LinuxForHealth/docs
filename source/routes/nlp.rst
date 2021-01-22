NLP
*******

Purpose
========
The LinuxForHealth (LFH) NLP route allows you to send unstructured data to an NLP REST API for analysis. The canonical NLP service response is persisted as a Kafka topic.
The NLP service employed for analysis is configurable via property settings as shown below. Basic auth is the supported authentication method. Both text/plain and application/json request content types are supported.

Details
=======
+-------------------------+--------------------------+
| Attribute               | Value                    |
+=========================+==========================+
| Route Name              | nlp                      |                                    |
+-------------------------+--------------------------+
| Route URI               | direct:nlp               |
+-------------------------+--------------------------+
| Supported Operations(s) | Camel direct invocation  |
+-------------------------+--------------------------+

Configuration
=============

The direct:nlp route expects the following LFH property settings:

Exchange State
--------------
+------------------------------+---------------------------------------------------------------------------+----------------------------+
| LFH Properties               | Description                                                               | Default                    |
+==============================+===========================================================================+============================+
| lfh.connect.nlp.uri          | NLP service request endpoint.                                             | Endpoint for ACD (us-east) |
+------------------------------+---------------------------------------------------------------------------+----------------------------+
| lfh.connect.nlp.auth         | Camel http component authMethod, authUsername, and authPassword settings. |                            |
+------------------------------+---------------------------------------------------------------------------+----------------------------+
| lfh.connect.nlp.dataformat   | Data format identifier for route (output Kafka topic prefix).             | NLP                        |
+------------------------------+---------------------------------------------------------------------------+----------------------------+
| lfh.connect.nlp.messagetype  | Message type identifier for route (output Kafka topic suffix).            | RESONSE                    |
+------------------------------+---------------------------------------------------------------------------+----------------------------+
| lfh.connect.nlp.request-json | Json request wrapper. "<REPLACE_TOKEN>" is replaced by the unstructured data at runtime. This is for nlp services that require application/json requests. If plain text is supported, this property may be left blank. | ACD json request wrapper. |
+------------------------------+---------------------------------------------------------------------------+----------------------------+

Sample Annotator for Clinical Data (ACD) properties:
----------------------------------------------------
lfh.connect.nlp.uri=https://us-east.wh-acd.cloud.ibm.com/wh-acd/api/v1/analyze/wh_acd.ibm_clinical_insights_v1.0_standard_flow?version=2020-10-22
lfh.connect.nlp.auth=&authMethod=Basic&authUsername=apikey&authPassword=ENC(<YOUR_ENCRYPTED_PASSWORD_HERE>)
lfh.connect.nlp.dataformat=NLP
lfh.connect.nlp.messagetype=RESPONSE
lfh.connect.nlp.request-json=

Sample Google Healthcare NLP service properties:
------------------------------------------------
lfh.connect.nlp.uri=https://healthcare.googleapis.com/v1beta1/projects/PROJECT_ID/locations/LOCATION/services/nlp:analyzeEntities
lfh.connect.nlp.auth=&authMethod=Basic&authUsername=apikey&authPassword=ENC(<YOUR_ENCRYPTED_PASSWORD_HERE>)
lfh.connect.nlp.dataformat=NLP
lfh.connect.nlp.messagetype=RESPONSE
lfh.connect.nlp.request-json={"nlpService":"projects/PROJECT_ID/locations/LOCATION/services/nlp","documentContent": <REPLACE_TOKEN> }

Note: <REPLACE_TOKEN> is replaced at runtime by this route with the plain text body contained within the incoming camel exchange message.

Calling the Route
=================
direct:nlp is invoked as part of a route you define when the exchange message contains unstructured data to be analyzed by an nlp service. This route invokes the direct:storeAndNotify route to persist the canonical response from an nlp service to the data store. The nlp response is wrapped in a LFH message envelope with the canonical response base64-encoded within the data element.
The `FHIR to Text <fhir-to-text.html>`_ route may optionally be enabled to automatically extract unstructured data within FHIR resources persisted in Kafka for nlp analysis via this route.

Results
=======
As a result of calling this route, the canonical response from an nlp service within the message body and LFH message attributes are stored in Kafka. These nlp responses can be retrieved at a later date and employed in a myriad of ways to reveal insights otherwise hidden in unstructured data.
