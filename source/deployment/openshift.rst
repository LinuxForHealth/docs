OpenShift Container Platform
****************************

Overview
========

Linux for Health (LFH) Connect supports provisoning it's applications and services on the OpenShift Container Platform (OCP) and Code Ready Containers, a local application used for OCP development and testing. The provisioning process is a "quick start" process designed to create OCP assets with reasonable defaults.

Pre-requisites
==============
* A `RedHat Developer Account <https://developers.redhat.com/register>`_
* `Code Ready Containers <https://github.com/code-ready/crc>`_ for local OCP environment, if desired
* The OpenShift CLI tool, OC, if Code Ready Containers is not installed

Code Ready Containers Setup
===========================

If a local OCP environment is desired, download the latest Code Ready Containers release and associated "pull secret" from the RedHat Developer Network site.

To install Code Ready Containers::

    # assume Code Ready Containers and the pull secret are downloaded to /$HOME/Downloads
    tar -xvzf crc-macos-amd64.tar.xz
    cd crc-<target platform>
    cp crc /usr/local/bin
    crc setup
    crc start -p $HOME/Downloads/pull-secret.txt
    # wait for crc to provision
    crc status

Following the installation add $HOME/.crc/bin to the system's executable path::

    # update profile with
    export PATH="$HOME/.crc/bin/oc:$PATH"

Note on OS X, the crc executable may register as unverified. Open the "Security & Privacy" pane in System Preferences.

To view the provided CRC credentials for the OpenShift console run::

    crc console --credentials

Additional CRC setup instructions are included wihin the doc.pdf file included with the distribution.

Note: CRC requires considerable resources. It is recommended to stop Docker Desktop, if installed, prior to running CRC unless the local machine is connected to a power source.

Manage LFH OpenShift QuickStart
===============================

The LFH Connect project includes scripts to manage the LFH OCP quickstart. The scripts are located in the `OpenShift directory <https://github.com/LinuxForHealth/connect/tree/master/container-support/openshift>`_ .

To provision resources::

    # log into the OCP API
    oc login -u [username] -p [password] [openshift api url]

    # install quickstart assets
    ./install-quickstart.sh

    # when install is complete view status information including exposed endpoint urls for integration
    oc status

To remove resources::

    # log into the OCP API
    oc login -u [username] -p [password] [openshift api url]

    # install quickstart assets
    ./remove-quickstart.sh

If the quickstart is used with CRC, use the following login information:

    oc login -u developer -p developer https://api.crc.testing:6443
