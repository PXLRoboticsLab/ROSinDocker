# ROSinDocker


# Install docker

```
1: sudo apt-get update
```
```
2: sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```
    
```
3: curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```
4: sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
 ``` 
 
 ```
5: sudo apt-get update
```
   
   ```
6: sudo apt-get install docker-ce
   ```
   To test the docker installation enter the following command
   ```
7: sudo docker run hello-world
   ```
   If the output is hello world your installion has succeeded 

# Start container with gpu rendering on

To run gazebo in the container we have to share the host's virtual environment with  the container. For more information on this topic you can look at the links on the bottom of this guide.

We have to allow our xserver to be able to make a connection with the container.
1: xhost +local:root

Now we can start the container.
2: sudo docker run -it     --env="DISPLAY"     --env="QT_X11_NO_MITSHM=1"     --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"     osrf/ros:kinetic-desktop-ful

You can check if your gpu is working in docker by installing mesa-utils and typing the glxinfo command.

# Installing turtlebot on the container
Now we have to install and configure the turtlebot so we can spawn it in gazebo.

All of the following commands are INSIDE the container 

1: apt-get update

2: install all packets related to the turtlebot ( make sure to install ros-kinetic verion )
apt-get install -y ros-kinetic-turtlebot 
apt-get install -y ros-kinetic-turtlebot-apps 
apt-get install -y ros-kinetic-turtlebot-interactions
apt-get install -y ros-kinetic-turtlebot-simulator
apt-get install -y ros-kinetic-kobuki-ftdi
apt-get install -y ros-kinetic-ar-track-alvar-msgs

Source setup.bash to make the ros commands available everywhere.

3: echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

4: source ~/.bashrc

Declare the Gazebo world file for the turtlebot

5: export TURTLEBOT_GAZEBO_WORLD_FILE=/opt/ros/kinetic/share/turtlebot_gazebo/worlds/playground.world

Launch an instance of the turtlebot , this may take a while to load into gazebo so dont freak out of your gazebo stays black for a couple of minutes. The load time decreases a lot after the first time.

6: roslaunch turtlebot_gazebo turtlebot_world.launch

The result should look like this : https://imgur.com/a/bYLPT

The next command allows you to take control of the turtlebot

7: roslaunch turtlebot_teleop keyboard_teleop.launch

Have fun crashing into walls without any consequences !

# Interesting links about ros/docker with gui or gazebo.

http://wiki.ros.org/docker/Tutorials/GUI

https://hub.docker.com/_/gazebo/

https://hub.docker.com/r/osrf/gazebo/

https://github.com/osrf/docker_images/tree/master/ros

https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce-1



Author : Benjamin Klingeleers
