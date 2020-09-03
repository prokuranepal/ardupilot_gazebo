# Ardupilot Gazebo Plugin & Models

## Requirements :
Native Ubuntu able to run full 3D graphics.
Gazebo version 8.x or greater
The dev branch will works on gazebo >= 8.x  
For Gazebo 7 use branch gazebo7

**Note :** Virtual Machine such as VMWare Player does not support full 3D graphics.

but, possible solution is here

Type follow in the terminal,
````
echo "export SVGA_VGPU10=0" >> ~/.bashrc
source ~/.bashrc
````
solution retreived from here http://answers.gazebosim.org/question/13214/virtual-machine-not-launching-gazebo/

**Note :** This just enables running gazebo in virtual machine, does not guarantee the performance and Gazebo require much of CPU & GPU processing power depending on what you are running the simulation.

ArduPilot setup for SITL launch
Gazebo version 7.x or 8.x  
The gazebo9 branch will works on gazebo >= 9.x  

## Disclamer :
This is a playground until I get some time to push the correct patch to gazebo master (I got hard time to work with mercurial..)!  
So you can expect things to not be up-to-date.  
This assume that your are using Ubuntu 16.04 or Ubuntu 18.04

## Usage :
I assume you already have Gazebo installed with ROS (or without).  
If you don't have it yet, install ROS with `sudo apt install ros-melodic-desktop-full`
(follow instruction here http://wiki.ros.org/melodic/Installation/Ubuntu).  
Due to a bug in current gazebo release from ROS, please update gazebo with OSRF version from http://gazebosim.org/tutorials?tut=install_ubuntu

libgazeboX-dev must be installed, X be your gazebo version (9 on ROS melodic).

For Gazebo X
````
sudo apt-get install libgazeboX-dev
````

````
git clone https://github.com/SwiftGust/ardupilot_gazebo
cd ardupilot_gazebo
mkdir build
cd build
cmake ..
make -j4
sudo make install
````

````
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
````

Set Path of Gazebo Models (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
````

Set Path of Gazebo Worlds (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_RESOURCE_PATH=~/ardupilot_gazebo/worlds:${GAZEBO_RESOURCE_PATH}' >> ~/.bashrc
````

````
source ~/.bashrc
````

DONE !

export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models:${GAZEBO_MODEL_PATH}
export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models_gazebo:${GAZEBO_MODEL_PATH}
export GAZEBO_RESOURCE_PATH=~/ardupilot_gazebo/worlds:${GAZEBO_RESOURCE_PATH}
````

Install is complete

Now launch a world file with a copter/rover/plane and ardupilot plugin, and it should work!
(I will try to add some world file and model later)

## HELP

How to Launch :  
Launch Ardupilot Software In the Loop Simulation for each vehicle.
On new terminal, launch Gazebo with basic demo world.

#####ROVER (no model provided for now)

On 1st Terminal (Launch Ardupilot SITL)
````
sim_vehicle.py -v APMrover2 -f gazebo-rover --map --console
````

On 2nd Terminal (Launch Gazebo with demo Rover model)
````
gazebo --verbose worlds/ (Please Add if there is one.)
````

##### COPTER

On 1st Terminal (Launch Ardupilot SITL)
````
sim_vehicle.py -v ArduCopter -f gazebo-iris --map --console
````

On 2nd Terminal (Launch Gazebo with demo 3DR Iris model)
````
gazebo --verbose worlds/iris_arducopter_runway.world
````

##### PLANE

On 1st Terminal (Launch Ardupilot SITL)
````
sim_vehicle.py -v ArduPlane -f gazebo-zephyr --map --console
````

On 2nd Terminal (Launch Gazebo with demo Zephyr flying wing model)
````
gazebo --verbose worlds/zephyr_ardupilot_demo.world
````

In addition, you can use any GCS of Ardupilot locally or remotely (will require connection setup).
If MAVProxy Developer GCS is uncomportable. Omit --map --console arguments out of SITL launch and use APMPlanner 2 or QGroundControl instead.
Local connection with APMPlanner2/QGroundControl is automatic, and recommended.

## Troubleshooting

### Missing libArduPilotPlugin.so... etc 

In case you see this message when you launch gazebo with demo worlds, check you have no error after sudo make install.  
If no error use "ls" on the install path given to see if the plugin is really here.  
If this is correct, check with `cat /usr/share/gazebo/setup.sh` the variable `GAZEBO_PLUGIN_PATH`. It should be the same as the install path. If not use `cp` to copy the lib to right path. 

For Example

````
sudo cp -a /usr/lib/x86_64-linux-gnu/gazebo-7.0/plugins/ /usr/lib/x86_64-linux-gnu/gazebo-7/
````

path mismatch is confirmed as ROS's glitch. It will be fixed.

````
sudo cp -a /usr/lib/x86_64-linux-gnu/gazebo-7.0/plugins/ /usr/lib/x86_64-linux-gnu/gazebo-7/
````
Rangefinder
