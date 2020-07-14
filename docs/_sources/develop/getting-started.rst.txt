Extending Linux for Health
**************************

Overview
========
These sections should provide you with enough information to easily extend Linux for Health (LFH) to incorporate routes and components that can perfom new processing functions and connect to new services.  For example, you may want to extend LFH to provide a route that users can call to access a new service.  Or you may need to provide a route and a Camel component to access your service.  The topics below will help you with these design decisions and provide implementation details.

Linux for Health Constructs
===========================
Linux for Health uses the following constructs to process healthcare information.

Routes
------
A Linux for Health route is designed to take data in, perform any required operations on that data (e.g. de-serialize, transform), then do something with that data (e.g. store, notify, forward, etc.).  Routes may be defined using standard protocols, such as REST or HTTP, and may use a `known data format <https://camel.apache.org/components/latest/dataformats/index.html>`_ such as HL7v2 or FHIR R4.  If you are adding access to a service, you will likely need to add a new route to LFH.

Components
----------
If access to your service requires the use of a library (e.g. a custom protocol), you may need to also create an Apache Camel component to access the service.  That component would be called from your route and would handle the connection to your service.  Camel contains hundreds of `components <https://camel.apache.org/components/latest/index.html>`_ that can serve as `examples <https://github.com/apache/camel/tree/master/components>`_.  However, if a service is accessible via HTTP or REST, you probably don't need to create a new component.

If you do need to create a new Camel component, or modify an existing component, consider contributing your changes back to the `Apache Camel project <https://camel.apache.org/components/latest/dataformats/index.html>`_ for others to use.

Exchanges
---------
A Camel `Exchange <https://www.javadoc.io/doc/org.apache.camel/camel-core/2.21.0/org/apache/camel/Exchange.html>`_ is a construct that encapsulates the processing of a single incoming message.  The Exchange instance includes all the input data and headers, as well as exchange properties, for that message.  Exchange properties are recommended for storing data needed by downstream steps in a route.

Processors
----------
A Linux for Health processor encapsulates code for route message processing and takes the message Exchange instance as input.  Within a processor, you can set headers, properties and the message body itself, all of which flow to the next step in the message processing of the route.  When creating your own route, create a processor instead of performing multiple operations inline in a route.  LFH contains many processors, which you can use as examples for your new processor, in the connect/java/com/linuxforhealth/connect/builder directory of the Linux for Health connect repo (see the  `Developer Setup <../developer-setup.html>`_ "Getting Started" step for instructions for cloning and building the repo).

Route Basics
============
Linux for Health contains routes for common healthcare protocols and services, such as HL7v2, FHIR R4 and CMS Blue Button 2.0.  Routes are created as RouteBuilder classes in the connect/java/com/linuxforhealth/connect/builder directory of the Linux for Health connect repo.

The LFH FHIR R4 route configuration method, shown here, defines a simple REST route that receives FHIR R4 resources and stores them::

    @Override
    public void configure() {
        EndpointUriBuilder uriBuilder = getEndpointUriBuilder();
        URI fhirBaseUri = URI.create(uriBuilder.getFhirR4RestUri());
        Processor setFhirR4Metadata = new FhirR4MetadataProcessor();

        restConfiguration()
                .host(fhirBaseUri.getHost())
                .port(fhirBaseUri.getPort());

        rest(fhirBaseUri.getPath())
                .post("/{resource}")
                .route()
                .routeId("fhir-r4-rest")
                .unmarshal().fhirJson("R4")
                .process(setFhirR4Metadata)
                .to("direct:storeandnotify");
    }

The configure() Method
----------------------
The configure() method contains the following elements:

- uriBuilder is an EndpointUriBuilder instance that contains helper methods for creating URIs from properties in the LFH application.properties.  You will add your route URI to the LFH application.properties file and add a method to return that URI in EndpointUriBuilder.java. (required)

- fhirBaseUri is the URI for this route.  You will have your own URI for your route. (required)

- setFhirR4Metadata is an LFH Processor instance that sets the expected LFH message headers as exchange properties.  This step will vary slightly between routes, so you will likely need to create a specific processor for your route. (required)

- restConfiguration().host().port() configures the host and port for this REST route.  The host and port will be the same for all LFH routes. (required)

The Route
---------
The route then contains the following steps:

- rest(fhirBaseUri.getPath()) defines the URL within LFH that will accept REST calls for this route.  This URL is defined in the LFH `applictaion.properties file <../application-configuration.rst>`_ and accessed via a method that you will create for your URL in the LFH EnpointURIBuilder class. (required)

- post("/{resource}") defines the REST operation supported by this route and the path parameter {resource}.  One route may support multiple REST operations, but in this case only POST is supported. (required)

- route() embeds a Camel route in the REST processing. (required)

- routeId() specifies a unique LFH route name for the embedded route. (required)

- unmarshal() de-serializes the input data to FHIR R4 JSON.

- process(setFhirR4Metadata) sets the expected LFH message headers as exchange properties.  This step will vary slightly between routes, so you will likely need to create a specific processor for your route. (required)

- to("direct:storeandnotify") encapsulates the storage of the LFH message properties and message body in Kafka and the notification of that storage via NATS.  Your route should include this step at or near the end. (required)

Testing
=======
Unit Tests
----------
If you are contributing a new route or component to Linux for Health, create unit tests that follow the examples under connect/src/test.  If you are adding a route to LFH, also add your route to connect/src/test/java/com/linuxforhealth/connect/builder/RouteGenerationTest.java, following the examples in that file. This will ensure that your new route can start up successfully as a part of the build process.

Test Services
-------------
To test your new route or component, you may need to add one or more services that can be started up with the Linux for Health services. To do this, create a new directory under connect/container-support/compose for your service and add a docker-compose.yml file in that directory.  That docker-compose.yml file can bring up any services you need for testing.  Then, to start the LFH services along with your services, issue the following command from the connect/container-support/compose directory:

    docker-compose -f ./docker-compose.yml -f ./<your_service>/docker-compose.yml up

In this mode, Ctrl-C will stop all the services when testing is complete.
