# RPi-based temperature and humidity logger for Shelly HT (Part 1: OS installation)

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

To login to your RPi using SSH (secure shell), it might be helpful to assign a fixed IP address to it. You may need the MAC address of the RPi. The MAC address is usually shown as a hexadecimal number having the format NN:NN:NN:NN:NN:NN (total of 12 hex digits (0-9, A-F)). If you don't know it, you can find it using a network scanner.

Edit your router's settings in your network's router such that it assigns an IP address you define to the MAC address of your RPi. Below, I will be assuming that the IP address of the RPi is 192.168.178.28. Depending on your network's settings, you might have to use a different address, e. g. 192.168.xxx.xxx or 10.xxx.xxx.xxx.


## First login

Use your favorite SSH client (e. g. PuTTY) to connect to the RPi from your laptop or desktop computer. Create an SSH connection to your RPi by providing its IP address. Use port 22, as this is the standard port for SSH. When the connection is established, you will see a terminal window showing a login prompt. Use the username and password defined during the OS installation to log in.

During the first login, you will be asked if you trust the client to be the correct one. In your home network, there is most likely no risk of someone else trying to set up a malicious, fake RPi. Therefore, it is ok to confirm this.

You should now see the Linux command prompt. Type the command `uname -a`. It will output the OS version you are using.

End the SSH session with the `exit` command. If you are using PuTTY as a SSH client, this will close your terminal window.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-02-21%20232418.png" alt="PuTTY parameters" width="200"/> <img src="/docs/assets/img/ht_logger/Screenshot%202025-02-21%20232459.png" alt="Terminal window with first login" width="200"/>
