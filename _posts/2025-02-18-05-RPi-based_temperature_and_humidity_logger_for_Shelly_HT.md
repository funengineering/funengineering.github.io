# RPi-based temperature and humidity logger for Shelly HT (Part 5: Configuration of the Shelly devices)

The Shelly H&T sensors will transmit their measured temperature and humidity data to your RPi. To make this work, you have to provide them with information about the target and the transmission channel. This part of the tutorial explains how to adjust the sensors' settings accordingly.


## Prerequisites

This tutorial assumes that you are using a [Shelly H&T](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white) Wi-Fi temperature and humidity sensor. I'm not affiliated in any way with the producer of these devices. However, I'm using them myself because they're not limited to proprietary protocols and because they also work if one does not want to feed all data into the supplier's cloud. Additionally, they work for many months on normal batteries, they display the data on their integrated e-paper display, and I like their design.

If you have a different sensor, you might also be able to use it. If it provides its data using MQTT, you just have to figure out (by yourself) the correct settings. If it uses a different protocol, you might still be able to access the data with Node-RED, but this will require additional changes (most likely in Node-RED) that will not be addressed in this tutorial.

The information shown here is based on the Shelly firmware version 1.5.1. If your device has a different firmware, things might look differently. However, it is very likely that you will nevertheless be able to set it up similarly.

If you have multiple Shelly sensors, you will have to perform the steps below with all of them. Note that you will want to have slightly different settings for each one of them, e. g. the sensor name and the channel it uses to publish its data. Adjust the settings accordingly in this case.


## Integrating a new Shelly device into your Wi-Fi network

Initially, the sensor does not have access to your Wi-Fi. Therefore, connecting to it for the first time requires some extra effort. Once it is connected to your Wi-Fi, it will be simpler. 

Open your sensor using an appropriate tool. Insert batteries and check the integrated display. It should change and show the current temperature. Note that it might show a higher temperature as long as you are in the process of setting it up. This is normal and it will later show the correct temperature once it is in normal operation.

<img src="/docs/assets/img/ht_logger/20250420_172733258_edited.jpg" alt="Opening of a Shelly Humidity and Temperature Wi-Fi sensor" width="400"/>

Next, press the small button on the back of the sensor. It is located next to the USB-C connector. This activates the sensor's own Wi-Fi network. The front display should change to "Set" and "AP" (for "access point").

<img src="/docs/assets/img/ht_logger/20250420_180343474_cropped.jpg" alt="Pressing the button" width="400"/>

<img src="/docs/assets/img/ht_logger/20250420_173648464_cropped.jpg" alt="Shelly in setup mode" width="400"/>

The sensor now provides a Wi-Fi access point. For the first steps of the setup, you'll have to connect to it. I normally do this using my mobile phone, but a PC or tablet will also work. Go to the settings of the mobile phone and select the Wi-Fi network called "ShellyHTG3-0123456789ABCDEF", where 0123456789ABCDEF is your sensor's individual 12 digit identifier (aka MAC address). Your phone might ask you for confirmation because this Wi-Fi does not provide internet access. This is ok and you can confirm.

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-230953_edited.png" alt="Shelly WLAN visible on mobile phone" width="250"/>

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231016_edited.png" alt="Confirm WLAN without internet access" width="250"/>

When you are connected to this Wi-Fi network, open a browser on your phone and enter `192.168.33.1` in the address field. This should take you to the web interface of your Shelly sensor.

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231119_edited.png" alt="Home screen of Shelly web interface" width="250"/>

Note: The Shelly sensor is built to use as little power as possible, which allows it to operate on batteries for months. However, this also means that it will return to its power-saving state quite quickly. If this happens, your mobile phone will lose its connection to the sensor and the sensor's front display will go back to the normal temperature display. In this case, you have to press the button on the back of the sensor again. Then, connect again to the "ShellyHTG3-0123456789ABCDEF" Wi-Fi, confirm the missing internet connection and browse to 192.168.33.1 again. Then continue with the setup. Depending on how long it takes you to perform the setup, this might happen more than once. This might be somewhat annoying, but you will later love the long battery life if you operate your sensor without an external power source.

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231214_edited.png" alt="Connection lost because Shelly is not in setup mode anymore" width="250"/>

To connect your Shelly sensor to your Wi-Fi network, navigate to the Wi-Fi settings in the web interface of the Shelly sensor (on your mobile phone). Click on the hamburger menu (the three bars left of the "H&T Gen3" title) and select "Wi-Fi". Alternatively, you can also click on the Wi-Fi symbol at the top (the second symbol from the left). You should see in the Wi-Fi status that your sensor is not connected.

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231142_edited.png" alt="Shelly initially not connected" width="250"/>

Enter the name (SSID) and the password of your Wi-Fi network in the corresponding fields of the "Wi-Fi 1 settings" and press "Save settings".

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231406_edited.png" alt="Entering Wi-Fi settings" width="250"/>

This should allow your Shelly sensor to connect to your Wi-Fi network. If it is successfully connected to your Wi-Fi, you should see your network's name in the Wi-Fi status. You can also see the IP address of the sensor there.

<img src="/docs/assets/img/ht_logger/Screenshot_20250417-231652_edited.png" alt="Connected to a Wi-Fi network" width="250"/>

From now on, you should be able to access the sensor's web interface from any computer in your local network. Just make sure that the sensor is in the setup mode (press its button if it isn't). Then, enter the sensor's IP address in the address bar of a browser on your computer. Accessing the sensor's web interface from a computer is more convenient, therefore I prefer it over the access from a mobile phone. You will not need the connection from the mobile phone to your Shelly sensor anymore.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20184829_edited.png" alt="Web interface as seen in a browser from a computer" width="400"/>


### Checking for firmware update

Once your Shelly sensor is connected to your Wi-Fi network, you should check if it has the latest firmware. Here is how to do this.

Make sure that your sensor is in setup mode: It should show "Set" on its front display. If it is not, press its button next to the USB-C connector.

Access the sensor's web interface from a browser on your computer. Then, click on "Settings" and select "Firmware" in the "Device settings" section.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20184934_edited.png" alt="Navigating to Settings/Device Settings/Firmware" width="600"/>

If there are any firmware updates available, you should be able to install it from there. This will take some time and you will have to reconnect to the device afterwards. The Wi-Fi settings should be preserved, so you should be able to reconnect from your computer. I recommend to use only the stable version of the firmware and not the beta version of the next revision. In the screenshot below, you can see what it looks like if the latest available stable version (1.5.1 at the time) is already installed.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20185103_edited.png" alt="Latest stable firmware installed" width="600"/>


## Adjusting the sensor's settings

Finally, you can proceed to the main step of this part of the tutorial: adjusting the settings for the sensor to transmit its measured data to your RPi.

On the sensor's web interface, go to Settings / Connectivity settings / MQTT. First, enable MQTT by checking "Enable". The connection type "No TLS" can remain as it is.

The MQTT prefix defines the topic it uses to publish its data (i. e. messages). In my case, I use a topic name that includes information about the general type of sensor (i. e. that it is one of my Shelly H&T sensors), the location (e. g. "altbau/eg", which refers to the building and the floor) and the MAC address of the sensor. This means that I'm using "shellies/altbau/eg/ht8554" as an MQTT prefix. Depending on your application, a different prefix structure might be more suitable.

In the "Server" field, enter the name or IP address of your RPi, as this is the device running Mosquitto. In my case, it is "192.168.178.28". In your local network, the RPi will most likely have a different IP address, so you have to adjust this setting accordingly.

For the "Client ID", I'm using the same structure I'm using for the MQTT prefix. Again, if your application differs, you can adjust this to your needs.

In the "Username" and "Password" fields, enter the user name and password you set during the installation of Mosquitto. In my case, the user name is "pi". For the password, enter the same one that you used during the installation.

Enable "Generic status update over MQTT". This will cause the sensor to publish additional information about its status via MQTT.

Finally, press the "Save settings" button.

You will see that you are requested to reboot the sensor. When the sensor is still in setup mode, you can do this by clicking on "reboot" in the corresponding message. If the sensor is not in setup mode anymore, press its button again. Then, on the web interface of the sensor, go to Settings / Device settings / Reboot device and click on the Reboot button. Confirm by clicking the "Reboot" button in the message box that appears. You will see that the front display of the device will be reset during the reboot. When the reboot has finished, you'll see the temperature and humidity values again.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20225648_edited.png" alt="MQTT settings adjusted" width="800"/>

Note: Since I have multiple sensors and I wanted to identify easily which sensor a particular message was coming from, I included the last four digits of each sensor's MAC address in its topic name. This might not be a wise choice, though. I had to replace a sensor (in fact, the one you are seeing here) and since the MAC address is unique for each sensor, the new one had a different MAC address. However, the MAC address of the original (replaced) sensor was already used in the software setup. Changing the topic name at a later stage is not that simple, thus I decided to keep it unchanged. This means that now the topic name (particularly the last four digits of the MAC address included in the topic name) don't match with the real MAC address of the replacement sensor anymore. It is not a problem with respect to the functionality of the system (it works), it is just a small inconsistency in the naming. You might want to refrain from using any part of the MAC address in the topic names for your sensors right from the beginning.


## Optional additional setting changes

### Changing the sensor's device name

In my case, I changed the default device name ("H&T Gen3") to a name that does not contain special characters (no space, no ampersand) and includes the sensor's MAC address. The removal of the special characters might not be necessary, but in case of problems, it removes any doubt that it might be related to special characters. The device name can be changed under Settings / Device settings / Device name.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20221248_edited.png" alt="Device name changed" width="600"/>

### Disable Bluetooth

If you don't need Bluetooth, you can disable it under Settings / Network settings / Bluetooth by removing the check mark in front of "Enable" and pressing the "Save settings" button. In my case, Bluetooth is disabled. The Bluetooth symbol at the top will change its color from blue to gray.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20221648_edited.png" alt="Bluetooth disabled" width="600"/>

### No cloud needed

For the purpose of the setup described here, no cloud connection is needed. All data is stored and processed on your own devices. Any cloud setting on the sensor can remain deactivated (Settings / Connectivity settings / Cloud).

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-21%20152757_edited.png" alt="Cloud settings disabled" width="600"/>

### Access point

It is wise to keep the access point setting enabled. If your sensor loses connection to your Wi-Fi network, you can only access its settings if you can connect to it via the access point it provides (as you did during the inital setup). Therefore, keep the access point settings enabled under Settings / Network settings / Access Point.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-20%20222625_edited.png" alt="Bluetooth disabled" width="600"/>


## Repeat if you have multiple sensors

If you have multiple sensors, repeat the above steps for each one of them. Make sure that you keep the "MQTT prefix" individual for each one of them, otherwise, you will not be able to distinguish the data from the different sensors later.


## What's next?

In the next step, you'll be collecting the data provided by the sensor(s) in Node-RED and store them in InfluxDB.


## Links

[to be added]

