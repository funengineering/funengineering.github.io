# RPi-based temperature and humidity logger for Shelly HT (Part 3: InfluxDB OSS v2 installation)

Collecting and storing the temperature and humidity data will be done by InfluxDB. Therefore, you will now install InfluxDB.

## Gaining access to the InfluxDB repository

InfluxDB is not stored in the standard software repositories. Therefore, an additional repository must be added. The commands in this section are taken from the InfluxDB documentation. If they don't work as expected, check the [InfluxDB documentation](https://docs.influxdata.com/influxdb/v2/install/?t=Linux). It is possible that the supplier influxdata makes changes to this process.

Before installing InfluxDB, you need a key that will grant you access the InfluxDB repository. Download this key with the following command.

`wget -q https://repos.influxdata.com/influxdata-archive.key`

Then, check the fingerprint of the key.

`gpg --with-fingerprint --show-keys ./influxdata-archive.key`

The fingerprint should be `24C9 75CB A61A 024E E1B6  3178 7C3D 5715 9FC2 F927`.

Install the key for the InfluxDB repository. Make sure your type the command _exactly_ as shown below.

`echo '943666881a1b8d9b849b74caebf02d3465d6beb716510d86a39f6c8e8dac7515 influxdata-archive.key' | sha256sum --check - && cat influxdata-archive.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive.gpg > /dev/null`

Update your APT sources to use the new key. Again, make sure to type everything _exactly_ as shown below.

`echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list`

Check if it worked.

`cat /etc/apt/sources.list.d/influxdata.list`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20224002.png" alt="Gaining access to the InfluxDB repository" width="400"/>

Check if you have an old key that should be removed.

`ls -al /etc/apt/trusted.gpg.d`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20224205.png" alt="Checking for non-existence of file influxdbd.gpg" width="400"/>

If you see a file called `influxdata-archive.gpg`, this is ok. If you see any file called `influxdb.gpg`, you should remove it (`sudo rm -f /etc/apt/trusted.gpg.d/influxdb.gpg`). Normally, `influxdb.gpg` should not exist and deleting it should not be necessary.

Update the package database.

`sudo apt-get update`


## Installing InfluxDB

Now that you have access to the InfluxDB repository, you can install InfluxDB with the following command.

`sudo apt-get install influxdb2`


[to be continued here...]

At this point, you might want to shut down your RPi, remove the SDCard and create a backup image of the SDCard. This will allow you go back to this point by flashing this image to SDCard if anything goes wrong later on.
