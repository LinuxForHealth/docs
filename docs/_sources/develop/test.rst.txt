Testing Your Code
*****************

Overview
========
If you are contributing a new route or component to LinuxForHealth (see `Developing for LinuxForHealth <./extend.html>`_), create unit tests and add any required test services.

Unit Tests
==========
Create unit tests that follow the examples under connect/src/test.

Test Services
=============
To test your new route or component, you may need to add one or more supporting services, such as a database or an external API. To do this, create a new Docker Compose configuration within connect/container-support/compose named as docker-compose.<service>.yml file in that directory. Then, to start the LFH services along with your services, issue the following command from the connect/container-support/compose directory::

    docker-compose -f ./docker-compose.yml -f docker-compose.<service>.yml up

In this mode, Ctrl-C will stop all the services when testing is complete.

An additional option for including additional services is to add a "profile" to connect/container-support/compose/start-stack.sh for the use-case. 
For example, to support a custom profile for "ftp", first create a Docker Compose override configuration, docker-compose.ftp.yml to support a ftp container. Next, update the start-stack.sh script to support the "ftp" profile and include the docker-compose.ftp.yml configuration. To start the profile::

    # change to compose directory
    cd container-support/compose
    # start the "ftp" profile in the current shell
    . ./start-stack.sh ftp
    # once testing is concluded
    docker-compose down -v
    unset COMPOSE_FILE
