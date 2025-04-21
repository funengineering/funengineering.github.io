# RPi-based temperature and humidity logger for Shelly HT (Part 6: Using Node-RED to store temperature and humidity values in InfluxDB)

Now the Shelly H&T sensors transmit their data to the MQTT broker running on your Raspberry Pi. You will now create a "flow" in Node-RED that takes this data and passes it on to InfluxDB, where it is stored. With InfluxDB, it is also possible to visualize the data, but that's not the topic of this part of the tutorial. The goal of this part is only to feed the collected into InfluxDB for permanent storage.


## Creating a flow for one sensor

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


## Title 2

[to be written]


## Save the flow

[Save the flow and store it somewhere outside of your RPi.]


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

- [IoT Made Easy with Node-RED and InfluxDB](https://www.influxdata.com/blog/iot-easy-node-red-influxdb/)
- [Documentation of node-red-contrib-influxdb 0.7.0](https://flows.nodered.org/node/node-red-contrib-influxdb)

