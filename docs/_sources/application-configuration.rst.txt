Application Configuration
*************************

TBD - Update to discuss config.py



Properties based configuration
==============================
LinuxForHealth application settings are configured in an application.properties file stored within the project under *src/main/resources*.
LinuxForHealth properties reside within the following namespaces:

- lfh.connect.bean
- lfh.connect.[route id]

The bean namespace specifies Java beans used to enrich route processing.
The route namespace contains properties used to drive and configure a data processing route.
Within the route namespace, the following properties are required:

- lfh.connect.[route id].uri
- lfh.connect.[route id].dataformat
- lfh.connect.[route id].messagetype

LinuxForHealth Bean Properties
=====================================
Bean properties format::

    lfh.connect.bean.[bean name]=[bean class]

The **[bean name]** is the lookup key used to search for the bean within the Camel Registry.
The **[bean class]** is the fully qualified class name for the bean, which is used to instantiate the bean/POJO and add it
to the Camel Registry.

A bean, or "Java Bean", is simply a Java class which adheres to the following principles:
- Has a default, no-argument constructor.
- Utilizes accessors, getters and setters, to access and set properties.
- Implements the Serializable interface. 

In contrast, a POJO, or Plain Old Java Object, is a Java class which does not adhere to a specific convention. 

LinuxForHealth utilizes the "bean" term as this is the term used in the underlying Camel framework when binding objects to a registry.

Beans are used within LinuxForHealth to assist with route and message processing. For example, the following properties define HL7 decoder
and encoder beans used to in HL7 messaging processing:: 

    lfh.connect.bean.hl7decoder=org.apache.camel.component.hl7.HL7MLLPNettyDecoderFactory
    lfh.connect.bean.hl7encoder=org.apache.camel.component.hl7.HL7MLLPNettyEncoderFactory

LinuxForHealth Route Properties
======================================
Route properties format::

    lfh.connect.[route id].[property]

Where **[route id]** is the route id defined in the Java DSL and **[property]** is the setting's property key.
LFH property keys consist of lower-case characters separated by "." to provide namespacing.
A namespace may contain a "-" if needed.

The following route properties are **required**

+------------------------------------+----------------------------------------------------+
| Property                           | Description                                        |
+====================================+====================================================+
| lfh.connect.[route id].uri         | The consumer URI for the route                     |
+------------------------------------+----------------------------------------------------+
| lfh.connect.[route id].dataformat  | The data format (HL7, FHIR, etc) used in the route |
+------------------------------------+----------------------------------------------------+
| lfh.connect.[route id].messagetype | The type of message within the format              |
+------------------------------------+----------------------------------------------------+

The **uri** property supports `Camel's Property Placeholder Component <https://camel.apache.org/manual/latest/using-propertyplaceholder.html#UsingPropertyPlaceholder-ExamplesUsingPropertiesComponent>`_ which allows developers to use `Simple Expressions <https://camel.apache.org/components/latest/languages/simple-language.html>`_ within the route's `Java DSL <https://camel.apache.org/manual/latest/java-dsl.html>`_ .

In the HL7v2 MLLP example below, properties are defined for the route's consumer host and uri. Note that the **uri** property employs the simple expression templating tags to reuse the host property.

HL7v2 MLLP Example::

    lfh.connect.hl7-v2.host=0.0.0.0:2575
    lfh.connect.hl7-v2.uri=netty:tcp://{{lfh.connect.hl7-v2.host}}?sync=true&encoders=#hl7encoder&decoders=#hl7decoder

Within the HL7v2 MLLP route, the simple templating tags are used to reference the **uri** property::

    @Override
    public void buildRoute(routePropertyNamespace) {
        from("{{lfh.connect.hl7-v2.uri}}")
                .routeId(HL7_V2_MLLP_ROUTE_ID)
                .unmarshal().hl7()
                .process(new MetaDataProcessor(routePropertyNamespace))
                .to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)
                .id(ROUTE_PRODUCER_ID);;
    }

Routes which utilize the REST DSL, such as the FHIR R4 REST route, parse properties using a convenience function in `CamelContextSupport <https://github.com/LinuxForHealth/connect/blob/master/src/main/java/com/linuxforhealth/connect/support/CamelContextSupport.java>`_::

    @Override
    public void buildRoute(String routePropertyNamespace) {

        CamelContextSupport contextSupport = new CamelContextSupport(getContext());
        String fhirUri = ctxSupport.getProperty("lfh.connect.fhir-r4.uri");
        rest(fhirUri)
                .post("/{resource}")
                .route()
                .routeId(FHIR_R4_ROUTE_ID)
                .unmarshal().fhirJson("R4")
                .marshal().fhirJson("R4")
                .process(new MetaDataProcessor(routePropertyNamespace))
                .to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)
                .id(ROUTE_PRODUCER_ID);
    }

The route's **messagetype** property supports simple expressions, which may be used to dynamically assign a value during route processing. In the  example below, the HL7 route's parses its **messageFormat** from the Camel message header::

    lfh.connect.hl7-v2.messagetype=\${header.CamelHL7MessageType}

The "$" is escaped using a "\\" to ensure that the expression is not evaluated using Groovy's templating syntax.

Property Evaluation
===================
Properties are resolved within LFH using a multi-pass approach which supports property overriding.

#. First, properties are loaded from the class path from the **application.properties** file within the LFH application jar.
#. Next, properties are loaded from an external file **[application working directory]/config/application.properties**.
#. Finally, properties are loaded from environment variables.

LFH environment variable names are use upper-case characters with "_" (underscore) separators. LFH environment variables are translated to property settings by translating characters to lower-case and replacing "_" separators with a "."::

    # environment variable
    LFH_CONNECT_FHIR-R4_URI=http://0.0.0.0:8080/fhir/r4

    # translated property
    lfh.connect.fhir-r4.uri=http://0.0.0.0:8080/fhir/r4

