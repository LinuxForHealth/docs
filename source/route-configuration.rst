Route Configuration
*******************

Properties based configuration
==============================
iDAAS-Connect data processing routes are configured in the application.properties file.
Properties related to iDAAS-Connect routes use the prefix, **idaas.connect**.
**idaas.connect** properties support iDAAS components, iDAAS consumers, and additional iDAAS producers.

iDAAS-Connect Component Format
==============================
HL7 Encoder/Decoder example::

    idaas.connect.component.hl7decoder=org.apache.camel.component.hl7.HL7MLLPNettyDecoderFactory
    idaas.connect.component.hl7encoder=org.apache.camel.component.hl7.HL7MLLPNettyEncoderFactory

Component properties format::

    idaas.connect.component.[component name]=[component class]

A "component" is a custom data processor used in message processing. Available components include components within the Camel 
framework, and it's supported libraries, or components authored within iDAAS-Connect.

iDAAS-Connect Consumer Format
==============================
HL7-MLLP consumer properties example::

    idaas.connect.consumer.hl7-mllp.scheme=netty:tcp
    idaas.connect.consumer.hl7-mllp.context=localhost:2575
    idaas.connect.consumer.hl7-mllp.options=sync=true,encoders=#hl7encoder,decoders=#hl7decoder

Consumer properties format::

    idaas.connect.consumer.[route id].[scheme|context|option]=[scheme|context|option value]

The route id is the logical name for the iDAAS-Connect data processing route.

The scheme, context, and option "fields" are used to generate a URI for the consumer in the underlying Camel framework::

   [scheme]://[context]?[option1=value&option2=value]

Options are "optional", and are separated using `&`. If options are not included the `?` separator is excluded.

The generated consumer URI is::

    netty:tcp://localhost:2575?sync=true&encoders=#hl7encoder&decoders=#hl7decoder

iDAAS-Connect Producer Format
==============================
iDAAS-Connect provides a single Kafka producer, used to store incoming data messages. Additional producers may be included
in the application.properties file and included in the data route.

HL7-MLLP producer properties example::

    idaas.connect.producer.hl7-mllp.0.scheme=stub
    idaas.connect.producer.hl7-mllp.0.context=hl7-stub

Producer properties format::

    idaas.connect.producer.[route id].[producer number][scheme|context|option]=[scheme|context|option value]

The route id is used to associate the producer with a configured consumer.

The `producer number` is a numeric identifier used to associate the property configurations for the producer URI.

The generated producer URI utilized within the Camel framework is::

    stub://hl7-stub
