# RPi-based temperature and humidity logger for Shelly HT (Part 3: InfluxDB OSS v2 installation)

Collecting and storing the temperature and humidity data will be done by InfluxDB. Therefore, you will now install InfluxDB.

## Gaining access to the InfluxDB repository

InfluxDB is not available from the default software repositories. Therefore, an additional repository must be added.

The package manager needs a key to get access to the repository where InfluxDB is available. The following command adds this key. (On Windows, copy it to the clipboard and paste it in the PuTTY window by pressing the right mouse button.)

`curl https://repos.influxdata.com/influxdata-archive.key | gpg --dearmor | sudo tee /usr/share/keyrings/influxdb-archive-keyring.gpg >/dev/null`

Add the InfluxDB to the sources the package manager will use. Again, make sure to type everything _exactly_ as shown below.

`echo "deb [signed-by=/usr/share/keyrings/influxdata-archive-keyring.gpg] https://repos.influxdata.com/debian stable main" | sudo tee /etc/apt/sources.list.d/influxdb.list`

Update the package database.

`sudo apt update`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20224002.png" alt="Gaining access to the InfluxDB repository" width="400"/>



## Installing InfluxDB

Now that you have access to the InfluxDB repository, you can install InfluxDB with the following command.

`sudo apt-get install influxdb2`


[to be continued here...]

At this point, you might want to shut down your RPi, remove the SDCard and create a backup image of the SDCard. This will allow you go back to this point by flashing this image to SDCard if anything goes wrong later on.


## Links

- 2023-12-29: [Installing InfluxDB to the Raspberry Pi](https://pimylifeup.com/raspberry-pi-influxdb/)
- undated: [influxdata download page](https://www.influxdata.com/downloads/)
- undated: [influxdata installation instructions](https://docs.influxdata.com/influxdb/v2/install/) (outdated for Raspberry Pi OS bookworm)
