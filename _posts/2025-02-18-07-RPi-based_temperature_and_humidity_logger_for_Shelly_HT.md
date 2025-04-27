# RPi-based temperature and humidity logger for Shelly HT (Part 7: Collecting humidity data and more)

In the previous part, you verfied that the collection of the temperature data is working. Now you will extend your Node-RED flow to collect additional data: the relative humidity and the battery status of the sensor. This data will also be stored permanently in InfluxDB.


## Collecting humidity data in Node-RED

Go to the web interface of Node-RED (`192.168.178.28:1880`). To create the nodes for the humidity data collection, you can just copy and paste the existing nodes and then edit the copied nodes.

Select all three existing nodes by dragging a rectangle around them with the mouse. Then press Ctrl-C to copy and Ctrl-V to paste. The placement of the pasted nodes is not yet final, they are following your mouse. Move the mouse until they are positioned below the original nodes, with some space in between for adding more nodes later. Then, press the left mouse button to complete pasting.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20144928.png" alt="Selecting the existing nodes" width="600"/>

Double-click on the newly pasted copy of the "Temp_EG" node to display its properties. In the "Topic" field, replace "temperature" by "humidity". It should say "shellies/altbau/eg/ht8554/status/humidity:0". Change the "Name" field to "rh_EG" (rh standing for relative humidity). All other settings of this node can remain unchanged. Click on the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20145926.png" alt="Modifying the mqtt in node" width="600"/>

Double-click on the copy of the "get °C temperature" node. Change the "Name" field to "get rel. humidity". In the rules, replace "tC" by "rh". The rule should now say "Set msg.payload to the value msg.payload.rh". Click the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20150218.png" alt="Modifying the change node" width="600"/>

Double-click on the copy of the "InfluxDB on ..." node. Change the "Measurement" field from "tC_eg" to "rh_eg". Click the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20150351.png" alt="Modifying the influxdb out node" width="600"/>

Your flow in Node-RED should now have six nodes as shown in the screenshot below. To activate the modified flow, click on "Deploy". The "rh_EG" should then also show "Connected".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20150415.png" alt="Flow with original nodes and modified copies of the original nodes" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20150640.png" alt="Flow deployed after modifications" width="800"/>

You just extended your flow to collect the humidity data, too. Wait some time, then check if you can also see the humidity data in InfluxDB's Data Explorer.


## Collecting battery status data

Similarly, you can now add nodes that collect the battery status of the sensor and store this data in InfluxDB, too.

Copy and paste the three nodes related to humidity. Edit the properties of all three of them.

In the copy of the "rh_EG" node, change "humidity" to "devicepower" in the "Topic" field. Change the "Name" field from "rh_EG" to "power_EG".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20155703.png" alt="Receiving power status data from the sensor via MQTT" width="600"/>

In the copy of the "get rel. humidity" node, change the "Name" field from "get rel. humidity" to "get battery %". In the rules, replace "rh" by "battery.percent". The rule should say "Set msg.payload to the value msg.payload.battery.percent".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20155939.png" alt="Extracting the battery level in percent" width="600"/>

In the copy of the "InfluxDB on ..." node, change the "Measurement" field from "rh_eg" to "batt_perc_eg".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20160049.png" alt="Transmitting battery level to InfluxDB" width="600"/>

Your flow should now look as shown in the screenshot below.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20160301.png" alt="Flow with battery level in percent added" width="600"/>

In addition to the battery status in percent, we also want to collect the battery voltage data. To add this, you only need to copy-paste the second and the third node.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20160357.png" alt="Nodes copied to record also the battery voltage" width="600"/>

In the copy of the "get battery %" node, change the "Name" field from "get battery %" to "get battery V". In the rules, replace "battery.percent" by "battery.V". The rule should be "Set msg.payload to the value msg.payload.battery.V".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20160810.png" alt="Extracting the battery voltage" width="600"/>

In the copy of the "InfluxDB on ..." node, change the "Measurement" field from "batt_perc_eg" to "batt_volt_eg".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20160926.png" alt="Transmitting the battery voltage to InfluxDB" width="600"/>

Finally, add a connection wire from the "power_EG" node to the "get battery V" node.

Deploy the updated flow to implement the changes. The "power_EG" also gets the "Connected" note below its node. The additional data should appear in InfluxDB's Data Explorer once the sensor has transmitted new data (which will take some time).

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20161223.png" alt="Deployed flow including battery power nodes" width="800"/>


## Adding calculated data: Absolute humidity and dew point (optional)

Based on the knowledge of the temperature and the humidity, it is possible to calculate more data. At this point, it starts to pay that you have a powerful setup that allows to process data. So far, Node-RED only takes data from the MQTT broker and passes this same data into InfluxDB. In the next step, you will add more nodes that perform calculations with the received data before passing the calculated data to InfluxDB for storage.

Note: As I don't have too much experience with Node-RED (I only started using it with this project), the solution shown here might not be the most simple one. After all, it works, which was the most important goal for me.

If you are not interested in this data, you can simply skip this step of the tutorial.


### Making temperature and relative humidity available for subsequent calculations

First, drag a new "function" node from the palette to your flow. Place it below the "InfluxDB on ... tC_eg" node. Double-click on the "function 1" node to edit its properties. Change the "Name" field from "function 1" to "store tC_eg".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20165223.png" alt="Function node added" width="600"/>

In the "Function" tab of the properties, add the following code.
```
var tC_eg = null;
flow.set('tC_eg', msg.payload);
return msg;
```
This allows to access "tC_eg" later in other functions. For everything else, the default values are fine. Click on the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20165638.png" alt="Function tC_eg" width="600"/>

Similarly, add another "function" node for the relative humidity. Change the node's name to "store rh_eg" and add the code below in the "Function" tab.
```
var rh_eg = null;
flow.set('rh_eg', msg.payload);
return msg;
```
Finish editing by clicking the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20170218.png" alt="Another function node added" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20170451.png" alt="Function rh_eg" width="600"/>

Connect the newly added "store ..." nodes to the corresponding "get ..." nodes by adding two wires.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20171336.png" alt="Functions tC_eg and rh_eg added" width="600"/>


### Adding a "join" node

Next, you add a "join" node, which can be found in the "Sequence" section of the palette. Drag it from the palette to the right of the "store tC_eg" node. Connect the output of the "store tC_eg" node to the input of the "join" node. Then, connect the output of the "store rh_eg" node to the "join" node, too.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20171400.png" alt="Join node added" width="600"/>

Double-click the "join" node to edit its properties. Set the node name to "join tC and rh". Change the "Mode" to "Manual". Change the details of the manual mode to "Join each msg.payload and create an array". Check "Use existing msg.parts property". Under "Send the message", set "after a number of message parts" to "2". Confirm these settings by clicking the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20172118.png" alt="Editing properties of join node" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20172221.png" alt="Join node edited and connected" width="600"/>


### Calculating the absolute humidity

Drag a "function" node from the palette into your flow, and place it behind the "join" node you just created. Edit the properties of the new function node by double-clicking on it. Change the name of the node to "rh_T_to_ah" (relative humidity and temperature to absolute humidity). In the "Function" tab, enter the code below.
```
var t = parseFloat(flow.get('tC_eg'));
var R = parseFloat(flow.get('rh_eg'));
var ah_eg = null;

msg.payload = 6.11 * R * Math.exp(17.6 * t / (243 + t)) / (0.4615 * (273 + t));
flow.set('ah_eg', msg.payload);

return msg;
```
Confirm the changes by clicking on the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20172554.png" alt="Function node added" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20202927.png" alt="Calculation of absolute humidity added" width="600"/>

Connect the newly added function node to the join node by adding the corresponding wire between the two.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20203100.png" alt="Connection added" width="600"/>


### Storing the absolute humidity in InfluxDB

To store this data in InfluxDB, copy-paste and edit one of the "InfluxDB on..." nodes. Edit its properties and set the "Measurement" field to "ah_eg". Click on the "Done" button and add the missing connection to the "rh_T_to_ah" node.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20203452.png" alt="Storing the ah_eg data in InfluxDB" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20203559.png" alt="InfluxDB out node connected" width="600"/>

Deploy the changes to start recording the absolute humidity in InfluxDB. The blue dots on the newly created nodes should disappear.


### Calculating the dew point

The temperature and humidity data can also be used to calculate the dew point. If the temperature drops to this value, the humidity in the air will start condensating, leading to the formation of dew. The dew point is also important for our body temperature: If the ambient air is very warm and at the same time very humid, the dew point may increase to a value above our body temperature. If this happens, our body is no longer able to control its temperature by sweating because the sweat will no longer evaporate and the cooling effect is gone. Therefore, being in an environment where the dew point is higher than the body temperature (37°C) can be dangerous. Luckily, this is normally not the case in the populated places on earth.

Similar to the absolute humidity, the dew point temperature can also be calculated from the relative humidity and the temperature. The calculation is a bit more complicated because it depends on the saturation vapor pressure, which differs for water and for ice. Accordingly, the function is divided into two cases.

Drag another function node from the palette to your flow and place it just below the "rh_T_to_ah" function. Double-click on the function to edit its properties. Name it "rh_T_to_dewpC". The "C" at the end indicates that the dew point calculated by the function is expressed in degrees Centigrade. In the "Function" tab, enter the code given below.
```
var t = parseFloat(flow.get('tC_eg'));
var R = parseFloat(flow.get('rh_eg'));
var dewpC_eg = null;

if (t>=0) {
    // using the Magnus formula for the saturation vapor pressure above water
    // (valid for -45°C...+60°C)
    // var K1 = 6.112 // hPa (not used)
    var K2 = 17.62
    var K3 = 243.12 // °C
} else {
    // using the Magnus formula for the saturation vapor pressure above ice
    // valid for -65°C...0.01°C)
    // var K1 = 6.112 // hPa (not used)
    var K2 = 22.46
    var K3 = 272.62 // °C
}
msg.payload = K3 * (K2 * t / (K3 + t) + Math.log(R/100)) / (K2 * K3 / (K3 + t) - Math.log(R/100));

flow.set('dewpC_eg', msg.payload);

return msg;
```
For all other properties, the default settings are ok. Press the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20211037.png" alt="Dew point calculation" width="600"/>

To store the calculated dew point in InfluxDB, copy-paste the corresponding node and edit its properties. Change the "Measurement" field to "dewpC_eg" and press the "Done" button.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20211352.png" alt="Storing the dew point in InfluxDB" width="600"/>

Finally, add the missing wire connections between the "join" node and "rh_T_to_dewpC" and on to the "InfluxDB on ..." node.

If everything is set up correctly, click on "Deploy" to activate the modified flow.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20211555.png" alt="Dew point calculation connected and deployed" width="600"/>


## Adjust the name of the flow

Now, the flow contains all nodes that you need for this project. As the nodes in this flow are related to the sensor on the ground floor, you might want to change the name of the flow accordingly. To change the name of the flow, click on the hamburger menu and select "Flow / Edit". The properties of the flow appear. Here, you can change the name of the flow to "EG". If you like, you can also add a description. To actiate the change, you have to click on "Deploy" again.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20212314.png" alt="Editing flow properties" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20212425.png" alt="Changing flow name" width="600"/>


## Save the flow

The flow is now saved and active on your RPi. However, you might want to save a copy of it and store it at another location, e. g. on your computer. It is always advisable to have a backup copy somewhere else.

Click on the hamburger menu and select "Export". The "Export" dialog box appears. Leave all settings at their default values and click on the "Download" button. This will create a file "flow.json" in the downloads folder of your computer. Rename it to "flow_EG.json". Store it in a safe place.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20213250.png" alt="Exporting a flow" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20213532.png" alt="Flow selected" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20213553.png" alt="Flow stored in downloads folder" width="600"/>

Note: For safety reasons, **the exported flow includes neither the credentials for the MQTT broker (Mosquitto) nor the credentials for InfluxDB**. If you'll ever have to rebuild your flow from scratch in Node-RED, importing the saved flow will not be sufficient. You'll also need the login information for these two services. Therefore, it was mentioned in the previous parts of this tutorial to **store this information in a safe place, too**. Node-RED stores this information in the "global configuration nodes", which are separate from the flows. You can see them in the right pane in the web interface of Node-RED.

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20214451.png" alt="Global configuration nodes not included in export of flow" width="600"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-27%20215154.png" alt="Exported JSON file containing the flow" width="600"/>


## Adding more sensors

If you have multiple Shelly H&T sensors, you can now create a separate flow for each one of them. To do so, just import the flow you just saved and edit it. In my case, I have a sensor also on the first floor, for example. Instead of "eg" ("Erdgeschoss"), I'm using "og" ("Obergeschoss") for this sensor.

Already at the configuration of the sensors, you'll have to use individual settings. Below is a table showing the MQTT configuration of my sensors. If you also have multiple sensors, you might want to create a similar table for your case.
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

For each sensor, add a flow by importing and editing the nodes. Change **all** occurrences of "_eg" by whatever you are using, according to **your** tables. I would recommend to perform these changes by copying he JSON file of your flow (copy flow_EG.json to flow_OG.json). Then, perform the changes using the search and replace functionality of a text editor. This ensures that you don't forget any change. You'll have to do three case-sensitive replacements: "_eg" to "_og" (20 replacements), "_EG" to "_OG" (3 replacements) and "EG" to "OG" (1 replacement). The last replacement only affects the name of the flow, therefore there is only one occurrence.

You can then import the modified JSON file into Node-RED and all modifications are already done.

To import the modified flow, open the hamburger menu and select "Import". Click on "Select file for import", navigate to the location of the modified JSON file and select "Flow_OG.json". The import dialog will show the contents of the JSON file. Check if it is the right one. Below the file preview, change the setting to "Import to a new flow". Then click the "Import" button.

A message box will pop up, saying that some of the nodes are already present in the workspace. The nodes already present are the two "global configuration nodes" containing the login data for Mosquitto and InfluxDB. Click on the "Show Nodes..." button. In the window that pops up, the global configuration nodes are already deselected from the import. This is ok, thus you can click on the "Import selection" button. You will see a notification saying "Imported: 1 flow, 18 nodes". There are now two tabs, one labelled "EG" and another one labelled "OG". Select the "OG" tab. You can see that this flow is not yet deployed. There are blue markers everywhere. Therefore, click on "Deploy" to activate the flow. This should remove all blue dots and add the "Connected" labels underneath the "MQTT in" nodes.

Repeat this for all sensors you have.

The screenshot below shows the final view in Node-RED with all flows deployed.


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

- [Magnus formula](https://de.wikipedia.org/wiki/Taupunkt) in the Wikipedia article on the dew point
