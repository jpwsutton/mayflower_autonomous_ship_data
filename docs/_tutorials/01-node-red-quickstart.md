---
title: "Node-RED Quickstart"
permalink: /tutorials/node-red-quickstart/
excerpt: "How to quickly get your own MAS400 map set up with Node-RED"
last_modified_at: 2022-03-30T20:48:05-04:00
toc: true
---

In this Tutorial, you'll see how you can set up your own map to monitor the location, and status of the MAS in your own custom dashboard built on [Node-RED](https://nodered.org/).

## Step 1. Install Node-RED

Install and start Node-RED by following the [Getting Started](https://nodered.org/docs/getting-started/) instructions. You can install Node-RED on your personal computer, Raspberry Pi or even in the Cloud.

## Step 2. Install Dashboard and WorldMap Nodes

Once you've got Node-RED up and running, open it in your browser. If its running on your local machine, you will be able to find it on [http://localhost:1880](http://localhost:1880).

Next we need to install some extra Nodes, these will add extra functionality to Node-RED, in this case a Dashboard and a Map view.

1. Open the Main Menu by clicking on the hamburger in the top right corner.
2. Select 'Manage palette'
3. In the panel that opens, select the 'Install' tab.
4. In the search modules box, search for `node-red-contrib-web-worldmap` and click install.
5. Then, search for `node-red-dashboard` and click install.

You should then see an alert pop up in the top middle of the screen showing that the nodes have been installed, they will then be shown in the Nodes tab. You can now click Close.

<figure>
  <img src="{{ '/assets/images/tut-01-node-red-node-install.png' | relative_url }}" alt="The Node-RED Node installer window">
</figure>

Double check that they are now available in the node palette on the left side of the canvas:
<figure>
  <img src="{{ '/assets/images/tut-01-node-red-nodes.png' | relative_url }}" alt="The Node-RED node palette">
</figure>


**Note:** Because of an issue with the Dashboard and worldmap nodes, you may not see the dashboard worldmap node (shown as the last node in the figure above). If this happens, simply restart Node-RED and refresh the page in your browser, it should then appear.
{: .notice--warning}


## Step 3. Start building your flow

To start off, lets first get our MQTT Subscription node and a debug node hooked up so that we can test that our connection is working.

1. Scroll through the nodes on the palette on the left hand side, looking the the `mqtt in` node (It's in the network category). Then drag it onto the canvas.
2. Double Click on the node that's now on your canvas, a panel should open up on the right side.
3. Click the pencil icon next to the server select box to add a new server. In the new window, set the server as `mqtt.mas400.com` and the port as `1883`. Then click Update.
   <figure>
      <img src="{{ '/assets/images/tut-01-node-red-mqtt-server.png' | relative_url }}" alt="The MQTT Server">
   </figure>      
4. Make sure the Action is: 'Subscribe to a single topic' and set the Topic to: `status/JSON` and change the Output to 'a parsed JSON object'.
   <figure>
      <img src="{{ '/assets/images/tut-01-node-red-mqtt-sub.png' | relative_url }}" alt="The MQTT subscription">
   </figure>   
5. Click Done.
6. Drag a debug node onto the canvas.
7. Drag a wire from the mqtt in node to the debug node (from the little grey boxes on the end of the nodes). And click the Deploy button.
   <figure>
      <img src="{{ '/assets/images/tut-01-node-red-mqtt-debug.png' | relative_url }}" alt="The MQTT and Debug nodes linked">
   </figure>   

