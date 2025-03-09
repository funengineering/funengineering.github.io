# RPi-based temperature and humidity logger for Shelly HT (Part 2: Mosquitto installation)

## Installation of Mosquitto

The communication between the Shelly sensor and the RPi requires a MQTT broker. It is able to communicate with the sensor. To install it, log into the RPi and issue the following command.

`sudo apt-get install mosquitto`

Confirm to continue by entering "y" (or just Enter as yes is the default response).

Also install the Mosquitto client, which will be used to test the communication with the MQTT broker.

`sudo apt-get install mosquitto-clients`

Enable the automatic start of the MQTT broker using the following command.

`sudo systemctl enable mosquitto`



## First test

(to be written)

## Test of Mosquitto

(to be written)
