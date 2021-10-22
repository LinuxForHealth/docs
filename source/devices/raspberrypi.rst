Raspberry Pi
************

Overview
========

LinuxForHealth is deployable to amd64, s390x and arm64 (aarch64) devices and may be built to support other chip architectures. These instructions explain how to bring up an arm64 Raspberry Pi device from scratch, straight out of the box, and were tested with a Raspberry Pi 3B+.  All steps are executed on the Raspberry Pi, with the exception of the first three steps to image the micro SD card.

Image and Run Your Raspberry Pi
===============================
These instructions explain how to image and run a new Raspberry Pi (or one you want to re-image). If you have an already configured and running Raspberry Pi with an up-to-date 64-bit Pi OS, skip to the next section.

1.  Download the Raspberry Pi imager for your laptop OS (ex: macOS):
     | https://www.raspberrypi.org/downloads/
     | This will allow you to image a micro SD card with the os image, since the os image is too big to copy.
2.  Download the 64-bit Raspberry Pi OS .zip file:
    https://downloads.raspberrypi.org/raspios_arm64/images/
3.  Image your micro SD card.
     | * Plug your micro SD reader into your laptop.
     | * Run the Raspberry Pi imager.
     | * Click "Choose OS", select "Use custom", then select the arm64 .zip file you just downloaded.
     | * Click "Choose SD card" then select your micro SD card and click "Write".
4.  Bring up your Raspberry Pi.
     | * Connect all your peripherals to your Raspberry Pi - usb keyboard, mouse, hdmi display.
     | * Insert your SD card in your Raspberry Pi and power up the pi.
5.  Walk through the setup in the Raspberry Pi desktop when prompted, including network configuration.  Be sure to enable the ssh interface under Preferences->Raspberry Pi Configuration->Interfaces, then restart your Raspberry Pi.
6.  On your Raspberry Pi desktop, open a command window and run ifconfig to find the Pi's IP address.  Make a note of this IP so you can ssh into the Pi, if desired.

Configure the Python Environment
================================
1. Upgrade Python on your Raspberry Pi to 3.8.  Python 3.8 is not currently available on Pi OS, so a workaround is provided via an additional repository.

   Get the GPG key::

      wget https://pascalroeleven.nl/deb-pascalroeleven.gpg
      sudo apt-key add deb-pascalroeleven.gpg

   Edit /etc/apt/sources.list and add this repository::

      deb http://deb.pascalroeleven.nl/python3.8 buster-backports main

   Install python 3.8 and related packages::

      sudo apt update
      sudo apt install python3.8 python3.8-venv python3.8-dev

2. Upgrade pip::

      pip install --upgrade pip

3. Add aliases to your .bashrc file to ensure the correct python and pip versions::

      alias python=python3.8
      alias python3=python3.8
      alias pip=pip3

Install Docker and docker-compose
=================================
Install docker and docker-compose on your Raspberry Pi, following these instructions:

Upgrade the pi::

   sudo apt-get update && sudo apt-get upgrade

Install docker::

   curl -sSL https://get.docker.com | sh

Add the current user to the docker group, which allows you to run docker without sudo::

   sudo usermod -aG docker ${USER}

Install docker-compose::

   sudo apt-get install libffi-dev libssl-dev
   sudo pip install docker-compose

Build and Install librdkafka
=================================
Remove the downlevel version of librdkafka that is distributed with Pi OS::

   sudo apt-get remove librdkafka-dev

Clone and build librdkafka::

   git clone https://github.com/edenhill/librdkafka.git
   cd librdkafka
   ./configure
   make
   sudo make install

Replace the aarch64 version of librdkafka (not covered by the librdkafka `sudo make install` step)::

   sudo cp /usr/local/lib/librdkafka.so.1 /usr/lib/aarch64-linux-gnu
   sudo cp /usr/local/lib/librdkafka++.so.1 /usr/lib/aarch64-linux-gnu

Configure and Run LinuxForHealth connect
========================================
Install pipenv::

   sudo apt install pipenv

Clone LinuxForHealth::

   git clone https://github.com/LinuxForHealth/connect.git

Create a connect virtual environment::

   cd connect
   pipenv --python 3.8 sync --dev

Add **KAFKA_HEAP_OPTS: "-Xmx256M"** memory restriction to kafka environment variables in connect/docker-compose.yml.  Example::

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

