Route Configuration
*******************

Properties based configuration
==============================
iDAAS-Connect data processing routes are configured in the application.properties file.
Properties related to iDAAS-Connect routes use the prefix, **idaas.connect**.
**idaas.connect** properties support iDAAS components, iDAAS consumers, and additional iDAAS producers.

The properties file configurations provide a convenient means of specifying an iDAAS-Connect data processing route, which is later transformed
into a Camel Endpoint URI. The Camel Endpoint URI has the general form::

    [scheme][context]?[options]

Each component is parsed literally.

The **scheme** maps to the underlying Camel component used to implement the endpoint, including the scheme separator (**:**, **://**, etc)
The **context** is the working area for the component, which varies per component. For example a networking service utilizes a hostname and
port, while a file component uses a directory, etc.
The **options** configure the endpoint's behavior. Please refer to the `Camel Component Documentation <https://camel.apache.org/components/latest/index.html>`_
for a list of available components and configuration options. If options are used, iDAAS-Connect, will append a `?` to the route.

iDAAS-Connect Component Format
==============================
HL7 Encoder/Decoder example::

    idaas.connect.component.hl7decoder=org.apache.camel.component.hl7.HL7MLLPNettyDecoderFactory
    idaas.connect.component.hl7encoder=org.apache.camel.component.hl7.HL7MLLPNettyEncoderFactory

Component properties format::

    idaas.connect.component.[component name]=[component class]

A "component" is a Java Bean utilized  in message processing. Available components include components within the Camel 
framework, or components authored within iDAAS-Connect.

iDAAS-Connect Consumer Format
==============================
HL7-MLLP consumer properties example::

    idaas.connect.consumer.hl7-mllp.scheme=netty:tcp://
    idaas.connect.consumer.hl7-mllp.context=localhost:2575
    idaas.connect.consumer.hl7-mllp.options=sync=true&encoders=#hl7encoder&decoders=#hl7decoder

Consumer properties format::

    idaas.connect.consumer.[route id].[scheme|context|option]=[scheme|context|option value]

The route id is the logical name for the iDAAS-Connect data processing route.
The scheme, context, and option "fields" are used to generate a URI for the consumer in the underlying Camel framework::

   [scheme][context]?[option1=value&option2=value]

Options are "optional", and are usually separated using **&**. If options are not included the **?** separator is excluded. Please refer to 
the Camel Documentation to verify that you are using the correct option separator.

The generated consumer URI is::

    netty:tcp://localhost:2575?sync=true&encoders=#hl7encoder&decoders=#hl7decoder

iDAAS-Connect Producer Format
==============================
iDAAS-Connect provides a single Kafka producer, used to store incoming data messages. Additional producers may be included
in the application.properties file and included in the data route.

HL7-MLLP producer properties example::

    idaas.connect.producer.hl7-mllp.0.scheme=stub://
    idaas.connect.producer.hl7-mllp.0.context=hl7-stub

Producer properties format::

    idaas.connect.producer.[route id].[producer number][scheme|context|option]=[scheme|context|option value]

The route id is used to associate the producer with a configured consumer.
The **producer number** is a numeric identifier used to associate the property configurations for the producer URI.
The generated producer URI utilized within the Camel framework is::

    stub://hl7-stub
