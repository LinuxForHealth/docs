Developer Setup
***************

Overview
========
Linux for Health provides an opinionated implementation of `Camel Routing <https://camel.apache.org/manual/latest/routes.html>`_, focused
on standard data and messaging formats. Linux for Health data processing routes are dynamically built from application.properties
configurations.

An Linux for Health data processing route consists of:

* A consumer endpoint which receives inbound data
* Optional "components" used to transform, filter, or route data
* Error handling and auditing 
* A Kafka based producer endpoint, used to store data messages
* Additional optional producer endpoints to support "multi-cast" routes when needed

Prerequisites
==============
* Java 1.8
* `Gradle 6.x <https://gradle.org/>`_

Getting Started 
===============
Clone and build the project::

    # clone the repo and confirm the build
    git clone https://github.com/linuxforhealth/connect.git
    cd connect
    ./gradlew build

The Test Summary is available within the project's build directory in *./build/reports/tests/test/index.html*

The Development Environment
===========================
The development environment provides additional systems and integration targets via a Docker Compose configuration (docker-compose.yml).

Systems include:

* `Kafdrop <https://github.com/obsidiandynamics/kafdrop>`_ - A Kafka Cluster Viewer
* `Kafka <https://kafka.apache.org/>`_ - For data storage
* `Nats <https://nats.io/>`_ - For real time event streaming and messaging

Additionally, the development environment can also support the "full stack" including the Linux for Health application, to support end-to-end
testing. The docker compose configurations are stored in the project's `compose <https://github.com/LinuxForHealth/connect/tree/master/container-support/compose>`_
directory.

To run the Linux for Health connect application "locally" with supporting services::

    # navigate to the compose configuration directory
    cd container-support/compose
    # start up supporting services
    docker-compose up -d
    # review logs to ensure that services are available
    docker-compose logs -f
    # return to the project root directory
    cd ../../
    # launch the application
    ./gradlew run

To run the entire Linux for Health application stack within a docker compose configuration::

    # navigate to the compose configuration directory
    cd container-support/compose
    # enable the override file to support the local Linux for Health container
    mv docker-compose.override.disabled docker-compose.override.yml
    # start up supporting services
    docker-compose up -d
    # review logs to ensure that the Linux for Health service is available
    docker-compose logs -f lfh
    # after testing is complete "disable" the override file
    mv docker-compose.override.yml docker-compose.override.disabled

For additional Docker Compose commands, please refer to the `Official Documentation <https://docs.docker.com/compose/reference/overview/>`_

To access Kafdrop, the Kafka Cluster View, browse to http://localhost:9000

To list all available Gradle tasks::

    ./gradlew tasks
