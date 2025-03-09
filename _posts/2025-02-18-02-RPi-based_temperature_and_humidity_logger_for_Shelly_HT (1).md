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

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20161536.png" alt="sudo systemctl status mosquitto" width="400"/>


The status should be "active (running)". Press `q` to exit from the status to the command line.


## Testing the changed configuration


