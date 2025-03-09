# RPi-based temperature and humidity logger for Shelly HT (Part 3: InfluxDB OSS v2 installation)

Collecting and storing the temperature and humidity data will be done by InfluxDB. Therefore, you will now install InfluxDB.

## Gaining access to the InfluxDB repository

Before installing InfluxDB, you need a key that will grant you access the InfluxDB repository. Download this key with the following command.

`wget -q https://repos.influxdata.com/influxdata-archive_compat.key`

Then, check the fingerprint of the key.

`gpg --with-fingerprint --show-keys ./influxdata-archive_compat.key`

The fingerprint should be `9D53 9D90 D332 8DC7 D6C8  D3B9 D8FF 8E1F 7DF8 B07E`.

Install the key for the InfluxDB repository. Make sure your type the command _exactly_ as shown below.

`echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null`

Update your APT sources to use the new key. Again, make sure to type everything _exactly_ as shown below.

`echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list`

Check if it worked.

`more /etc/apt/sources.list.d/influxdata.list`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20215121" alt="Gaining access to the InfluxDB repository" width="400"/>

Check if you have an old key that should be removed.

`ls -al /etc/apt/trusted.gpg.d`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-09%20220029" alt="Checking for non-existence of file influxdbd.gpg" width="400"/>

If you see a file called `influxdata-archive_compat.gpg`, this is ok. If you see any file called `influxdb.gpg`, you should remove it (`sudo rm -f /etc/apt/trusted.gpg.d/influxdb.gpg`). Normally, `influxdb.gpg` should not exist and deleting it should not be necessary.

Update the package database.

`sudo apt-get update`


## Installing InfluxDB

Now that you have access to the InfluxDB repository, you can install InfluxDB with the following command.

`sudo apt-get install influxdb2`


[to be continued here...]

At this point, you might want to shut down your RPi, remove the SDCard and create a backup image of the SDCard. This will allow you go back to this point by flashing this image to SDCard if anything goes wrong later on.
