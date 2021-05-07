Developer Setup
***************

Where to Contribute
===================
.. list-table::
   :widths: 50 50
   :header-rows: 1

   * - Resource Type
     - Link
   * - 🚨 Bug Reports
     - `GitHub Bug List <https://github.com/LinuxForHealth/connect/labels/bug>`_
   * - 🎁 Feature Requests & Ideas
     - `GitHub Issues Tracker <https://github.com/LinuxForHealth/connect/issues>`_
   * - ❔ Questions
     - `LFH Slack Channel <https://ibm-watsonhealth.slack.com/archives/G01639WJEMA>`_
   * - 🚙 Roadmap
     - `Project Board <https://github.com/LinuxForHealth/connect/projects/1>`_


Getting Started
===============
Required Software
-----------------
The LinuxForHealth connect development environment requires the following:

* `git <https://git-scm.com/>`_ for project version control
* `mkcert <https://github.com/FiloSottile/mkcert>`_ for local trusted certificates
* `Python 3.8 or higher <https://www.python.org/downloads/mac-osx/>`_ for runtime/coding support
* `Pipenv <https://pipenv.pypa.io>`_ for Python dependency management
* `Docker Compose <https://docs.docker.com/compose/install/>`_ for a local container runtime

For Windows 10 users, we suggest using `Windows Subsystem for Linux <https://docs.microsoft.com/en-us/windows/wsl/install-win10>`_

Set Up A Local Environment
--------------------------
Clone the project and navigate to the root directory
""""""""""""""""""""""""""""""""""""""""""""""""""""
::

    git clone https://github.com/LinuxForHealth/connect
    cd connect

Confirm Python build tooling is installed
"""""""""""""""""""""""""""""""""""""""""
::

    pip --version
    pipenv --version

Install core and dev dependencies
""""""""""""""""""""""""""""""""""""""""""""""
::

    pip install --upgrade pip
    pipenv sync --dev

Run tests
"""""""""
::

    pipenv run pytest


Black code formatting integration    
"""""""""""""""""""""""""""""""""

LinuxForHealth connect utilizes the `black library <https://black.readthedocs.io/en/stable/index.html>`_ to provide standard code formatting. Code formatting and style are validated as part of the `LinuxForHealth connect ci process <https://github.com/LinuxForHealth/connect/blob/main/.github/workflows/connect-ci.yml>`_. LinuxForHealth connect provides developers with an option of formatting using pipenv scripts, or a git pre-commit hook.

Pipenv Scripts
""""""""""""""

Check for formatting errors
"""""""""""""""""""""""""""
::

    pipenv run check-format

Format code
"""""""""""
::

    pipenv run format


Git pre-commit hook integration
"""""""""""""""""""""""""""""""

Install git pre-commit hooks (initial setup)
""""""""""""""""""""""""""""""""""""""""""""
::

    pip run pre-commit install


Commit Output - No Python Source Committed
""""""""""""""""""""""""""""""""""""""""""
::

    black................................................(no files to check)Skipped
    [black-formatter 95bb1c6] settings black version to latest release
    1 file changed, 1 insertion(+), 1 deletion(-)

Commit Output - Python Source is Correctly Formatted
""""""""""""""""""""""""""""""""""""""""""""""""""""
::

    black....................................................................Passed
    [format-test c3e1b4a] test commit
    1 file changed, 1 insertion(+)

Commit Output - Black Updates Python Source
"""""""""""""""""""""""""""""""""""""""""""
::

    black....................................................................Failed
    - hook id: black
    - files were modified by this hook

    reformatted connect/routes/api.py
    All done! ✨ 🍰 ✨
    1 file reformatted.


Generate trusted local certs for connect and supporting services
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
::

    ./local-config/install-certificates.sh

For more information on connect and HTTPS/TLS support, please refer to the `local cert readme <https://github.com/LinuxForHealth/connect/blob/main/local-config/README.md>`_.

Start connect and supporting services
"""""""""""""""""""""""""""""""""""""
::

    docker-compose up -d
    docker-compose ps
    pipenv run connect


Browse to https://localhost:5000/docs to view the Open API documentation

Docker Image
------------
The connect docker image is an "incubating" feature and is subject to change. The image is associated with the "deployment" profile to provide separation from core services.

Build the image
"""""""""""""""
The connect image build integrates the application's x509 certificate (PEM encoded) and configurations into the image. In the local, or development environment, certificates and configuration are stored in the ./local-config/connect directory. Deployed environments may use separate directories for certificates and configuration.


Build the image with Docker CLI
"""""""""""""""""""""""""""""""
::

    docker build --build-arg CONNECT_CERT_PATH_BUILD_ARG=./local-config/connect \
                 --build-arg CONNECT_CONFIG_PATH_BUILD_ARG=./local-config/connect \
                 -t linuxforhealth/connect:0.42.0 .

Build the image with Docker-Compose
"""""""""""""""""""""""""""""""""""
The docker-compose command below parses the build context, arguments, and image tag from the docker-compose.yaml file.::

    docker-compose build connect

Run connect and Supporting Services
"""""""""""""""""""""""""""""""""""
::

    docker-compose --profile deployment up -d

Links and Resources
===================
.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Resource Type
     - Link
   * - 📰 Documentation
     - `LinuxForHealth Docs Site <https://linuxforhealth.github.io/docs/>`_
   * - 📰 Documentation
     - `IPFS <https://github.com/LinuxForHealth/connect/blob/main/IPFS.md>`_
