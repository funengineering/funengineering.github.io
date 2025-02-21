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

## Installation of the Raspberry Pi OS Lite (64-bit) on the SD card
Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install the Raspberry Pi OS Lite (64-bit) on the SD card. At the time I performed this step, it was called "A port of Debian Bookworm with no desktop environment (Compatible with Raspberry Pi 3/4/400/5)" and had a publication date of 2024-11-19. This corresponds to version 6.6.51 of the Raspberry Pi OS Lite (64-bit) ("bookworm").

Edit the additional settings by clicking the corresponding button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224825.png" alt="Choose OS to write to SDCard" width="300"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224856_edited.png" alt="Change settings" width="300"/>

General tab:
- set username and password: checked, username `pi` and a safe password (i. e. not `raspberry`)
- configure wifi (only if needed): enter the SSID and the password of your local wireless network and select the appropriate country
- set locale settings: checked, select the time zone and keyboard layout that match your location.

Services tab:
- enable SSH: checked, option "use password authentication"

Options tab:
- keep the default settings

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225346_edited.png" alt="General settings" width="300"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225402.png" alt="Services settings" width="300"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225422.png" alt="Options settings" width="300"/>

Save the settings. Then confirm the application of these settings by clicking the "Yes" button. Confirm to delete any data that the MicroSDCard might contain. The Raspberry Pi OS will be written on to the MicroSDCard. A window will inform you of the completion of the process.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225451_edited.png" alt="Confirm to use settings" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225517_edited.png" alt="Delete all data on SDCard" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20230116.png" alt="OS successfully written to SDCard" width="200"/>

Note:
Wired LAN is more reliable than WLAN, therefore it is recommended to connect the RPi to the local network using a cable.


## Network settings

To login to your RPi using ssh (secure shell), it might be helpful to assign a fixed IP address to it. You may need the MAC address of the RPi. The MAC address is usually shown as a hexadecimal number having the format NN:NN:NN:NN:NN:NN (total of 12 hex digits (0-9, A-F)). If you don't know it, you can find it using a network scanner.

Edit your router's settings in your network's router such that it assigns an IP address you define to the MAC address of your RPi. Below, I will be assuming that the IP address of the RPi is 192.168.178.28. Depending on your network's settings, you might have to use a different address, e. g. 192.168.xxx.xxx or 10.xxx.xxx.xxx.


## First login

Use your favorite ssh client (e. g. PuTTY) to connect to the RPi from your laptop or desktop computer.
