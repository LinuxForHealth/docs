FHIR R4 TO ACD
**************

Purpose
========
The Linux for Health (LFH) FHIR R4 to ACD (Annotator for Clinical Data) route extracts unstructured data from FHIR Resources such as DocumentReference and sends the unstructured data to the "direct:acd-analyze" route.

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
None

Configuration Properties
------------------------
None

Calling the Route
=================
Using a Camel ProducerTemplate or a ``to()`` operation within a route, send FHIR resources to the "direct:fhir-r4-to-acd" consumer uri. For supported FHIR resources known to contain unstructured data (e.g DocumentReference), the unstructured data is extracted and sent to the ACD analyze route. For all other FHIR resources sent to this route, a warning message is logged and nothing is sent to ACD for analysis.

Results
=======
The unstructured data this route extracts from FHIR resources is sent to the ACD route, which sends the unstructured data to the ACD service for analysis. This route does not persist the unstructured data extracted from FHIR resources.

See Also
========
