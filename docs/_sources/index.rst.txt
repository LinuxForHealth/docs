.. Linux for Health documentation master file, created by
   sphinx-quickstart on Tue May 26 17:27:09 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

What is Linux for Health?
*************************

.. image:: images/LinuxforHealth.png
   :width: 600
   :alt: Linux for Health

Linux For Health (LFH) has captured the attention of industry leaders in the field.  While the project is in its nascent stages, industry leaders have already forked the code and committed additonal features. The goal is to be become the reference for a trusted interoperable transactional development system for data, deployment, and machine learning within the healthcare industry.  As the project evolves, the initial companies who have committed to develop on LFH will be providing APIs and build recipes that support LFH deployment from embedded device, fully distributed agent-based, multi-tenant multi cloud and LinuxOne distributions.  We hope you join our journey in making the Linux For Health.

Core to Linux for Health is the concept of a route, or a sequential set of operations on a transaction.  Linux for Health routes are designed to ingest healthcare data, perform any required operations on that data (e.g. de-serialize, transform, enrich), then do something with that data (e.g. store, notify, forward), providing a transformative capability which has the potential to revolutionize the healthcare industry.

Linux for Health is built on:

* Apache Camel for integration, supported by one of the most active development communities.
* Kafka and NATS for world-class data streaming and messaging.
* Standard data formats, including HL7 FHIR R4, HL7 MLLP and EDI, with easy extensiblity to support any format.

Linux for Health provides data connectivity and routing, dynamic extensibility, custom events and data-driven insights for clinical data standards, including HL7v2 and FHIR R4, and a variety of standard inputs, including JDBC, Kafka, FTP/sFTP and sFTP, AS400, HTTP(s) and REST.

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
   develop/contribute.rst
   develop/build.rst
   develop/test.rst
   develop/property-encryption.rst
   develop/route-basics.rst

.. toctree::
   :maxdepth: 3
   :caption: Deployment:

   deployment/container.rst
   deployment/compose.rst
   deployment/openshift.rst
   deployment/oci.rst
