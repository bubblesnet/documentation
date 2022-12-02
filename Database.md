# Database

The database is a Postgres database running in the database container on the controller
edge-device. The database code in contained 
in [the migrations directory](https://github.com/bubblesnet/controller/tree/develop/server/migrations).  The 
database is kept up-to-date by running all the migrations in the directory every time
the container restarts.  The migration utility is a distinct [Go project](https://github.com/golang-migrate/migrate)


| Table              | Description                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------|
| ac_device          | Single AC device that is associated with a single outlet                                    |
| additive           | A single liquid that can be loaded into a dispenser                                         |      
| alertcondition     | NOT USED - intended to represent a condition that might trigger a notification              |
| automationsettings | The stages of a crop and the target environmental conditions for each stage                 |
| contact            | NOT USED                                                                                    |
| crop               | NOT USED - intended to represent a single cycle through all the stages of the crop          |
| currentevent       | NOT USED                                                                                    |
| device             | An edge device - one each for cabinet and external                                          |
| devicetype         | Type of edge device (Raspberry Pi)                                                          |
| dispenser          | A dispenser that can dispense a single additive                                             |
| displaysettings    | Station-specific settings like language, font, units                                        |
| event              | Each sensor reading becomes a row in the event table                                        |
| growlog            | NOT USED - intended to hold freeform events like "lollipopped plants for harvest"           |
| module             | An add-on module to the Pi that can hold 1 or more sensors                                  |
| notification       | NOT USED - intended to represent a notification that has been delivered                     |
| outlet             | An AC mains outlet that is associated with a particular GPIO pin driving an AC mains relay. |
| plant              | NOT USED - intended to represent a single plant                                             |
| registrationcode   |                                                                                             |
| schema_migrations  | USED BY THE MIGRATION TOOL. DO NOT MODIFY.                                                  |
| seed               | NOT USED - intended to represent a single seed (breed ...)                                  |
| sensor             | A single sensor that can be standalone, or one of many on a module.                         |
| site               | The farm that the one or more stations belong to                                            |
| station            | The cabinet that contains the edge-devices                                                  |
| user               | The user that logs in to the user interface                                                 |
| usersettings       | User specific setting, mostly not used.                                                     |

## The key tables

### site
| Column      | Description                                      |
|-------------|--------------------------------------------------|
| siteid      | The unique identifier for this site.             | 
| sitename    | The name of this site (e.g. "John's farm")       |
| userid_user | Foreign key link to the user that owns this site |

### station
| Column                                 | Description                                                                                                                         |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| stationid                              | The unique identifier for this station.                                                                                             | 
| siteid_site                            | Foreign key link to the site this station belongs to                                                                                |
| humidifier                             | Flag indicating the presence of this capability                                                                                     |
| humidity_sensor_internal               | Flag indicating the presence of this capability                                                                                     |
| humidity_sensor_external               | Flag indicating the presence of this capability                                                                                     |
| light_sensor_internal                  | Flag indicating the presence of this capability                                                                                     |
| light_sensor_external                  | Flag indicating the presence of this capability                                                                                     |
| light_bloom                            | Flag indicating the presence of this capability                                                                                     |
| light_vegetative                       | Flag indicating the presence of this capability                                                                                     |
| light_germinate                        | Flag indicating the presence of this capability                                                                                     |
| heater                                 | Flag indicating the presence of this capability                                                                                     |
| water_pump                             | Flag indicating the presence of this capability                                                                                     |
| air_pump                               | Flag indicating the presence of this capability                                                                                     |
| station_door_sensor                    | Flag indicating the presence of this capability                                                                                     |
| outer_door_sensor                      | Flag indicating the presence of this capability                                                                                     |
| movement_sensor                        | Flag indicating the presence of this capability                                                                                     |
| pressure_sensors                       | Flag indicating the presence of this capability                                                                                     |
| root_ph_sensor                         | Flag indicating the presence of this capability                                                                                     |
| water_level_sensor                     | Flag indicating the presence of this capability                                                                                     |
| intake_fan                             | Flag indicating the presence of this capability                                                                                     |
| exhaust_fan                            | Flag indicating the presence of this capability                                                                                     |
| heat_lamp                              | Flag indicating the presence of this capability                                                                                     |
| heating_pad                            | Flag indicating the presence of this capability                                                                                     |
| height_sensor                          | Flag indicating the presence of this capability                                                                                     |
| water_heater                           | Flag indicating the presence of this capability                                                                                     |
| ec_sensor                              | Flag indicating the presence of this capability                                                                                     |
| voc_sensor                             | Flag indicating the presence of this capability                                                                                     |
| co2_sensor                             | Flag indicating the presence of this capability                                                                                     |
| water_dispenser                        | Flag indicating the presence of this capability                                                                                     |
| phup_dispenser                         | Flag indicating the presence of this capability                                                                                     |
| phdown_dispenser                       | Flag indicating the presence of this capability                                                                                     |
| automatic_control                      | Flag indicating that the device, in sense-go, should control the on/off state of things like heater to keep parameters within range |
| tamper_xmove                           | Size of the X movement that triggers a TILT alarm                                                                                   |
| tamper_ymove                           | Size of the Y movement that triggers a TILT alarm                                                                                   |
| tamper_zmove                           | Size of the Z movement that triggers a TILE alarm                                                                                   |
| time_between_pictures_in_seconds       | How often in seconds should look-inside pictures be taken                                                                           |
| time_between_sensor_polling_in_seconds | How often should environmental sensors be polled?                                                                                   |

### device
| Column                        | Description                                                            |
|-------------------------------|------------------------------------------------------------------------|
| deviceid                      | Unique id for this device                                              |
| userid_user                   | HISTORICAL RELIC - Foreign key link to the user this device belongs to | 
| stationid_station             | Foreign key link to the station this belongs to                        | 
| devicetypeid_device           | Foreign key link to the devicetype this device is                      |
| created                       | NOT USED - timestamp of the creation of this device                    |
| lastseen                      | NOT USED - timestampe of last communication received from this device  |
| devicename                    | Name of this device (e.g. "cabinet")                                   |
| externals                     | The balena UUID of this device                                         |
| log_level                     | NOT USED - Text log level the device should use                        |
| picamera                      | Flag indicating this device has Pi camera attached                     |
| picamera_resolutionx          | X resolution of the pi camera                                          |
| picamera_resolutiony          | Y resolution of the pi camera                                          |
| latest_picture_filename       | Local filename of the latest look-inside picture for this device       |
| latest_picture_datetimemillis | Timestamp of the latest look-inside picture                            |


### module
| Column          | Description                                                                    |
|-----------------|--------------------------------------------------------------------------------|
| moduleid        | Unique id of this module                                                       |
| deviceid_device | Foreign key link to the edge-device this is attached to                        |
| module_name     | Common name for the module "e.g. bme280"                                       | 
| container_name  | Name of the container this module belongs to                                   |
| module_type     | Official type name of the module (e.g. "bme280")                               |
| i2caddress      | The i2c address of the module in hex format (e.g. "0x23")                      |
| protocol        | Name of the protocol the module uses to communicate (e.g. "i2c" or "one-wire") |

### sensor
| Column          | Description                                                               |
|-----------------|---------------------------------------------------------------------------|
| sensorid        | Unique id for this sensor                                                 | 
| moduleid_module | Foreign key link to the module that hosts this sensor |
| sensor_name | The name of the sensor (e.g. "pH") |
| extraconfig | |
| latest_calibration_datetimemillis | NOT USED |


### ac_device
| Column          | Description                                                                |
|-----------------|----------------------------------------------------------------------------|
| ac_deviceid     | Unique id for this ac_device                                               |
| outletid_outlet | Foreign key link to the outlet that this device is plugged into            |
| ac_device_name  | The name of the device (e.g. "heater", "humidifier")                       | 
| autonomous      | NOT USED - Flag true if the device controls itself and should always be ON |

### outlet
| Column            | Description                                                                                          |
|-------------------|------------------------------------------------------------------------------------------------------|
| outletid          | Unique id of this outlet                                                                             | 
| stationid_station | Foreign key link to the station this belongs to                                                      | 
| deviceid_device   | Foreign key link to the device that controls this outlet                                             | 
| name              | Name of this outlet (e.g. "humidifier")                                                              |
| index             |                                                                                                      |
| bcm_pin_number    | The BCM pin number of the GPIO pin that controls this outlets ON/OFF state                           |
| autonomous        | NOT USED - Flag true if the device controls itself and should always be ON, redundant with ac_device |


### dispenser
| Column                      | Description                                                         |
|-----------------------------|---------------------------------------------------------------------|
| stationid_station           | Foreign key link to the station                                     | 
| deviceid_device | Foreign key link to the edge-device this is attached to             |
| currently_loaded_additiveid | Foreign key link to the additive currently loaded in this dispenser | 
| dispenser_name | A name for this dispenser                                           |
| index | |
| bcm_pin_number | The BCM pin number of the GPIO pin that controls this dispensers ON/OFF state |
| milliliters_per_millisecond | The number of milliliters that will be dispensed when the valve is open for one millisecond |


### additive
| Column            | Description                                                         |
|-------------------|---------------------------------------------------------------------|
| manufacturer_name | The name of the producer of the nutrient (e.g. General Hydroponics) | 
| additive_name | The name of the nutrient (e.g. RapidGro) |
| spec_url | URL of the manufacturers datasheet for the nutrient |
| is_pgr | Flag true if the nutrient is considered PGR |

### event
| Column             | Description                                                            |
|--------------------|------------------------------------------------------------------------|
| eventid            | Unique id for this event                                               | 
| siteid_site        | Foreign key link to the site                                           | 
| stationid_station  | Foreign key link to the station                                        | 
| userid_user        | HISTORICAL RELIC - Foreign key link to the user this device belongs to | 
| deviceid_device    | Foreign key link to the edge-device that generated this event          |
| datetimemillis     | Milliseconds since the epoch timestamp for this event                  |
| subeventdatemillis | NOT USED                                                               |
| type               | what type of event is this? e.g. "measurement"                         |
| message            |                                                                        |
| filename           | If the event is stored in a .json file, the filename of that file      |
| floatvalue         | The float value of this measurement                                    |
| intvalue           | The integer value of this measurement                                  |
| stringvalue        | The string value of this measurement                                   |
| textvalue          | NOT USED                                                               |
| rawjson            | The entire raw JSON that was sent by the edge-device for this event    |
| time               ||
| units              | The units that this value represents (e.g. "gallons")                  |
| value_name         | The name of this measurement (e.g. "temperature_middle")               |



### automationsettings
| Column                    | Description                                                       |
|---------------------------|-------------------------------------------------------------------|
| current_lighting_schedule | Text description of the lighting schedule (e.g. 12/12)            | 
| light_on_start_hour | 0-23 hour in local time when light should go ON                   |
| target_temperature | Target air temperature for this stage                             |
| temperature_min | Minimum air temperature to display on chooser slider              |
| temperature_max | Maximum air temperature to display on chooser slider              |
| humidity_min | Minimum humidity to display on chooser slider                     |
| humidity_max | Maximum humidity to display on chooser slider                     |
| target_humidity | The desired humidity for this stage                               |
| humidity_target_range_low |                                                                   |
| humidity_target_range_high |                                                                   |
| current_light_type | Which light - blooming or vegetative should be used in this stage |
| target_water_temperature | The desired water temperature for this stage                      |
| water_temperature_max | Maximum water temperature to display on chooser slider            |
| water_temperature_min | Minimum water temperature to display on chooser slider            |
| stage_name | The name of this stage - e.g. BLOOMING, VEGETATIVE ...            |
