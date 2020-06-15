Message Structure
*****************
This section describes the message structure used by Linux for Health route processing and shows typical examples.  

Structure
=========
The following JSON message structure is used throughout Linux for Health::

    {
        'meta': {
            'routeId': string,
            'uuid': string,
            'routeUrl': string,
            'dataFormat': string,
            'status': success | error, optional
            'dataStoreUrl': string, optional
            'dataRecordLocation': string[], optional
        }, 
        'data': object, optional
    }

Description
===========
The message consists of two primary keys: 'meta' for describing information about a transaction and 'data' to contain the transaction payload.  The table below further describes each attribute.

+--------------------+-----------+------------------------------------------------------------------+
| Attribute          | Type      | Description                                                      |
+====================+===========+==================================================================+
| routeId            | string    | The identifier assigned to the route during route construction.  |
+--------------------+-----------+------------------------------------------------------------------+
| uuid               | string    | The message uuid assigned during route invocation.               |
+--------------------+-----------+------------------------------------------------------------------+
| routeUrl           | string    | The url for the route associated with the message.               |
+-------------------------+-----------+-------------------------------------------------------------+
| dataFormat         | string    | The format of the data sent as the message payload.              |
+-------------------------+-----------+-------------------------------------------------------------+
| status             | string    | Optional result success indicator.                               |
+-------------------------+-----------+-------------------------------------------------------------+
| dataStoreUrl       | string    | Optional url of the data store.                                  |
+-------------------------+-----------+-------------------------------------------------------------+
| dataRecordLocation | string    | Optional location of data stored successfully in the data store. |
+-------------------------+-----------+-------------------------------------------------------------+
| data               | object    | Optional data payload or error message.                          |
+-------------------------+-----------+-------------------------------------------------------------+

Examples
========
Successful route processing notification::

    {
       "meta": {
            "routeId":"fhir-r4-rest",
            "uuid":"ID-MBP-2-attlocal-net-1592167260404-1-1",
            "routeUrl":"http://0.0.0.0:8080/fhir/r4",
            "dataFormat":"fhir-r4",
            "dataStoreUrl":"kafka:FHIR_R4_PATIENT?brokers=localhost:9092",
            "dataRecordLocation":["FHIR_R4_PATIENT-0@5"],
            "status":"success"
        }
    }

Notification of error caused by unavailable storage service::

    {
        "meta": {
            "routeId": "fhir-r4-rest",
            "uuid": "ID-MBP-2-attlocal-net-1592229483323-1-1",
            "routeUrl": "http://0.0.0.0:8080/fhir/r4",
            "dataFormat": "fhir-r4",
            "status": "error"
        },
        "data": "Topic FHIR_R4_PATIENT not present in metadata after 60000 ms."
    }

Internal message sent to the storage service::

    {
        "meta": {
            "routeId": "hl7-v2-mllp",
            "routeUrl": "netty:tcp://localhost:2575?sync=true&encoders=#hl7encoder&decoders=#hl7decoder",
            "dataFormat": "hl7-v2",
            "uuid": "ID-Caroles-MBP-2-attlocal-net-1592229483323-2-1"
        },
        "data": "MSH|^~\\&|system1|W|system2|UHN|200105231927||ADT^A01^ADT_A01|22139243|P|2.4\rEVN|A01|200105231927\rPID||9999999999|2216506||Duck^Donald^^^MR.^MR.||19720227|M|||123 Foo ST.^^TORONTO^ON^M6G 3E6^CA^H~123 Foo ST.^^TORONTO^ON^M6G 3E6^CA^M|1811|(416)111-1111||E^ENGLISH|S|PATIENT DID NOT INDICATE|211004554\rPV1|||ZFAST TRACK^WAITING^13|E^EMERGENCY||369^6^13^U EM EMERGENCY DEPARTMENT^ZFAST TRACK WAITING^FT WAIT 13^FTWAIT13^FT WAITING^FTWAIT13|^MOUSE^MICKEY^M^^DR.^MD|||SUR||||||||I|211004554||||||||||||||||||||W|||||200105231927\rPV2||F|^R/O APPENDICIAL ABSCESS\rIN1|1||001001|OHIP|||||||||||||||^^^^^^M|||||||||||||||||||||||||^^^^^^M\r"
    }
