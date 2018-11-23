# Dilithium Power Systems: Photon 3 CAN DBC

[![Build Status](https://travis-ci.org/karlding/dilithium-power-photon3-dbc.svg?branch=master)](https://travis-ci.org/karlding/dilithium-power-photon3-dbc)

CAN DBC file for the [Dilithium Power Systems](https://www.dilithiumpower.com) [Photon 3](https://www.dilithiumpower.com/products/photon-3) MPPT.

**Note**: This assumes that your MPPT is sending CAN messages for each channel
on `0x600` and `0x601`. If your MPPT is using different CAN IDs, change the ID
to match. [Kvaser Database Editor](https://www.kvaser.com/downloads-kvaser/)
is a pretty good option for a free DBC GUI editor.

## Requirements

These instructions use [eerimoq/cantools](https://github.com/eerimoq/cantools)
over SocketCAN to decode messages with the DBC. If you're using PCAN-Explorer
or something similar, feel free to ignore those steps/requirements and
directly import the DBC.

```bash
# Ensure that can-utils is installed
sudo apt-get update
sudo apt-get install can-utils

# Start the slcand userspace daemon and create slcan0 interface
# We assume 500 kbps bitrate, see (https://elinux.org/Bringing_CAN_interface_up)
sudo slcand -o -c -s6 /dev/ttyUSB0 slcan0

# Bring up the SocketCAN interface
sudo ifconfig slcan0 up

# Install cantools (either in a virtualenv or something).
pip install cantools
```

## Usage

```bash
# View the DBC file
cantools dump dbc/photon-3.dbc

# Decode CAN messages with the DBC
candump slcan0 | cantools decode --single-line dbc/photon-3.dbc
```
