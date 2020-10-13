Property Encryption
*******************

Overview
========
In LinuxForHealth (LFH), if you need to use properties to store private information (passwords, client ids, secrets, etc.), you can encrypt those using Jasypt.  The following sections explain how to encrypt your property values and reference those encrypted properties within LFH in a way that Jasypt can decrypt them.

Encryption Using Jasypt
=======================
There are two ways to run Jasypt to encrypt your property values.  You can use the `Camel Jasypt jar <https://people.apache.org/~dkulp/camel/jasypt.html#>`_ if you want to clone and build Camel, but this can be time-consuming.  You can also download Jasypt and run the encrypt.sh utility via the following instructions.

1. Download `Jasypt <https://github.com/jasypt/jasypt>`_ and unzip it.

2. cd to the Jasypt bin directory and make encrypt.sh executable::

    chmod +x encrypt.sh

3. Run encrypt.sh to encrypt your property, using the LFH password shown (for development only)::

    ./encrypt.sh input="value to encrypt" password="ultrasecret"

4. Add your encrypted value to your property in the LFH connect/container-support/compose/application.properties file::

    property=ENC(encrypted_value)

   Example::

    lfh.connect.my_route.password=ENC(lY4mNzx+VVFQ71k54tt/KF4OA2PFtEgnLlkMM+j78nU=)

Using Encrypted Properties
==========================
To use your Jasypt-encrypted property in a LinuxForHealth processor, use the CamelContextSupport class::

    String password = camelContextSupport.getProperty("lfh.connect.my_route.password");

To use your encrypted property in a Java DSL route, use the simple() function::

    .setProperty("password", simple("${properties:lfh.connect.my_route.password}")

Changing the Master Password
============================
For development, using the LinuxForHealth master password will allow you to automatically decrypt your properties without further configuration.  

For production environmments, set the JASYPT_ENCRYPTION_PASSWORD environment variable to your encryption password on LFH startup.::

    JASYPT_ENCRYPTION_PASSWORD=<your_new_password> ./gradlew run

Note: If you change the LFH master password, the `Blue Button 2.0 tutorial <../tutorials/blue-button-20.html>`_ will no longer work and you will see EncryptionOperationNotPossibleException.  In this case, you could register with Blue Button to get your own client ID and password, then encrypt those with your own master password, replacing the values for linuxforhealth.connect.endpoint.bluebutton_20_rest.clientId and linuxforhealth.connect.endpoint.bluebutton_20_rest.clientSecret in the LFH application.properties with your own.
