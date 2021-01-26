ETL
***

Purpose
========
The LinuxForHealth (LFH) ETL route supports custom text based data formats such as CSV and fixed length. Inbound data is parsed, validated, and transformed prior to transmitting to an external system.
Inbound data messages are parsed and validated using `Bindy <https://www.hl7.org/fhir/http.html#create>`_. Data transformations are implemented using Camel's `Bean Producer <https://camel.apache.org/components/latest/bean-component.html>`_.
The custom inbound and transform components reside within the 'com.linuxforhealth.connect.support.etl' package

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Attribute               | Value                                                               |
+=========================+=====================================================================+
| Route Name              | etl                                                                 |
+-------------------------+---------------------------------------------------------------------+
| URL Property            | lfh.connect.etl.uri                                                 |
+-------------------------+---------------------------------------------------------------------+
| Route URL               | https://{host}:8443/etl                                             |
+-------------------------+---------------------------------------------------------------------+
| Example URL             | https://127.0.0.1:8443/etl                                          |
+-------------------------+---------------------------------------------------------------------+
| Protocol                | REST                                                                |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | POST                                                                |
+-------------------------+---------------------------------------------------------------------+

Configuration
=============

Required Headers
----------------
+--------------------+---------------------------------------------------------------------------------+
| Header             | Value                                                                           |
+====================+=================================================================================+
| ETLMessageType     | specifies the type anf format of data being processed. Example practitioner_csv |
+--------------------+---------------------------------------------------------------------------------+
| Content-Type       | text/csv or text/plain                                                          |
+--------------------+---------------------------------------------------------------------------------+
| Content-Length     | size of JSON in bytes                                                           |
+--------------------+---------------------------------------------------------------------------------+

Configuration Properties
------------------------

The following properties are used in conjunction with the core properties (uri, dataformat, messagetype) to configure the ETL route.

+------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Property                                 | Value                                                                                                                                                               |
+====================+=====================+=====================================================================================================================================================================+
| lfh.connect.bean.[messagetype]format     | specifies the fully qualifed classname for the bean used to parse and validate inbound data. Example: com.linuxforhealth.connect.support.etl.PractitionerCsvFormat  |
+------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.bean.[messagetype]transform  | specifies the fully qualifed classname for the bean used to transform inbound data. Example: com.linuxforhealth.connect.support.etl.PractitionerCsvTransform        |
+------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lfh.connect.etl.producerUri              | the target system's uri/url                                                                                                                                         |
+------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Calling the Route
=================
Using REST (e.g. via curl or Postman), send a file resource to the route URL as the body of a POST message.  See the LinuxForHealth Examples postman collection in your connect repo::

    connect/src/test/resources/messages/postman/LinuxForHealth Examples.postman_collection.json 

for an example of calling this route with a sample CSV file.

Results
=======
Results are stored in Kafka, viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the LinuxForHealth JSON message you receive upon submitting the file resource.
