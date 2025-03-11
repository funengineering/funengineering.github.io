# RPi-based temperature and humidity logger for Shelly HT (Part 3: InfluxDB OSS v2 installation)

Collecting and storing the temperature and humidity data will be done by InfluxDB. Therefore, you will now install InfluxDB.

## Gaining access to the InfluxDB repository

(This is not working, checking for the solution...)

InfluxDB is not stored in the standard software repositories. Therefore, an additional repository must be added. The commands in this section are taken from the InfluxDB documentation. If they don't work as expected, check the [InfluxDB documentation](https://docs.influxdata.com/influxdb/v2/install/?t=Linux). It is possible that the supplier influxdata makes changes to this process.

Before installing InfluxDB, you need a key that will grant you access the InfluxDB repository. Download this key with the following command.

`wget -q https://repos.influxdata.com/influxdata-archive_compat.key`

Then, check the fingerprint of the key.

`gpg --with-fingerprint --show-keys ./influxdata-archive_compat.key`

The fingerprint should be `9D53 9D90 D332 8DC7 D6C8  D3B9 D8FF 8E1F 7DF8 B07E`.

Install the key for the InfluxDB repository. Make sure your type the command _exactly_ as shown below. (On Windows, copy it to the clipboard and paste it in the PuTTY window by pressing the right mouse button.)

`echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum --check - && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /usr/share/keyrings/influxdata-archive_compat.gpg > /dev/null`

Update your APT sources to use the new key. Again, make sure to type everything _exactly_ as shown below.

`echo 'deb [signed-by=/usr/share/keyrings/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list`

Check if it worked.

`cat /etc/apt/sources.list.d/influxdata.list`

Remove the downloaded key.

`rm influxdata-archive_compat.key`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20224002.png" alt="Gaining access to the InfluxDB repository" width="400"/>

Update the package database.

`apt policy influxdb*`

`wget http://archive.raspberrypi.org/debian/raspberrypi.gpg.key`

`sudo apt-key add raspberrypi.gpg.key`

`rm raspberrypi.gpg.key`

`sudo apt update`


## Installing InfluxDB

Now that you have access to the InfluxDB repository, you can install InfluxDB with the following command.

`sudo apt-get install influxdb2`


[to be continued here...]

At this point, you might want to shut down your RPi, remove the SDCard and create a backup image of the SDCard. This will allow you go back to this point by flashing this image to SDCard if anything goes wrong later on.


## Links

- undated: [influxdata download page](https://www.influxdata.com/downloads/)
- undated: [influxdata installation instructions](https://docs.influxdata.com/influxdb/v2/install/) (outdated for Raspberry Pi OS bookworm)
- 2023-01-24: [Update: Linux Package Signing Key Rotation (influxdata)](https://www.influxdata.com/blog/linux-package-signing-key-rotation/)
- 2023-07-28: [Influx DB utner bookworm installieren](https://forum.iobroker.net/topic/67216/influx-db-unter-bookworm-installieren/19)
