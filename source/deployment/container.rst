Container Support
*****************

Overview
========

Images for LinuxForHealth applications and services are available under the `LinuxForHealth Organization on Docker Hub <https://hub.docker.com/u/linuxforhealth>`_ . Supported platforms include:

* x86/amd64
* arm64
* s390x/Z

Alpine base images are utilized wherever possible to enable small memory footprints (Kafka & Zookeeper), especially to enable the arm64 arch-type.

LFH images run within any standard container runtime and orchestration environment. The connect project provides example yaml files to deploy the application and support services on a kubernetes cluster.

LFH provides management scripts to support the following runtimes:

* Docker Compose (in ``./pyconnect``)
* Kubernetes Cluster Deployments (in ``./pyconnect/k8s-deployment``)

LFH Base Image
==============

The LFH base image is used as the base image for all LFH images when possible. The LFH base image provides the following:

* a standard non-privileged application user, lfh
* a root directory, /opt/lfh, for applications and services
* updated permissions which allow the "root group" (not root user) access to /opt/lfh

Building Images
===============

LFH images are built to support multiple architectures, including x86/amd, arm64, and s390x/Z. Multi-arch builds are supported in OCI compliant tools such as docker. The following docker command illustrates a multi-arch build for the LFH pyconnect project::

    docker buildx build \
        --pull \
        --push \
        --platform linux/amd64,linux/arm64,linux/s390x \
        -t [image repository]/pyconnect:1.0.0 .

The image tag, specified by "-t", may be updated to push the image to a different repository.

If a multi-arch build is not required, a standard "build" command may be used. The following command builds the LFH connect image for the target platform::

    docker build -t [image repository]/pyconnect:1.0.0 .

For more information on available images and specific build commands please visit the `LFH Images Repo <https://github.com/LinuxForHealth/images>`_ .
