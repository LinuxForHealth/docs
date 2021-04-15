Kubernetes Support
******************

Overview
========
We provide example kubernetes yaml files that enable the deployment of connect and support services (Kafka, Zookeeper, NATS & IBM FHIR Server) on a Kubernetes Cluster. Advanced users can tweak these yaml files to allow for customizations and achieving granular control on deployments.

Requirements
------------
Deploying LFH connect on a Kubernetes cluster requires the following::

1. A working k8s cluster - check `Minikube <https://minikube.sigs.k8s.io/>`_ & Docker Desktop Kubernetes support.
2. `mkcert <https://github.com/FiloSottile/mkcert>`_ for generating local trusted certificates.
3. Basic know-how on creating k8s configmaps (examples provided) and enabling volume mounts in order to make signed root CA certs & keys avaiable to containers.

------------
Create Certs
------------
Generate trusted local certs for connect and supporting services for your kubernetes environment::

    # Run the ``install-certificates.sh`` script provided in ``connect/local-certs``
    ./local-certs/install-certificates.sh
-------------------------------
Create Configmaps for connect
-------------------------------
The following configmaps are required to be created for connect (as defined in `connect-deployment.yml <https://github.com/LinuxForHealth/connect/blob/main/k8s-deployment/connect/connect-deployment.yml>`_)::

    kubectl -n <namespace-for-config-map> create configmap lfh-pemstore --from-file=lfh.pem
    
    kubectl -n <namespace-for-config-map> create configmap lfh-keystore --from-file=lfh.key
    
    kubectl -n <namespace-for-config-map> create configmap ca-nats --from-file=rootCA.pem
    
    kubectl -n <namespace-for-config-map> create configmap nats-pemstore --from-file=nats-server.pem
    
    kubectl -n <namespace-for-config-map> create configmap nats-keystore --from-file=nats-server.key
    
Please see example `here <https://github.com/LinuxForHealth/connect/blob/main/k8s-deployment/connect/connect-deployment.yml>`_.

-------------------------
Make NATS certs available
-------------------------
NATS certificates are mounted as a `hostPath <https://kubernetes.io/docs/concepts/storage/volumes/#hostpath>`_ volume. That would require making the NATS certs available on each node of the kubernetes cluster in the location specified in the yaml file for ``nats-js``.
The directory path on the host node(s) should be referenced `here <https://github.com/LinuxForHealth/connect/blob/main/k8s-deployment/nats-js/nats-with-jetstream.yml>`_ under the ``volumes`` label.

Deploying connect & Support Services
--------------------------------------
Example deployment yaml's are provided for reference in each of the sub-folders within ``connect/k8s-deployment`` directory.

- ``nats-js/`` - Provides an out-of-the-box deployment for NATS Server and Jetstream – mount ``/path/to/nats-server-certs/`` directory with the `hostPath` directive - check `nats-with-jetstream.yml <https://github.com/LinuxForHealth/connect/blob/main/k8s-deployment/nats-js/nats-with-jetstream.yml>`_ for example.
- ``kafka-zk/`` - Provides an out-of-the-box deployment for Kafka and ZooKeeper - exposes ``localhost:9094`` as the broker.
- ``ibm-fhir/`` - Fires up the IBM FHIR Server; `documentation here <https://ibm.github.io/FHIR/guides/FHIRServerUsersGuide/>`_.
- ``connect/`` - Deploys the ``connect`` application to work with NATS and Kafka in the same namespace - create configmaps as described above and reference them in the deployment yaml's before connect can be deployed.

-----------------------------
Helper scripts for deployment
-----------------------------
Although the deployment yaml's in the sub-directories can be altered for achieving granular control, we provide helper shell scripts to deploy ``connect`` and required supporting services for users who are not familiar with tuning k8s deployments. All helper scripts require the ``-n`` (namespace) option.

Here are helpful descriptions for each script:

- ``deployment-up.sh`` - Creates the input namespace (if it doesn't exist) and deploys connect along with all supporting services (Note: Configmaps referenced in this README should be created and appropriately referenced in the yamls for all services to work correctly)
- ``deployment-down.sh`` - Deletes all kubernetes resources for `connect` and supporting services for the input namespace. This script does not delete the namespace.
- ``delete-k8s-ns.sh`` - Deletes the input kubernetes namespace. NOTE: Using this script will permanently delete all kubernetes resources on the namespace. If you are not sure what this means or if you have deployed connect and supporting services on a kuberentes namespace that has other software artifacts deployed, please do not use this script.
