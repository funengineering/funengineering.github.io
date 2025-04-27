# RPi-based temperature and humidity logger for Shelly HT (Part 7: Collecting humidity data and more)

In the previous part, you verfied that the collection of the temperature data is working. Now you will extend your Node-RED flow to collect additional data: the relative humidity and the battery status of the sensor. This data will also be stored permanently in InfluxDB.


### Collecting humidity data in Node-RED

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




## Save the flow

[Save the flow and store it somewhere outside of your RPi.]


## What's next?

Now that you have a growing collection of temperature and humidity data, you might want to visualize it nicely. Take a look at the next step of the tutorial to see how this is done.


## Links

No links for this part of the tutorial.
