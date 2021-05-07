Application Configuration
*************************

LinuxForHealth connect settings and configurations are defined within a `Pydantic Settings Configuration <https://pydantic-docs.helpmanual.io/usage/settings/>`_ model. Pydantic settings provide robust configuration support using Python types with the convenience of overriding configurations with environment variables.
Configurations are defined within the config module.

Configuration Override
======================
Configuration settings are overriden using environment variables. Pydantic performs type conversions on the variable values to ensure compatibility with the types defined in the config module. Non-scalar values such as lists and dictionaries are encoded as JSON strings.

Example - Override Scalar Setting::

    # python configuration
    uvicorn_host: str = '0.0.0.0'
    # environment variable override
    UVICORN_HOST=some-server

Example - Override Non-scalar Setting::

    # python configuration
    kafka_bootstrap_servers: List[str] = ['localhost:9094']
    # environment variable override
    KAFKA_BOOTSTRAP_SERVERS='["kafka:9092"]'

Local/development environments may override settings using an `.env file <https://pipenv.pypa.io/en/latest/advanced/#automatic-loading-of-env>`_.

Supported Properties
====================


+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| Property                                         | Data Type    | Description                                                                            | Default                          |
+==================================================+==============+========================================================================================+==================================+
| uvicorn_app                                      | String       | The fully qualified uvicorn app string                                                 | 'connect.asgi:app'               |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| uvicorn_host                                     | String       | The uvicorn host name or ip address                                                    | '0.0.0.0'                        |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| uvicorn_port                                     | Integer      | The uvicorn listening port                                                             | 5000                             |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| uvicorn_reload                                   | Boolean      | Indicates if hot debugging/reload is enabled                                           | False                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| certificate_authority_path                       | String       | Path to the directory used to house additional or intermediate Certificate Authorities | certifi package location         |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| certificate_verify                               | Boolean      | Indicates if x509 certificates are verified                                            | False                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_ca_file                                  | String       | The path to the concatenated CA certificates in PEM format                             | certifi package location         |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_ca_path                                  | String       | The path to the directory containing PEM formatted certificates                        | /usr/local/share/ca-certificates |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_cert_name                                | String       | The connect certificate file name                                                      | lfh-connect.pem                  |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_cert_key_name                            | String       | The connect certificate private key file name                                          | lfh-connect.key                  |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_config_directory                         | String       | The path to the connect application configuration directory                            | /home/lfh/connect/config         |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_lfh_id                                   | String       | The LinuxForHealth connect node identifier - unique within the "network"               | the machine host name            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_logging_path                             | String       | Path to logging.yaml file                                                              | logging.yaml                     |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_external_fhir_server                     | String       | External FHIR server URL                                                               |                                  |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| connect_rate_limit                               | String       | Request rate limit per client                                                          | 5/second                         |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_bootstrap_servers                          | List         | List of Kafka broker/server locations                                                  | ['kafka:9092']                   | 
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_segments_purge_timeout                     | Float        | Timemout used for Kafka segment cache                                                  | 10 minutes/600 seconds           |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_message_chunk_size                         | Integer      | I/O read size used for streaming Kafka messages                                        | 900KB                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_producer_acks                              | String       | Number of acks required before a request is considered complete                        | 'all'                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+   
| kafka_consumer_default_group_id                  | String       | The default Kafka consumer group id                                                    | 'lfh_consumer_group'             |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_consumer_default_enable_auto_commit        | Boolean      | Indicates if the Kafka consumer auto-commits it's current offset                       | False                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_consumer_default_enable_auto_offset_store  | Boolean      | Indicates if an in-memory store is used to manage Kafka consumer commit metadata       | False                            |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_consumer_default_poll_timeout_secs         | Float        | Timeout setting for Kafka consumer polling                                             | 1.0                              |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_consumer_default_auto_offset_reset         | String       | Allows the Kafka consumer to set the current offset under certain conditions           | 'error'                          |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_admin_new_topic_partitions                 | Integer      | The number of partitions the Kafka Admin client creates for a new topic                | 1                                |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_admin_new_topic_replication_factor         | Integer      | Replication setting for new Kafka partitions                                           | 1                                |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_listener_timeout                           | Float        | Kafka connection/polling timeout in seconds                                            | 1.0                              |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| kafka_topics_timeout                             | Float        | Number of seconds Kafka consumer waits before polling a broker                         | 0.5                              |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| nats_servers                                     | List         | List of NATS servers                                                                   | ['tls://nats-server:4222']       |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| nats_sync_subscribers                            | List         | List of NATS servers to sync messages too                                              | []                               |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| nats_allow_reconnect                             | Boolean      | Indicates if the NATS client will retry connections                                    | True                             |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+
| nats_max_reconnect_attempts                      | Integer      | The maximum number of retry attempts if nats_allow_reconnect is enabled                | 10                               |
+--------------------------------------------------+--------------+----------------------------------------------------------------------------------------+----------------------------------+

Secured Communications
======================
LinuxForHealth connect is configured for secure communications by default using TLS and x509 certificates. Outbound transactions from LinuxForHealth connect to external services are expected to be secured in a similar manner. Secure transmissions are supported within the `local development environment <developer-setup.html>`_ and `container image <../deployment/container.html>`_.

LinuxForHealth also utilizes secure communications between LinuxForHealth nodes for data synchronization via `NATS messaging <https://nats.io/>`_. Both TLS and `NATS NKeys <https://docs.nats.io/nats-server/configuration/securing_nats/auth_intro/nkey_auth>`_ are used to secure NATS connections between LinuxForHealth nodes.

Please see `Secured Communications <../develop/secured-communications.html>`_ and `Data Synchronization <../develop/data-synchronization.html>`_ for detailed information about configuring secured communications.
