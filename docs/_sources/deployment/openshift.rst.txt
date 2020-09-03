OpenShift Container Platform
****************************

Overview
========

Linux for Health (LFH) Connect supports provisoning it's applications and services on the OpenShift Container Platform (OCP). Additional scripts are provided to provision OCP on supported cloud platforms. The OCP provisioning scripts are "general-use" and may be adapted as needed.
Supported OCP platforms include Azure and Code Ready Containers (CRC) for local development, with additional environments coming soon!


General Requirements
====================

* A `Red Hat Developer Account <https://developers.redhat.com/register>`_
* The OpenShift CLI tool. Note: this tool is provided with the Code Ready Containers (CRC) installation.

Code Ready Containers Setup
===========================

First, download the latest Code Ready Containers release and "pull secret" from the Red Hat Developer Site. The "pull secret" is used to enable access to Red Hat image repositories within the CRC VM.

To install Code Ready Containers::

    # assume $HOME/Downloads contains the CRC installation and pull secret
    cd $HOME/Downloads
    tar -xvzf crc-macos-amd64.tar.xz
    cd crc-<target platform>
    cp crc /usr/local/bin
    crc setup
    crc start -p pull-secret.txt
    # wait for crc to provision
    crc status

Following the installation add $HOME/.crc/bin to the system's executable path::

    # update profile with
    export PATH="$HOME/.crc/bin/oc:$PATH"

Note on OS X, the crc executable may register as unverified. Open the "Security & Privacy" pane in System Preferences.

To view the provided CRC credentials for the OpenShift console run::

    crc console --credentials

Additional CRC setup instructions are included wihin the doc.pdf file included with the distribution.

Azure Red Hat OpenShift Setup
=============================

Provisioning Azure Red Hat OpenShift (ARO) requires the following:

* An active Azure Account that is a member of the "User Access Admin" role within the subscription.
* Configuration to allow "users" to create application registrations and service principals.
* A minimum CPU quota of 40 within the desired region and subscription for the DSV3 instance types.
* The Azure CLI tool (az).

To create an ARO cluster::

    # within the LFH connect project directory
    cd container-support/openshift
    # specify subscription, resource group (new or existing), region, and setup mode
    ./aro-quickstart "My Subscription" "lfh-rg" "eastus" "install"
    # the provisioning process will take approximately 30 - 40 minutes, based on current workloads.

After the ARO cluster is provisioned, fetch credential and access information::

    # within the LFH connect project directory
    cd container-support/openshift
    # specify subscription, resource group (new or existing), region, and setup mode
    ./aro-quickstart "My Subscription" "lfh-rg" "eastus" "connection-info"
    # returns OpenShift console credentials and URLs for the console and api endpoints

To remove the ARO cluster::

    # within the LFH connect project directory
    cd container-support/openshift
    # specify subscription, resource group (new or existing), region, and setup mode
    ./aro-quickstart "My Subscription" "lfh-rg" "eastus" "remove"
    # submits the delete request and provides commands to run to remove dependent objects once the cluster is deleted


LFH OpenShift QuickStart
========================

The LFH OpenShift QuickStart is compatible with any OpenShift 4.x installation. The quickstart uses the `oc` command line tool to manage OpenShift resources including projects, applications, services, and routes.

To install the LFH QuickStart::

    # log into the OCP API
    oc login -u [username] -p [password] [openshift api url]

    # install quickstart assets
    ./lfh-quickstart.sh install

    # when the install is complete, use `oc` to view cluster status information
    oc status

To remove the LFH QuickStart::

    # log into the OCP API
    oc login -u [username] -p [password] [openshift api url]

    # install quickstart assets
    ./lfh-quickstart.sh remove
