# ROSinDocker


xhost +local:root ( uitvoeren op host )


sudo docker run -it --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" --volume="/tmp/.X11-unix:/tmp/.X11:rw" osrf/ros:kinetic-desktop-full

( vanaf hier gebeurd alles in de container ) 

apt-get update

echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

source ~/.bashrc

apt-get install ros-kinetic-turtlebot ros-kinetic-turtlebot-apps 
ros-kinetic-turtlebot-interactions ros-kinetic-turtlebot-simulator 
ros-kinetic-kobuki-ftdi ros-kinetic-ar-track-alvar-msgs



export TURTLEBOT_GAZEBO_WORLD_FILE=/opt/ros/kinetic/share/turtlebot_gazebo/worlds/playground.world

roslaunch turtlebot_gazebo turtlebot_world.launch

roslaunch turtlebot_teleop keyboard_teleop.launch
