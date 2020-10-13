Hello World
***********

Overview
========
This tutorial shows you how to build your first route starting with the LinuxForHealth "Hello World" route.  

Prerequisites
=============
* `Developer Setup <../developer-setup.html>`_

Steps
=====
Once you have completed the Prerequisites, follow these steps to build your first LinuxForHealth route.

Hello World Route
-----------------
LinuxForHealth provides a "Hello World" route in ``connect/src/main/java/com/linuxforhealth/connect/builder/ExampleRouteBuilder.java``::

       /**
         * "Hello World" example route.
         */
        from("{{lfh.connect.example.uri}}")
            .routeId(HELLO_WORLD_ROUTE_ID)
            .process(exchange -> {
                String name = simple("${headers.name}").evaluate(exchange, String.class);
                String result = "Hello World! It's "+name+".";
                // Add your code here
                exchange.getIn().setBody(result);
            })
            .process(new MetaDataProcessor(routePropertyNamespace))
            .to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)
            .id(HELLO_WORLD_PRODUCER_ID);

You can run this route using the curl command.  From a console window, copy and paste the following command.  You can change the value of name being passed in as a query param from ``me`` to anything, then hit ``return``::

   curl http://localhost:8080/hello-world?name=me

This sends an HTTP GET request to LinuxForHealth, which is processed by the route code above.  The ``.process`` step gets the value of the query param ``name``, which was passed in as an exchange message header by Camel, then forms the response and sets the exchange message body to the result.

The ``.process(new MetaDataProcessor(routePropertyNamespace))`` and ``.to(LinuxForHealthRouteBuilder.STORE_AND_NOTIFY_CONSUMER_URI)`` are built-in LinuxForHealth steps that form the message to be stored in Kafka based on your data, then send that message to Kafka and then to NATS subscribers for further use.

You should see the following result (all in one line)::

   {
      "meta": {
         "routeId": "hello-world",
         "uuid": "71257051-f2e0-495b-b0d9-beb785b3f850",
         "routeUri": "jetty:http://0.0.0.0:8080/hello-world?httpMethodRestrict=GET",
         "dataFormat": "EXAMPLE",
         "messageType": "TEXT",
         "timestamp": 1599076120,
         "dataStoreUri": "kafka:EXAMPLE_TEXT?brokers=localhost:9094",
         "dataRecordLocation":["EXAMPLE_TEXT-0@0"]
      },
      "data": "SGVsbG8gV29ybGQhIEl0J3MgbWUu"
   }

The data value is base64-encoded.  Decode this with any base64 decoder (several web sites do this) and you get the string "Hello World! It's me."  To recap, you sent an HTTP GET to LinuxForHealth, which stored the result in a topic called EXAMPLE_TEXT.

Modify the Route and Rebuild
----------------------------
In this step, you will modify the "Hello World" route, rebuild it and run it.  First, open ``connect/src/main/java/com/linuxforhealth/connect/builder/ExampleRouteBuilder.java`` in an editor.  Change the line::

   String result = "Hello World! It's "+name+".";

to::

   String result = "It's always sunny in "+name+".";

Save the file and in the console window where you started LinuxForHealth connect via ``./gradlew run``, type ctrl-C to stop it.  Rebuild LinuxForHealth by typing ``./gradlew clean build`` and then ``./gradlew run``.  The run the following command::

  curl http://localhost:8080/hello-world?name=Austin

You should see the following result (all in one line)::

   {
      "meta": {
         "routeId": "hello-world",
         "uuid": "71257051-f2e0-495b-b0d9-beb785b3f850",
         "routeUri": "jetty:http://0.0.0.0:8080/hello-world?httpMethodRestrict=GET",
         "dataFormat": "EXAMPLE",
         "messageType": "TEXT",
         "timestamp": 1599076120,
         "dataStoreUri": "kafka:EXAMPLE_TEXT?brokers=localhost:9094",
         "dataRecordLocation":["EXAMPLE_TEXT-0@0"]
      },
      "data": "SXQncyBhbHdheXMgc3VubnkgaW4gQXVzdGluCg=="
   }

Decode the data field with any base64 decoder and you get the string "It's always sunny in Austin".

So far, you've run a route, modified it, rebuilt it and run it again to see your changes.  As a next step, you can copy the whole ``connect/src/main/java/com/linuxforhealth/connect/builder/ExampleRouteBuilder.java`` file, rename it in the same directory and use it as a basis for a brand new route, taking care to create any new environment variables in ``src/main/resources/application.properties`` when needed.

Also, as you work more with routes, consider installing `Postman <http://postman.com>`_ and importing the LinuxForHealth Postman collection ``connect/src/test/resources/messages/postman/LinuxForHealth Examples.postman_collection.json`` to make HTTP and REST calls.
