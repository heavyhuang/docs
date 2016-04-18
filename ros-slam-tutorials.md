[ROS-SLAM](http://wiki.ros.org/indigo/Installation)配置教程
------------------------------------------------
------------------------------------------------

###一．arch上安装[ROS](http://wiki.ros.org/indigo/Installation/Arch)


```Bash
yaourt --noconfirm ros-indigo
yaourt -S python2-rosdep
yaourt --nocomfirm rosrun
yaourt --noconfirm rosmake
yaourt --noconfirm vtk
yaourt --noconfirm glx
```	
####安装[catkin](http://wiki.ros.org/catkin/Tutorials/create_a_workspace)
在终端输入：
```Bash
source /opt/ros/indigo/setup.bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python2 -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so
```	
`~/.bashrc`中加入：
```Bash
# ROS
indigo() {
  source /opt/ros/indigo/setup.bash
  export PYTHONPATH=/opt/ros/indigo/lib/python2.7/site-packages:$PYTHONPATH
  export PKG_CONFIG_PATH="/opt/ros/indigo/lib/pkgconfig:$PKG_CONFIG_PATH"
  # Optionally, you can set:
  #export ROS_PACKAGE_PATH=/path/to/your/package/path:$ROS_PACKAGE_PATH

  # Useful aliases
  alias catkin_make="catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python2 -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so"

  # If you use Gazebo:
  #source /usr/share/gazebo/setup.sh
} 
source ~/catkin_ws/devel/setup.bash
```
终端输入：
```Bash
sudo rosdep init
rosdep update
```
###三．使用hector-slam

1.终端输入：
```Bash
yaourt --noconfirm ros-indigo-hector-slam
```
2.下载[bag](https://code.google.com/p/tu-darmstadt-ros-pkg/downloads/list)文件

3.终端输入：
```Bash
roscore
```
4.新终端输入： 
```Bash
roslaunch hector_slam_launch tutorial.launch
```
5.新终端输入： 
```Bash
rosbag play Team_Hector_MappingBox_RoboCup_2011_Rescue_Arena.bag  --clock
```
6.`~/.bashrc`末尾加入：
```Bash
export ROS_PACKAGE_PATH=/opt/ros/indigo/share/hector_slam_launch:$ROS_PACKAGE_PATH
```
![](https://github.com/heavyhuang/docs/blob/master/pic/ros-slam.png)

###四．安装[ethzasl_icp_mapping](http://wiki.ros.org/ethzasl_icp_configuration)
终端输入：
```Bash
git clone --recursive git://github.com/ethz-asl/ethzasl_icp_mapping.git
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:`pwd`/ethzasl_icp_mapping
rosdep update
rosmaster ethzasl_icp_mapping
rosdep install ethzasl_icp_mapping
rosmake ethzasl_icp_mapping
```	
`~/.bashrc`末尾加入：
```Bash
export ROS_PACKAGE_PATH=/work/ros/ethzasl_icp_mapping:$ROS_PACKAGE_PATH
```

