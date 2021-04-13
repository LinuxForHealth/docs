Docker Compose Support
**********************

Overview
========

LinuxForHealth (LFH) pyconnect provides an out-of-the-box Docker Compose configuration with support for `profiles <https://docs.docker.com/compose/profiles/>`_ that enable deploying specific support services in addition to pyconnect. We will go through the supported profiles and usage below.

Starting the LFH Compose Stack
==============================

The syntax to launch the LFH compose stack is::

    docker-compose --profile [profile name] up -d
    # Multiple profiles can be specified in this fashion:
    docker-compose --profile [profile name] --profile [profile-name] up -d

Where [profile name] is the name of a supported LFH profile. Supported profiles include:: deployment, ipfs and fhir. Additional profiles will be added as needed to support external systems for new LFH integration use-cases.

**NOTE::** All ``docker-compose`` commands always deploy support services Kafka, Zookeeper & NATS by default.

+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| Profile Name       | Profile Description                                                                                                        |
+====================+============================================================================================================================+
| deployment         | For bundled deployment of ``pyconnect`` along with the support services with ports mapped to localhost.                    |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| ipfs               | Deploys IPFS nodes and an IPFS cluster for managing pinsets.                                                               |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| fhir               | Deploys the IBM Fhir Server.                                                                                               |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+


Not specifying a profile name would lead to deploying Kafka, Zookeeper & NATS which are the default support services for pyconnect::

    # navigate to the ``pyconnect`` project directory
    cd ./pyconnect
    # run the docker-compose command with no profile specified
    docker-compose up -d
    # Kafka, Zookeeper & NATS should be deployed
Deploying only the support services can come in handy in use-cases where pyconnect is run external of the compose stack.


The ``deployment`` profile supports spinning up pyconnect application along with default support services::

    # navigate to the ``pyconnect`` project directory
    cd ./pyconnect
    # run the docker-compose command with ``deployment`` profile specified
    docker-compose --profile deployment up -d
    # pyconnect, Kafka, Zookeeper & NATS should now be deployed
Using the ``deployment`` profile would be the out-of-the-box solution for most users of the compose stack


The ``ipfs`` profile deploys a 3 node IPFS peer and IPFS cluster in addition to the default support services. Using the ``deployment`` profile in addition to ``ipfs`` profile would ensure that pyconnect, Kafka, Zookeeper, NATS and IPFS are deployed simultaneously. Note, the modular nature of docker-compose profiles would allow users to only deploy ``ipfs`` as an additional support service to an already ``deployment`` profile stack that is up and running.

    # navigate to the ``pyconnect`` project directory
    cd ./pyconnect
    # run the docker-compose command with ``deployment`` and ``ipfs`` profile specified
    docker-compose --profile deployment --profile ipfs up -d
    # pyconnect, Kafka, Zookeeper, NATS & IPFS should now be deployed

The ``fhir`` profile allows deploying the `IBM FHIR Server <https://ibm.github.io/FHIR/>`_ in addition to other support services::

    # navigate to the ``pyconnect`` project directory
    cd ./pyconnect
    # run the docker-compose command with ``fhir`` profile specified
    docker-compose --profile fhir up -d
    # chain together deployment & fhir profiles
    docker-compose ---profile deployment --profile fhir up -d

Use standard docker-compose commands to interact with the stack and it's services. Stop containers leaving them intact for starting them back up again or tear down down deployments using profiles similarly as for when deploying them initially, as shown in the examples above::

    # stops all containers leaving them intact
    docker-compose stop # containers may be removed with docker-compose down -v
    # tear down deployment of pyconnect, Kafka, Zookeeper & NATS
    docker-compose --profile deployment down
    # tear down ipfs & fhir server (together or separately)
    docker-compose --profile ipfs --profile fhir down
    
