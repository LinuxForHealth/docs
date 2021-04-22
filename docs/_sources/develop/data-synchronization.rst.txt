Data Synchronization
**********************
The following sections explain how to configure and test data synchronization between LinuxForHealth nodes.

Configure TLS
-------------
To configure TLS for data synchronization between LinuxForHealth nodes using the default self-signed NATS certificates, add the connect/local-certs/rootCA.pem certificate of the remote LinuxForHealth node to the local LinuxForHealth connect/local-certs/rootCA.pem file.  Simply append the exact contents of the remote file to the local file and be sure to leave a blank line at the end of the file.

If the remote NATS server certificate is CA-issued, this step is not required.

Configure NATS NKeys
--------------------
Out of the box, LinuxForHealth provides a NATS NKey key pair that allows NATS connections between LinuxForHealth nodes.  Each NATS client connection requires that the NKey private key be provided to create the connection.  If each LinuxForHealth node to be configured for data synchronization uses the same NATS NKey (default), there is nothing to do for this step.

In practice, though, NATS NKeys will be different.  In this case, add your local NKey public key to the remote connect/local-certs/nats-server.conf file::

    authorization: {
      users: [
        { nkey: UAOG3WGHPVLI3OQQVVTAP33Q3VOJKHYRZWA7A3REHZ3AD4U7YJJUYXO4 }
        { nkey: UC2VSUPV3KX22XSD25K4Y2RLOXN4DGJFD5IMTQ326UZYBPEDVMTL7EER }
      ]
    }


Configure Settings
------------------
Change LinuxForHealth settings to configure data synchronization.  In connect/config.py, add a connection string for any remote LinuxForHealth node from which this LinuxForHealth node will receive notifications, as in this example::

    nats_sync_subscribers: List[str] = ['tls://192.168.1.201:4222', 'tls://192.168.1.202:4222']

In this case, the LinuxForHealth node with this :code:`nats_sync_subscribers` configuration will receive data synchronization messages from NATS servers with IP addresses 192.168.1.201 and 192.168.1.202.

Change a NATS NKey (optional)
-----------------------------
Since the default NATS NKey is published in github, it's recommended that you change it for production deployments. To generate a new NATS NKey, use the `NATS nk tool <https://docs.nats.io/nats-tools/nk>`_::

    > nk -gen user -pubout
    SUAIT6XNSPT2JZXX2XCPQPQO6F3BZ3VUGDP73WMEXUEBBVBMXMIJHUQHWE
    UC2VSUPV3KX22XSD25K4Y2RLOXN4DGJFD5IMTQ326UZYBPEDVMTL7EER

The first key, beginning with :code:`SU`, is the secret key.  Place this in connect/local-certs/nats-server.nk, taking care to not leave a blank line at the end of the file.

The second key, beginning with :code:`U`,is the public key.  Add this to the local connect/local-certs/nats-server.conf, as shown in the *Configure NATS NKeys* section, and add it to the same file on the remote LinuxForHealth nodes in the :code:`nats_sync_subscribers` configuration.

Restart Services
----------------
Restart the NATS and LinuxForHealth connect services after making configuration changes.

Test
----
Once your configuration changes have been made and the NATS and LinuxForHealth connect services have been restarted, test data synchronization by sending a FHIR-R4 message to LinuxForHealth connect on one of the nodes in the :code:`nats_sync_subscribers` configuration.  See the `QuickStart tutorial <../tutorials/quickstart.html>`_ for instructions.  With debug logging on, in the connect log for the LinuxForHealth node you sent the message to directly, you should see entries similar to::

    2021-04-22 08:41:59,658 - connect.clients.nats - DEBUG - nats_sync_event_handler: received a message on EVENTS.sync : {"uuid": "3e3dbd97-f06c-4761-8af9-c78301efa5d1", "lfh_id": "Caroles-MBP-2.attlocal.net", "creation_date": "2021-04-22T13:41:52+00:00", "store_date": "2021-04-22T13:41:52+00:00", "consuming_endpoint_url": "/fhir/Patient", "data": "<the base64-encoded message you submitted>", "data_format": "FHIR-R4_PATIENT", "status": "success", "data_record_location": "FHIR-R4_PATIENT:0:0", "target_endpoint_url": "https://fhiruser:change-password@localhost:9443/fhir-server/api/v4/Patient", "elapsed_storage_time": 1.630286, "transmit_date": "2021-04-22 08:41:54Z", "elapsed_transmit_time": 5.583757, "elapsed_total_time": 1.870391}
    2021-04-22 08:41:59,661 - connect.clients.nats - DEBUG - nats_sync_event_handler: detected local LFH message, not storing in kafka

That message comes from a default NATS subscriber, present on each LinuxForHealth node, and indicates that the node received the message.

In the connect log for node you did not send the message to (the node with the :code:`nats_sync_subscribers` configuration), you should see that the NATS data synchronization message is received and processed, similar to::

    2021-04-21 20:41:18,972 - connect.clients.nats - DEBUG - nats_sync_event_handler: stored msg in kafka topic LFH_SYNC at LFH_SYNC:0:0
    2021-04-21 20:41:18,974 - connect.workflows.core - INFO - Running CoreWorkflow, message={<the message you submitted>}
    2021-04-21 20:41:18,978 - connect.workflows.core - DEBUG - CoreWorkflow: incoming message type = <class 'dict'>
    2021-04-21 20:41:20,500 - connect.clients.kafka - DEBUG - Produced record to topic FHIR-R4_PATIENT partition [0] @ offset 0
    2021-04-21 20:41:20,501 - connect.workflows.core - DEBUG -  CoreWorkflow persist: stored resource location = FHIR-R4_PATIENT:0:0
    2021-04-21 20:41:20,505 - connect.workflows.core - DEBUG - CoreWorkflow persist: outgoing message = {'uuid': UUID('58128d4e-4956-468f-94e2-6aa83d92ed69'), 'lfh_id': 'Caroles-MBP-2.attlocal.net', 'creation_date': datetime.datetime(2021, 4, 21, 20, 41, 18, tzinfo=datetime.timezone.utc), 'store_date': datetime.datetime(2021, 4, 21, 20, 41, 18, tzinfo=datetime.timezone.utc), 'consuming_endpoint_url': '/fhir/Patient', 'data': '<the base64-encoded message you submitted>', 'data_format': 'FHIR-R4_PATIENT', 'status': 'success', 'data_record_location': 'FHIR-R4_PATIENT:0:0', 'target_endpoint_url': None, 'elapsed_storage_time': 1.518526, 'transmit_date': None, 'elapsed_transmit_time': None, 'elapsed_total_time': 1.528799}
    2021-04-21 20:41:20,507 - connect.clients.nats - DEBUG - nats_sync_event_handler: replayed nats sync message, data record location = FHIR-R4_PATIENT:0:0

In this case, the FHIR message was processed into the LPR data store, at topic:partition:offset FHIR-R4_PATIENT:0:0.

This is a simple test of one-way data synchronization, but the configuration can be extended so that every LinuxForHealth node synchronizes data to every other LinuxForHealth node.