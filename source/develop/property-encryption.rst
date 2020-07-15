Property Encryption
*******************

Overview
========
In Linux for Health (LFH), if you need to use properties to store private information (passwords, client ids, secrets, etc.), you can encrypt those in the LFH application.properties file using Jasypt.  The following sections explain how to encrypt your property values and reference those encrypted properties within LFH in a way that Jasypt can decrypt them.

Encryption Using Jasypt
=======================
While some Camel instructions will tell you to clone and build all of Camel, then use java -jar to run the Camel Jasypt jar, the easiest way to use Jasypt to encrypt your property values is to download it and run the encrypt.sh utility.

1. Download `Jasypt <https://github.com/jasypt/jasypt>`_ then unzip it.

2. cd to the Jasypt bin directory and make encrypt.sh executable::

    chmod +x encrypt.sh

3. Run encrypt.sh to encrypt your property, using the LFH password shown (for development only).::

    ./encrypt.sh input="value to encrypt" password="ultrasecret"

For development, using the password used by Linux for Health will allow you to automatically decrypt your password without further configuration in LFH.  For production environmments, LFH will add the ability to set an environment variable containing the encryption password before starting LFH, then you can delete that environment variable after LFH starts.

4. Add your encrypted value to your property in the LFH connect/container-support/compose/application.properties::

    property=ENC(encrypted_value)

Example::

    linuxforhealth.connect.endpoint.myService.password=ENC(lY4mNzx+VVFQ71k54tt/KF4OA2PFtEgnLlkMM+j78nU=)

Using Encrypted Properties
==========================
To use your Jasypt-encrypted property in a Linux for Health processor, use the SimpleBuilder class::

    String password = SimpleBuilder.simple("${properties:linuxforhealth.connect.endpoint.myService.password}") 
            .evaluate(exchange, String.class);

To use your encrypted property in a Java DSL route, use the Simple() function::

    .setProperty("password", simple("${properties:linuxforhealth.connect.endpoint.myService.password}")
