---
title: "Node-RED Quickstart Part 2"
permalink: /tutorials/node-red-dashboard/
excerpt: "Building a MAS Mission Control Dashboard with Node-RED"
last_modified_at: 2022-04-03T20:48:05-04:00
classes: wide
author: jpwsutton
header:
  teaser: /assets/images/tut-02-final-dashboard.png
---
Following on from [Part 1](/tutorials/node-red-quickstart/), this tutorial will show you how to use the Node-RED dashboard nodes to build your own Mission control dashboard for the Mayflower Autonomous Ship. If you haven't already, follow the instructions in Part one to get your map set up and nodes installed.

By the end of this tutorial, you should have something like the picture below!

<figure>
    <img src="{{ '/assets/images/tut-02-final-dashboard.png' | relative_url }}" alt="Change Node" style="width: 800px;">
</figure>


## Step 1. Switch to the Worldmap Dashboard Node

We need to switch out the worldmap node for a worldmap dashboard node. This will add a mini version of the map into our new dashboard.

Delete the Worldmap In Node, and replace it with the worldmap in node that sits in the dashbaord section of the pallet. 

Once you've done that, you'll need to edit the node settings and set up the dashboard page and tabs. For the tab that the map sits on, change the width to 12 cells so that it's a good size.

<figure>
  <img src="{{ '/assets/images/tut-02-flow-with-dashboard-worldmap-node.png' | relative_url }}" alt="Adding the worldmap dashboard node" style="width: 800px;">
</figure>


<figure>
  <img src="{{ '/assets/images/tut-02-map-dashboard-tab.png' | relative_url }}" alt="Dashboard tab settings"  style="width: 500px;">
</figure>

Click Deploy and then go to `/ui` or press `⌘⇧d - ctrl-shift-d` and the dashboard should open up:

<figure>
  <img src="{{ '/assets/images/tut-02-dashboard-map-1.png' | relative_url }}" alt="Dashboard tab settings" style="width: 600px;">
</figure>

## Step 2. Speed, Bearing and Rudder

Lets set up some guages that show us the current speed, course and the rudder position of the MAS.

1. Add a change node onto the canvas and draw a line from the MQTT in node to the change node.
   1. Change the Name to something like `Format Data`
   2. Open the Change node properties
   3. Change the type to `payload.speedKts` 
   4. Change the type from a/z to expression
   5. Set the value to `$number(payload.shipSpeedOverGroundMS) * 1.944`  (This will convert from m/s to knots)
   
<figure>
    <img src="{{ '/assets/images/tut-02-change-node.png' | relative_url }}" alt="Change Node" style="width: 500px;">
</figure>

2. Drag four Guage nodes onto the canvas and draw three wires from the change node to each of them.
3. Set the Guage nodes up with a new Nagivagion Group on the Home Tab (12 wide as wel)
4. Modify each Guage with the following (Also, set the size to 4x4):
   1. Speed:
        - Set the Value to `{{msg.payload.speedKts | number : 1 }}`
        - Set the Unit to `kts` 
   2. Course:
        - Set the Value to `{{msg.payload.shipCourseOverGroundDeg}}`
        - Set the Units to `degrees`
        - Set the range to 0-360
        - Set the Type to Compass
   3. Heading
        - Set the Value to `{{msg.payload.shipHeadingDeg}}`
        - Set the Units to `degrees`
        - Set the range to 0-360
        - Set the Type to Compass
   4. Rudder angle:
        - Set the Value to `{{msg.payload.shipRudderAnglePercentThousands}}`
        - Set the Units to `degrees`
        - Set the Type to Compass
5. Now Click deploy and you should see the following on your dashboard:

<figure>
    <img src="{{ '/assets/images/tut-02-guages.png' | relative_url }}" alt="Change Node" style="width: 500px;">
</figure>
 
Now you might be asking, _Whats the difference between course and heading?.._
Great question! Heading is the direction that the MAS **wants** to be traveling in, whereas course is the direction that the MAS is **actually** traveling in.
When out at sea, tides, currents and the wind can all change how the MAS behaves in the water, so these two values are linked, but paint an interesting picture for how the MAS is traveling and planning its course.

## Step 3. Live Stream

Nearly there! Lets add the Video Live stream from the MAS to the dashboard.

1. Drag a template node onto the dashboard, you can leave it un-connected.
2. Go into its settings and add a new group for the live stream, 12 wide again.
3. Set the size to 12x8
4. Copy the following into the text field:

```<iframe height="100%" src="https://www.ustream.tv/embed/24088054"
     scrolling="no" allowfullscreen webkitallowfullscreen frameborder="0"
    style="border: 0 none transparent;">
</iframe>
```

5. Click Done and deploy.

<figure>
    <img src="{{ '/assets/images/tut-02-final-dashboard.png' | relative_url }}" alt="Change Node" style="width: 500px;">
</figure>


Congratulations, Now you can watch the Live stream, see the MAS on the map and see the navigational data.
For extra credit, have a play with the other dashboard nodes and see if you can display some of the other data fields like the weather or solar panel and battery performance.
You can also modify the settings for the dashboard to change the colour scheme as well as the layout. 