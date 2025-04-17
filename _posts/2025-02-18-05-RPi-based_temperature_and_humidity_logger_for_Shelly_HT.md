# RPi-based temperature and humidity logger for Shelly HT (Part 5: Configuration of the Shelly devices)

The Shelly H&T sensors will transmit their measured temperature and humidity data to your RPi. To make this work, you have to provide them with information about the target and the transmission channel. This part of the tutorial explains how to adjust the sensors' settings accordingly.


## Prerequisites

This tutorial assumes that you are using a [Shelly H&T](https://www.shelly.com/de/products/shelly-h-t-gen3-matte-white) Wi-Fi temperature and humidity sensor. I'm not affiliated in any way with the producer of these devices. However, I'm using them myself because they're not limited to proprietary protocols and because they also work if one does not want to feed all data into the supplier's cloud. Additionally, they work for many months on normal batteries, they display the data on their integrated e-paper display, and I like their design.

If you have a different sensor, you might also be able to use it. If it provides its data using MQTT, you just have to figure out (by yourself) the correct settings. If it uses a different protocol, you might still be able to access the data with Node-RED, but this will require additional changes (most likely in Node-RED) that will not be addressed in this tutorial.

The information shown here is based on the Shelly firmware version 1.5.1. If your device has a different firmware, things might look differently. However, it is very likely that you will nevertheless be able to set it up similarly.

If you have multiple Shelly sensors, you will have to perform the steps below with all of them. Note that you will want to have slightly different settings for each one of them, e. g. the sensor name and the channel it uses to publish its data. Adjust the settings accordingly in this case.


## Integrating a new Shelly device into your Wi-Fi network

Initially, the sensor does not have access to your Wi-Fi. Therefore, connecting to it for the first time is a bit complicated. Once it is connected to your Wi-Fi, it will be simpler. 

Open your sensor using an appropriate tool. Insert batteries and check the integrated display. It should change and show the current temperature. Note that it might show a higher temperature as long as you are in the process of setting it up. This is normal and it will later show the correct temperature once it is in normal operation.

Next, press the small button on the back of the sensor. It is located next to the USB-C connector. This activates the sensor's own Wi-Fi network. The front display should change to "Set" and "AP" (for "access point"). The sensor now provides a Wi-Fi access point. For the first steps of the setup, you'll have to connect to it. I normally do this using my mobile phone, but a PC or tablet will also work. Go to the settings of the mobile phone and select the Wi-Fi network called "ShellyHTG3-0123456789ABCDEF", where 0123456789ABCDEF is your sensor's individual 12 digit identifier (aka MAC address). Your phone might ask you for confirmation because this Wi-Fi does not provide internet access. This is ok and you can confirm.

When you are connected to this Wi-Fi network, open a browser on your phone and enter `192.168.33.1` in the address field. You should see the web interface of your Shelly sensor.

Note: The Shelly sensor is built to use as little power as possible, which allows it to operate for months on batteries. However, this also means that it will return to its power-saving state quite quickly. If this happens, the front display will go back to the normal temperature display and your mobile phone will lose its connection to the sensor. In this case, you have to press the button on the back of the sensor again. Then, connect again to the "ShellyHTG3-0123456789ABCDEF" Wi-Fi, confirm the missing internet connection and browse to 192.168.33.1 again. Then continue with the setup. Depending on how long it takes you to perform the setup, this might happen more than once. This might be somewhat annoying, but you will later love the long battery life if you operate your sensor without an external power source.


### 

[to be written]

[Firmware update once it is in the network]


## Adjusting the settings

[to be written]


## What's next?

[to be written]


## Links

[to be added]

