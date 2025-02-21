# RPi-based temperature and humidity logger for Shelly HT

## Record and visualize your temperature/humidity data for free &ndash; locally (no cloud required)
This Raspberry Pi based data logger allows you to record and visualize the temperature and humidity data measured by your [Shelly H&T sensors](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white). This sensor can provide its measured data using the MQTT protocol. This article describes how to set up a Raspberry Pi to record and visualize the recorded temperature and humidity data continuously. It also shows the correct settings that make the sensor(s) aware of the logger. The solution is based on Mosquitto and Node-RED.

## What you need
To follow these instructions, you should have the following material available
- a Raspberry Pi, the corresponding power supply and MicroSD card<br>(in my case, it is a Raspberry Pi Model B V1.2)
- at least one Shelly H&T sensor
- during the installation: access to the (headless) RPi via SSH (e. g. from a laptop or desktop computer)

This text is based on version 6.6.51 of the Raspberry Pi OS Lite (64-bit) ("bookworm").

## Installation of the Raspberry Pi OS Lite (64-bit) on the SD card
Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install the Raspberry Pi OS Lite (64-bit) on the SD card. At the time I performed this step, it was called "A port of Debian Bookworm with no desktop environment (Compatible with Raspberry Pi 3/4/400/5)" and had a publication date of 2024-11-19.
Edit the additional settings by clicking the corresponding button.

General tab:
- set username and password: checked, username `pi` and a safe password (i. e. not `raspberry`)
- configure wifi (only if needed): enter the SSID and the password of your local wireless network and select the appropriate country
- set locale settings: checked, select the time zone and keyboard layout that match your location.

Services tab:
- enable SSH: checked, option "use password authentication"

Options tab:
- keep the default settings

Save the settings. Then confirm the application of these settings by clicking the "Yes" button. Confirm to delete any data that the MicroSDCard might contain. The Raspberry Pi OS will be written on to the MicroSDCard. A window will inform you of the completion of the process.

Note: Wired LAN is more reliable than WLAN, therefore it is recommended to connect the RPi to the local network using a cable.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224825.png" alt="Choose OS to write to SDCard" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224856_edited.png" alt="Change settings" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225346_edited.png" alt="General settings" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225402.png" alt="Services settings" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225422.png" alt="Options settings" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225451_edited.png" alt="Confirm to use settings" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225517_edited.png" alt="Delete all data on SDCard" width="200"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20230116.png" alt="OS successfully written to SDCard" width="200"/>

(To be continued here)
