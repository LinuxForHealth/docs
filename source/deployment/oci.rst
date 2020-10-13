Open Container Initiative (OCI)
*******************************

Overview
========

LinuxForHealth (LFH) Connect includes scripts to manage containers within an OCI compliant runtime. The OCI runtime provides services to start, stop, and manage containers. The LFH Connect OCI scripts are located in the project's `oci <https://github.com/LinuxForHealth/connect/tree/master/container-support/oci>`_ directory. LFH Connect OCI support is currently limited to launching and removing containers. 

OCI Command Parameter
=====================

The OCI Command parameter specifies the name of the command used to start and stop containers. The OCI command defaults to "docker". If the target system has a different container runtime, pass in the appropriate command name to the `lfh-oci-services.sh` script.

Managing OCI services
=====================

To start OCI services::

    # navigate to oci directory
    cd container-support/oci
    
    # launch containers using docker command
    ./lfh-oci-services.sh start

To remove OCI services (destructive operation)::

    # navigate to oci directory
    cd container-support/oci
    
    # remove containers using docker command
    ./lfh-oci-services.sh remove

To start OCI services using "rooted" podman::

    # navigate to oci directory
    cd container-support/oci
    
    # launch containers using "rooted" podman command
    ./lfh-oci-services.sh start "sudo podman"
