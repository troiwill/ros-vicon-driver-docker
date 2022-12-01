FROM ros:melodic-ros-core-bionic

SHELL ["/bin/bash", "-c"]

# Install necessary packages.
RUN apt-get update
RUN apt-get -y install build-essential git libeigen3-dev libyaml-cpp-dev \
    ros-melodic-tf2-ros ros-melodic-tf

# Make a catkin workspace.
RUN mkdir -p /home/root/catkin_ws/src
WORKDIR /home/root/catkin_ws
RUN . /opt/ros/melodic/setup.sh && catkin_make

# Clone the vicon driver repository.
WORKDIR /home/root/catkin_ws/src
RUN git clone https://github.com/KumarRobotics/vicon.git
WORKDIR /home/root/catkin_ws

# Run catkin make once more to build the new ROS packages.
RUN . /opt/ros/melodic/setup.sh && catkin_make
RUN sed -i "s|129|78|g" /home/root/catkin_ws/src/vicon/vicon/launch/vicon.launch

CMD /bin/bash
