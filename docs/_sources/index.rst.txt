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

Linux for Health is built on:

* Apache Camel for integration, supported by one of the most active development communities.
* Kafka and NATS for world-class data streaming and messaging.
* Standard data formats, including HL7 FHIR R4, HL7 MLLP and EDI, with easy extensiblity to support any format.

Linux for Health provides data connectivity and routing, dynamic extensibility, custom events and data-driven insights for clinical data standards, including HL7v2 and FHIR R4, and a variety of standard inputs, including JDBC, Kafka, FTP/sFTP and sFTP, AS400, HTTP(s) and REST.

You can get started by visiting our `developer setup page <./developer-setup.html>`_ and visit us on `Github <https://github.com/linuxforhealth/connect>`_.


.. toctree::
   :maxdepth: 3
   :caption: Installation:

   developer-setup.rst  
   application-configuration.rst

.. toctree::
   :maxdepth: 3
   :caption: Tutorials:

   tutorials/quickstart.rst
   tutorials/fhir-r4.rst
