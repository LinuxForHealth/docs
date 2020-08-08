Developing for Linux for Health
*******************************

Overview
========
This discussion should provide you with enough information to easily extend Linux for Health (LFH) to incorporate routes and components that can perfom new processing functions and connect to new services.  For example, you may want to extend LFH to provide a route that users can call to access a new service.  Or you may need to provide a route and a Apache Camel component to access your service.  You could also provide a new route to take data from one source, transform it if necessary, then send it to another service or data store.  The topics below will help you with these design decisions and provide implementation details.

Linux for Health Constructs
===========================
Linux for Health uses the following constructs to process healthcare information.

Routes
------
A Linux for Health route is designed to ingest healthcare data, perform any required operations on that data (e.g. de-serialize, transform, enrich), then do something with that data (e.g. store, notify, forward).  Routes may be defined using standard protocols, such as REST or HTTP, and may use a `known data format <https://camel.apache.org/components/latest/dataformats/index.html>`_ such as HL7v2 or FHIR R4.  If you are adding access to a service, or a data format not yet support in LFH, you will likely need to add a new route.

Components
----------
If access to your service requires the use of a library (e.g. a custom protocol), you may need to also create a Camel component to access the service.  That component would be called from your route and would handle the connection to your service.  Camel contains hundreds of `components <https://camel.apache.org/components/latest/index.html>`_ that can serve as `examples <https://github.com/apache/camel/tree/master/components>`_.  However, if a service is accessible via HTTP or REST, you probably don't need to create a new component.

If you do need to create a new Camel component, or modify an existing component, consider contributing your changes back to the `Apache Camel project <https://camel.apache.org/components/latest/dataformats/index.html>`_ for others to use.

Exchanges
---------
A Camel `Exchange <https://www.javadoc.io/doc/org.apache.camel/camel-core/2.21.0/org/apache/camel/Exchange.html>`_ is a construct that encapsulates the processing of a single incoming message.  The Exchange instance includes all the input data and headers, as well as exchange properties, for that message.  Exchange properties are recommended for storing data needed by downstream steps in a route.

Processors
----------
A Linux for Health processor encapsulates code for route message processing and takes the message Exchange instance as input.  Within a processor, you can set headers, properties and the message body itself, all of which flow to the next step in the message processing of the route.  

Processors may be defined inline, within the context of a route, or as a separate class. Inline processors are preferred when the processor's operations are route specific. Processsor classes are recommended if the processor is able to be reused across routes. Examples of "reusable" processor class implementations are found within the connect/java/com/linuxforhealth/connect/processor directory of the Linux for Health connect repo.

Development Steps
=================
Follow the steps below to extend Linux for Health:

1. Clone and build the Linux for Health repo. See the `Developer Setup <../developer-setup.html>`_ Getting Started step for instructions.
2. Determine if you need a route and component or just a route, based on the discussion above.
3. Create a new component, if needed, by cloning and modifying a Camel component that most closely matches your requirements.  You can find simple Camel components that include only 4 required files as a starting point.
4. Determine if you need to create a new route, or modify an existing route. New routes are created to support new LFH services and data formats. Existing routes are updated to integrate the route, or data format, with a new target system, or producer. Consult the `Route Development <./route-basics.html>`_ for additional guidance.
5. Add any new dependencies to the LFH build.gradle file, following the guidance in `Building Your Code <./build.html>`_.
6. Create unit tests and add test services, following the guidance in `Testing Your Code <./test.html>`_.
7. Build, test, then open a feature issue and a pull request against that feature to contribute your changes to `Linux for Health <https://github.com/LinuxForHealth/connect>`_!
