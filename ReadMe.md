# CyberDog simulator

This simulation platform uses a form of gazebo plugin to eliminate the effects of ros communication desynchronization on control. It also provides a visualization tool based on Rviz2 to forward lcm data of the robot state to ROS.
The simulation platform needs to communicate with the cyberdog_locomotion silo to be used, and it is recommended to install it through the cyberdog_sim silo.

For more information, please refer to [**Documentation of the simulation platform**](https://miroboticslab.github.io/blogs/#/cn/cyberdog_gazebo_cn).

Recommended installation environment: Ubuntu 20.04 + ROS2 Galactic

## Dependencies

The following dependencies are required to run the emulation platform  
**ros2_galactic**

```
$ sudo apt update && sudo apt install curl gnupg lsb-release
$ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
$ echo “deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main” | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
$ sudo apt update
$ sudo apt install ros-galactic-desktop
```

**Gazebo**

```
$ curl -sSL http://get.gazebosim.org | sh
$ sudo apt install ros-galactic-gazebo-ros
$ sudo apt install ros-galactic-gazebo-msgs
```

**LCM**

```
$ git clone https://github.com/lcm-proj/lcm.git
$ cd lcm
$ mkdir build
$ cd build
$ cmake -DLCM_ENABLE_JAVA=ON ...
$ make
$ sudo make install
```

**Eigen**

```
$ git clone https://github.com/eigenteam/eigen-git-mirror
$ cd eigen-git-mirror
$ mkdir build
$ cd build
$ cmake ...
$ sudo make install
```

**xacro**

```
$ sudo apt install ros-galactic-xacro
```

**vcstool** ``` $ sudo apt install ros-galactic-xacro

```
$ sudo apt install python3-vcstool
```

**colcon** ```` $ sudo apt install python3-vcstool
**colcon** \*\*colcon
$ sudo apt install python3-colcon-common-extensions

```

Note: If there are other versions of yaml-cpp installed in your environment, it may conflict with the yaml-cpp that comes with ros galactic, so it is recommended to compile without other versions of yaml-cpp in your environment.

## Download
``
$ git clone https://github.com/MiRoboticsLab/cyberdog_sim.git
$ cd cyberdog_sim
$ vcs import < cyberdog_sim.repos
```

## Compile

You need to set BUILD_ROS in src/cyberdog locomotion/CMakeLists.txt to ON.
Then you need to compile in the cyberdog_sim folder

```
$ source /opt/ros/galactic/setup.bash
$ colcon build --merge-install --symlink-install --packages-up-to cyberdog_locomotion cyberdog_simulator
``

## Use
It needs to be run in the cyberdog_sim folder
```

$ python3 src/cyberdog_simulator/cyberdog_gazebo/script/launchsim.py

```

### It is also possible to run each program separately with the following commands:

First start the gazebo program in the cyberdog_sim folder as follows:
```

$ source /opt/ros/galactic/setup.bash
$ source install/setup.bash
$ ros2 launch cyberdog_gazebo gazebo.launch.py
``
The lidar can also be turned on with the following command

```
$ source /opt/ros/galactic/setup.bash
$ source install/setup.bash
$ ros2 launch cyberdog_gazebo gazebo.launch.py use_lidar:=true
``

Then launch the control program for the cyberdog_locomotion bin. Run it in the cyberdog_sim folder:
```

$ source /opt/ros/galactic/setup.bash
$ source install/setup.bash
$ ros2 launch cyberdog_gazebo cyberdog_control_launch.py

```

Finally open the visualization and run it in the cyberdog_sim folder:
```

$ source /opt/ros/galactic/setup.bash
$ source install/setup.bash
$ ros2 launch cyberdog_visual cyberdog_visual.launch.py

```

### Play live lcm log data
The platform is able to playback and visualize motion control lcm data from real machines through rviz2.
This is done as follows:
Run the lcm data visualization interface in the cyberdog_sim folder as follows:
```

$ source /opt/ros/galactic/setup.bash
$ source install/setup.bash
$ ros2 launch cyberdog_visual cyberdog_lcm_repaly.launch.py

```
After opening the visualization interface, play the lcm log data by running the script in the script directory of the cyberdog_locomotion bin.
Do the following in the cyberdog_sim folder:
```

$ cd src/cyberdog_locomotion/scripts
$ . /make_types.sh # For first time use you need to run
$ . /launch_lcm_logplayer.sh

```
After running, select the lcm log file you want to play, then you can play the log data, and then you can reproduce the robot's posture through the rviz visualization interface.

Translated with DeepL.com (free version)
```
