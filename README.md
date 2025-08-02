# ASP Containerlab SRv6 Lab

The ASP Containerlab SRv6 lab is an Ansible playbook that dynamically generates configurations for the nodes hosted on Cisco dcloud using Containerlab. 

This project is modifed from the original ASP Innovation Lab automation that builds day1 configurations. 

Authors: Marty Fierbaugh (mfierbau@cisco.com), Chris Olson (christoo@cisco.com)

![ASP SRv6 Lab on Cisco dCloud](https://github.com/cisco-asp/asp-srv6-lab/blob/main/images/example.png?raw=true)


## Features 

Features in the Ansible playbook:
 - Segment Routing v6 uSID using ISIS
 - Transport Independant Loop Free Alternate (TI-LFA)
 - Flexable Algorithm
 - Core [QOS](Qos.md)
 - Model-Driven Telemetry
 - Segment Routing Performance Measurement (SR-PM)
 - BGP with PIC Edge/Core

## Devices 
A collection of xrd (Containerized XR) and VXR (Cisco 8000 Series Emulators that have been wrapped in containers) are used in the topology.

- Cisco XRd running IOS-XR 25.2.1 (PE and CE)
- Cisco 8212-48FH-M running IOS-XR 25.2.1 (CORE.101, CORE.102)
- Cisco 8711-32FH-M running IOS-XR 25.2.1 (CORE.103, CORE.104)
- Cisco T-Rex ver 3.06 (Trex-1, Trex-2)

## Usage

1. Start the Containerlab topology 
    
```
clab deploy
```

2. Once the topology is fully booted, deploy the ansible configuration via the shell script that passes the required variables to ansible_playbook.

```
./config_lab
```

It can take 8-10 minutes for the VXR nodes to fully boot. Follow the container logs and make sure you see the message "Router up"

```
docker logs -f <container id>
```

WARNING - this is a full commit replace and will restore the Agile Services Networking configuration. 

3. Access the nodes via SSH.  

To execute the full lab, please use the lab guide (currently PPTX format).


## Start T-REX 

The dcloud lab includes pre-built traffic generators based upon trex.

1. Start the interactive trex console which will make a local connection to the running interactive daemon
```
./trex-console
```

 Start generating some traffic
```
start -f stl/imix.py
```

2. To interact with and view statistics for the current stream launch the text-based user interface (tui)
```
tui
```

3. Attempt to increase per interface traffic rate to 200mbps (400mbps rx/tx total). Throughput achievable in the Docker environment is dependent primarily on single core\thread CPU performance.
```
update -m 200mbps
```