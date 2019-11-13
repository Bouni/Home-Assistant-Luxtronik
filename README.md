# Home-Assistant-Luxtronik



**DEPRECATED: This repo is no longer maintained as an [official Home-Assitant component](https://github.com/home-assistant/home-assistant/pull/27230) is comming soon&trade;**



A Luxtronik heatpump component for Home Assistant

This component is going to use the "old" binary protocol that Alpha Innotec, the manufacturer of the Luxtronik heatpumps, used in its Java based webinterface. They added a new, websockets based webinterface in newer versions, but in order to make this a useful component to as much users as possible we decided to stick with the binary protocol.
Maybe we implement the new protocol in the future and make it configurable which protocol the component is going to use.

## Binary protocol

A Luxtronik heatpump runs a TCP server that servers the binary protocol on port 8888 or 8889 depending on the firmware version. 

The binary protocol has 4 commands which are decribed in seperate files:

- 3001 : Read Attribute, not implmented (at least in V3.79)
- [3002](3002.md) : Writing parameters to the heatpump
- [3003](3003.md) : Read parameters from the heatpump
- [3004](3004.md) : Read calculations from the heatpump (measurement values, etc.)
- [3005](3005.md) : Read visibilitys from  the heatpump (not clear at the Moment what the purpose of these is)
- 3006 : Authenticate, not implmented (at least in V3.79)


## Usage

**Disclaimer**
This not meant to be the final repo for the Component / Platforms, just for others to test the custom_component.

Copy the whole `luxtronik` folder as it is into your `custom_components` directory.

Then configure the Component and the sensors/binary sensors like this example:

In configuration.yaml:
```
luxtronik:
  host: "192.168.88.11"
  port: 8889
```
In sensors.yaml:
```
- platform: luxtronik
  scan_interval: 60
  sensors:
    - id: ID_WEB_Temperatur_TVL
      friendly_name: Temperatur Vorlauf
    - id: ID_WEB_Temperatur_TBW
      friendly_name: Temperatur Rücklauf
```
In binary_sensors.yaml:
```
- platform: luxtronik
  scan_interval: 60
  sensors:
    - id: ID_WEB_VBOout
      name: Brunnenwasserpumpe
```

If a `friendly_name` is set, the entity added is named `luxtronik.temperatur_rucklauf` for example else the given `id`is used, `luxtronik.id_web_temperatur_tvl` for example.

You'll need to select the sensors by ID, look at the [data.txt](data.txt) file for reference.
