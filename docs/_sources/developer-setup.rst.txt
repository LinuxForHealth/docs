Developer Setup
***************

Overview
========
LinuxForHealth provides an opinionated implementation of `Camel Routing <https://camel.apache.org/manual/latest/routes.html>`_, focused
on standard data and messaging formats. 

A LinuxForHealth data processing route consists of:

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
The development environment provides additional systems and integration targets via a Docker Compose configuration, docker-compose.yml), and "profile" specific override files.

LFH Supporting Systems include:

* `Kafdrop <https://github.com/obsidiandynamics/kafdrop>`_ - A Kafka Cluster Viewer
* `Kafka <https://kafka.apache.org/>`_ - For data storage
* `Nats <https://nats.io/>`_ - For real time event streaming and messaging

Use docker-compose and gradle to start the local development environment::

    # navigate to the compose configuration directory
    cd container-support/compose
    # execute the compose start script within the current shell
    . ./start-stack.sh
    # review logs
    docker-compose logs -f
    # return to the project root directory
    cd ../../
    # launch LFH
    ./gradlew run

The start-stack.sh script sets the Docker Compose `COMPOSE_FILE <https://docs.docker.com/compose/reference/envvars/#compose_file/>`_ . COMPOSE_FILE specifies the path to each of the configuration files for the current LFH stack profile.
COMPOSE_FILE remains in scope with the current shell session.

To remove the local development environment::

    # remove all containers, networks, and volumes
    docker-compose down -v
    # remove current compose file paths
    unset COMPOSE_FILE

For additional Docker Compose commands, please refer to the `Official Documentation <https://docs.docker.com/compose/reference/overview/>`_

To access Kafdrop, the Kafka Cluster View, browse to http://localhost:9000

To list all available Gradle tasks::

    ./gradlew tasks
