Configuring Kong
****************

Overview
========
LinuxForHealth (LFH) uses the `Kong API Gateway <https://konghq.com/kong>`_.  When you add a route to LinuxForHealth, you'll need to add the route to Kong as well.  The steps below explain how to do that.

Kong Configuration
==================
The Kong configuration is implemented via curl commands to Kong's Admin API in 2 files:

- connect/container-support/compose/configure-kong.sh (docker-compose)
- connect/container-support/oci/configure-kong.sh (oci)

In order to use a new LinuxForHealth route, you'll need to update the configure-kong script for the container system you are using.  If you plan to contribute your route back to LinuxForHealth you'll need to update both of these scripts as follows.

1. Create a new Kong service (if needed)
----------------------------------------
If you are just adding a new http or REST route, you probably don't need a new Kong service.  However, if you want specific Kong plugins to work with your route, you will need to create a Kong service.  To create a Kong service, add a curl command to configure-kong.sh::

  curl --insecure https://localhost:8444/services \
    -H 'Content-Type: application/json' \
    -d '{"name": "my-new-service", "url": "http://'"${host}"':'"${lfhhttp}"'"}'

Give the service a new name and either use the same lfhhttp port or a different port, depending on your use case. ${host} is set for you automatically, depending on where LinuxForHealth is running. Use --insecure to avoid self-signed certificate verification.

2. Create a new Kong route
--------------------------
A Kong route matches incoming transactions and sends them to the LinuxForHealth url contained in the associated Kong service.  To create a Kong route for your LinuxForHealth route, add a line to configure-kong.sh.

To add a new http or REST route, you can use the lfh-http-service::

  add_http_route "my-new-route" "GET" "/new-route" "lfh-http-service"

The first argument is the name of your new route.  The second is the HTTP method supported by your route.  The third argument is the path for your new route.  This should not include any query parameters, as even partial paths will be matched. The last argument is the name of the Kong service that traffic on this Kong route will be sent to, either "lfh-http-service" to use the existing service or your own service, if you created one in the previous step.
