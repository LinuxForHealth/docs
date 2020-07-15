Testing Your Code
*****************

Overview
========
If you are contributing a new route or component to Linux for Health (see `Developing for Linux for Health <./extend.html>`_), create unit tests and add any required test services.

Unit Tests
==========
Create unit tests that follow the examples under connect/src/test.  If you are adding a route to LFH, also add your route to connect/src/test/java/com/linuxforhealth/connect/builder/RouteGenerationTest.java, following the examples in that file. This will ensure that your new route can start up successfully as a part of the build process.

Test Services
=============
To test your new route or component, you may need to add one or more services that can be started up with the Linux for Health services. To do this, create a new directory under connect/container-support/compose for your service and add a docker-compose.yml file in that directory.  That docker-compose.yml file can bring up locally any services that you need for testing.  Then, to start the LFH services along with your services, issue the following command from the connect/container-support/compose directory::

    docker-compose -f ./docker-compose.yml -f ./<your_service>/docker-compose.yml up

In this mode, Ctrl-C will stop all the services when testing is complete.
