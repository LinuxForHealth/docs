FHIR TO TEXT
************

Purpose
========
The LinuxForHealth (LFH) FHIR to Text route consumes FHIR resources persisted as Kafka topics for the purpose of extracting unstructured data within to be analyzed by an nlp service.
Unstructured data extracted from FHIR resources is sent to the "direct:nlp" route for processing; see `NLP Route <nlp.html>`_ for details.
The set of FHIR resource topics consumed from Kafka is configurable as shown below.

Details
=======
+-------------------------+----------------------------------------------------------------------------------+
| Attribute               | Value                                                                            |
+=========================+==================================================================================+
| Route Name              | kafka-fhir-to-text                                                               |
+-------------------------+----------------------------------------------------------------------------------+
| Route URI               | kafka:{{lfh.connect.nlp.fhir-topics}}?brokers={{lfh.connect.datastore.brokers}}  |
+-------------------------+----------------------------------------------------------------------------------+
| Example URI             | kafka:FHIR-R4_DOCUMENTREFERENCE,FHIR-R4_DIAGNOSTICREPORT?brokers=localhost:9094  |
+-------------------------+----------------------------------------------------------------------------------+
| Supported Operations(s) | Kafka Consumer Route                                                             |
+-------------------------+----------------------------------------------------------------------------------+

Configuration
=============

The following LFH properties are used to enable this route.

+-----------------------------+--------------------------------------------------------------------+-------------------------------------------------------------------+
| LFH Properties              | Description                                                        | Default                                                           |
+=============================+====================================================================+===================================================================+
| lfh.connect.nlp.enable      | Boolean setting designating whether/not this route runs.           | False                                                             |
+-----------------------------+--------------------------------------------------------------------+-------------------------------------------------------------------+
| lfh.connect.nlp.fhir-topics | Comma separated list of Kafka FHIR resource topics to process.     | FHIR-R4_DOCUMENTREFERENCE,FHIR-R4_DIAGNOSTICREPORT,FHIR-R4_BUNDLE |
+-----------------------------+--------------------------------------------------------------------+-------------------------------------------------------------------+

FHIR Resource Text Extraction
=============================
This route extracts unstructured data from the following FHIR R4 resources and elements therein:
- Any FHIR resource containing XHTML narratives at json path $.text.div
- DocumentReference and DiagnosticReport inline attachments of content type html, pdf, or plain text

Calling the Route
=================
This route need not be invoked by another. Simply enable the route, configure your desired nlp service and FHIR resources will be consumed from Kafka, then unstructured data within will be extracted and sent to the direct:nlp route to be analyzed.
