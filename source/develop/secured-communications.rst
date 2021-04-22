Secured Communications
**********************
The following sections explain how to configure LinuxForHealth secured communications.  Out-of-the-box, no changes are needed for secured communication.  However, when developing for LinuxForHealth, you may encounter these common use cases.

Configure TLS to an External Server
===================================
A TLS connection from LinuxForHealth connect to an external server can be configured by supplying the :code:`https` form of the URL, as in this example setting for an external FHIR server in connect/config.py::

    connect_external_fhir_server: str = 'https://fhiruser:change-password@localhost:9443/fhir-server/api/v4'

If your external server uses self-signed certificates, you'll also need to set :code:`certificate_verify` in connect/config.py to False and use this setting to make your connection to your external server (see transmit() in connect/connect/workflows/core.py)::

    certificate_verify: bool = False

Connections to any external servers that you integrate with LinuxForHealth connect should be secured in a similar manner.

Regenerate LinuxForHealth Certificates
======================================
Should you need to generate a new set of self-signed certificates for LinuxForHealth connect, uninstall the existing certificates, then install new certificates::

    cd connect/local-certs
    ./uninstall-certificates.sh
    ./install-certificates.sh

This set of steps installs new LinuxForHealth connect certificates at the OS level, which allows Python to trust these certificates.  Restart LinuxForHealth connect to utilize these new certificates.

Use Your Own Certificates
=========================
If you have your own set of CA-issued certificates that you want to use for LinuxForHealth, set the APPLICATION_CERT_PATH environment variable in connect/.env or specify APPLICATION_CERT_PATH on the command line to point to your certificate location when starting connect.  Use an absolute directory or specify a directory relative to the connect directory::

    APPLICATION_CERT_PATH=./local-certs \
    pipenv run connect

