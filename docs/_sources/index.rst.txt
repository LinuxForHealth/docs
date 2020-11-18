.. LinuxForHealth documentation master file, created by
   sphinx-quickstart on Tue May 26 17:27:09 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

What is LinuxForHealth?
*************************

.. image:: images/LinuxforHealth.png
   :width: 600
   :alt: LinuxForHealth

Welcome to the future of healthcare software development - The world's first true HealthOS.

LinuxForHealth (LFH) is a distributed processing network operating system which allows edge devices (IoT, workstations, app servers) to connect directly to health care transaction systems.  The processing model abstracts the need for intermediary third-party organizations, resulting in a developer-extensible trust protocol.

Among the features provided by LinuxForHealth are:

* An open source interoperable health stack to enable developers to build further platforms and applications

* Connectivity across embedded devices, multi-cloud and LinuxONE

* Data acquisition, integration, routing

* Real-time and batch processing

* Abstraction of the details of health standards, enabling extensibility, modularity, scalability

* Community support & enterprise readiness.

LinuxForHealth has captured the attention of industry leaders.  While the project is in its nascent stages, leading companies are already committing significant features, including APIs and build recipes that support LFH deployment from embedded device, fully distributed agent-based, multi-tenant multi cloud and LinuxONE distributions.  We hope you join our journey in making the LinuxForHealth.

Core to LinuxForHealth data transformation and synchronization is the concept of a route, or a sequential set of operations on a transaction.  LinuxForHealth routes are designed to ingest healthcare data, perform any required operations on that data (e.g. de-serialize, transform, enrich), then do something with that data (e.g. store, notify, forward), providing a transformative capability which has the potential to revolutionize the healthcare industry.

LinuxForHealth is built on:

* Apache Camel for integration, supported by one of the most active development communities.
* Kafka and NATS JetStream for world-class data streaming and messaging.
* Standard data formats, including HL7 FHIR R4, HL7, MLLP and EDI, with easy extensibility to support any format.

You can get started by visiting our `developer setup page <./developer-setup.html>`_ and visit us on `Github <https://github.com/linuxforhealth/connect>`_.

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



