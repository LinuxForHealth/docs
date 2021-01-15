What is LinuxForHealth?
*************************
Welcome to the future of healthcare software development - the world's first true HealthOS.

LinuxForHealth (LFH) is a distributed processing network operating system which allows edge devices (IoT, workstations, app servers) to connect directly to health care transaction systems.  The processing model abstracts the need for intermediary third-party organizations, resulting in a developer-extensible trust protocol.

The LinuxForHealth architecture builds on successive levels of industry standards, from AMD64, ARM64 and NVIDIA chip architectures to Docker and PodMan containers, supporting an extensible set of APIs accessible via multiple languages.

.. image:: images/LinuxForHealthArch.png
   :width: 600
   :alt: LinuxForHealth

This architecture facilitates these key LinuxForHealth features:

* An open source interoperable health stack to enable developers to build further platforms and applications
* Connectivity across embedded devices, multi-cloud and LinuxONE
* Data acquisition, integration, routing
* Real-time and batch processing
* Abstraction of the details of health standards, enabling extensibility, modularity, scalability
* Community support & enterprise readiness

LinuxForHealth has captured the attention of industry leaders.  While the project is in its nascent stages, leading companies are already committing significant features, including APIs and build recipes that support LFH deployment from embedded device, fully distributed agent-based, multi-tenant multi cloud and LinuxONE distributions.

You can get started by visiting our `developer setup page <./developer-setup.html>`_ and visit us on `Github <https://github.com/linuxforhealth/connect>`_.  We hope you join our journey in creating LinuxForHealth, the world's first true HealthOS.

.. toctree::
   :maxdepth: 3
   :caption: Getting Started:

   developer-setup.rst
   tutorials/firstroute.rst
   application-configuration.rst
   feature-configuration.rst
   message-structure.rst

.. toctree::
   :maxdepth: 3
   :caption: Tutorials:

   tutorials/quickstart.rst
   tutorials/fhir-r4.rst
   tutorials/hl7-v2.rst
   tutorials/blue-button-20.rst

.. toctree::
   :maxdepth: 3
   :caption: Routes:

   routes/fhir-r4-rest.rst
   routes/etl.rst
   routes/orthanc-post.rst
   routes/direct-storeandnotify.rst

.. toctree::
   :maxdepth: 3
   :caption: Develop:

   develop/extend.rst
   develop/build.rst
   develop/test.rst
   develop/property-encryption.rst
   develop/route-basics.rst
   develop/configure-kong.rst

.. toctree::
   :maxdepth: 3
   :caption: Deployment:

   deployment/container.rst
   deployment/compose.rst
   deployment/openshift.rst
   deployment/oci.rst



