Direct Store and Notify
***********************

Purpose
========
The Linux for Health (LFH) direct:storeAndNotify route allows you to store transaction data as part of the LFH Longitudinal Patient Record (LPR) and notify listeners of the stored data via NATS for downstream integration.

Details
=======
+-------------------------+---------------------------------------------------------------------+
| Attribute               | Value                                                               |
+=========================+=====================================================================+
| Route Name              | direct:storeAndNotify                                               |
+-------------------------+---------------------------------------------------------------------+
| Supported Operation(s)  | Camel direct invocation                                             |
+-------------------------+---------------------------------------------------------------------+

Configuration
=============
The direct:storeAndNotify route expects the following input state in the Camel message Exchange.

Exchange State
--------------
+-------------------------+--------------------------------------------------------------------+
| Element                 | Description                                                        |
+=========================+====================================================================+
| Message Properties      | |msgconfig_def|                                                    |
+-------------------------+---------------+----------------------------------------------------+
| Message Body            | |msgbody_def|                                                      |
+-------------------------+---------------+----------------------------------------------------+

.. |msgconfig_def| replace:: LFH message properties are stored as Exchange properties.  Before direct:storeAndNotify is called, these Exchange properties must be set via the Linux for Health MetaDataProcessor. This ensures that the message will be stored in the data store using the LFH standard format.

.. |msgbody_def| replace:: The message body contains the data to be stored in the data store, e.g. a HL7v2 message or a FHIR R4 resource.

Calling the Route
=================
direct:storeAndNotify is invoked as a part of a route you define, at or near the end of your route, once the incoming message is fully processed.  In this example, the LFH HL7v2 route unmarshals the data, does further processing, then invokes direct:storeAndNotify to store the HL7v2 input message and LFH message properties in the data store::

    protected void buildRoute(String routePropertyNamespace) {
        from("{{lfh.connect.hl7_v2_mllp.uri}}")
                .routeId(ROUTE_ID)
                .unmarshal().hl7()
                .process(new MetaDataProcessor(routePropertyNamespace))
                .to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)
                .id(ROUTE_PRODUCER_ID);
    }

Results
=======
As a result of calling this direct:storeAndNotify route, the message body and LFH message attributes are stored in Kafka and a message indicating the location in Kafka is sent via NATS to subscribers.  The data in Kafka is viewable via the `Kafdrop viewer <http://localhost:9000/>`_ at the topic, partition and offset shown in the Linux for Health JSON message you receive upon submitting the FHIR resource.  

See Also
========
* `Linux for Health Message Structure <../message-structure.html>`_
