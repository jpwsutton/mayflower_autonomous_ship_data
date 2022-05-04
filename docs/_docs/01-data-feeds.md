---
title: "Data Feeds"
permalink: /docs/data-feeds/
excerpt: "A summary of live data feeds available from the Mayflower Autonomous Ship."
last_modified_at: 2022-03-31T08:48:05-04:00
toc: true
---

The Mayflower Autonomous Ship (MAS) has a number of live data feeds that are usually available whilst it is out at sea.

You can use the data feeds to find out all kinds of things about the ship, including:

- Live Navigational Data & AIS track
- Live Ship Status
- Live Science & Data Collection Status and readings
- Live Weather Data
- Replay Data

The live data feed is available via MQTT from `mqtt.mas400.com`, we are also looking into making historical data avaialble once the MAS has completed it's voyage from the UK to US in Spring 2022.


## Connecting using MQTT

You can connect to our public MQTT broker using both traditional MQTT and MQTT over websockets (useful if you want to build a web page that consumes the data).

The connection details are:

**MQTT**
```
Host: mqtt.mas400.com
Port: 1883
```

**MQTT Websockets**
```
Host: mqttws.mas400.com
Port: 443
```



Because our broker is public and read only, there is currently no authentication required.

## MQTT Topic Tree

As of March 2022, the current high level topic tree is as follows:


- status
- replay
- AIS
- science
  - science_pod
  - depth
  - ctd
  - flurometer
  - o2



[@andysc](https://twitter.com/andysc) is also hosting a [Hypercat](https://hypercatiot.github.io/) catalogue [here](http://hypercat.stanford-clark.com:1884/cat/MAS400). This can be a great way of programatically exploring the topics and data available.


## Status Feed

When connected via Cellular or Satellite to the internet, the MAS can send a small packet of data up on a semi-regular basis (conditions dependant) containing a summary of the ships status.

| topic | Data Type |  Notes |
|-------|-----------|--------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| date |  Timestamp | When the status update was recieved |
| shipDataTimeStamp | float | Unix Epoch timestamp for when the update was sent on the MAS |
| shipStatus | string | Ship Status: offline, standby, autonomous, remote |
| shipConnectivity | string | Whether the satelite link is active (Connected / Disconnected) | 
| shipRudderAnglePercentThousands | float | The Rudder angle in degrees, the key name is misleading | 
| shipLatitudeDeg | float | MAS Latitude in Degrees |
| shipLongitudeDeg | float | MAS Longitude in Degrees |
| shipHeadingDeg | float | MAS Heading in Degrees |
| shipSpeedOverGroundMS | float | MAS Speed over ground in m/s |
| shipCourseOverGroundDeg | float | MAS Course over Ground in Degrees | 
| shipThrottlePercentThousands | float | MAS Throttle - TODO | 
| shipPitchDeg | float  | MAS Pitch in Degrees |
| shipRollDeg | float | MAS Roll in Degrees | 
| waterDepthM | float | fathometer depth in meters | 
| batteryVoltageTenths | float | Overall battery voltage in tenths | 
| batteryCapacityPct | float | Battery charge capacity in percent | 
|solarChargerPowerWatts | float | Solar Power charging rate in Watts | 
| weatherDataTimestamp | float | Unix Epoch timestamp for when the weather update was made | 
| temperatureDegC | int | The Temperature where the MAS is | 
| windSpeedKmH | int | Windspeed where the MAS is | 
| windDirectionDeg | int | Wind Direction where the MAS is in Degrees | 
| weather | string | short weather conditions description | 
| weatherPhrase | string | longer weather conditions description | 
| sunrise | timestamp | Sunrise time where the MAS is | 
| sunset | timestamp | Sunset time where the MAS is | 
| _id | string | Database record ID for this update |


**JSON Example**
```
{
   "shipDataTimeStamp": "1648751244.0",
   "shipLatitudeDeg": "50.36032",
   "shipLongitudeDeg": "-4.11589",
   "shipPitchDeg": "0.0",
   "shipRollDeg": "0.0",
   "shipSpeedOverGroundMS": "0.004",
   "shipCourseOverGroundDeg": "265.996",
   "waterDepthM": "0.0",
   "shipHeadingDeg": "168.7925",
   "batteryCapacityPct": "94.186989",
   "batteryVoltageTenths": "539.831846",
   "shipThrottlePercentThousands": "0.0",
   "shipRudderAnglePercentThousands": "-20.0",
   "solarChargerPowerWatts": "0.0", "shipStatus": "Standby",
   "weatherDataTimestamp": "1648751251.827717",
   "date": "2022-03-31 18:27:31.827717+00:00",
   "temperatureDegC": "7", "windSpeedKmH": "26",
   "windDirectionDeg": "350", "weather": "sun",
   "weatherPhrase": "Fair",
   "sunrise": "2022-03-31 05:55:25+00:00",
   "sunset": "2022-03-31 18:46:53+00:00",
   "shipConnectivity": "Connected",
   "_id": "6245f2931317b53e48ecc133"
}
```






## AIS Feed

The MAS is equipped with an [AIS](https://en.wikipedia.org/wiki/Automatic_identification_system) transponder which allows it to transmit it's location, speed and bearing from almost anywhere in the world. This data is recieved either locally by other ships or via satelite so that it can be tracked by services online like [MarineTraffic](https://www.marinetraffic.com/en/ais/home/centerx:-4.109/centery:50.357/zoom:15). In cases where the MAS onboard satellite connection fails, then AIS is the best way to track it.

| topic | Data Type |  Notes |
|-------|-----------|--------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the AIS update was recieved |
| latitude | float | Latitude of the MAS in degrees |
| longitude | float | Longitude of the MAS in degrees |
| speed | float | Speed in Knots | 
| heading | float | Heading in degrees | 
| courseOverGround | float | The Actual course of the ship | 
| turnRate | float | Rate of turn (°/min) |

**JSON Example**

```
{
   "latitude":50.36039,
   "longitude":-4.115903,
   "speed":0,
   "heading":0,
   "courseOverGround":0,
   "turnRate":0,
   "timeStamp":"2022-03-31 18:28:12"
}
```



## Replay

Whilst the MAS is alongside the dock, the live data feed may not be particularly eventful. For those who might want to demonstrate their projects, a Replay feed is available on the `replay` topic. This has the same or similar underlying topic structure as the live topics with a few bonus topics only available once the MAS has offloaded certain datasets. Currently, the replay feed is replaying the June 2021 main voyage and should restart once complete on a loop.

For brevitys sake, the subtopics below will link to their live counterparts if they have them:

  - [status](#status-feed)
  - [AIS](#ais-feed)
  - [hazard](#hazard-feed)

### Hazard Feed
One of the experiments onboard the MAS is looking at how to optimise machine learning algorithms on custom silicon and so was running a simple object detection algorithm on the 6 camera feeds around the mast (__Note: This is not the same system used as part of the AI captain, which does not have a data feed__). Once the captured images and recorded classifications were uploaded off the MAS, these were added to the replay feed on the following topics:

| topic | Data Type |  Notes |
|-------|-----------|--------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the classification was made |
| epoch_time | Unix Epoch Timestamp | When the classification was made |
| object_name | string | The classified object name | 
| object_id | integer | The classified object ID | 
| prob | float | The classification probability |
| cam_id | integer | The camera ID (1 to 6) |
| annotation | URL | A URL to the captured image with the annotation |

**JSON Example**

```
{
   "time":"2021-06-18T02:10:44.000Z",
   "epoch_time":1623982244,
   "object_name":"traffic light",
   "object_id":10,
   "prob":0.5565,
   "cam_id":1,
   "annotation":"https://mas-science-data-publicimages-mission1-jun21.s3.eu-gb.cloud-object-storage.appdomain.cloud/hds-run_y2021m06d17_h19m50s26_cam_1_frame_02377.png"
}
```

**Note:** This object detection model was not built specifcially for the environment the MAS is in (the sea), or the cameras (Wide angle, with Infra Red at night). As such, you may see some entertaining / bizzare classifications for objects. Please be assured that this is absolutely fine!
{: .notice--warning}


## Science Feeds

These feeds are generated by a number of sensors and other science based systems on board.

### science_pod

This is a fairly low level feed of diagnostic information about the Science Pod cluster running on the MAS400. It probably won't be hugely interesting to many people, but is used to drive a few aspects of the MAS400.com dashboard.

__Topic:__ `science/science_pod`


| Topic | Data Type |  Notes |
|-------|-----------|--------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| timestamp | integer | Unix Epoch Timestamp (seconds since 1970) when the reading was sent |
| timestamp_iso |  ISO8601 timestamp | When the reading was recieved |
| environment/temperatureC | float | The temperature in degrees C within part of the science Pod. This is expected to be around the 40° mark during full operation |
| environment/humidity | float | % humidity within part of the science pod. |
| environment/pressureHPA | float | Atmospheric pressure within part of the science pod in hPa (hectopascals) | 
| ups/batteryCapacity | integer | percentage capacity of the science pod UPS |
| ups/batteryCharge | integer | percentage charge of the science pod UPS |
| ups/batteryVoltage | float | voltage of the science pod UPS batteries - usually around 10v | 
| ups/inputVoltage | float | input voltage from the MAS into the science pod - usually around 12v |
| ups/outputVoltage | float | output voltage from the UPS to science pod internal components - usually around 12v| 
| ups/batteryTemperature | float | Average temperature on the UPS board - usually around 40° when the pod is active. | 
| experiments/hydrophone | string | Shows the state of the hydrophone experiment RUNNING / OFFLINE / ERROR |
| experiments/gnssimu | string | Shows the state of the gnss / imu experiment RUNNING / OFFLINE / ERROR |
| experiments/video | string | Shows the state of the ocean wave energy experiment RUNNING / OFFLINE / ERROR |
| experiments/tds | string | Shows the state of the hazard detection experiment RUNNING / OFFLINE / ERROR |

#### Science Pod Raspberry Pi Status

This is low level remote telemetry, giving us information about how the four Raspberry Pi 4s that make up the Science Pod are operating.
For most people, this data is pretty useless, but it allows the MAS team to understand how running the experiments over long periods of time effects the hardware.

__Topic:__ `science/science_pod/system/etsMasPi<1,2,3,4>`

| Topic | Data Type |  Notes |
| freeMemory |  float | Free RAM in megabytes (higher number is better) | 
| cpuLoad | float | CPU Usage on the Pi (These Pis have 4 cores) |
| cpuVolts | float | CPU Voltage |
| cpuClock | float | CPU Clock |
| cpuTemp | float | CPU Temp in degrees C|
| diskRoot | float | |
| throttleUndervolt | boolean | Set to true if the Pi has had to throttle the CPU due to undervoltage | 
| throttleFrequency | boolean | Set to true if the Pi ARM CPU has been frequency capped |
| throttleThrottled | boolean | Set to true if the Pi is currently Throttled |
| throttleSoftTempLimit | boolean | Set to true if the Pi has throttled due to the CPU temperature getting to hot |



### depth

The MAS has a [Valeport VA500 Underwater Altimeter](https://www.valeport.co.uk/products/va500-altimeter/) installed that will allow it to track the depth of the sea floor under the MAS up to 100m deep.

__Topic:__ `science/depth/#`

| Topic | Data Type |  Notes |
|-------|-----------|--------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the reading was recieved |
| depth | integer | The measured depth of the surface below the MAS400 |
| depthUnits | string | The depth units from the sensor (M) - Meters | 
| pressure | integer | The current water pressure. | 
| pressureUnits | string | The pressure units from the sensor (dbar) |


### fluorometer

The MAS has a [Chelsea Vlux AlgaePro](https://chelsea.co.uk/product-category/sensors/) fluorometer installed that tracks characteristics of algae in the water, this can be useful for tracking algal blooms.

**Note:** The fields for this sensor are still being documented.
{: .notice--warning}


__Topic:__ `science/fluorometer`

| Topic | Data type | Notes |
|-------|-----------|-------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the reading was recieved |
| ChlAc2 | float |  |
| ChlBc2 | float |  |
| phycoerythrin2 | float |  |
| phycocyanin2 | float |  |
| turb | float |  |
| ABS1 | float | Absorbance at 450nm |
| ABS2 | float | Absorbance at 470nm |
| ABS3 | float | Absorbance at 530nm |
| ABS4 | float | Absorbance at 625nm |
| ChlAc3 | float |  |
| ChlBc3 | float |  |
| phycoerythrin3 | float | Uncorected fluorescence |
| phycocyanin3 | float |  |
| intTemp | float |  |
| gain | float |  |

### ctd

A CTD (Conductivity, Temperature, Depth) sensor from [Valeport](https://www.valeport.co.uk/product-types/ctd-environmental/) is installed which allows us to track the depth, temperature, and salinity of the water.

__Topic:__ `science/ctd`

| Topic | Data type | Notes |
|-------|-----------|-------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the reading was recieved |
| pressDepth | float | Pressure in dBar (decibar) |
| temperatureDegC | float | Water Temperature in degrees C |
| soundSpeedms1 | float | Sound velocity in the water in m/s |
| salinity | float | Salinity of sea water in PSU |
| densitykgm3 | float | Density of the sea water in kg/m3 |


### o2

An [Aanderaa o2 sensor](https://www.aanderaa.com/oxygen-sensors) is installed which provides o2 readings of the sea water the MAS is passing through.

__Topic:__ `science/o2`


| Topic | Data type | Notes |
|-------|-----------|-------|
| JSON | JSON object | This is a JSON Object summarising the data to save you from subscribing to individual topics |
| time | ISO8601 Timestamp | When the reading was recieved |
| measurement1 | integer | |
| measurement2 | integer | |
| o2Concentration | float | O2 Concentration in µM |
| airSaturationPct | float | Air Saturation % |
| temperatureDegC | float | Water Temperature in Degrees C |
| calPhaseDeg | float | Calibrated Phase|
| tcPhaseDeg | float |Temperature compensated phase |
| c1rphDeg | float | Phase measurement with blue excitation light (deg)|
| c2rphDeg | float | Phase measurement with red excitation light (deg)|
| c1AmpvV | float | Amplitude measurement with blue excitation light (mv)|
| c2AmpvV | float | Amplitude measurement with red excitation light (mv) |
| rawTemp | float | Voltage from thermistor bridge.  (mV) |


## Future planned feeds

As we get closer to departure, more feeds will come online to monitor the status of the MAS, it's sensors and experiments. Once they are brought online, they will be documented in the sections above.

**Planned feeds**

| topic | description |
|-------|-------------|
| science_pod | Status and alerts from the onboard science pod |
| hazard_detection | Classifications from the experimental hazard detection system |
| hydrophone | Classifications from the Marine Mammals Experiment |
| sensors | sensor data from the 'wet' sensors mounted on the MAS |
| microphone | Clasifcaitons from the internal microphone onbaord the MAS |


---
