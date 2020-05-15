# How to Communicate with Evohome

Honeywell Evohome CH/DHW systems utilize a 868 Mhz RF protocol called RAMSES II. 

Using easily-obtained, relatively inexpensive hardware, this (unecrypted) RF traffic can be harvested for useful information, and there is also means to inject commands into the network.

Other systems, including non-Honeywell systems, also use this protocol, or a similar/older protocols such as EnviraCOM, or the residential network protocol - YMMV.

[This wiki's](https://github.com/Evsdd/The-Evohome-Project/wiki) purpose is to document the data structures and protocols of RAMSES II. It is a work-in-progress and contributions are encouraged.

## Decoding evohome (RAMSES-II) RF traffic
This is a sequence of RAMSES II packets sent over the RF network:
```
045  I --- 01:145038 --:------ 01:145038 30C9 018 0008310107FD02076503077D04075C050758
053  I --- 30:082155 --:------ 30:082155 1F09 003 000537
055 RQ --- 07:045960 01:145038 --:------ 10A0 006 0013740003E4
045 RP --- 01:145038 07:045960 --:------ 10A0 006 0013880003E8
045  I --- 12:010740 --:------ 12:010740 0008 002 00BE
045  I --- 04:056057 --:------ 01:145038 3150 002 0326
045  I --- 34:064023 --:------ 34:064023 3120 007 0070B0000000FF
```

Detailed descriptions of the [packet structure](https://github.com/Evsdd/The-Evohome-Project/wiki/Packet-structure) and each command code is available within this wiki.

By a combination of _eavesdropping_ traffic, and _probing_ the system via carefullt-constructed command packets, much can be learnt about the system - some of which is not exposed via the vendor's UI/API.  For example, consider this zone (using [this](https://github.com/zxdavb/evohome_rf) tool):

```json
    "04": {
        "actuators": ["04:189082", "04:189083"],
        "configuration": {
            "local_override": true,
            "multi_room_mode": false,
            "openwindow_function": true
        },
        "heat_demand": 0.43,
        "name": "Main Hall",
        "sensor": "22:012345",
        "setpoint_capabilities": {
            "max_temp": 24.5,
            "min_temp": 5.5
        },
        "setpoint_status": {
            "setpoint": 21.5,
            "mode": "FollowSchedule",
            "until": null
        },
        "temperature": 19.28,
        "window_open": false,
        "zone_type": "Radiator Valve"
    }
```

A similar level of detail can be obtained about the devices that make up an evohome system (e.g. battery state).

## Communication Hardware

The 'standard' device for interfacing with Evohome RF networks is the Honeywell HGI80. 

These will be the most compatible option, but are relatively expensive and can be difficult to obtain.  However, they cannot be made to emulate other Evohome devices, such as a TRV.

Alternatively, there are several 'hobbyist' USB devices which may be used, including:

  - NanoCUL (modified firmware has been published)

  - In-circuit Signalduino (modified firmware to be published)

  - Seeedstudio RFBee (firmware to be published)

These devices may be less compatible than a HGI80, but are easily available and very likely cheaper. They can be used to emulate Evohome devices (even, theoretically, an Evohome controller). 

### Acknowledgements

This work builds upon the work by CrazyDiamond, Hydrogenetic and fullTalgoRythm. We're very grateful to them and the many other contributors.

### Disclaimer

This documentation has been developed by observing the messages sent between the various Honeywell devices and is not supported or endorsed in any way by Honeywell or Resideo. The use of this information to control or monitor your home heating system is your own responsibility although we not aware of any risk of damage to devices through the use of these commands.
