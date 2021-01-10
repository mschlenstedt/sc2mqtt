# sc2mqtt
Skoda Connect 2 MQTT 

This version is based on sc2mqtt from https://github.com/DBa2016/sc2mqtt

This is a connector between Skoda Connect and your smart home software via MQTT. This version is modified to have also Loxone timestamps (epoche date is from 01.01.2009 in Loxone). Also MQTT handling and config is enhanced compared to the original version.

## Limitations
- no trip and position information right now published in MQTT
- only Skoda Connect, no VW/Seat/Audi
- only read-only operations right now; e.g. no remote safelock operation nor heating nor remote start etc.

## Requirements
- Python 3. Tested with 3.8.2. Newer versions will likely work, too.
- supported OS. Tested with Rapbian on a Raspi4, but any linux should work. Windows might work, too, but that is entirely untested.
- MQTT broker. SSL and authentication currently not supported yet, but will be supported in future.
- a Skoda car and a Skoda Connect account. You should avoid " and \ in your username and password; also having local characters (accents, umlauts, entirely non-latin characters) in your username (email) and/or password has not been tested at all; while it might work, your are at your own risk here.

## Installation
1. Create a directory writable by the user sc2mqtt is going to be executed with
2. Copy the sc2mqtt.py file into this directory
3. Install Python 3. Tested with Python 3.8.2, higher versions should work.
4. Install following python3 modules: time, hashlib, base64, requests, pyquery, re, json, logging, asyncio, functools, paho.mqtt, pathlib, nest_asyncio. Some of them will be already available, some will be installable through your package manager software, and some you will need to install with pip3.
5. Run ./sc2mqtt.py (see "Usage").

The program does not go to background, I recommend using a daemon manager. My personal choice is PM2, because it is easy to configure and to run, and everything about a user process can be configured and maintained directly by the user.

## Usage
Call the sc2mqtt.py file directly. It will search for `config.json` in the current directory; if none found (or an invalid one), it will create a `config.json.sample` and exit.

You can use the following command line options:

__--configfile=/path/to/configfile/config.json__: Use the configfile `/path/to/configfile/config.json` instead von config.json in current folder

__--logfile=/path/to/configfile/logfile.log__: Log all messages also to `/path/to/configfile/logfile.log``

__--loglevel=[info|error|debug]__: Use this loglevel instead of the standard loglevel

Upon successful start, it will poll Skoda Connect every 60 seconds for a status update on every vehicle detected for the account and post the sensor values over MQTT.

## Configuration
Configuration is done in the file `config.json` in the same directory as `sc2mqtt.py`. You can use another location with --configfile option (see above). The following options exist:

   * broker :  Address of your mqtt broker
   * brokerport : Port of your mqtt broker
   * topic : Topic under which all messages will be published. Defaults to "sc2mqtt"
   * brokeruser : Username of your broker if you use authentication
   * brokerpassword : Password of your broker if you use authentication
   * user : SkodaConnect Username
   * password : SkodaConnect Password
   * expand_json_data : Set to "1" if you would like to expand the json data in the mqtt topics. Set to "0" will send json data to mqtt.

## TODO
- add more queryable content (trip data, heater, etc.)
- add SSL for MQTT
- tidy up code; it has been written for me only, so is probably difficult to read for others.
- extend funcitonality to allow "write" operations (lock/unlock, switch standby heating on/off etc.)
- extend functionality to other VAG brands (VW, Audi, Seat)

## Thanks
- original idea by https://github.com/TA2k/ioBroker.vw-connect ; some information (URLs etc.) borrowed from there, too
- original script by https://github.com/DBa2016/sc2mqtt
