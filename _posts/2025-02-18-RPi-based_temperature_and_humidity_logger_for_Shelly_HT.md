# RPi-based temperature and humidity logger for Shelly HT

## Record and visualize your temperature/humidity data for free &ndash; locally (no cloud required)
This Raspberry Pi based data logger allows you to record and visualize the temperature and humidity data measured by your [Shelly H&T sensors](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white). This sensor can provide its measured data using the MQTT protocol. This article describes how to set up a Raspberry Pi to record and visualize the recorded temperature and humidity data continuously. It also shows the correct settings that make the sensor(s) aware of the logger. The solution is based on Mosquitto and Node-RED.

## What you need
To follow these instructions, you should have the following material available
- a Raspberry Pi, the corresponding power supply and MicroSD card<br>(in my case, it is a Raspberry Pi Model B V1.2)
- at least one Shelly H&T sensor
- during the installation: access to the (headless) RPi via SSH (e. g. from a laptop or desktop computer)

This text is based on version X.X.XX of the Raspberry Pi OS Lite (64-bit) ("bookworm").

## Installation of the Raspberry Pi OS Lite (64-bit) on the SD card
Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install the Raspberry Pi OS Lite (64-bit) on the SD card. At the time I performed this step, it was called "A port of Debian Bookworm with no desktop environment (Compatible with Raspberry Pi 3/4/400/5)" and had a publication date of 2024-11-19.
Edit the additional settings by clicking the corresponding button.
General tab:
- set username and password: checked, username `pi` and a safe password (i. e. not `raspberry`)
- configure wifi (if needed)[^1]: enter the SSID and the password of your local wireless network and select the appropriate country
- set locale settings: checked, select the time zone and keyboard layout that match your location.
Services tab:
- enable SSH: checked, option "use password authentication"
Options tab:
- keep the default settings
Save the settings. Then confirm the application of these settings by clicking the "Yes" button. Confirm to delete any data that the MicroSDCard might contain. The Raspberry Pi OS will be written on to the MicroSDCard. A window will inform you of the completion of the process.
![Choose OS to write to SDCard](/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224825.png)
<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224825.png" alt="Choose OS to write to SDCard" width="200"/>
[^1]: Wired LAN is more reliable than WLAN, therefore it is recommended to connect the RPi to the local network using a cable.

## Create Inkycal settings
Inkycal is customizeable and lets you choose what it shall display on the ePaper display. The author of Inkycal created a [user-friendly web interface](https://aceinnolab.com/inkycal/ui) that allows you to interactively configure the look of your Inkycal.
For me, it worked with the following settings:
- Model: 9.7in ePaper
- Update interval: every 15 minutes[^2]
- Orientation: Flex cable left
- Show border around each module: no (unchecked)
- Info about calibration: 0; 12; 18 (unchanged)
- Show time of last update: yes (checked), height 30 pixels
- Language: (select according to your preference)
- Font size: 30
- Padding top/bottom: 10 pixels
- Padding right/left: 10 pixels
- Module 1: Calendar
- Module height: 100
- Start of week: Monday
- Show parsed events: True
- iCalendar URL(s): `https://calendar.google.com/calendar/ical/xyz%40gmail.com/private-0123456789abcdef0123456789abcdef/basic.ics`[^3]
- iCalendar file paths: (leave empty)
- Token for custom date formatting: (leave empty)
- Token for custom time formatting: (leave empty)
[^2]: If you are using a PiSugar2 UPS, this setting will be overridden
[^3]: If you are using Google calendar, look this up in your account settings. The 32 digit hexadecimal number after `private-` will be different from the one shown here. If you are using another calendar, enter the corresponding iCal URL.
Using the web interface, download the settings file `settings.json` to your local computer.
Copy the `settings.json` file to the SD card ("bootfs"). Eject the the SD card from your computer and insert it into the RPi.

## Preparation of the Raspberry Pi OS
(to be continued here...)
