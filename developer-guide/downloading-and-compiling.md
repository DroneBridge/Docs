---
description: How to setup and compile DroneBridge modules
---

# Downloading & Compiling

## Prerequisites/Requirements

* Raspberry Pi
* Raspbian Stretch or similar
* Python 3.6+
* python3-pyric
* python3-netifaces
* OpenVG by paeryn for the OSD \(see below\)

**For the OSD download & install the following library:**  
[https://github.com/paeryn/openvg/tree/screenshot](https://github.com/paeryn/openvg/tree/screenshot)  
OSD is only supported on Raspberry Pi platform

## Default SSH login data: 

User: **pi**  
Password: **raspberry**

## Download and compile from git

Open the shell and type:

```bash
git clone https://github.com/seeul8er/DroneBridge.git
cd DroneBridge
# git checkout nightly
git submodule init
git submodule update
```

To compile enter the root directory \(DroneBridge/\) and run:

```bash
cmake .
make
cd communication/DroneBridgeLib
sudo pip install .
```

This will compile all C modules and install the DroneBridge Python Common library

