# node-red-torque
A Node-RED flow for capturing data sent from the Torque OBDII app and creating a formatted JSON message. Example configuration provided for storing the data in an InfluxDB database via MQTT sent to Telegraf.

## Dependencies

### Required

1. Node-RED. I run via Docker using the latest official image.

2. node-red-contrib-moment. This parses the `session` field into a human readable date.

3. Torque app, either free or paid (limitations of free described below), and a suitable OBD2 reader.

### Optional

1. MQTT Broker for sending messages out.

2. Telegraf for consuming MQTT messages and writing metrics to InfluxDB.

3. InfluxDB for storing metrics.

4. Grafana for viewing data.

## Installation

### Server Setup

1. Import the contents of flow.json into a new flow in Node-RED.

2. Change the output TZ of the "Date/Time Formatter" node.

3. Ensure you can access the Node-RED server while on a mobile network. See security considerations below.

4. Set the correct parameters for your MQTT broker in the MQTT output node.

5. Add the Telegraf example to your Telegraf config, changing the parameters to connect to your MQTT broker.

### Torque App Setup

This works with both the paid and free versions of Torque. The free version is limited to 6 PIDs reporting simultaneously, and has no good way to identify separate vehicles. For either version:

1. Navigate to the app settings and enable "Upload to webserver".

2. Enter the address of your Node-RED instance in the webserver URL field. Default is `http://<ip address>:<port>/upload`. This will change due to port forwarding, reverse proxy settings, domain names, dynamic DNS, etc.

3. Select which PIDs to log.

4. If using the paid version of Torque, enter a descriptive name for your vehicle in the "User Email Address" field. This information is parsed and used as the metric name in the Telegraf/InfluxDB example. It is the only way I have found to differentiate multiple vehicles, and must be changed manually when switching vehicles and keeping the same android device. The free version of Torque does not send this field, so all data received is stored under the same metric: `liteVersion`.

## Security Considerations

The Torque app does not support sending data over HTTPS, so HTTPS access to your Node-RED server, either directly or via reverse proxy will not work. The data sent by Torque potentially contains sensitive data, such as GPS coordinates. This is low risk when sending data across a private network, but in order to upload real-time data to your server while not on the same network (i.e. driving in your car), you must either:

* Expose your Node-RED HTTP endpoint to the internet, using port forwarding/reverse proxy (Not recommended). 

* Use a VPN to create a secure connection to the network that has your server. I use [Wireguard](https://www.wireguard.com/) for this, but that set up is outside the scope of this documentation.
