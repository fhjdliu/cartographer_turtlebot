﻿# cartographer_turtlebot using leishen 2d lidar


---

 - 说明
主要是在turtlebot上实现google的激光slam--cartographer，由于cartographer本身已经具备了用在turtlebot上的ROS包，但由于cartographer的版本不同导致的编译不同过，本项目保留了一份可以完全通过编译并且正常运行的源代码。本项目采用的2d激光雷达为镭神科技的LS01C。


----------


 - 注意
由于网上比较流行的cartographer_ros编译方法是[hitcm][1]改进后的，该方法只对cartographer_ros进行了编译，在catkin_ws工作空间内省掉了独立编译和安装，编译起来方便许多。若想进一步编译google自己的cartographer时存在以下几点问题：
    - hitcm的cartographer版本太旧，编译cartographer_turtlebot会出现错误
    - hitcm的方法是先分别编译安装cartographer、ceres-solver、cartographer_ros，且前两者安装在本地/usr/local目录下，这样就算cartographer版本是最新的，但在编译cartographer_turtlebot时会找不到cartographer的一些必要文件。
 
所以按照官方提供的方法编译cartographer_turtlebot是最妥当的方式，该方法在编译cartographer_turtlebot时一并编译安装了cartographer、ceres-solver、cartographer_ros，所有编译和安装均独立于catkin_ws工作空间内，也易于移植和删除。


----------
## 编译程序包

 - 编译环境
Ubuntu 14.04
ROS(indigo)

 - 编译方法
保证在编译安装本项目之前，系统里没有任何有关于cartographer的安装文件。若之前已经安装了cartographer和ceres-solver，则最好能够删除掉，包括/user/local/下的库和头文件，以及之前catkin_ws工作空间里的build文件夹。不然在编译cartographer_turtlebot包时可能会找不到function.cmake等文件。
本项目编译和安装方法参考官方提供的方法：
```
#Install wstool and rosdep.
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build

#Create a new workspace in 'cartographer_catkin_ws'.
cd ~
mkdir -p cartographer_catkin_ws/src
cd ~/cartographer_catkin_ws/src

#Git code from github.
git clone git@github.com:wangqi123412431/cartographer_turtlebot.git

#Install deb dependencies
rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y

#Build and install
cd ~/cartographer_catkin_ws
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```


----------
## 运行

 - 运行leishen激光雷达：

```
#如果该ros包是在另一个workspace编译的，则需要先在workspace下：
source /devel/setup.bash 

#再运行以下指令，注意此时激光雷达发布的主题为“/laser_scan”，frame_id为“plate_middle_link”。
roslaunch talker leishen.launch
```
- 运行cartographer_turtlebot:
```
#在cartographer_catkin_ws下：
source instal_isolated/setup.bash

roslaunch cartographer_turtlebot turtlebot_leishen_lidar_2d.launch
```

  [1]: http://www.cnblogs.com/hitcm/p/5939507.html