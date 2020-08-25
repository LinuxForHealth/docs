Container Support
=================

Overview
========

Linux for Health (LFH) provides first class support for `OCI Compliant <https://opencontainers.org/>`_ container runtimes. Images for LFH applications and services are available under the `Linux For Health Organization on Docker Hub <https://hub.docker.com/u/linuxforhealth>`_ . Platforms supported include:
* x86/amd64
* arm64
* s390x/Z

Red Hat Universal Base Images (UBI) are utilized when possible. UBI images provide an optimizied runtime with an emphasis on security and operational stability. Within the UBI family of images, the minimal variant is preferred, followed by the init and standard variants.

LFH images run within any standard container runtime and orchestration environment. Additionally, all LFH images are supported on Red Hat's Open Shift Container Platform (OCP) and adhere to "best practices".

LFH provides management scripts to support the following runtimes:
* OCI (podman or docker)
* Docker Compose
* Open Shift Container Platform and Code Ready Containers (provides a local environment for OCP development)

LFH Base Image
==============

The LFH base image is used as the base image for all LFH images when possible. The LFH base image in turn is based off of the Red Hat minimal UBI image. The LFH base image provides the following:
* a standard non-privileged application user, lfh
* a root directory, /opt/lfh, for applications and services
* updated permissions which allow the "root group" (not root user) access to /opt/lfh

Building Images
===============

LFH images are built to support multiple architectures, including x86/amd, arm64, and s390x/Z. Multi-arch builds are supported in OCI compliant tools such as docker and podman. The following docker command illustrates a multi-arch build for the LFH connect project::

    docker buildx build \
        --pull \
        --push \
        --platform linux/amd64,linux/arm64,linux/s390x \
        -t [image repository]/connect:1.0.0 .

The image tag, specified by "-t", may be updated to push the image to a different repository.

If a multi-arch build is not required, a standard "build" command may be used. The following command builds the LFH connect image for the target platform::

    docker build -t [image repository]/connect:1.0.0 .

The additional information on the LFH build process, please review the `LFH Images Repo <https://github.com/LinuxForHealth/images>`_ .

Supported Container Runtimes
============================

LFH provides management scripts to provision containers within the following runtimes:
* OCI (docker or podman)
* Docker Compose
* OpenShift Container Platform
