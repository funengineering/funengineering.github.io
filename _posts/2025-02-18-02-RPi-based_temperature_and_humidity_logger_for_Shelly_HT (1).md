# RPi-based temperature and humidity logger for Shelly HT (Part 2: Mosquitto installation)

## Installation of Mosquitto

The communication between the Shelly sensor and the RPi requires a MQTT broker. It is able to communicate with the sensor. To install it, log into the RPi and issue the following command.

`sudo apt-get install mosquitto`

Confirm to continue by entering "y" (or just Enter as yes is the default response).

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20143822.png" alt="Installation of Mosquitto" width="400"/>

Also install the Mosquitto client, which will be used to test the communication with the MQTT broker.

`sudo apt-get install mosquitto-clients`

Enable the automatic start of the MQTT broker using the following command.

`sudo systemctl enable mosquitto`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20144104.png" alt="Installation of Mosquitto clients and enabling automatic startup" width="400"/>



## First test of the MQTT broker

To check the communication with the MQTT broker, you need two separate terminal windows. Create another ssh session to the RPi and log in. Arrange the two terminal windows side-by-side. The left window will be the sender (publisher), the right will be the receiver (subscriber). You are going to send a test message from the left terminal to the MQTT broker. The right window will receive the message from the MQTT broker.

First, start the receiver in the right terminal window.

`mosquitto_sub -h localhost -t /test/topic`

This will put the right terminal into a waiting state.

Now, publish a message in the left terminal.

`mosquitto_pub -h localhost -t /test/topic -m "Hello Mosquitto!"`

The message, which you just published in the topic "/test/topic", was received in the right terminal window because it is subscribed to messages in the topic "/test/topic".

If the message does not appear in the right terminal window, check if mosquitto (your MQTT broker) is running.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20150215.png" alt="Test message sent from publisher to subscriber via MQTT broker" width="400"/>


## Test of Mosquitto

(to be written)
