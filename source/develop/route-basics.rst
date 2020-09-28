Developing Routes
*****************

Overview
========
Linux for Health (LFH) contains routes for common healthcare data formats, protocols and services, such as FHIR R4, HL7v2 and CMS Blue Button 2.0.  LFH routes reside within the connect/java/com/linuxforhealth/connect/builder directory of LFH connect repo. 

Route Basics
============
LFH routes extend the BaseRouteBuilder class. `BaseRouteBuilder <https://github.com/LinuxForHealth/connect/blob/master/src/main/java/com/linuxforhealth/connect/builder/BaseRouteBuilder.java/>`_ is an abstract class which provides property validations, error handling configuration, and a method for defining LFH routes. The LFH FHIR R4 route, shown here, defines a simple REST route that receives FHIR R4 resources and stores them::

    public final static String ROUTE_ID = "fhir-r4";
    public final static String ROUTE_PRODUCER_ID = "fhir-r4-producer-store-and-notify";


    @Override
    protected String getRoutePropertyNamespace() {
        return "lfh.connect.fhir-r4";
    }


    @Override
    protected void buildRoute(String routePropertyNamespace) {
        CamelContextSupport contextSupport = new CamelContextSupport(getContext());
        String fhirUri = ctxSupport.getProperty("lfh.connect.fhir-r4.uri");
        rest(fhirUri)
                .post("/{resource}")
                .route()
                .routeId(ROUTE_ID)
                .unmarshal().fhirJson("R4")
                .marshal().fhirJson("R4")
                .process(new MetaDataProcessor(routePropertyNamespace))
                .to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)
                .id(ROUTE_PRODUCER_ID);
    }

ROUTE_ID and ROUTE_PRODUCER_ID
------------------------------
Each route implementation should have publically accessible "constants" for it's route and producer endpoint's id. These ids are used within the route definition and for mocking/intercepting endpoints in unit tests.

getRoutePropertyNamespace()
---------------------------
Returns the "property namespace", or property prefix, for the route's property settings. This value aligns with the route's application.property settings.

buildRoute(String routePropertyNamespace)
-----------------------------------------
The Linux for Health route builder buildRoute() method contains the route and the following additional elements:

+-----------------------------------+---------------------------------------------+--------------------+
| Step                              | Explanation                                 | Required/Optional  |
+===================================+=============================================+====================+
| contextSupport                    | |contextSupport_def|                        | Required           |
+-----------------------------------+---------------------------------------------+--------------------+
| fhirBaseUri                       | |baseUri_def|                               | Required           |
+-----------------------------------+---------------------------------------------+--------------------+

.. |contextSupport_def| replace:: The CamelContextSupport class provides convenience methods for working with the underlying CamelContext, including parsing application.property settings.

.. |baseUri_def| replace:: The URI for this route.  You will have your own URI for your route.


Route
-----
The Linux for Health route contains the following steps:

+---------------------------------------------------------------+---------------------------------------------+--------------------+
| Step                                                          | Explanation                                 | Required/Optional  |
+===================================+===========================+=============================================+====================+
| rest(fhirUri)                                                 | |restUri_def|                               | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| post("/{resource}")                                           | |restOp_def|                                | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| route()                                                       | |route_def|                                 | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| routeId()                                                     | |routeId_def|                               | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| unmarshal()                                                   | |unmarshall_def|                            | Optional           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| marshal()                                                     | |marshall_def|                              | Optional           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| |process_def|                                                 | |setMetadata_def|                           | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| |to_def|                                                      | |storeNotify_def|                           | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+
| id(ROUTE_PRODUCER_ID);                                        | |producerId_def|                            | Required           |
+---------------------------------------------------------------+---------------------------------------------+--------------------+

.. |process_def| replace:: process(new MetaDataProcessor(routePropertyNamespace))

.. |to_def| replace:: to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)

.. |restUri_def| replace:: Defines the URL within LFH that will accept REST calls for this route.

.. |restOp_def| replace:: Defines the REST operation supported by this route and the path parameter.  One route may support multiple REST operations, but in this case only POST is supported.

.. |route_def| replace:: Embeds a Camel route in the REST processing.

.. |routeId_def| replace:: Specifies a unique LFH route name for the embedded route.

.. |unmarshall_def| replace:: De-serializes the input data to FHIR R4 JSON data models.

.. |marshall_def| replace:: Serializes FHIR R4 JSON data models to JSON.

.. |setMetadata_def| replace:: Sets the expected LFH message headers as exchange properties.  The MetaDataProcessor is a common and required processor for all LFH routes.

.. |storeNotify_def| replace:: Encapsulates the storage of the LFH message properties and message body in Kafka and the notification of that storage via NATS.  Your route should include this step at or near the end.

.. |producerId_def| replace:: Specifies a unique name for the producer endpoint within a LFH route.
