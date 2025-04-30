# RPi-based temperature and humidity logger for Shelly HT (Part 8: Using multiple sensors)

If you have multiple temperature and humidity sensors, it's not hard to integrate them in your setup. It's even easier than setting everything up for the first sensor: Instead of redoing things, you can copy and modify, which is faster. Below is a description of how it can be done.

If you have just a single sensor: No problem! It will work fine and you can just skip this part of the tutorial, which is optional.


### Setting up additional sensors

Before you can process the sensor's data in Node-RED, your sensor needs appropriate settings. This topic was covered in [part 5 of this tutorial](/2025/02/18/05-RPi-based_temperature_and_humidity_logger_for_Shelly_HT.html). You can follow the same procedure for every additional sensor. However, each one of them needs some individual settings. Below is a table showing the MQTT configuration of my sensors. You might want to create a similar table for your case.

| Location     | MQTT prefix                   | Client ID                     |
| ------------ | ----------------------------- | ----------------------------- |
| Ground floor | shellies/altbau/eg/ht8554     | shellies/altbau/eg/ht8554     |
| First floor  | shellies/altbau/og/ht2DA4     | shellies/altbau/og/ht2DA4     |
| Basement     | shellies/altbau/ug/ht2D6C     | shellies/altbau/ug/ht2D6C     |
| Cellar       | shellies/altbau/wk/ht2FF8     | shellies/altbau/wk/ht2FF8     |
| Annex        | shellies/neubau/ug/ht46AC     | shellies/neubau/ug/ht46AC     |
| Outside      | shellies/altbau/aussen/ht1FB0 | shellies/altbau/aussen/ht1FB0 |

The names of the measurements in InfluxDB are summarized in the next table.

|                 |      | Ground floor | 1st floor    | Basement     | Cellar       | Annex            | Outside          |
| --------------- | ---- | ------------ | ------------ | ------------ | ------------ | ---------------- | ---------------- |
| Temperature     | °C   | tC_eg        | tC_og        | tC_ug        | tC_wk        | tC_neubau        | tC_aussen        |
| Rel. humidity   | %    | rh_eg        | rh_og        | rh_ug        | rh_wk        | rh_neubau        | rh_aussen        |
| Abs. humidity   | g/m3 | ah_eg        | ah_og        | ah_ug        | ah_wk        | ah_neubau        | ah_aussen        |
| Dew point       | °C   | dewpC_eg     | dewpC_og     | dewpC_ug     | dewpC_wk     | dewpC_neubau     | dewpC_aussen     |
| Battery level   | %    | batt_perc_eg | batt_perc_og | batt_perc_ug | batt_perc_wk | batt_perc_neubau | batt_perc_aussen |
| Battery voltage | V    | batt_volt_eg | batt_volt_og | batt_volt_ug | batt_volt_wk | batt_volt_neubau | batt_volt_aussen |

As you can see, the naming follows a systematic scheme. This makes it easier to avoid mistakes.

Note: The Shelly H&T sensors are **not** designed to be used outside. In my case, the one that measures the "outside" conditions is located outside under the roof, where it measures the outside temperature and humidity but is at the same time sheltered from the adverse outside conditions. Also, it is still close enough to the Wi-Fi network to have a reliable connection.


## Adding a separate flow for each additional sensor

With multiple sensors, it is reasonable to create a separate flow for each one of them. To do so, you could import the flow you just saved and edit it in Node-RED. In my case, I have a sensor also on the first floor. Instead of "eg" ("Erdgeschoss"), I'm using "og" ("Obergeschoss") for this sensor. However, manually editing the properties of all nodes and performing the changes manually is tedious and error prone. If you miss one replacement, your flow will probably not work as desired. Therefore, it is better to do these replacements automatically.

I recommend to perform these changes by copying he JSON file of your flow (copy flow_EG.json to flow_OG.json). Then, perform the changes using the search and replace functionality of a text editor. This ensures that you don't forget any change. You'll have to do four case-sensitive replacements:
- "_eg" to "_og" (20 replacements)
- "_EG" to "_OG" (3 replacements)
- "EG" to "OG" (1 replacement) and
- "altbau/eg/ht8554" to "altbau/og/ht2DA4" (3 replacements).

The third replacement only affects the name of the flow, therefore there is only one occurrence. The replacements above are valid for my setup. For **your** setup, change all occurrences of "eg" by whatever you are using, according to **your** tables.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20224034.png" alt="Search and replace _eg" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20224108.png" alt="Search and replace _EG" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20224447.png" alt="Search and replace EG" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-30%20163910.png" alt="Search and replace ht8554" width="600"/>

You can then import the modified JSON file into Node-RED. Like this, there is no more need to perform modifications in Node-RED because they are all done already.

To import the modified flow, open the hamburger menu and select "Import". Click on "Select file for import", navigate to the location of the modified JSON file and select "Flow_OG.json". The import dialog will show the contents of the JSON file. Check if it is the right one. Below the file preview, change the setting to "Import to a new flow". Then click the "Import" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225239.png" alt="Import a flow" width="300"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225250.png" alt="Import from file" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225356.png" alt="Select JSON file to import" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225441.png" alt="Preview shown and import to new flow" width="400"/>

A message box will pop up, saying that some of the nodes are already present in the workspace. The nodes already present are the two "global configuration nodes" containing the login data for Mosquitto and InfluxDB. Click on the "Show Nodes..." button. In the window that pops up, the global configuration nodes are already deselected from the import. This is ok, thus you can click on the "Import selection" button. You will see a notification saying "Imported: 1 flow, 18 nodes". There are now two tabs, one labelled "EG" and another one labelled "OG". Select the "OG" tab. You can see that this flow is not yet deployed. There are blue markers everywhere. Therefore, click on "Deploy" to activate the flow. This should remove all blue dots and add the "Connected" labels underneath the "MQTT in" nodes.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225525.png" alt="Importing duplicate nodes" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225556.png" alt="Resolution of import conflict" width="300"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225611.png" alt="Flow successfully imported" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225627.png" alt="Switching to tab of newly imported flow" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225637.png" alt="Newly imported flow successfully deployed" width="600"/>

Repeat this for all sensors you have.

The screenshot below shows the final view in Node-RED with all flows deployed.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20225845.png" alt="All imported flows successfully deployed" width="600"/>


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

No links for this part of the tutorial.
