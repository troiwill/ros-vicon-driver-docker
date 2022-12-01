# ROS Vicon Driver Docker

This tool publishes the pose of objects tracked with Vicon cameras. We wrapped a docker image around the ROS Vicon driver (which was developed by [Kumar Robotics](https://github.com/KumarRobotics/vicon)).

## Setting up the Docker Image

### Required Tools

First, ensure that you have the `docker` tool installed. You can find the tool at [docker.com](https://www.docker.com). Then clone this repository.

### Modifying the Docker Compose File

Once the repository is on your computer, edit the docker compose file: `docker-compose.yaml`. Specifically, you need to change two pieces of information:

1. The `IP address` of the Vicon server. Change the IP address `127.0.0.1` in the line `vicon_server_ip: 127.0.0.1` to the IP address of the Vicon server.
2. The `host name` of the computer that is running this docker. You can find your computer's host name by running `hostname` in your terminal on a Mac or Linux computer. Then substitute `HOSTNAME` in the compose file for the output of the command you ran.

Your resulting compose file should have the following new lines:
```
... # Other lines
        vicon_server_ip: 192.168.1.132
... # Other lines
      ROS_HOSTNAME: mypc
      ROS_MASTER_URI: http://mypc:11311
```
if the IP address of your Vicon server is `192.168.1.132`, and the name of your computer is `mypc`. If you are using a Mac computer, you may need to add `.local` to the end of your computer name (i.e., `mypc.local`).

### Building the Docker Image

The **recommended** method for building the docker image is running docker compose. Once you edited the compose file (described above), run the following command in the `dockerfiles` directory:
```
docker compose up
```
For some systems, you may need to add `sudo` to the beginning of the command.

The `docker compose up` command will build the docker image and then start a new docker container named `vicon-driver-c`.

## Using the Docker Image

To publish the poses for each object of interest, open 2 + `N` terminals, where `N` is the number of objects you will track with the Vicon system. For example, `N` is 3 if you will track one drone and two ground vehicles.

- **Terminal #1:** Run
```
docker compose up
```

- **Terminal #2:** Run
```
# This command will log into the docker container.
docker exec -it <CONTAINER ID> /bin/bash

# These commands will launch the vicon driver.
source devel/setup.bash
roslaunch vicon vicon.launch
```
`CONTAINER ID` is the alphanumeric ID for the `vicon-driver-c` container. You can find this ID when you run `docker ps` (the ID will be in the first column).

- **Remaining terminals:** In each, run
```
# This command will log into the docker container.
docker exec -it <CONTAINER ID> /bin/bash

# These commands will launch the vicon driver.
source devel/setup.bash
roslaunch vicon_odom vicon_odom.launch model:=<MODEL>
```
Here, `MODEL` is the name of the model (object) you created in the Vicon system.
