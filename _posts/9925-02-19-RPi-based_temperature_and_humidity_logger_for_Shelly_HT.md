# RPi-based temperature and humidity logger for Shelly H&T

## Record and visualize your temperature/humidity data locally for free &ndash; No cloud required
This Raspberry Pi based data logger allows you to record and visualize the temperature and humidity data measured by your [Shelly H&T sensors](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white). This sensor can provide its measured data using the MQTT protocol. This article describes how to set up a Raspberry Pi to record and visualize the recorded temperature and humidity data continuously. It also shows the correct settings that make the sensor(s) aware of the logger. The solution is based on Mosquitto and Node-RED.

## What you need
To follow these instructions, you should have the following material available
- a Raspberry Pi, the corresponding power supply and MicroSD card (in my case, it is a Raspberry Pi Model B V1.2)
- at least one Shelly H&T sensor
- during the installation: access to the (headless) RPi via SSH (e. g. from a laptop or desktop computer)

This text is based on version X.X.XX of the Raspberry Pi OS Lite (64-bit) ("bookworm").

## Installation of the Raspberry Pi OS Lite (32-bit) on the SD card
Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install the Raspberry Pi OS Lite (32-bit) on the SD card. When I performed this step, it was called "A port of Debian Bullseye with security updates and no desktop environment" in the list and had a publication date of 2023-05-03. Inkycal 2.0.2 does not work with the newer "bookworm" version of Raspberry Pi OS[^1].
[^1]: The newer version 2.0.3 of Inkycal is compatible with the "bookworm" version of Raspberry Pi OS. I have not tried using this combination. However, this text is *not* based on these newer versions.
Use these addtional settings (available when clicking on the gear button):
- set hostname: checked, `inkycal`
- enable SSH: checked, option "use password authentication"
- set username and password: checked, username `pi` and password `raspberry` (or any other password you like)
- configure wifi: enter the SSID and the password of your local wireless network and select the appropriate country
- set locale settings: checked, select the time zone and keyboard layout that match your location.
Write the OS to the SD card.

Note: The SD card containing the Raspberry Pi OS is formatted for Linux. This can cause strange messages when inserting this SD card into a computer running Windows 10. If asked, always reject requests to format the SD card.

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
