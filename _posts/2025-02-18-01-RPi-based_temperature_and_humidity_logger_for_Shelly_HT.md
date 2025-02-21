# RPi-based temperature and humidity logger for Shelly HT (Overview)

## Record and visualize your temperature/humidity data for free &ndash; locally (no cloud required)
This Raspberry Pi based data logger allows you to record and visualize the temperature and humidity data measured by your [Shelly H&T sensors](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white). This sensor can provide its measured data using the MQTT protocol. This article describes how to set up a Raspberry Pi to record and visualize the recorded temperature and humidity data continuously. It also shows the correct settings that make the sensor(s) aware of the logger. The solution is based on Mosquitto and Node-RED.

## What you need
To follow these instructions, you should have the following material available
- a Raspberry Pi, the corresponding power supply and MicroSD card<br>(in my case, it is a Raspberry Pi Model B V1.2)
- at least one Shelly H&T sensor
- during the installation: access to the (headless) RPi via SSH (e. g. from a laptop or desktop computer)

## How it works
By following these instructions, you will be installing these software components on your RPi:
- [Mosquitto](https://mosquitto.org/): An open source MQTT broker. This component allows the RPi to receive the temperatur and humidity data sent by your Shelly devices. They are communicating using the "Message Queueing Telemetry Transport" protocol.
- [Node-RED](https://nodered.org/): A popular home automation software. Its graphical interfaces allows you to set up the collection and processing of the real-time data of the Shelly sensors. You will access its web interface from the browser of any computer in your network.
- [InfluxDB](https://www.influxdata.com/products/influxdb/): A database solution that is specialized in time series data. It stores the temperature and humidity values and provides the visualization of the data in its web interface. It allows to generate time-series graphs, for example.


## Procedure
The main steps are:
1. [Part 1: Installation of the Raspberry Pi OS](https://funengineering.github.io/)
2. [Part 2: Installation of Mosquitto on the RPi](https://funengineering.github.io/)
3. [Part 3: Installation of InfluxDB on the RPi](https://funengineering.github.io/)
4. [Part 4: Installation of Node-RED on the RPi](https://funengineering.github.io/)
5. [Part 5: Configuration of the Shelly devices](https://funengineering.github.io/)

Read the parts, follow the instructions and enjoy the result!
