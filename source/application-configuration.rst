Application Configuration
*************************

Properties based configuration
==============================
Linux for Health application settings are configured in an application.properties file stored within the project under *src/main/resources*.
Linux for Health properties reside within the following namespaces:

- lfh.connect.bean
- lfh.connect.[route id]

The bean namespace specifies Java beans used to enrich route processing.
The route namespace contains properties used to drive and configure a data processing route.

Linux for Health Bean Property Format
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

Linux for Health utilizes the "bean" term as this is the term used in the underlying Camel framework when binding objects to a registry.

Beans are used within Linux for Health to assist with route and message processing. For example, the following properties define HL7 decoder
and encoder beans used to in HL7 messaging processing:: 

    lfh.connect.bean.hl7decoder=org.apache.camel.component.hl7.HL7MLLPNettyDecoderFactory
    lfh.connect.bean.hl7encoder=org.apache.camel.component.hl7.HL7MLLPNettyEncoderFactory

Linux for Health Route Property Format
======================================
The route property format follows the convention::

    lfh.connect.[route id].[property]

Where **[route id]** is the route id defined in the Java DSL and **[property]** is the setting's property key. 

*Note that using a route's [route id] within the properties file is a convention, not a strict requirement*.

Linux for Health provides support for `Camel's Property Placeholder Component <https://camel.apache.org/manual/latest/using-propertyplaceholder.html#UsingPropertyPlaceholder-ExamplesUsingPropertiesComponent>`_ which allows
developers to use `Simple Expressions <https://camel.apache.org/components/latest/languages/simple-language.html>`_ within the property files and the `Java DSL <https://camel.apache.org/manual/latest/java-dsl.html>`_.

In the HL7v2 MLLP example below, properties are defined for the route's consumer host and uri. Note that the **uri** property employs the simple expression templating tags
to reuse the host property.

HL7v2 MLLP Example::

    lfh.connect.hl7_v2_mllp.host=0.0.0.0:2575
    lfh.connect.hl7_v2_mllp.uri=netty:tcp://{{lfh.connect.hl7_v2_mllp.host}}?sync=true&encoders=#hl7encoder&decoders=#hl7decoder

Within the HL7v2 MLLP route, the simple templating tags are used to reference the **uri** property::

    @Override
    public void configure() {
        from("{{lfh.connect.hl7_v2_mllp.uri}}")
                .routeId(HL7_V2_MLLP_ROUTE_ID)
                .unmarshal().hl7()
                .process(new Hl7v2MetadataProcessor())
                .to("direct:storeandnotify");
    }

Routes which utilize the REST DSL, such as the FHIR R4 REST route, parse properties using a convenience function in `CamelContextSupport <https://github.com/LinuxForHealth/connect/blob/master/src/main/java/com/linuxforhealth/connect/support/CamelContextSupport.java>`_::

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

Property Evaluation
===================
Properties are loaded from the classpath using the **application.properties** file. Property settings may be overriden by creating an *override* file within **[application working directory]/config/application.properties**
