Orthanc DICOM
*************

Purpose
========
The LinuxForHealth (LFH) Orthanc DICOM route allows you to send DICOM images to LFH, store those images as part of the LFH Longitudinal Patient Record (LPR) and notify listeners of the stored images via NATS for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Attribute               | Value                                                               |
+=========================+=====================================================================+
| Route Name              | orthanc-post                                                        |
+-------------------------+---------------------------------------------------------------------+
| URL Property            | lfh.connect.orthanc.uri                                             |
+-------------------------+---------------------------------------------------------------------+
| Route URL               | https://{host}:8443/orthanc/instances                               |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | https://127.0.0.1:8443/orthanc/instances                            |
+-------------------------+---------------------------------------------------------------------+
| Protocol                | HTTP                                                                |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | POST                                                                |
+-------------------------+---------------------------------------------------------------------+

Configuration
=============

Path Parameters
---------------
None

Query Parameters
----------------
None

Required Headers
----------------
+--------------------+---------------------------+
| Header             | Value                     |
+====================+===========================+
| Content-Type       | application/octet-stream  |
+--------------------+---------------------------+
| Content-Length     | size of file in bytes     |
+--------------------+---------------------------+

Configuration Properties
------------------------
None

Calling the Route
=================
Using HTTP (e.g. via curl or Postman), send a DICOM file to the route URL as the body of a POST message.  See the LinuxForHealth Examples postman collection in your connect repo::

    connect/src/test/resources/messages/postman/LinuxForHealth Examples.postman_collection.json 
    
for an example of calling this route with a DICOM image file.

Results
=======
Results are stored in Kafka, viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the LinuxForHealth JSON message you receive upon submitting the FHIR resource.
