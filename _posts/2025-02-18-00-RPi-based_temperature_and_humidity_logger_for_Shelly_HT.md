# RPi-based temperature and humidity logger for Shelly HT (Overview)

## Record and visualize your temperature/humidity data for free &ndash; locally (no cloud required)
This Raspberry Pi based data logger allows you to record and visualize the temperature and humidity data measured by your [Shelly H&T sensors](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white). These sensors provide their measured data using the MQTT protocol. This article describes how to set up a Raspberry Pi to receive this data, to store it in a database and to visualize it. It also shows the correct settings for the sensors, enabling them to send their data to your RPi.

## What you need
To follow these instructions, you should have the following material available
- a Raspberry Pi, the corresponding power supply and MicroSD card<br>(in my case, it is a Raspberry Pi 4 Model B with 4GB RAM)
- at least one Shelly H&T sensor
- access to the (headless) RPi via SSH and with a web browser (e. g. from a laptop or desktop computer in the same network)

## How it works
By following these instructions, you will be installing these software components on your RPi:
- [Mosquitto](https://mosquitto.org/): An open source MQTT broker. This component allows the RPi to receive the temperature and humidity data sent by your Shelly devices. They are communicating using the "Message Queueing Telemetry Transport" protocol.
- [Node-RED](https://nodered.org/): A popular home automation software. Its graphical interfaces allows you to set up the collection and processing of the real-time data of the Shelly sensors. You will access its web interface from the browser of any computer in your network.
- [InfluxDB](https://www.influxdata.com/products/influxdb/): A database solution that is specialized in time series data. It stores the temperature and humidity values and provides the visualization of the data in its web interface. It allows to generate time-series graphs, for example.


## Procedure
The main steps are:
1. [Part 1: Installation of the Raspberry Pi OS](/2025/02/18/01-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
2. [Part 2: Installation of Mosquitto on the RPi](/2025/02/18/02-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
3. [Part 3: Installation of InfluxDB on the RPi](/2025/02/18/03-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
4. [Part 4: Installation of Node-RED on the RPi](/2025/02/18/04-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
5. [Part 5: Configuration of the Shelly devices](/2025/02/18/05-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
6. [Part 6: Using Node-RED to collect temperature data](/2025/02/18/06-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
7. [Part 7: Collecting humidity data (and more)](/2025/02/18/07-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
8. [Part 8: Using multiple sensors](/2025/02/18/08-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html)
9. Part 9: Creating a dashboard that shows the data graphically

Read the parts, follow the instructions and enjoy the result!

And if you're still interested in going further, read the bonus part on how to calculate and store extra data such as the absolute humidity or the dew point.
