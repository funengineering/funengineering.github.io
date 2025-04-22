# RPi-based temperature and humidity logger for Shelly HT (Part 6: Using Node-RED to store temperature and humidity values in InfluxDB)

Now the Shelly H&T sensors transmit their data to the MQTT broker running on your Raspberry Pi. You will now create a "flow" in Node-RED that takes this data and passes it on to InfluxDB, where it is stored. With InfluxDB, it is also possible to visualize the data, but that's not the topic of this part of the tutorial. The goal of this part is only to feed the collected into InfluxDB for permanent storage.


## Preparations

### Web interface of Node-RED

Navigate to the web interface of Node-RED running on your RPi. In my case, I enter the address `192.168.178.28:1880` in a browser of a computer running in the same local network. `192.168.178.28` is the IP address of my RPi and `1880` is the port where it provides the web interface of Node-RED. If you have followed the tutorial, the port number should also be 1880 in your case. However, the IP address of your RPi depends on your particular local network and might be different. If you are unsure about the IP address of your RPi, review [part 1 of this tutorial](/2025/02/18/01-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html), particularly the section "Network settings".

When your browser shows the web interface of Node-RED, you should see an empty flow at the center of the window.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20161506.png" alt="Initial view of Node-RED with empty flow" width="400"/>


### Installation of the InfluxDB Node-RED package

By default, Node-RED does not have nodes to access InfluxDB. The first step is thus to add them by installing the InfluxDB Node-RED package.

To do so, click on the hamburger menu icon (the three horizontal bars right of the deploy button) and select "Manage palette". The palette settings appear. Switch to the "Installation" tab at the top. In the "search modules" field, enter "node-red-contrib-influxdb". There should be only a few hits for this search. For the one that is called exactly "node-red-contrib-influxdb", click on the install button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20162548_cropped.png" alt="Manage palette menu" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20162726_edited.png" alt="Search for node-red-contrib-influxdb" width="600"/>

A message will appear, allowing you to review the documentation of the module. Once you've done that, confirm the installation by clicking the "install" button. Some moving bars indicate the installation process. Then, you will receive a notification that new nodes were added to the palette. Close the settings.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20163051_cropped.png" alt="Confirm installation" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20163340_cropped.png" alt="Installation in progress" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20163347_cropped.png" alt="Installation complete" width="600"/>

Back in the main view of Node-RED, scroll down in the palette on the left. At the bottom, in the "Storage" section, you should see three nodes called "influxdb in", "influxdb out" and "influxdb out".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20163438_cropped.png" alt="InfluxDB nodes now available at bottom of palette" width="600"/>


## Creating a flow for the temperature data of one sensor

### Node for fetching the temperature data from the MQTT broker

First, you need a node that receives data every time the sensor publishes a new set of values. In the palette, scroll down to the "Network" section and drag an "mqtt in" node into your blank flow. Double-click on the mqtt node in your flow to open its properties.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20214208.png" alt="Adding an mqtt in node to an empty flow" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20214322.png" alt="Default properties of the mqtt node" width="600"/>

As no server is defined so far, click on the plus sign on the "Server" line to add the details of the MQTT server to use. The properties view opens.

In the "Name" field, you can freely choose a server name. In my case, I used "Mosquitto on 192.168.178.28". You may want to name it similarly.

In the "Server" field, enter the IP address of your RPi. In my case, it is 192.168.178.28. The predefined port number 1883 is fine, leave it as it is. You can also leave the check mark for "Connect automatically". The TLS option can remain unchecked as we are not using any encryption for the data transfer from the MQTT broker to Node-RED.

All other options can remain at their default values (Protocol: MQTT V3.1.1, Client-ID: empty, Keep-Alive: 60, Session: clean session checked).

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20215458.png" alt="Server settings: connection tab" width="800"/>

Switch to the Security tab. Enter the user name and password you defined for Mosquitto in [part 2](/2025/02/18/02-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html) of this tutorial (section "Mosquitto configuration).

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20215514.png" alt="Server settings: security tab" width="600"/>

In the "Messages" tab, there is no need to change the default settings. Click the "Add" button at the top right to activate the server settings.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20215946.png" alt="Server settings: messages tab" width="600"/>

This will take you back to the properties of the mqtt node. Now, the "Server" field shows the server name you entered before.

For "Action", leave the default "Subscribe to single topic". In the "Topic" field, repeat what you entered as "MQTT prefix"  during the sensor configuration (part 5 of this tutorial, section "Adjusting the sensor’s settings"). In the same field, just behind of what you just entered, add "/status/temperature:0" (no spaces). In my case, the "Topic" field contains "shellies/altbau/eg/ht8554/status/temperature:0".

Leave QoS (quality of service) at the default value of 2.

Change "Output" to "a parsed JSON object".

Finally, choose a meaningful name for the node. In my case, it is named "Temp_EG", referring to the fact that it provides the temperature value ("Temp") of the sensor on the ground floor ("EG").

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20221638.png" alt="Correct settings of mqtt in node" width="800"/>

Click on "Done" to activate the settings. This will take you back to the flow.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20221726.png" alt="mqtt in node added and configured" width="600"/>

The node you just created is not yet active. It will only become active once you press the "Deploy" button at the top right of the Node-RED web interface. This allows you to perform changes to a flow while the unchanged version still keeps on working in the background. Only when the deploy button is pressed, all changes to the flow become active.

Now, press the deploy button. You'll see a message saying that the changes were successfully deployed. There is now no more blue dot at the top right of the node. Below the node, there is a green marker and a message saying "connected".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-22%20212514.png" alt="mqtt in node added and configured" width="600"/>

This node by itself is not yet useful. Therefore, we will continue adding more nodes to process the data.


### Extracting the temperature

Next, you need a "Change" node to extract the temperature (in degrees Centrigrade) from the temperature data provided by the sensor. In the palette, scroll to the "Function" group and drag a "change" node to the right of the existing "Temp_EG" node. By default, it will appear as "set msg.payload" in the flow.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20225843.png" alt="Change node added to the flow" width="600"/>

Double-click the newly added node to show its settings. Change the node's name to "get °C temperature". Change the rule to "Set" "msg.payload" to the value "msg.payload.tC".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20230556.png" alt="Adjusted settings of the change node" width="800"/>

Click on the "Done" button to return to the flow.

If you are interested in the Fahrenheit temperature, use "msg.payload.tF" instead of "msg.payload.tC" and change the name of the node accordingly.

Caution: All further steps in this tutorial assume that you are using degree Centigrade temperatures. Later on, you will add calculations, e. g. for the absolute humidity. This will only work correctly if you are using the Celsius temperature scale.

Connect the output of the "Temp_EG" node to the input of the "get °C temperature" node by dragging a connection with your mouse. Click and hold the output of "Temp_EG", drag the mouse to the input of "get °C temperature" and release the mouse button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20230833.png" alt="Configured change node connected to mqtt in node" width="600"/>


### Writing the temperature to InfluxDB

Your goal is to store the temperature and humidity data permanently in InfluxDB. You'll thus add an "influxdb out" node to your flow. You can find this node at the bottom of the palette in the "storage" section. Drag an "influxdb out" node into your flow and double click on it to show its properties.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-22%20221228.png" alt="influxdb out node added to flow in Node-RED" width="600"/>

You can leave the Name field empty, the node will then be labelled with its key properties.

There is no InfluxDB server defined so far in Node-RED. Add one by clicking on the plus button, which will take you to the properties page for the server. Use "InfluxDB on 192.168.278.28" in the Name field. Switch to 2.0 in the Version field. In the URL field, enter "http://192.168.178.28:8086". (Node-RED and InfluxDB are running on the same RPi, therefore the default "http://localhost:8086" should also work, but I have not tried it.)

In the Token field, enter the "operator API token" you have obtained in [part 3 of this tutorial](/2025/02/18/03-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html) (section "Setting up InfluxDB"). Click on the "Add" button to establish this InfluxDB server in Node-RED.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-22%20221325.png" alt="InfluxDB server settings" width="800"/>

This will take you back to the properties of the "influxdb out" node. Make sure the Server field contains the newly created "InfluxDB on 192.168.178.28".

Change the "Organization" field to "AS26". Change the "Bucket" field to "MQTT_Live". (You created this organization and this bucket in [part 3 of this tutorial](/2025/02/18/03-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html), section "Setting up InfluxDB".)

Set the "Measurement" field to "tC_eg" (temperature in degrees Centigrade (tC) on the ground floor (eg)). The data will be accessible by this identifier in InfluxDB.

Set the "Time Precision" field to seconds. The Shelly sensors provide their data rather on a time scale of hours, therefore it is sufficient to set the accuracy of the time stamp to seconds. Implement your changes by pressing the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-22%20222217.png" alt="Settings of the influxdb out node" width="800"/>

Finally, connect the "influxdb out" node to the "get °C temperature" by adding a wire between them. Then activate the flow by pressing the "Deploy" button. You should see a message confirming the successful deployment. The blue circle at the top right of the node should have disappeared with the deployment.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-22%20222348.png" alt="influxdb out node added to Node-RED flow" width="600"/>


### Wait

Now, your RPi is collecting temperature data from the Shelly sensor and storing it in InfluxDB. Eventually, you should see the collected data in InfluxDB. However, as the sensor publishes data rather infrequently, you will have to wait some time until you see the result in InfluxDB. Wait approximately one day at this point.

I'm not aware of a way to force the Shelly sensors to publish data.


## Save the flow

[Save the flow and store it somewhere outside of your RPi.]


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

- [IoT Made Easy with Node-RED and InfluxDB](https://www.influxdata.com/blog/iot-easy-node-red-influxdb/)
- [Documentation of node-red-contrib-influxdb 0.7.0](https://flows.nodered.org/node/node-red-contrib-influxdb)

