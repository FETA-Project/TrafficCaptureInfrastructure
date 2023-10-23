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

## Installation Using Docker

Note: You can also run the whole system using docker compose (see [Getting Started](../getting_started.md#running-the-tci-system-with-docker-compose))

First download the drone tar package (see [Packages](#packages)).
Extract the package and in the root folder run `make docker`.
This will build the docker image tci_drone:\<version\>.

### Supported Enviromental Variables

* `HIVE_URL`
    * URL of hive to connect to
    * default: `http://localhost:8080`
* **`TOKEN`**
    * token generated on hive for the drone
    * the only **required** variable
* `OUTPUT_FOLDER`
    * directory to use for output files (pcaps)
    * default: `/pcaps/`
* `LOG_DIR`
    * directory to use for logs
    * default: `/var/log/tci/drone/`
* `NETWORK_INTERFACE`
    * network interface to use
    * default: `eth0`
* `TCPDUMP_PATH`
    * path to tcpdump
    * default: `/usr/sbin/tcpdump`

### Example of Running a Docker Container:
`docker run -d tci_drone:<version> -e TOKEN="GENERATED_TOKEN" -e HIVE_URL="http://hive.url"`


## Installation From Source 
...