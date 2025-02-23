# RPi-based temperature and humidity logger for Shelly HT (Part 2: Mosquitto installation)

## Update the package manager

Software installation is managed by the package manager. Before using it, make sure that it knows about the latest updates. Do so by logging into your RPi via SSH. Then, issue the command:

`sudo apt update`

Then, do a reboot with the command

`sudo reboot`

This will terminate the SSH connection. Wait for the RPi to reboot. Then, establish a new SSH connection with PuTTY.


## Installation of Mosquitto

The communication between the Shelly sensor and the RPi requires a MQTT Broker. It is able to communicate with the sensor. To install it, log into the RPi and issue the following command.

`sudo apt-get install mosquitto`



## First test

(to be written)

## Test of Mosquitto

(to be written)
