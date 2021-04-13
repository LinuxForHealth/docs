Kubernetes Support
******************

Overview
========
Enables deployment of pyConnect and supporting services on a Kubernetes Cluster

Requirements
------------
Deploying LFH pyconnect on a Kubernetes cluster requires the following::

- A working k8s cluster - check `Minikube <https://minikube.sigs.k8s.io/>`_ & Docker Desktop Kubernetes support.
- `mkcert <https://github.com/FiloSottile/mkcert>`_ for local trusted certificates.
- Basic know-how on creating k8s configmaps (examples provided) and enabling volume mounts in order to make signed root CA certs & keys avaiable to containers.

