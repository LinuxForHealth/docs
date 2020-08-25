Docker Compose Support
**********************

Overview
========

Linux for Health (LFH) Connect provides a standard Docker Compose configuration with support for file overrides. The compose configurations are included in the project's `compose directory <https://github.com/LinuxForHealth/connect/tree/master/container-support/compose>`_ . A management script, start-stack.sh, is used to launch the LFH application stack for a requested profile.

Starting the LFH Compose Stack
==============================

The syntax to launch the LFH compose stack is::

    . ./start-stack.sh [profile name]

Where [profile name] is the name of a supported LFH profile such as dev, server, and pi. Additional profiles will be added as needed to support external systems for new LFH integration use-cases. The script is executed within the current shell to ensure that Docker Compose's COMPOSE_FILE variable is available to the CLI.

+--------------------+------------------------------------------------------------------------------------------------------------------+
| Profile Name       | Profile Description                                                                                              |
+====================+==================================================================================================================+
| dev                | For local development use. Runs LFH supporting services with ports mapped to localhost.                          |
+--------------------+------------------------------------------------------------------------------------------------------------------+
| server             | For deployment environments or integrated testing. Includes the LFH connect application and supporting services. |
+--------------------+------------------------------------------------------------------------------------------------------------------+
| pi                 | Similar to the server stack. Optimized for arm64/Raspberry Pi usage.                                             |
+--------------------+------------------------------------------------------------------------------------------------------------------+

The dev profile is the "default" profile and is started if start-stack.sh is executed without arguments. Run LFH locally to integrate with the "dev" profile::

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

When local development work is complete stop or remove the LFH compose stack and reset the COMPOSE_FILE variable::

    # stops all containers leaving them intact
    docker-compose stop # containers may be removed with docker-compose down -v
    # unset the COMPOSE_FILE variable
    unset COMPOSE_FILE

To start the "server" profile, which includes the LFH container::

    # navigate to the compose configuration directory
    cd container-support/compose
    # execute the compose start script within the current shell
    . ./start-stack.sh server
    