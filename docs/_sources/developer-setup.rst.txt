Developer Setup
***************

Overview
========
iDAAS Connect provides an opinionated implementation of `Camel Routing <https://camel.apache.org/manual/latest/routes.html>`_, focused
on standard data and messaging formats. iDAAS Connect data processing routes are dynamically built from application.properties
configurations.

An iDAAS Connect data processing route consists of:

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
    git clone https://github.com/idaas-connect/idaas-connect
    cd idaas-connect
    ./gradlew build

The Test Summary is available within the project's build directory in *./build/reports/tests/test/index.html*

The Development Environment
===========================
The development environment provides additional systems and integration targets via a Docker Compose configuration (docker-compose.yml).

Systems include:

* `Kafdrop <https://github.com/obsidiandynamics/kafdrop>`_ - A Kafka Cluster Viewer
* `Kafka <https://kafka.apache.org/>`_ - For real time streaming and messaging

To start the docker compose stack::

    docker-compose up -d

To tail current processing logs::

    docker-compose logs -f 

For additional Docker Compose commands, please refer to the `Official Documentation <https://docs.docker.com/compose/reference/overview/>`_

To access Kafdrop, the Kafka Cluster View, browse to http://localhost:9000

To start the iDAAS Connect application::

    ./gradlew run

To list all available Gradle tasks::

    ./gradlew tasks