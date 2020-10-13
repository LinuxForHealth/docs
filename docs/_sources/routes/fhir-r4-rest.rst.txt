FHIR R4
*******

Purpose
========
The LinuxForHealth (LFH) FHIR R4 route allows you to send FHIR R4 data to LFH, store that data as part of the LFH Longitudinal Patient Record (LPR) and notify listeners of the stored data via NATS for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Attribute               | Value                                                               |
+=========================+=====================================================================+
| Route Name              | fhir-r4                                                             |
+-------------------------+---------------------------------------------------------------------+
| URL Property            | lfh.connect.fhir-r4.uri                                             |
+-------------------------+---------------------------------------------------------------------+
| Route URL               | http://{host}:8080/fhir/r4/{resource}                               |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | http://127.0.0.1:8080/fhir/r4/Patient                               |
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
| resource           | ResourceType  | The type of the FHIR R4 JSON resource.                   |
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
| Content-Length     | size of JSON in bytes     |
+--------------------+---------------------------+

Configuration Properties
------------------------
None

Calling the Route
=================
Using REST (e.g. via curl or Postman), send a FHIR resource to the route URL as the body of a POST message.  See the LinuxForHealth Examples postman collection in your connect repo::

    connect/src/test/resources/messages/postman/LinuxForHealth Examples.postman_collection.json 

for an example of calling this route with a Patient resource.

Results
=======
Results are stored in Kafka, viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the LinuxForHealth JSON message you receive upon submitting the FHIR resource.

See Also
========
* `FHIR R4 Tutorial <../tutorials/fhir-r4.html>`_
