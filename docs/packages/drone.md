# Drone

A drone for the Traffic Capture Infrastructure system.

Sole purpose of a drone is to capture network traffic with a specified filter
and send the data back to hive, that controls it.

## Capture Backend

Drone can use couple of tools, that do the capturing.

### TCPDump

The simplest capturing.
Select the appropriate network interface in `config.toml`

#### Required Tools

* [tcpdump](https://www.tcpdump.org/)

### NDP

Used for capturing with high speed network cards.

#### Required Tools

* [Network Development Kit](https://github.com/CESNET/ndk-sw)
  * filterctl
  * ndp-receive
  * nfb-dma
  * nfb-bus


## Config

Example configuration file (default path: `/etc/tci/drone/config.toml`):

```conf
[hive]
url = "http://localhost:8080/hive/v1"
token = ""

[drone]
name = "drone1"
description = "This is a description"
output_folder = "./"
capture_backend = "tcpdump"
#capture_limit = 0
#update_captured_interval = 0.05

[tcpdump]
ifc = "eth0"
#tcpdump_path = "/bin/tcpdump"

#[ndp]
#device = "/dev/nfb/by-serial-no/NFB-200G2QL/61"
#dma_have = 16
#cores = 2
#filterctl_path = "/bin/filterctl"
#ndp-receive_path = "/bin/ndp-receive"
#nfb-dma_path = "/bin/nfb-dma"
#nfb-bus_path = "/bin/nfb-bus"
```

## Packages

Find packages [here](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/drone)

## Installation with RPM

The drone in provided in a RPM form, so it can be installed like this:
`dnf -y install /path/to/tci_drone.rpm`

## Installation from source
...