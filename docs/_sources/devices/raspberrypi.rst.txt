Raspberry Pi
************

Overview
========

LinuxForHealth is deployable to amd64, s390x and arm64 (aarch64) devices and may be built to support other chip architectures. These instructions explain how to bring up an arm64 Raspberry Pi device from scratch and were tested with a Raspberry Pi 3B+.  All steps are executed on the Raspberry Pi, with the exception of the first three steps to image the Raspberry Pi micro SD card.

Image and Run Your Raspberry Pi
===============================
These instructions explain how to image and run a new Raspberry Pi (or one you want to re-image). If you have a running Raspberry Pi with an up-to-date 64-bit Pi OS, skip to the next section.

1. Download the `Raspberry Pi imager <https://www.raspberrypi.org/downloads/>`_ for your computer's OS. This will allow you to image a micro SD card with the Raspberry Pi OS image, since the OS image is too big to copy.

2. Download the `64-bit Raspberry Pi OS -arm64.zip file <https://downloads.raspberrypi.org/raspios_arm64/images>`_. Example: 2021-05-07-raspios-buster-arm64.zip

3. Image your micro SD card

   * Plug your micro SD reader into your computer.
   * Run the Raspberry Pi imager.
   * Click "Choose OS", select "Use custom", then select the arm64 .zip file you just downloaded.
   * Click "Choose SD card" then select your micro SD card and click "Write".

4. Bring up your Raspberry Pi

   * Connect all your peripherals to your Raspberry Pi - usb keyboard, mouse, hdmi display.
   * Insert your micro SD card in your Pi and power it up.

5. Walk through the setup in the Raspberry Pi desktop when prompted, including network configuration.  Be sure to enable the ssh interface under Preferences->Raspberry Pi Configuration->Interfaces, then restart your Raspberry Pi.

6. On your Raspberry Pi desktop, open a command window and run ifconfig to find the Pi's IP address.  Make a note of this IP so you can ssh into the Pi.  The remaining steps can be done via ssh, if desired.

Configure the Python Environment
================================
1. Upgrade Python on your Raspberry Pi to 3.8.  Python 3.8 is not currently available on Pi OS, so a workaround is provided via an additional repository.

   Get the GPG key::

      wget https://pascalroeleven.nl/deb-pascalroeleven.gpg
      sudo apt-key add deb-pascalroeleven.gpg

   Edit /etc/apt/sources.list (use sudo) and add this repository::

      deb http://deb.pascalroeleven.nl/python3.8 buster-backports main

   Install python 3.8 and related packages::

      sudo apt update
      sudo apt install python3.8 python3.8-venv python3.8-dev

2. Upgrade pip::

      pip3 install --upgrade pip

3. Install pipenv::

      pip3 install pipenv

4. Add aliases to your .bashrc file to ensure the correct python and pip versions::

      alias python=python3.8
      alias python3=python3.8
      alias pip=pip3

   Source the new .bashrc to start using the aliases::

      source .bashrc

Install Docker and docker-compose
=================================
Install docker::

   curl -sSL https://get.docker.com | sh

Add the current user to the docker group, which allows you to run docker without sudo::

   sudo usermod -aG docker ${USER}

Install docker-compose::

   sudo apt-get install libffi-dev libssl-dev
   sudo pip3 install docker-compose

Reboot for user group changes to take effect::

   sudo reboot

Build and Install librdkafka
=================================
Clone, build and install librdkafka::

   git clone https://github.com/edenhill/librdkafka.git
   cd librdkafka
   ./configure
   make
   sudo make install
   sudo ldconfig
   cd ..

Configure and Run LinuxForHealth connect
========================================
Clone LinuxForHealth connect::

   git clone https://github.com/LinuxForHealth/connect.git

Create a virtual environment::

   cd connect
   pipenv --python 3.8 sync --dev

Add **KAFKA_HEAP_OPTS: "-Xmx256M"** Raspberry Pi memory restriction to kafka environment variables in connect/docker-compose.yml.  Example::

   kafka:
     networks:
       - main
     image: docker.io/linuxforhealth/kafka-alpine:2.5.0
     restart: "always"
     ports:
       - "9094:9094"
     environment:
       KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
       KAFKA_LISTENERS: INTERNAL://kafka:9092,EXTERNAL://kafka:9094
       KAFKA_ADVERTISED_LISTENERS: INTERNAL://kafka:9092,EXTERNAL://localhost:9094
       KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: INTERNAL:PLAINTEXT,EXTERNAL:PLAINTEXT
       KAFKA_INTER_BROKER_LISTENER_NAME: INTERNAL
       KAFKA_HEAP_OPTS: "-Xmx256M"

Bring up connect services::

   docker-compose up -d

Start connect::

   pipenv run connect

