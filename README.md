# ROSinDocker UBUNTU16.04
For a school project we are configuring robots with the robot operating system ( ros ) . For our testing environment we have set up a docker container which runs ros and gazebo. Like this we play around and test our scripts without having to install ros and gazebo ourselves. This also provides a perfect test environment so we dont have to take any risks with expensive robots.
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
The output should be this : https://imgur.com/a/dCWBo

# Start container with gpu rendering on
To run gazebo in the container we have to share the host's virtual environment with  the container. For more information on this topic you can look at the links on the bottom of this guide.
We have to allow our xserver to be able to make a connection with the container.
```
1: xhost +local:root
```
When lauching the container we have to add some arguments in order to make everything work.

--env="DISPLAY"

--env="QT_X11_NO_MITSHM=1"

--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"

These arguments allow us to expose our xhost to the container. By reading and writing through the X11 unix socket.

Image : https://github.com/osrf/docker_images/tree/master/ros/kinetic/ubuntu/xenial/desktop-full

Let's test it
```
2: sudo docker run -it     --env="DISPLAY"     --env="QT_X11_NO_MITSHM=1"     --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"     osrf/ros:kinetic-desktop-full
```
If for some reason you leave it and want to enter the container again use the following command
```
1: sudo docker ps -a
```
Copy the container ID
```
sudo docker exec -it "container ID" bash
```
You can check if your gpu is working in docker by installing mesa-utils inside the container
```
1: apt-get update
```
```
2: apt-get -y install mesa-utils
```
```
3: glxinfo | grep dire
```
the result should look like this : https://imgur.com/a/fWWad

# Installing turtlebot on the container
Now we have to install and configure the turtlebot so we can spawn it in gazebo.
All of the following commands are INSIDE the container 
```
1: apt-get update
```
```
2: install all packets related to the turtlebot ( make sure to install ros-kinetic verion )
apt-get install -y ros-kinetic-turtlebot 
apt-get install -y ros-kinetic-turtlebot-apps 
apt-get install -y ros-kinetic-turtlebot-interactions
apt-get install -y ros-kinetic-turtlebot-simulator
apt-get install -y ros-kinetic-kobuki-ftdi
apt-get install -y ros-kinetic-ar-track-alvar-msgs
```
Source setup.bash to make the ros commands available everywhere.
```
3: echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
```
```
4: source ~/.bashrc
```
Declare the Gazebo world file for the turtlebot
```
5: export TURTLEBOT_GAZEBO_WORLD_FILE=/opt/ros/kinetic/share/turtlebot_gazebo/worlds/playground.world
```
Launch an instance of the turtlebot , this may take a while to load into gazebo so dont freak out of your gazebo stays black for a couple of minutes. The load time decreases a lot after the first time.
```
6: roslaunch turtlebot_gazebo turtlebot_world.launch
```
The result should look like this : https://imgur.com/a/bYLPT
The next command allows you to take control of the turtlebot
```
7: roslaunch turtlebot_teleop keyboard_teleop.launch
```
Have fun crashing into walls without any consequences !
# Troubleshooting

If you get errors looking like this, kill all processes containing gazebo in their name
```
[gazebo_gui-3] process has died [pid 14341, exit code 139, cmd /opt/ros/kinetic/lib/gazebo_ros/gzclient __name:=gazebo_gui __log:=/root/.ros/log/9d06f718-e014-11e7-aaea-0242ac110002/gazebo_gui-3.log].
log file: /root/.ros/log/9d06f718-e014-11e7-aaea-0242ac110002/gazebo_gui-3*.log
```
Get a list of the processes
```
ps ax
```
Copy paste the processes ID ( PID ) and kill it.
```
kill -9 PID
```
For other problems go to 

https://answers.ros.org/ 

http://answers.gazebosim.org/
# Interesting links about ros/docker with gui or gazebo.

http://wiki.ros.org/docker/Tutorials/GUI

https://hub.docker.com/_/gazebo/

https://hub.docker.com/r/osrf/gazebo/

https://github.com/osrf/docker_images/tree/master/ros

https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce-1



Author : Benjamin Klingeleers
