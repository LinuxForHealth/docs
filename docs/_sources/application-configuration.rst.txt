Application Configuration
*************************

Properties based configuration
==============================
iDAAS-Connect application settings are configured in an application.properties file stored within the project under *src/main/resources*.
iDAAS-Connect settings are stored within the following namespaces:

- idaas.connect.component
- idaas.connect.endpoint

The component namespace specifies Java beans used to enrich route processing. These beans are loaded into the Camel Registry at startup,
and may be used within a processing route or embeedded within a route component's options.

The endpoint namespace specifies connection parameters for iDAAS endpoint URIs.

iDAAS-Connect Component Format
==============================
Component properties format::

    idaas.connect.component.[component name]=[component class]

The component name is the lookup key used to search for the component within the Camel Registry.
The component class is the fully qualified class name for the component. The component class is used to instantiate the component and add it
to Camel Registry.

HL7 Encoder/Decoder example::

    idaas.connect.component.hl7decoder=org.apache.camel.component.hl7.HL7MLLPNettyDecoderFactory
    idaas.connect.component.hl7encoder=org.apache.camel.component.hl7.HL7MLLPNettyEncoderFactory


iDAAS-Connect Endpoint Format
==============================
Endpoint properties format::

    idaas.connect.endpoint.[endpoint name].baseUri=[scheme][context]
    idaas.connect.endpoint.[endpoint name].options=[options]

Endpoint properties are used to convey connection information and provide the convenience of supporting separate properties for the 
baseUri and options. The baseUri typically includes the endpoint scheme (protocol or component) and context. Options are *&* separated which
configure an endpoint's processing route.

Endpoint properties are typically used to provide URIs for processing routes located within the *src/main/java/com/redhat/idaas/connect/routes*.

HL7-V2-MLLP endpoint properties example::

idaas.connect.endpoint.hl7_v2_mllp.baseUri=netty:tcp://localhost:2575
idaas.connect.endpoint.hl7_v2_mllp.options=sync=true&encoders=#hl7encoder&decoders=#hl7decoder


