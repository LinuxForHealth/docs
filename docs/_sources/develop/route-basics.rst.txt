Developing Routes
*****************

Overview
========
Linux for Health (LFH) contains routes for common healthcare data formats, protocols and services, such as FHIR R4, HL7v2 and CMS Blue Button 2.0.  Routes are created as RouteBuilder classes in the connect/java/com/linuxforhealth/connect/builder directory of the Linux for Health connect repo.

Route Basics
============
The LFH FHIR R4 route configuration method, shown here, defines a simple REST route that receives FHIR R4 resources and stores them::

    @Override
    public void configure() {

        CamelContextSupport contextSupport = new CamelContextSupport(getContext());
        URI fhirBaseUri = URI.create(contextSupport.getProperty("lfh.connect.fhir_r4_rest.uri"));

        restConfiguration()
                .host(fhirBaseUri.getHost())
                .port(fhirBaseUri.getPort());

        rest(fhirBaseUri.getPath())
                .post("/{resource}")
                .route()
                .routeId(FHIR_R4_ROUTE_ID)
                .unmarshal().fhirJson("R4")
                .process(new FhirR4MetadataProcessor())
                .to("direct:storeandnotify");
    }

configure()
-----------
The Linux for Health route builder configure() method contains the route and the following additional elements:

+-----------------------------------+---------------------------------------------+--------------------+
| Step                              | Explanation                                 | Required/Optional  |
+===================================+=============================================+====================+
| contextSupport                    | |contextSupport_def|                        | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| fhirBaseUri                       | |baseUri_def|                               | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| setFhirR4Metadata                 | |metadata_def|                              | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| restConfiguration().host().port() | |restconfig_def|                            | Required           |
+-----------------------------------+---------------------------------------------+--------------------+

.. |contextSupport_def| replace:: The CamelContextSupport class provides convenience methods for working with the underlying CamelContext, including parsing application.property settings.

.. |baseUri_def| replace:: The URI for this route.  You will have your own URI for your route.

.. |metadata_def| replace:: An LFH Processor instance that sets the expected LFH message headers as exchange properties.  This step will vary slightly between routes, so you will likely need to create a specific processor for your route.

.. |restconfig_def| replace:: Configures the host and port for this REST route.  The LFH host and port may be the same for all LFH routes.

Route
-----
The Linux for Health route contains the following steps:

+-----------------------------------+---------------------------------------------+--------------------+
| Step                              | Explanation                                 | Required/Optional  |
+===================================+=============================================+====================+
| rest(fhirBaseUri.getPath())       | |restUri_def|                               | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| post("/{resource}")               | |restOp_def|                                | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| route()                           | |route_def|                                 | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| routeId()                         | |routeId_def|                               | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| unmarshal()                       | |unmarshall_def|                            | Optional           |
+-----------------------------------+---------------------------------------------+--------------------+
| process(setFhirR4Metadata)        | |setMetadata_def|                           | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| to("direct:storeandnotify")       | |storeNotify_def|                           | Required           |
+-----------------------------------+---------------------------------------------+--------------------+

.. |restUri_def| replace:: Defines the URL within LFH that will accept REST calls for this route.

.. |restOp_def| replace:: Defines the REST operation supported by this route and the path parameter.  One route may support multiple REST operations, but in this case only POST is supported.

.. |route_def| replace:: Embeds a Camel route in the REST processing.

.. |routeId_def| replace:: Specifies a unique LFH route name for the embedded route.

.. |unmarshall_def| replace:: De-serializes the input data to FHIR R4 JSON.

.. |setMetadata_def| replace:: Sets the expected LFH message headers as exchange properties.  This step will vary slightly between routes, so you will likely need to create a specific processor for your route.

.. |storeNotify_def| replace:: Encapsulates the storage of the LFH message properties and message body in Kafka and the notification of that storage via NATS.  Your route should include this step at or near the end.
