Developer Setup
***************

Overview
========
Linux for Health provides an opinionated implementation of `Camel Routing <https://camel.apache.org/manual/latest/routes.html>`_, focused
on standard data and messaging formats. 

A Linux for Health data processing route consists of:

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
The development environment provides additional systems and integration targets via a Docker Compose configuration, docker-compose.yml), and "stack" specific override files.

LFH Supporting Systems include:

* `Kafdrop <https://github.com/obsidiandynamics/kafdrop>`_ - A Kafka Cluster Viewer
* `Kafka <https://kafka.apache.org/>`_ - For data storage
* `Nats <https://nats.io/>`_ - For real time event streaming and messaging

LFH supports three Docker Compose container stacks: dev, server, and pi. 

The dev stack supports the local development environment. LFH supporting services run in containers with host port mappings.
The server stack includes a containerized LFH connect application in addition to LFH supporting services.
Finally, the pi stack supports optimized containers for arm64/Raspberry Pi usage.

Stacks are supported using Docker Compose file overrides and the `COMPOSE_FILE <https://docs.docker.com/compose/reference/envvars/#compose_file>`_ variable.

To run the Linux for Health connect application "locally", use the dev stack::

    # navigate to the compose configuration directory
    cd container-support/compose
    # start up the dev stack (default)
    # execute the script within the current shell to ensure that the compose CLI works as expected
    . ./start-stack.sh
    # review logs to ensure that services are available
    docker-compose logs -f
    # return to the project root directory
    cd ../../
    # launch the application
    ./gradlew run

To run the entire Linux for Health application stack, use the server stack::

    # navigate to the compose configuration directory
    cd container-support/compose
    # start the server stack
    . ./start-stack.sh server
    # review logs to ensure that the Linux for Health service is available
    docker-compose logs -f lfh

For additional Docker Compose commands, please refer to the `Official Documentation <https://docs.docker.com/compose/reference/overview/>`_

To access Kafdrop, the Kafka Cluster View, browse to http://localhost:9000

To list all available Gradle tasks::

    ./gradlew tasks
