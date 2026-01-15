# Monitoring a Test MQTT Topic in Node-RED

This document provides the necessary JSON and instructions to create a Node-RED flow that monitors button presses on a specific MQTT topic.

## Objective
The goal is to create a simple flow that subscribes to an MQTT topic and displays any incoming messages in the Node-RED debug panel. This verifies that messages sent from a device (like a test button) are being correctly received by the MQTT broker and processed by Node-RED.

## Node-RED Flow JSON
Below is the JSON representation of the flow. Copy this code and import it directly into your Node-RED editor.

```json
[
    {
        "id": "c7a8a8b3.a8e4a8",
        "type": "mqtt in",
        "z": "f8f8f8f8.f8f8f8",
        "name": "Listen for Test Button",
        "topic": "home/button/test/state",
        "qos": "2",
        "datatype": "auto",
        "broker": "YOUR_MQTT_BROKER_ID",
        "x": 200,
        "y": 200,
        "wires": [
            [
                "a2b1b1b1.b2a2a2"
            ]
        ]
    },
    {
        "id": "a2b1b1b1.b2a2a2",
        "type": "debug",
        "z": "f8f8f8f8.f8f8f8",
        "name": "Display Button Press",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "x": 450,
        "y": 200,
        "wires": []
    },
    {
        "id": "YOUR_MQTT_BROKER_ID",
        "type": "mqtt-broker",
        "name": "My Home MQTT Broker",
        "broker": "localhost",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willPayload": ""
    }
]
```

## Setup and Testing Instructions

### 1. Configure the MQTT Broker
- In the JSON above, the `mqtt in` node is linked to an `mqtt-broker` configuration node.
- **Before deploying**, double-click the "Listen for Test Button" node.
- Click the pencil icon next to the "Broker" field.
- **Update the "Server" and "Port" fields** to match the address (e.g., `192.168.1.100` or `mqtt.local`) and port of your MQTT broker.
- Click "Update" and then "Done".

### 2. Import and Deploy the Flow
1.  **Copy** the entire JSON code block from this document.
2.  Open your Node-RED editor.
3.  Click the **main menu** (three horizontal lines) in the top-right corner.
4.  Select **Import**.
5.  **Paste** the JSON code into the import dialog.
6.  Click the **Import** button. The nodes will appear in your workspace.
7.  Click the **Deploy** button in the top-right corner to activate the flow.

### 3. Test the Flow
1.  Open a terminal on a machine that has MQTT tools installed (like `mosquitto-clients`).
2.  Publish a test message to the topic using the following command. Replace `localhost` with your MQTT broker's address if needed.

    ```bash
    mosquitto_pub -h localhost -t "home/button/test/state" -m "ON"
    ```

3.  In your Node-RED editor, open the **Debug panel** (the bug icon on the right-hand sidebar).
4.  You should see the message "ON" appear, confirming the flow is working correctly.

This completes the setup for monitoring a test MQTT topic.
