# RPi-based temperature and humidity logger for Shelly HT (Part 4: Node-RED installation)

Node-RED will receive the temperature and humidity data from the Shellies and pass it on to InfluxDB, where it is stored. This post describes the installation and setup of Node-RED for this purpose.

## Installation of Node-RED

The installation of Node-RED is performed by running a script. The following command downloads the installation script (`curl` command) and passes it directly to a `bash` shell to execute it. It is always good practice to mistrust any script before running it. You can check the [script](https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered) before executing it by opening it in your browser. Note that you'll have to answer multiple questions during the installation. There is a time limit for responding to these questions (even though it is not visible). If no answer is provided within the time limit, the default option will be chosen automatically.

If you want to follow the installation process, open a second SSH session and view the installation log file in realtime by issuing the command `tail -f /var/log/nodered-install.log`. This command only works once the installation has started in the original SSH session.

Execute the installation script by entering the command below on the command line of the original SSH session.

`bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)`

Choose the following answers to the questions:
- Are you really sure to do this? Yes
- Would you like to install Pi specific nodes? Yes
- _[Now the installation will run on its own for a while. Follow the process in the second SSH session.]_
- Settings file: Just press Enter to accept the default `/home/pi/.node-red/settings.js`
- Do you want to setup user security: No (use cursor keys and press Enter)
- Do you want to enable the Projects feature: No
- Enter a name for your flows file: `flows.json` (accept the default by pressing Enter)
- Provide a passphrase to encrypt your credentials file: _[enter a passphrase of your choice and store it in a safe place]_
- Select a theme for the editor: default
- Select the text editor component to use in the Node-RED Editor: monaco (default)
- Allow Function nodes to load external modules: Yes

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-15%20224007.png" alt="Installation of Node-RED (part 1)" width="400"/>

<img src="/docs/assets/img/ht_logger/Screenshot%202025-03-15%20224038.png" alt="Installation of Node-RED (part 2)" width="400"/>

After completing the installation, you will see the message `Settings file written to /home/pi/.node-red/settings.js`. If you want to revise any of the choices you took just now, you can edit this file to adjust it.


## Finalizing the installation on the command line

### Require password for commands run as root using sudo

The installation script displays instructions requesting you to increase the security of your system by prompting for a password when performing commands as root using sudo. The command below will implement this by disabling the file allowing the execution of sudo without entering a password.

`sudo mv /etc/sudoers.d/010_pi-nopasswd /etc/sudoers.d/010_pi-nopasswd~`

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-16%20223421.png" alt="Enable prompt for password when executing sudo" width="400"/>

### Activate the automatic start of Node-RED

Check the status of the Node-RED service. Note that you will now be asked to enter your password because you are using the `sudo command`. This shows that the previous step was successful.

`sudo systemctl status nodered.service`

You should see the status of nodered.service. Its state is "disabled" and "inactive (dead)". Leave the status display by pressing "q".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-16%20224210.png" alt="Node-RED initially in disabled and inactive state." width="400"/>

Enable the automatic start of the Node-RED service. (The command also works without the trailing ".service".) As you have previously entered your password for the sudo command, the password prompt does not show again this time.

`sudo systemctl enable nodered.service`

Check again the status of the Node-RED service.

`sudo systemctl status nodered.service`

The state has changed to "enabled" and "inactive (dead)".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-16%20224806.png" alt="Node-RED in enabled but inactive state." width="400"/>

Now you can start the service.

`sudo systemctl start nodered`

Then, check again for the status of the service.

`sudo systemctl status nodered`

The status is now "enabled" and "active (running)". Leave the status display again by pressing "q".

<img src="/docs/assets/img/ht_logger/Screenshot%202025-04-16%20225227.png" alt="Node-RED in enabled and active state." width="400"/>


_[To be continued here.]_


## What's next?

The only thing missing now are the Shelly sensors. Adjust their settings such that they provide their data to your RPi!


## Links

- undated: [Node-RED](https://nodered.org/)

