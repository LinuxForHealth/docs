Installation
************

==============
Pre-requisites
==============
- Java 1.8
- [Gradle](https://gradle.org/) 6.x

====================================
Starting the Development Environment
====================================
The development environment provides additional systems and integration targets via a [Docker Compose configuration](docker-compose.yml).

Systems include:
- [Kafdrop](https://github.com/obsidiandynamics/kafdrop) - A Kafka Cluster Viewer
- Kafka - For real time streaming and messaging

Additional systems will be added to support the MVP as needed.

To start the docker compose stack
``
docker-compose up -d
``

To tail current processing logs
``
docker-compose logs -f 
``

For additional Docker Compose commands, please refer to the [Official Documentation](https://docs.docker.com/compose/reference/overview/)

To access Kafdrop, the Kafka Cluster View, browser to http://localhost:9000

To start the iDAAS Connect application
``
./gradlew clean run
``
