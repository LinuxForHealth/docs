Testing LinuxForHealth
**********************
The following sections explain how to unit test LinuxForHealth connect and load test the LinuxForHealth ecosystem.

Required Software
=================
Make sure you have the required software installed from `Developer Setup <../develop-setup.html>`_.  To run the load tests you'll also need to install `locust <https://locust.io/>`_::

    pip install locust

Run Unit Tests
==============
LinuxForHealth connect uses pytest for unit testing.  To run the unit tests, run pytest from the root directory of connect::

    pipenv run pytest

Run Load Tests
==============

Example 1
---------
To run the load tests, make sure you have a LinuxForHealth environment running (see `Developer Setup <../develop-setup.html>`_ for instructions), then run the load tests from the connect/load-test directory::

    cd load-test
    locust --host https://localhost:5000 --run-time 5m --users 500 --spawn-rate 20 --headless

This set of steps runs the default load test defined by the connect/load-test/locustfile.py.  You can adjust the length of the test (--run-time), the number of simulated concurrent users (--users) and rate at which those users are added to the test (--spawn-rate).  Furthermore, you can adjust which endpoints are tested by adding/removing endpoints from locustfile.py.  See the `locust documentation <https://docs.locust.io/en/stable/>`_ for additional load test configuration options.

Example 2
---------
The default load test POSTs to the /fhir endpoint using randomly selected FHIR resources from connect/load-test/messages/fhir[1...NUM_FILES].json files. You can replace the contents of these fhir*.json files with your own FHIR resources or add fhir resource files to connect/load-test/messages using the same naming convention. To separate FHIR resources, place them in separate files. To change the number of files used by the load test (default = 2)::

    NUM_FILES=1 locust --host https://localhost:5000 --run-time 5m --users 500 --spawn-rate 20 --headless

In the test output, you will see that only the first of the two sample fhir messages is loaded::

    Loading message file = ./messages/fhir1.json

Example 3
---------
To use the locust UI to run load tests, run locust from the command line::

    locust --host https://localhost:5000

then point your browser to http://127.0.0.1:8089 to run the test, specifying the number of users and spawn rate via the UI.
