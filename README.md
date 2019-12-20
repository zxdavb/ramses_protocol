# How to Communicate with Evohome

Honeywell Evohome CH/DHW systems utilize a 868 Mhz RF protocol called RAMSES II. 

Using easily-obtained, relatively inexpensive hardware, this RF traffic can be harvested for useful information, and there is also means to inject commands into the network.

Other systems, including non-Honeywell systems, also use this protocol, or a similar/older protocol called the residential network protocol - YMMV.

This wiki's purpose is to document the data structures and protocols of RAMSES II. It is a work-in-progress and contributions are encouraged.

## Basic Packet Structure
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

The basic packet structure a number of fields separated by spaces, with a variable-length payload at the end:
```
aaa XX bbb DEVICE_ID DEVICE_ID DEVICE_ID CODE nnn PAYLOAD.....
```

The three main elements of any packet are:
 - the 3 **device ids** (e.g. `04:056057`, `01:145038`) for controllers, TRV, DHW relays, etc.
 - the packet **type** (`I`, `W`, `RQ`, `RP`) and **code** (e.g. `30C9`, `10A0`, and `0008`)
 - the **payload** (e.g. `0008310107FD0...`), which can be decoded further

Detailed descriptions of the [packet structure](https://github.com/Evsdd/The-Evohome-Project/wiki/Packet-structure) and each command code is available within this wiki.

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
