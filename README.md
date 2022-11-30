# BubblesNet
BubblesNet is a system for the monitoring and automatic control of a secure, indoor deep-water-culture hydroponic 
setup using either a hard cabinet or a tent.  It has features that are unique to marijuana growth 
(i.e. door security, odor control) but is mostly applicable to any plant that you want to grow under
tightly controlled and monitored conditions.

To install and run an instance of BubblesNet, [start here](GettingStarted.md)

Here is what the system looks like in operation:
![A screenshot of the control tab](user_interface/Screen_Station_Control.png "The system control tab")

### Functional Requirements
  * Provide automated and manual environmental control of an enclosed deep-water-culture hydroponics setup
  * Provide notification of out-of-range events
  * Provide near-realtime vision of the crop
  * Generate a stream of environmental time-series data that can be correlated post-harvest with plant growth
  * Automate as much of the entire seed to harvest pipeline as possible
  * Provide manual control of dispensable liquids for control of water level, pH level and other nutrient levels
  * Odor control
  * All controllable components (light, heat ...) can be manually controlled from the user interface

### Non-functional Requirements
* As little physical intervention as possible
* Once collected, data is never lost
* Standard-of-care authentication
* Encryption in flight and at rest
* Graceful degradation - no component in the chain should crash/blow-up just because another component
has failed or bogged down.

### Devices
The system software has 2 devices:
* Controller - A Raspberry Pi 4 8GB that contains the data storage, web user interface and alerting functions. 
* Edge-device - A Raspberry Pi 3B+ that contains the actual sensors and relay controls to monitor and control the environment in the
grow space.  Designed to run on a Pi (3CMM, 3B, 3B+, 4), port to another type of device would be painful.

Within the system these devices are typically referred to as "edge_device".

## Extensibility
The components of the system are behind APIs that should allow users to add
on to an installation of Bubbles.

### Examples:

A user could add a new container to the edge-device that
simply sends properly formatted gRPC messages to the store-and-forward container
and they will end up in the database and accessible as events in the UI.

A user could send properly formatted messages to the Edge/Server API that
would end up in the database and UI.

## Pre-requisites
* Postgres 
* ActiveMQ
* Balena (IoT fleet maintenance service)

### Controller
The controller is a control, and data collection and analysis service designed to 
work with the bubblesnet edge-device.  It consists of:

Client - a web user interface for controlling one or more edge devices
and viewing the data from those devices.  Written in React.

Server - an API server that serves both the controller React application AND 
the edge devices manipulating the physical environment and queueing data for storage.  Written
mostly in NodeJS.

The Server contains five container, 2 of them with prepackaged balena blocks and 3 of them with distinct nodejs express applications:
* (database) Postgresql XX.X database container
* (activemq) ActiveMQ XX.X queue system container
* (api) API server - takes measurement and event messages from edge-devices and queues them for processing.  Also
  serves UI data requests. 
* (queue) Queue server - takes measurement and event messages off the queue and stores them in the database and also publishes
those messages to a "topic" (queue) for the web socket server to consume.
* (websocket) WebSocket server - pushes messages from the "topic" out to any Client instance (UI) that has connected to it. This
is what gives the user interface its ability to show environment changes in real-time.
  
### Edge-device
The edge device is one or more Raspberry Pi devices running BalenaOS (Yocto). The device runs at least
4 containers:
* sense-go - sensor code written in Go along with relay control, sends all measurement to store-and-forward container via gRPC
* sense-python - sensor code written in Python3, sends all measurement to store-and-forward container via gRPC
* store-and-forward - stores messages from the sensor containers until it can forward them via the Edge-Server API
* wifi-connect - a service to let you connect the device to the correct wifi network


## Links to deeper dives
* [Stuff you may see references to that may not work](StuffThatDoesntWorkRightNow.md)
* [Messaging](Messaging.md)
* [Data Structures](DataStructures.md)
* [API: GRPC](APIGRPC.md)
* [API: Edge/Server](APIEdge.md)
* [API: UI Service](APIUIService.md)

## History (i.e. where the bugs come from)
This open-source project is a port and integration of an existing set of unrelated personal projects.  This
original set of projects included:
* Server written in Java J2EE as APIs that write JSON files to filesystem
* File processors written in Java that read/write ActiveMQ, move file data into databases and archive files
* A MySQL database
* An Android/Android Things edge device that itself was a rewrite of a RPi python edge device

## Lists of things

### List of hardware modules
* [ADS1115 A to D converter (2)](datasheets/ads1115.pdf)
* [BME280 temperature/humidity/pressure sensor (4 top, middle, bottom and external)](datasheets/bst-bme280-ds002.pdf)
* BMP280 temperature/pressure sensor (as substiture for BME280)
* [ADXL345 accelerometer for tamper detection](datasheets/ADXL345.pdf)
* Pi Camera
* 8-port relay
* Single port relay (1 for each dispenser)
* X Volt Valve (1 for each dispenser)
* [CCS811 CO2/VOC sensor](datasheets/CCS811_Datasheet-DS000459.pdf)
* [DS18B20 analog water temperature sensor](datasheets/DS18B20.pdf)
* [ezoPH pH sensor](datasheets/pH_EZO_Datasheet.pdf)
* ezoPH pH sensor carrier board
* ezoPH pH sensor probe
* [eTape water lever sensor](datasheets/Standard eTape Data Sheet.pdf)

Within the system, these modules are generally referred to as "module".  Datasheets for these modules are in the datasheets directory.

### List of other hardware
* Exhaust can filter
* Exhaust fan
* 5V computer fan
* 12V power supply
* 5V power supply
* LED grow light
* Water pump
* Air pump
* Water heater
* Space heater



