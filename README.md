# ROS Vicon Driver Docker

This tool publishes the pose of objects tracked with Vicon cameras. We wrapped a docker image around the ROS Vicon driver (which was developed by [Kumar Robotics](https://github.com/KumarRobotics/vicon)).

## Building the Dockerfile
Download Docker for your system. Use the command line to navigate to the `dockerfiles` folder. Then run the following command:
```
docker build --network=host -t ros-vicon-driver .
```
This will build the image.

## Modifying the Compose File
The compose file already has the required parameters for running.

## Running the Docker Image
We use compose to run the image. Run the following command:
```
docker-compose -f vicon-compose.yaml up
```
In a new terminal, run `docker ps`. You should see the container ID for the `ros-vicon-driver` image. Using that, run
```
docker exec -it <CONTAINER ID> /bin/bash
```
The container should be running on this terminal.

## Launching Vicon
To launch the vicon packages run
```
source devel/setup.bash
roslaunch vicon vicon.launch
```
