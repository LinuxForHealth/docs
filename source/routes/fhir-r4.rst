FHIR R4
*******

Purpose
========
The Linux for Health (LFH) FHIR R4 route allows you to send FHIR R4 data to Linux for Health, store that data as part of the LFH Longitudinal Patient Record (LPR) and notify listeners of the stored data via NATS for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Attribute               | Value                                                               |
+=========================+=====================================================================+
| Route Name              | fhir-r4                                                             |
+-------------------------+---------------------------------------------------------------------+
| URL Property            | lfh.connect.fhir_r4_rest.uri                                        |
+-------------------------+---------------------------------------------------------------------+
| Default URL             | http://0.0.0.0:8080/fhir/r4/{resource}                              |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | http://0.0.0.0:8080/fhir/r4/Patient                                 |
+-------------------------+---------------------------------------------------------------------+
| Protocol                | REST                                                                |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | POST                                                                |
+-------------------------+---------------------------------------------------------------------+

Configuration
=============

Path Parameters
---------------
+--------------------+---------------+----------------------------------------------------------+
| Parameter          | Type          | Description                                              |
+====================+===============+==========================================================+
| resource           | ResourceType  | The type of the FHIR R4 JSON resource to save.           |
+--------------------+---------------+----------------------------------------------------------+

Query Parameters
----------------
None

Required Headers
----------------
+--------------------+---------------------------+
| Header             | Value                     |
+====================+===========================+
| Content-Type       | application/json          |
+--------------------+---------------------------+

Configuration Properties
------------------------
None

Calling the Route
=================
Using REST (e.g. via curl or Postman), send a FHIR resource to the route URL as the body of a POST message.  See the `FHIR R4 Postman collection <https://github.com/LinuxForHealth/connect/blob/master/src/test/resources/messages/postman/Linux%20for%20Health%20FHIR%20R4%20Tutorial.postman_collection.json>`_ for an example of calling this route with a Patient resource.

Results
=======
Results are stored in Kafka, viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the Linux for Health JSON message you receive upon submitting the FHIR resource.

See Also
========
* `FHIR R4 Tutorial <../tutorials/fhir-r4.html>`_
