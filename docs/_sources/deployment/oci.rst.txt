Open Container Initiative (OCI)
*******************************

Overview
========

Linux for Health (LFH) Connect includes scripts to manage containers within an OCI compliant runtime. The OCI runtime provides services to start, stop, and manage containers. The LFH Connect OCI scripts are located in the project's `oci <https://github.com/LinuxForHealth/connect/tree/master/container-support/oci>`_ directory. LFH Connect OCI support is currently limited to launching and removing containers. 

Configuring OCI services
========================

Configure the OCI_COMMAND within the container-support/oci/.env file to support the "container executable" on the target platform::

    OCI_COMMAND="sudo podman"

Managing OCI services
=====================

To start OCI services::

    # navigate to oci directory
    cd container-support/oci
    # launch containers
    ./start.sh

To remove OCI services (destructive operation)::

    # navigate to oci directory
    cd container-support/oci
    # launch containers
    ./remove.sh
