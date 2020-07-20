FHIR R4
*******

Purpose
========
The Linux for Health (LFH) FHIR R4 route allows you to send FHIR R4 data to Linux for Health, store that data as part of the LFH  Longitudinal Patient Record (LPR) and notify listeners of the stored data via NATS, for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Parameter               | Description                                                         |
+=========================+=====================================================================+
| URL Property            | linuxforhealth.connect.endpoint.fhir_r4_rest.baseUri                |
+-------------------------+---------------------------------------------------------------------+
| Default URL             | http://0.0.0.0:8080/fhir/r4/{resource}                              |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | http://0.0.0.0:8080/fhir/r4/Patient                                 |
+-------------------------+---------------------------------------------------------------------+
| Protocol                | REST                                                                |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | POST                                                                |
+--------------------+--------------------------------------------------------------------------+

Configuration
=============

Path Parameters
---------------
+--------------------+-----------+--------------------------------------------------------------+
| Parameter          | Type      | Description                                                  |
+====================+===========+==============================================================+
| resource           | Resource  | The FHIR R4 JSON resource to save.                           |
+--------------------+-----------+--------------------------------------------------------------+

Query Parameters
----------------
None

Required Headers
----------------
None

Configuration Properties
------------------------
None

Calling the Route
=================
Using REST (e.g. via curl or Postman), send a FHIR resource to the route URL as the body of a POST message.  See 'connect/test/resources/postman/Linux for Health FHIR R4 Tutorial.postman_collection' for an example of calling this route with a Patient resource from Postman.

Results
=======
Results are stored in Kafka, viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the Linux for Health JSON message you recieve upon submitting the FHIR resource.

See Also
========
* `FHIR R4 Tutorial <../tutorials/fhir-r4.html>`_
