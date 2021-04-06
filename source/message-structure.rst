Message Structure
*****************
This section describes the message structure used by LinuxForHealth and shows typical examples.

TBD - Update based on pyconnect



Structure
=========
The following JSON message structure is used throughout LinuxForHealth::

    {
        'meta': {
            'routeId': string,
            'uuid': string,
            'routeUrl': string,
            'dataFormat': string,
            'messageType': string,
            'timestamp': long,
            'dataStoreUri': string,
            'status': string, optional
            'dataRecordLocation': string[], optional
        }, 
        'data': object, optional
    }

Description
===========
The message consists of two top-level keys: 'meta' for describing information about a transaction and 'data' to contain the transaction payload.  The table below further describes each key.

+--------------------+-----------+---------------------------------------------------------------------+
| Key                | Type      | Description                                                         |
+====================+===========+=====================================================================+
| routeId            | string    | The identifier assigned to the route during route construction.     |
+--------------------+-----------+---------------------------------------------------------------------+
| uuid               | string    | The message uuid assigned during route invocation.                  |
+--------------------+-----------+---------------------------------------------------------------------+
| routeUrl           | string    | The url for the route associated with the message.                  |
+--------------------+-----------+---------------------------------------------------------------------+
| dataFormat         | string    | The format of the data sent as the message payload.                 |
+--------------------+-----------+---------------------------------------------------------------------+
| messageType        | string    | The data format's message type.                                     |
+--------------------+-----------+---------------------------------------------------------------------+
| timestamp          | long      | The timestamp in seconds since the epoch.                           |
+--------------------+-----------+---------------------------------------------------------------------+
| dataStoreUri       | string    | The uri of the data store.                                          |
+--------------------+-----------+---------------------------------------------------------------------+
| status             | string    | Optional result success indicator: "success" | "error".             |
+--------------------+-----------+---------------------------------------------------------------------+
| dataRecordLocation | string[]  | Optional location(s) of data stored successfully in the data store. |
+--------------------+-----------+---------------------------------------------------------------------+
| data               | object    | Optional data payload: byte[] or error message.                     |
+--------------------+-----------+---------------------------------------------------------------------+

Examples
========
Successful route processing notification::

    {
       "meta": {
            "routeId": "fhir-r4-rest",
            "uuid": "ab2385c6-ebb8-4ddd-a4b2-fc37c61761d4",
            "routeUrl": "http://0.0.0.0:8080/fhir/r4",
            "dataFormat": "FHIR-R4",
            "messageType": "PATIENT",
            "timestamp": 1592574430,
            "dataStoreUri": "kafka:FHIR-R4_PATIENT?brokers=localhost:9092",
            "dataRecordLocation": ["FHIR-R4_PATIENT-0@0"],
            "status": "success"
        }
    }

Notification of error caused by unavailable storage service::

    {
        "meta": {
            "routeId": "fhir-r4-rest",
            "uuid": "ab2385c6-ebb8-4ddd-a4b2-fc37c61761d4",
            "routeUrl": "http://0.0.0.0:8080/fhir/r4",
            "dataFormat": "FHIR-R4",
            "messageType": "PATIENT",
            "timestamp": 1592574430,
            "dataStoreUri": "kafka:FHIR-R4_PATIENT?brokers=localhost:9092",
            "status": "error"
        },
        "data": "Topic FHIR-R4_PATIENT not present in metadata after 60000 ms."
    }

Internal message sent to the storage service::

    {
        "meta": {
            "routeId": "hl7-v2-mllp",
            "uuid": "ab2385c6-ebb8-4ddd-a4b2-fc37c61761d4",
            "routeUrl": "netty:tcp://localhost:2575?sync=true&encoders=#hl7encoder&decoders=#hl7decoder",
            "dataFormat": "HL7-V2",
            "messageType": "ADT",
            "timestamp": 1592574430,
            "dataStoreUri": "kafka:HL7-V2_ADT?brokers=localhost:9092"
        },
        "data": <base64 encoded message>
    }
