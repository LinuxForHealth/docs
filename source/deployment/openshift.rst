OpenShift Container Platform
****************************

Overview
========

LinuxForHealth provides quick-start scripts to provision an `OpenShift Container Platform (OCP) cluster <https://www.openshift.com/>`_, and create a LFH OpenShift project.
OCP provisioning support is available for Azure using `Azure's Red Hat OpenShift (ARO) service <https://azure.microsoft.com/en-us/services/openshift/>`_, with additional platforms coming soon!
The provisioning and project creation scripts are independent of one another. In other words the LFH project creation script does not directly depend on a LFH OCP provisioning script, but it does require an existing OCP cluster running on a supported platform (Red Hat, AWS, Azure, Code Ready Containers, private, etc).  

General Requirements
====================

- `Register <https://developers.redhat.com/register>`_ for a free Red Hat Developer Account.
- Download the OC CLI toolkit from the Red Hat developer site

Where to Start
==============

If you do not have access to an existing OCP cluster, you will need to create one. `Code Ready Containers <https://github.com/code-ready/crc>`_ provides an OCP compatible environment for local/development use. Red Hat, IBM, Azure, AWS, and GCP provide hosted OCP services environments.

LFH provides documentation and limited support for provisioning :ref:`Code_Ready_Containers` and :ref:`Azure_Red_Hat_Openshift`

If you have access to an existing OCP cluster, you may proceed to :ref:`LFH_OpenShift_QuickStart`

.. _LFH_OpenShift_QuickStart:

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

.. _Code_Ready_Containers:

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
    crc start -p pull-secret.txt --nameserver 1.1.1.1
    # wait for crc to provision
    crc status

Add $HOME/.crc/bin to the system's executable path::

    # update profile with
    export PATH="$HOME/.crc/bin/oc:$PATH"

Note on OS X, the crc executable may register as unverified. Open the "Security & Privacy" pane in System Preferences.

To view the provided CRC credentials for the OpenShift console run::

    crc console --credentials

Additional CRC setup instructions are included within the doc.pdf file included with the distribution.

.. _Azure_Red_Hat_Openshift:

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
    # specify setup mode, subscription, resource group (new or existing), region
    ./aro-quickstart "install" "My Subscription" "lfh-rg" "eastus"
    # the provisioning process will take approximately 30 - 40 minutes, based on current workloads.

After the ARO cluster is provisioned, fetch credential and access information::

    # within the LFH connect project directory
    cd container-support/openshift
    # specify setup mode, subscription, resource group (new or existing), region
    ./aro-quickstart "connection-info" "My Subscription" "lfh-rg" "eastus"
    # returns OpenShift console credentials and URLs for the console and api endpoints

To remove the ARO cluster::

    # within the LFH connect project directory
    cd container-support/openshift
    # specify setup mode, subscription, resource group (new or existing), region
    ./aro-quickstart "remove" "My Subscription" "lfh-rg" "eastus" 
    # submits the delete request and provides commands to run to remove dependent objects once the cluster is deleted

