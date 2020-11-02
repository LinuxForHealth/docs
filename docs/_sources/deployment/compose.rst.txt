Docker Compose Support
**********************

Overview
========

LinuxForHealth (LFH) Connect provides a standard Docker Compose configuration with support for file overrides. The compose configurations are included in the project's `compose directory <https://github.com/LinuxForHealth/connect/tree/master/container-support/compose>`_ . A management script, `start-stack.sh`, is used to launch the LFH application stack for a requested profile.

Starting the LFH Compose Stack
==============================

The syntax to launch the LFH compose stack is::

    # note that the script is "sourced" into the current shell
    . ./start-stack.sh [profile name]
    # alternate command: source start-stack.sh [profile name]    

Where [profile name] is the name of a supported LFH profile. Supported profiles include: dev, server, and pi. Additional profiles will be added as needed to support external systems for new LFH integration use-cases. The script is executed within the current shell to ensure that Docker Compose's COMPOSE_FILE variable is set correctly.

+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| Profile Name       | Profile Description                                                                                                        |
+====================+============================================================================================================================+
| dev                | For local development use. Runs LFH supporting services with ports mapped to localhost.                                    |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| integration        | Supports integration testing with external systems. Includes LFH connect application, supporting and external services.    |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| server             | For deployment environments or integrated testing. Includes the LFH connect application and supporting services.           |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+
| pi                 | Similar to the server stack. Optimized for arm64/Raspberry Pi usage.                                                       |
+--------------------+----------------------------------------------------------------------------------------------------------------------------+

The dev profile is the "default" profile and is started if start-stack.sh is executed without arguments. The dev profile is intended for local development use::

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

The integration profile supports a full LFH stack with external systems (e.g. FHIR R4 Servers). The integration profile is launched using::

    # navigate to the compose configuration directory
    cd container-support/compose
    # execute the compose start script within the current shell
    . ./start-stack.sh integration

The "server" and "pi" profiles support a full LFH stack, which includes the LFH container. The example below launches the server profile::

    # navigate to the compose configuration directory
    cd container-support/compose
    # execute the compose start script within the current shell
    . ./start-stack.sh server

Use standard docker-compose commands to interact with the stack and it's services. Unset the `COMPOSE_FILE` variable following a `docker-compose stop` or `docker-compose down -v`::

    # stops all containers leaving them intact
    docker-compose stop # containers may be removed with docker-compose down -v
    # unset the COMPOSE_FILE variable
    unset COMPOSE_FILE
    