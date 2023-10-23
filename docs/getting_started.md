# Getting Started

To be able to use the TCI system
download and install [hive](packages/hive.md#installation) and [drone](packages/drone.md#installation) packages.


# More Information

For detailed information see specific package:

- [drone](packages/drone.md)
- [hive](packages/hive.md)
- [libraries](packages/libraries.md)


# Running the TCI System With Docker Compose

1. Download [docker-compose.yaml](https://github.com/FETA-Project/TrafficCaptureInfrastructure/blob/main/docker-compose.yml).
2. Download and extract tar packages of drone and hive.
(see [packages](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/drone))
3. Run `make docker` in root directories of drone and hive to build docker images of both packages.
4. Pull a mysql docker image `docker pull mysql`.
5. Edit `docker-compose.yaml` as you see fit.
    * Might be necessary to edit the version of docker images based on the version you downloaded.
    Look for `image: tci_hive:<version>`, same for `tci_drone`.
    * This docker-compose automatically generates tokens in hive and sets them in drones. If you wish to do this manually change `entrypoint` in `tci_drone` (Just delete `-a` in `entrypoint: "/drone/wait-for-it.sh -t 60 tci_hive:8080 -- /drone/entrypoint.sh -a"`)
6. Finally run `docker compose up` in the directory of `docker-compose.yaml`.