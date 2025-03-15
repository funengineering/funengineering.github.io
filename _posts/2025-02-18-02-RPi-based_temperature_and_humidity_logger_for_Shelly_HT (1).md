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

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20150215.png" alt="Test message sent from publisher to subscriber via MQTT broker" width="700"/>

Stop the subscriber in the right terminal window by activating the window and pressing Ctrl-C. This brings you back to the command line. Close the second window by entering `exit`.

If the message does not appear in the right terminal window, check if mosquitto (your MQTT broker) is running (`sudo systemctl status mosquitto`).


## Mosquitto configuration

Now, set a username and a password for interactions with the MQTT broker. You can define the username and password as you like. If the command below is performed as is, the username will be pi. You are then asked to enter the password twice. The password can be different from the password you use to log into your RPi.

`sudo mosquitto_passwd -c /etc/mosquitto/pwfile pi`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20154318.png" alt="Setting the username and password for mosquitto" width="400"/>

Next, you are going to create a copy of the default configuration file and edit its contents. Start by creating the copy of the default config file.

`sudo cp /usr/share/doc/mosquitto/examples/mosquitto.conf /etc/mosquitto/conf.d/default.conf`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20155009.png" alt="Copying the Mosquitto configuration file" width="400"/>

Edit the configuration file using the editor nano.

`sudo nano /etc/mosquitto/conf.d/default.conf`

Perform the following changes to the file:
- In the general configuration section, set `per_listener_settings true` by adding this text on a new line (e. g. below the commented line `#per_listener_settings false`).
- In the listeners section, set the listener port to 1883 by adding the line `listener 1883`.
- In the persistence section, activate the default autosave interval by removing the hash sign at the beginning of the line. The line should then read `autosave_interval 1800`.
- In the security section, prohibit anonymous usage of the MQTT broker by setting `allow_anonymous false`.
- In the same section, set the location of the password file: `password_file /etc/mosquitto/pwfile`.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20155919.png" alt="per_listener_settings true" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20160231.png" alt="listener 1883" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20160514.png" alt="autosave_interval 1800" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20160804.png" alt="allow_anonymous false" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20161130.png" alt="password_file /etc/mosquitto/pwfile" width="400"/>

Save the changed file by pressing Ctrl-X ("Exit"), followed by pressing "Y" to confirm saving the changes. Press enter to accept saving the changes to the original file.

To activate the changes in the configuration file, restart Mosquitto.

`sudo systemctl restart mosquitto`

Check the status of Mosquitto.

`sudo systemctl status mosquitto`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20162311.png" alt="sudo systemctl status mosquitto" width="400"/>


The status should be "active (running)". Press `q` to exit from the status to the command line.


## Testing the changed configuration

The changed configuration should now be active. First, check if anonymous publishing is now forbidden.

`mosquitto_pub -h localhost -t /test/topic -m "Hello Mosquitto!"`

This should fail with an error.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20162808.png" alt="Connection refused, not authorized" width="400"/>

Open a second terminal window again as you will again publish a message in the first window and receive it in the second window.

In the second terminal window, subscribe to the topic that will later receive the message. Note that the command now includes the username and password (replace "your_password" by your actual password).

`mosquitto_sub -h localhost -t /test/topic -u pi -P your_password`

In the first terminal window, publish the test message, now also providing the username and password.

`mosquitto_pub -h localhost -t /test/topic -m "Hello Mosquitto!" -u pi -P your_password`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20165734_edited.png" alt="Test message sent from publisher to subscriber via MQTT broker with authentication" width="700"/>

Now, you can also try to connect to your MQTT broker from another device in your network, e. g. from your desktop computer or from a mobile phone with an MQTT client app (e. g. MyMQTT on Android). To establish a connection, you will need the IP address of your RPi, the port number of the MQTT broker (we set it to 1883), the username (pi) and the password. In my case, I also had to provide the MQTT version, where I chose V3.

<img src="/docs/assets/img/ht_logger/Screenshot_20250309-170957.png" alt="MQTT settings" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot_20250309-171035.png" alt="Subscription to topic" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot_20250309-171046.png" alt="Subscription activated" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot_20250309-171133.png" alt="Publishing a message" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot_20250309-171215.png" alt="Seeing the message on the mobile device" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20171300.png" alt="Seeing the message on the RPi" width="400"/>


## What's next?

At this point, you might want to shut down your RPi, remove the SDCard and create a backup image of the SDCard. On Windows, you can use [Win32 Disk Imager](https://win32diskimager.org/) to do this. It reads the complete SDCard and stores it to an img file. This will allow you go back to this point by flashing this image to SDCard if anything goes wrong later on. To save space, you can compress the img file with [7-Zip](https://www.7-zip.org/).

Then, you'll be setting up InfluxDB. It will store all the temperature and humidity data of your Shelly sensors.

## Links

- [undated: Mosquitto download page](https://mosquitto.org/download/) (very condensed, contains only rudimentary installation instructions)
- [undated: mosquitto.conf man page](https://mosquitto.org/man/mosquitto-conf-5.html) (documentation of the configuration file)
- [2022-08-14: Install an MQTT Server and Node-RED on Raspberry Pi for Home Automation](https://www.makeuseof.com/install-mqtt-server-node-red-raspberry-pi-home-automation/)
- [undated: Mosquitto MQTT-Broker-Installation auf Raspberry pi](https://myhomethings.eu/de/mosquitto-mqtt-broker-installation-auf-raspberry-pi/) (the config file does not work (anymore) as described in this tutorial)
