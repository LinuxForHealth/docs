.. iDAAS documentation master file, created by
   sphinx-quickstart on Tue May 26 17:27:09 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

What is iDAAS?
**************

Healthcare integration has been a challenge for decades. iDAAS is the Intelligent Data As A Service platform, designed to integrate any healthcare data, in any format, and provide new insights on that data.  In addition to comprehensive transformation and insights, iDAAS enables downstream services to securely receive and operate on healthcare data, effectively bridging the disparate systems that comprise the healthcare industry.

iDAAS is built on:

* Apache Camel for integration, supported by one of the most active development communities.
* Kafka and NATS for world-class data streaming and messaging.
* Standard data formats, including HL7 FHIR R4, HL7 MLLP and EDI, with easy extensiblity to support any format.

iDAAS Platform Components
=========================
The iDAAS Platform provides healthcare data connectivity and routing, dynamic extensibility, custom events and data-driven insights via the components described below.

iDAAS Connect
-------------
iDAAS Connect focuses on receiving data in industry standard formats and routing that data to consumers that provide transformation, insights and downstream connectivity.  The following iDAAS Connect packages are supported:

* iDAAS Connect Clinical Industry Standards: Supports clinical integration standards including HL7v2 and FHIR. The HL7v2 capability supports versions 2.1 to 2.8 of the following message types: ADT (Admissions), ORM (Orders), ORU (Results), SCH (Schedules), PHA (Pharmacy), MFN (Master File Notifications), MDM (Medical Document Management) and VXU (Vaccinations). The FHIR capability includes many of the commonly used Clinical Resources.
* iDAAS Connect Financial Industry Standards: Supports financial integration standards via FHIR Financial Resources. 
* iDAAS Connect Third Party : Allows iDAAS to receive data from almost two dozens connectors, including JDBC, Kafka, FTP/sFTP and sFTP, AS400, HTTP(s), REST and many more.

iDAAS Data Hub
--------------
iDAAS Data Hub is the iDAAS platform data store, designed to ensure message-defined resources have data driven insights from any iDAAS platform activity. Data enablement in the iDAAS platform focuses on sending detailed events to iDAAS Data Hub for any platform activities, specifically via a transaction event consisting of a rich set of data attributes:

* The current iDAAS action
* The current iDAAS component
* The date and time of the action
* The current component data
* Additional attributes, including: sending application, transactions processed, transactions generated, processing times and response times.

Additional Capabilities
-----------------------
* Ability to add iDAAS capabilities dynamically, without the need to restart the platform.
* Ability to extend or enhance the platform's ability to process data via custom events.
* User Interface for accesing iDAAS Data Hub.
* APIs built from the data tier within iDAAS Data Hub.


.. toctree::
   :maxdepth: 2
   :caption: Contents:

   about.rst
   installation.rst
