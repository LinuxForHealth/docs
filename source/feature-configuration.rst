Feature Configuration
*********************

As described in `Application Configuration <./application-configuration.html>`_, Linux for Health is configured via application properties and environment variables, where environment variables can override application properties.  

Use the following guidance to configure each of the Linux for Health features below.  When running Linux for Health Connect via Gradle, configure a property in connect/src/main/resources/application.properties.  When running Linux for Health Connect in a container, add an environment variable to the container to override the default value of property defined in application.properties.

NATS Subscribers
================
A Linux for Health instance can listen for events from another Linux for Health instance by configuring a NATS subscriber. A NATS subscriber will listen for events from the configured Linux for Health instance, then place them in the Kafka lfh-remote-events topic.

A Linux for Health Kafka consumer will then take each event as it is received in lfh-remote-events and store it in the correct topic for the message type. This creates an exact copy of the data stored in the Linux for Health instance that sent the original NATS message and is used to form a synchronized longitudinal patient record.

NATS subscribers can also be configured in external applications to perform a workflow upon receiving a NATS message, but that custom configuration is out of scope for this document.

To configure a NATS subscriber, configure the lfh.connect.fhir-r4.externalservers property with a comma-delimited list of host:port.

Property::

    lfh.connect.messaging.subscribe.hosts=localhost:4222,1.2.3.4:4222

Environment Variable::

    LFH_CONNECT_MESSAGING_SUBSCRIBE_HOSTS=nats-server:4222,5.6.7.8:4222

+-------------------------+----------------------------------------+---------------------+
|                         | Name                                   | Default Value       |
+=========================+========================================+=====================+
| Property                | lfh.connect.messaging.subscribe.hosts  | localhost:4222      |
+-------------------------+----------------------------------------+---------------------+
| Environment Variable    | LFH_CONNECT_MESSAGING_SUBSCRIBE_HOSTS  | nats-server:4222    |
+-------------------------+----------------------------------------+---------------------+

External FHIR Servers
=====================
The Linux for Health FHIR R4 route can optionally send FHIR resources to external FHIR servers.  To configure external FHIR servers, configure the lfh.connect.fhir-r4.externalservers property with a comma-delimited list of URIs.

Property::

    lfh.connect.fhir-r4.externalservers=http://localhost:9081/fhir-server/api/v4,http://localhost:9083/fhir-server/api/v4

Environment Variable::

    LFH_CONNECT_FHIR-R4_EXTERNALSERVERS=http://localhost:9081/fhir-server/api/v4,http://localhost:9083/fhir-server/api/v4

+-------------------------+----------------------------------------+---------------------+
|                         | Name                                   | Default Value       |
+=========================+========================================+=====================+
| Property                | lfh.connect.fhir-r4.externalservers    | None                |
+-------------------------+----------------------------------------+---------------------+
| Environment Variable    | LFH_CONNECT_FHIR-R4_EXTERNALSERVERS    | None                |
+-------------------------+----------------------------------------+---------------------+
