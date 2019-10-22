# node-red-torque
A Node-RED flow for capturing data sent from the Torque OBDII app and creating a formatted JSON message. Example configuration provided for storing the data in an InfluxDB database via MQTT sent to telegraf.

## Required Dependencies

1. Node-RED. I run via Docker using the latest official image.

2. node-red-contrib-moment. This parses the `session` field into a human readable date.

## Optional Dependencies

1. MQTT Broker for sending messages out.

2. Telegraf for consuming MQTT messages and writing metrics to InfluxDB.

3. InfluxDB for storing metrics.

4. Grafana for viewing data.

## Installation

1. Import the contents of flow.json into a new flow in Node-RED.

2. Change the output TZ of the "Date/Time Formatter" node.

3. Set the correct parameters for your MQTT broker in the MQTT output node.

4. Add the telegraf example to your telegraf config, changing the parameters to connect to your MQTT broker.

## Torque App Setup

## Security Considerations
