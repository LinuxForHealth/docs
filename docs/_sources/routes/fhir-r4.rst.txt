FHIR R4
*******

Purpose
=======
The LinuxForHealth connect FHIR R4 route allows you to post FHIR R4 data to connect and store that data as part of the LinuxForHealth Longitudinal Patient Record (LPR).  You can also optionally configure LinuxForHealth connect to send your FHIR R4 data to an external FHIR server.

Details
=======
.. list-table::
   :widths: 30 70
   :header-rows: 0

   * - Route URL
     - https://{host}:5000/fhir/{resource_type}
   * - Example URL
     - https://127.0.0.1:5000/fhir/Patient
   * - Path Parameter: resource_type
     - The FHIR R4 type of the resource

Calling the Route
=================
Navigate to the LinuxForHealth connect Open API UI at https://127.0.0.1:5000/docs and select :code:`POST /fhir/{resource_type}` to post a FHIR resource as discussed in `QuickStart <../tutorials/quickstart.rst>`_.  You may also use the tool of your choice, such as curl or Postman, to send data to LinuxForHealth connect.

Optional Config
===============
In addition to storing data in the LinuxForHealth LPR, the FHIR R4 route can be configured to send the FHIR resource to an external FHIR server.  To configure this feature, add your external FHIR server URL to config.py as follows and restart LinuxForHealth connect::

    fhir_r4_externalserver: str = 'https://user:password@localhost:9443/fhir-server/api/v4'

Results
=======
The FHIR resource you supplied is stored in Kafka as part of the LinuxForHealth LPR, viewable via the Open API UI :code:`GET /data` API.  Please see `QuickStart <../tutorials/quickstart.rst>`_ for instructions.
