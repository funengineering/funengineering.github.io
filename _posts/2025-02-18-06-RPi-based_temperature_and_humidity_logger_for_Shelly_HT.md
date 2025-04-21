# RPi-based temperature and humidity logger for Shelly HT (Part 6: Using Node-RED to store temperature and humidity values in InfluxDB)

Now the Shelly H&T sensors transmit their data to the MQTT broker running on your Raspberry Pi. You will now create a "flow" in Node-RED that takes this data and passes it on to InfluxDB, where it is stored. With InfluxDB, it is also possible to visualize the data, but that's not the topic of this part of the tutorial. The goal of this part is only to feed the collected into InfluxDB for permanent storage.


## Creating a flow for one sensor

### Web interface of Node-RED

Navigate to the web interface of Node-RED running on your RPi. In my case, I enter the address `192.168.178.28:1880` in a browser of a computer running in the same local network. `192.168.178.28` is the IP address of my RPi and `1880` is the port where it provides the web interface of Node-RED. If you have followed the tutorial, the port number should also be 1880 in your case. However, the IP address of your RPi depends on your particular local network and might be different. If you are unsure about the IP address of your RPi, review [part 1 of this tutorial](/2025/02/18/01-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html), particularly the section "Network settings".

When your browser shows the web interface of Node-RED, you should see an empty flow at the center of the window.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20161506.png" alt="Initial view of Node-RED with empty flow" width="400"/>


## Title 2

[to be written]


## Save the flow

[Save the flow and store it somewhere outside of your RPi.]


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

[Links to be added here.]

