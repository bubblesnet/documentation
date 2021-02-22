## Messaging

The system runs on a single JSON message format.  These messages:
* Are sent by the UI to the edge devices to control devices (Grow lights, heat ...) within the cabinet.
* Are sent by the devices to tell the UI about conditions within the cabinet and are archived in the database.

In addition to crossing API boundaries, these messages are typically passed around within individiual
component applications (edge-device, UI ...) rather than decomposing the message and passing around
individual elements.

### Common Message Header Format
FROM: anywhere
TO: anywhere

These fields appear in all messages:

|  Field Name | Description/Values  |
|---|---|
| deviceid | Edge-device unique ID assigned by server|
| executable_version | major.minor.patch_string-build_timestamp
| githash | commit has as calculated by git and embedded in executable |
| message_type | ["measurement","switch_state","switch_event"] |

### Edge-device originated messages
FROM: edge-device
TO: server

Include message header and these fields in all messages originating on the edge-device:

|  Field Name | Description/Values  |
|---|---|
| container_name | The name of the container generating the message ['sense-go','sense-python'] |

### Common Measurement Message Format
FROM: edge-device
TO: server/UI

Include message header, edge-device origination, and set header value
* message_type = "measurement"

Add:

|  Field Name | Description/Values  |
|---|---|
| sample_timestamp | timestamp in millis since epoch|
| sensor_name | name of the sensor as understood by the UI ["","","",""]|
| measurement_name | name of the measurement as understood by UI - one of ['tamper','root_ph', 'temp_middle_air' ...]|
| value   | value as FLOAT|
| units   | units as string one of ['cm','in','lux', 'hg', 'hPa', 'Volts' ...]|
| direction   | direction as ['up','down']|

### Distance Sensor Measurement Message
FROM: edge-device
TO: server/UI

Include message header and set header value

|  Field Name | Description/Values  |
|---|---|
| message_type | "measurement"|

Include common measurement items and set values:

|  Field Name | Description/Values  |
|---|---|
| measurement_name | "plant_height"|
| sensor_name | ""|

Include message header and common measurement items AND:

|  Field Name | Description/Values  |
|---|---|
| distanceCm   | convenience value as centimeters|
| distanceIn   | convenience value as inches|

#### Example
{"deviceid":70000007,"container_name":"sense-go","executable_version":"2.1.1 'Sat_02/20/2021__02:16_PM' 3391466ebb176ce7688f0f30a38829f6 2a886e72","sample_timestamp":1613951479676,"message_type":"measurement","sensor_name":"height_sensor","measurement_name":"plant_height","value":12.5310764312744 14,"units":"cm","direction":"up","distance_cm":12.531076431274414,"distance_in":4.9334946579820524}

### ADC message
FROM: edge-device
TO: server/UI

Include message header and set header value

|  Field Name | Description/Values  |
|---|---|
| message_type | "measurement" |

Include common measurement items and set value:

|  Field Name | Description/Values  |
|---|---|
| measurement_name | ""|
| sensor_name | ""|


|  Field Name | Description/Values  |
|---|---|
| channel | the ADC channel 0-3|
| gain    | gain applied|
| rate    | rate applied|

#### Example


### Tamper Sensor message
FROM: edge-device
TO: server/UI

Include message header and set header value

|  Field Name | Description/Values  |
|---|---|
| message_type | "measurement"|

Include common measurement items and set value:

|  Field Name | Description/Values  |
|---|---|
| measurement_name | "movement" |
| sensor_name | "" |

Add

|  Field Name | Description/Values  |
|---|---|
|	xmove| movement on the X axis in ?? units |
|	ymove| movement on the Y axis in ?? units |
|	zmove| movement on the Z axis in ?? units |

#### Example
{"deviceid":70000007,"container_name":"sense-go","execut able_version":"2.1.1 'Sat_02/20/2021__02:16_PM' 3391466ebb176ce7688f0f30a38829f6 2a886e72","sample_timestamp":1613951443358,"message_type":"measurement","sensor_name":"tamper_sensor","measurement_name":"movement","value":0,"units":"","direct ion":"","xmove":0.00390625,"ymove":0.00390625,"zmove":0.03515625}

### Switch Event message
FROM: edge-device
TO: server/UI

Indicates that the edge device has turned on/off or detected on/off state change
for a particular switch.  The UI shall not indicate a switch state change until
it receives this message from the edge-device to minimize the possibility
of the UI indicating a wrong state.

Include message header and set header value

|  Field Name | Description/Values  |
|---|---|
| message_type | "switch_event"|

Add

|  Field Name | Description/Values  |
|---|---|
| event_timestamp | event timestamp as millis |
| switch_name | switch name as understood by UI []|
| on          | if TRUE, switch is ON/closed`|

## Sensor and Measurement names
|  Sensor Name | Measurement Name  |
|---|---|
| humidity_sensor_internal | humidity_internal |
| pressure_sensor_internal | pressure_internal|
| thermometer_top | temp_air_top |
| thermometer_middle | temp_air_middle |
| thermometer_bottom | temp_air_bottom |
| humidity_sensor_external | humidity_external |
| pressure_sensor_external | pressure_external|
| thermometer_external | temp_air_external |
| thermometer_water | temp_water |
|  light_sensor_internal |  light_internal |
|  water_level |  water_level |
|   |  adc49_1_volts |
|   |  adc49_2_volts |
|   |  adc49_3_volts |
|   |  adc48_0_volts |
|   |  adc48_1_volts |
|   |  adc48_2_volts |
|   |  adc48_3_volts |
|  movement_sensor |  movement |
|  root_ph_sensor |  root_ph |
|  distance_sensor |  plant_height |
|  cabinet_door_sensor |  cabinet_door_open |
|  external_door_sensor |  external_door_open |

## Switch names
|  Switch Name | Notes  |
|---|---|
| humidifier  | Turned on/off to keep humidity in range  |
|  heater | Turned on/off to keep air-temp in range  |
| water_pump  |  Turned on/off to control drip irrigation into pot-top |
|  air_pump |  Provides oxygen to water |
|  intake_fan |  Provides airflow over leaves |
|  exhaust_fan |   |
| heat_lamp  |  Used during germination to provide mild heat/light |
| heating_pad  |  Used underneath germination plate for gentle warmth |
|  light_germinate |  The grow light used in germinate phase |
|  light_vegetative |  The grow light used in vegetative phase |
|  light_bloom |  The grow light used in bloom phase |
