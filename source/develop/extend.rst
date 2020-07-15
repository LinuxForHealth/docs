Extending Linux for Health
**************************

Overview
========
These sections should provide you with enough information to easily extend Linux for Health (LFH) to incorporate routes and components that can perfom new processing functions and connect to new services.  For example, you may want to extend LFH to provide a route that users can call to access a new service.  Or you may need to provide a route and a Apache Camel component to access your service.  You could also provide a new route to take data from one source, transform it if necessary, then send it to another service or data store.  The topics below will help you with these design decisions and provide implementation details.

Linux for Health Constructs
===========================
Linux for Health uses the following constructs to process healthcare information.

Routes
------
A Linux for Health route is designed to take data in, perform any required operations on that data (e.g. de-serialize, transform), then do something with that data (e.g. store, notify, forward, etc.).  Routes may be defined using standard protocols, such as REST or HTTP, and may use a `known data format <https://camel.apache.org/components/latest/dataformats/index.html>`_ such as HL7v2 or FHIR R4.  If you are adding access to a service, you will likely need to add a new route to LFH.

Components
----------
If access to your service requires the use of a library (e.g. a custom protocol), you may need to also create a Camel component to access the service.  That component would be called from your route and would handle the connection to your service.  Camel contains hundreds of `components <https://camel.apache.org/components/latest/index.html>`_ that can serve as `examples <https://github.com/apache/camel/tree/master/components>`_.  However, if a service is accessible via HTTP or REST, you probably don't need to create a new component.

If you do need to create a new Camel component, or modify an existing component, consider contributing your changes back to the `Apache Camel project <https://camel.apache.org/components/latest/dataformats/index.html>`_ for others to use.

Exchanges
---------
A Camel `Exchange <https://www.javadoc.io/doc/org.apache.camel/camel-core/2.21.0/org/apache/camel/Exchange.html>`_ is a construct that encapsulates the processing of a single incoming message.  The Exchange instance includes all the input data and headers, as well as exchange properties, for that message.  Exchange properties are recommended for storing data needed by downstream steps in a route.

Processors
----------
A Linux for Health processor encapsulates code for route message processing and takes the message Exchange instance as input.  Within a processor, you can set headers, properties and the message body itself, all of which flow to the next step in the message processing of the route.  When creating your own route, create a processor instead of performing multiple operations inline in a route.  LFH contains many processors that you can use as examples for your new processor in the connect/java/com/linuxforhealth/connect/processor directory of the Linux for Health connect repo.

Steps to Extend
===============
Follow the steps below to extend Linux for Health:

1. Clone and build the Linux for Health repo. See the `Developer Setup <../developer-setup.html>`_ Getting Started step for instructions.
2. Determine if you need a route and component or just a route, based on the discussion above.
3. Create a new component, if needed, by cloning and modifying a Camel component that most closely matches your requirements.  You can find simple Camel components that include only 4 required files as a starting point.
4. Create a new route by cloning and modifying the LFH route that most closely matches your requirements, following the `Route Development <./route-basics.html>`_ guidance.  Be sure to include the steps that are required for LFH routes.
5. Create new processors to encapsulate the route message processing steps by cloning and modifying the LFH processor that most closely matches your requirements.
6. Add any new dependencies to the LFH build.gradle file.  Look at any new classes that you use, determine the Java packages they are in and make sure those packages are in the build.gradle file.
7. Create unit tests and add test services following the guidance in the Testing section below.
8. Build, test and open a PR to contribute to `Linux for Health <https://github.com/LinuxForHealth/connect/pulls>`_!

Testing
=======
Unit Tests
----------
If you are contributing a new route or component to Linux for Health, create unit tests that follow the examples under connect/src/test.  If you are adding a route to LFH, also add your route to connect/src/test/java/com/linuxforhealth/connect/builder/RouteGenerationTest.java, following the examples in that file. This will ensure that your new route can start up successfully as a part of the build process.

Test Services
-------------
To test your new route or component, you may need to add one or more services that can be started up with the Linux for Health services. To do this, create a new directory under connect/container-support/compose for your service and add a docker-compose.yml file in that directory.  That docker-compose.yml file can bring up any services you need for testing.  Then, to start the LFH services along with your services, issue the following command from the connect/container-support/compose directory::

    docker-compose -f ./docker-compose.yml -f ./<your_service>/docker-compose.yml up

In this mode, Ctrl-C will stop all the services when testing is complete.
