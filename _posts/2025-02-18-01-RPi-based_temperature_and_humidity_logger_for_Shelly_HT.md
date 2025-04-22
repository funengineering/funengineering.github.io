# RPi-based temperature and humidity logger for Shelly HT (Part 1: OS installation)

## Installation of the Raspberry Pi OS Lite (64-bit) on the SD card
Use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install the Raspberry Pi OS Lite (64-bit) on the SD card. At the time I performed this step, it was called "A port of Debian Bookworm with no desktop environment (Compatible with Raspberry Pi 3/4/400/5)" and had a publication date of 2024-11-19. This corresponds to version 6.6.51 of the Raspberry Pi OS Lite (64-bit) ("bookworm").

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224825.png" alt="Choose OS to write to SDCard" width="400"/>

Edit the additional settings by clicking the corresponding button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20224856_edited.png" alt="Change settings" width="400"/>

General tab:
- set username and password: checked, username `pi` and a safe password (i. e. not `raspberry`)
- configure wifi (only if needed): enter the SSID and the password of your local wireless network and select the appropriate country
- set locale settings: checked, select the time zone and keyboard layout that match your location.

If you are planning to operate the RPi on a wired network, disable wifi.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225346_edited.png" alt="General settings" width="400"/>

Services tab:
- enable SSH: checked, option "use password authentication"

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225402.png" alt="Services settings" width="400"/>

Options tab:
- keep the default settings

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225422.png" alt="Options settings" width="400"/>

Save the settings. Then confirm the application of these settings by clicking the "Yes" button. Confirm to delete any data that the MicroSDCard might contain. The Raspberry Pi OS will be written on to the MicroSDCard. A window will inform you of the completion of the process.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225451_edited.png" alt="Confirm to use settings" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20225517_edited.png" alt="Delete all data on SDCard" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-19%20230116.png" alt="OS successfully written to SDCard" width="400"/>

Note:
Wired LAN is more reliable than WLAN, therefore it is recommended to connect the RPi to the local network using a cable.


## Network settings

Insert the SDCard into your RPi. If you are using a wired network, also connect the LAN cable to the RPi. Start the RPi by connecting it to its power supply.

To login to your RPi using SSH (secure shell), it might be helpful to assign a fixed IP address to it. By default, your RPi receives its IP address from your router via the dynamic host configuration protocol (DHCP). Thus, you have to tell your router to recognize your RPi by its unique MAC address and to assign the IP address of your choice to it.

If you don't know your RPi's MAC address, you can find it using a network scanner. Download a network scanner app for your mobile phone (e. g. Fing or Net Analyzer), make sure the phone is connected to your home network and perform a network scan. Your RPi should show up in the list of network devices. In the details of this device, you should see its MAC address. The MAC address is typically shown as a hexadecimal number having the format NN:NN:NN:NN:NN:NN (total of 12 hex digits (0-9, A-F), separated in groups of two digits by colons). 

Edit the settings of your network's router such that it assigns the IP address you define to the device having the MAC address you found (which is your RPi). **Below, I will be using the IP address 192.168.178.28, which is the IP address of *my* RPi. Depending on your network's settings, *your* RPi probably has a different IP address**, e. g. 192.168.xxx.xxx or 10.xxx.xxx.xxx (where xxx are numbers). **You will need the IP address of *your* RPi at multiple places throughout this tutorial. Make sure you know it and use it instead of 192.168.178.28 below.**


## First login

Use your favorite SSH client (e. g. PuTTY) to connect to the RPi from your laptop or desktop computer. Create a SSH connection to your RPi by providing its IP address. Use port 22, which is the default for SSH.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-21%20232418.png" alt="PuTTY parameters" width="400"/>

During the first login, you will be asked if you trust the client to be the correct one. In your home network, there is most likely no risk of someone else trying to set up a malicious, fake RPi. Therefore, it is ok to confirm this.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-11%20231928.png" alt="PuTTY parameters" width="400"/>

When the connection is established, you will see a terminal window showing a login prompt. Use the username and password defined during the OS installation to log in.

You should now see the Linux command prompt. As a test, type the command `uname -a`. It will output the OS version you are using.

End the SSH session with the `exit` command. If you are using PuTTY as a SSH client, this will close your terminal window.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-21%20232459.png" alt="Terminal window with first login" width="600"/>


## Update the package manager

Software installation is managed by the package manager. Before using it, make sure that it knows about the latest updates. Do so by logging into your RPi via SSH. Then, issue the command:

`sudo apt update`

Then, do a reboot with the command

`sudo reboot`

This will terminate the SSH connection. Wait for the RPi to reboot. Then, establish a new SSH connection with PuTTY.
