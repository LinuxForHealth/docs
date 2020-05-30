Installation
************

Prerequisites
==============
* Java 1.8
* `Gradle 6.x <https://gradle.org/>`_

Starting the Development Environment
====================================
The development environment provides additional systems and integration targets via a `Docker Compose configuration <https://github.com/idaas-connect/iDAAS-Connect/docker-compose.yml>`_.

Included Systems

* `Kafdrop <https://github.com/obsidiandynamics/kafdrop/>`_ - A Kafka Cluster Viewer
* Kafka - For real time streaming and messaging

Additional systems will be added as needed.

Clone the iDAAS-Connect project::

    git clone https://github.com/idaas-connect/iDAAS-Connect

To start the docker compose stack::

    docker-compose up -d

To tail current processing logs::

    docker-compose logs -f 

For additional Docker Compose commands, please refer to the `Official Documentation <https://docs.docker.com/compose/reference/overview/>`_

To access Kafdrop, the Kafka Cluster View, browse to http://localhost:9000

To start the iDAAS Connect application::

    ./gradlew clean run
