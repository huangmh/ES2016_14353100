# ROS安装


---

## 什么是ROS
ROS的全名是`Robot Operating System`，即机器人操作系统。它原本是斯坦福大学的一个机器人项目，后来由Willow Garage公司发展，目前由OSRF（Open Source Robotics Foundation, Inc）公司维护的开源项目。

### 1. 首先是一个操作系统

根据wikipedia定义，`OS is system software that manages computer hardware and software resources and provides common services for computer programs`。也就是说操作系统是用来管理计算机硬件与软件资源，并提供一些公用的服务的系统软件。而ROS也自称是一个OS。

计算机的操作系统将计算机硬件封装起来，而应用软件运行在操作系统之上，不用管计算机具体应用的是什么类型的硬件产品。这能大大提高软件开发效率（否则大家只能都写汇编了）。

同理，ROS则是对机器人的硬件进行了封装，不同的机器人、不同的传感器，在ROS里可以用相同的方式表示（topic等），供上层应用程序（运动规划等）调用。

### 2. 是一种跨平台模块化软件通讯机制

ROS用节点（Node）的概念表示一个应用程序，不同node之间通过事先定义好格式的消息（Topic），服务（Service），动作（Action）来实现连接。

![](http://images2015.cnblogs.com/blog/607055/201609/607055-20160907225125848-1315048094.png)

三种通讯方式的优缺点可看上表，由于很多模块化编程工具都有类似功能，这里就不具体展开了。

基于这种模块化的通讯机制，开发者可以很方便地替换、更新系统内的某些模块；也可以用自己编写的节点替换ROS的个别模块，十分适合算法开发。

此外，ROS可以跨平台，在不同计算机、不同操作系统、不用编程语言、不同机器人上使用。

### 3. 是一系列开源工具
**rqt_plot**：可以实时绘制当前任意Topic的数值曲线；

**rqt_graph**：可以绘制出各节点之间的连接状态，和正在使用的Topic等；

**TF**：TF是Transform的简写，利用它，我们可以实时知道各连杆坐标系的位姿，也可以求出两个坐标系的相对位置。

**Rviz**：超强大的3D可视化工具，可以显示机器人模型、3D电影、各种文字图标、也可以很方便二次开发；

### 4. 是一系列最先进的算法
　　
## 二：ROS配置
按照安装进程进行安装，这里有[中文教程](http://wiki.ros.org/cn/jade/Installation/Ubuntu)。

### 安装前
首先确定各仓库的状态为enable，如下图所示：

![ubuntusoft](http://ogd1nxbhk.bkt.clouddn.com/1ab5_ubuntusoft.png)

![other](http://ogd1nxbhk.bkt.clouddn.com/1ab5_othersoft.png)

### 安装过程

- 添加source.list,指令如下：  
       
`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

- 添加keys,指令如下：  
  
`sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116 `

- 安装
  - 首先确保虚拟机的软件包索引是最新的
`sudo apt-get update`  
  - 桌面完整版安装 
`sudo apt-get install ros-jade-desktop-full`  
  - 初始化rosdep:  
`sudo rosdep init
rosdep update`  
  - 环境配置
`echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc`  
   - 安装rosinatall 
`sudo apt-get install python-rosinstall`  

## 测试ROS
安装ROS成功后，在Beginner Tutorials有一个简单的示例程序  
  
- 打开一个终端，输入指令：  
`roscore`
 
得到：

![roscore_error](http://ogd1nxbhk.bkt.clouddn.com/1ab5_roscore_err.png)
    
嗯，出错了。    
而且错误是我们很熟悉的Permission Denied 。当年配置pintos的时候不断出现的错误。
把权限提高：
`sudo chmod -777 ~/.ros/`
这样就好了。

![roscore](http://ogd1nxbhk.bkt.clouddn.com/1ab5_roscore.png)
  
 roscore是你在使用ros之前应该首先运行的程序。所以，后来的程序要运行，一定一定要保持着这个终端是运行的。
 
 不然，就是：
 
 ![turtle](http://ogd1nxbhk.bkt.clouddn.com/1ab5_tertle_err.png)
 
- 打开第二个终端，输入以下指令，开启一个小乌龟界面：  
`rosrun turtlesim turtlesim_node`

![turtle](http://7xrn7f.com1.z0.glb.clouddn.com/16-11-7/2995849.jpg)

- 打开第三个终端，输入以下指令，接收键盘输入，控制小乌龟移动
`rosrun turtlesim turtle_teleop_key`

选中最后打开的Terminal,键盘按下上下左右按键,可看到控制小乌龟移动.
出现以下界面：
![tuetle_go](http://ogd1nxbhk.bkt.clouddn.com/1ab5_turtle_go.png)  
  （本图的乌龟和上图的不同，是因为第一次运行时没有让乌龟移动就关闭了窗口，再次打开时乌龟就变了样子。）

- 打开第四个终端，输入指令，可以看到ROS nodes以及Topic等图形展示：

`rosrun rqt_graph rqt_graph`

![all](http://ogd1nxbhk.bkt.clouddn.com/1ab5_turtle_go_dot.png)
由上图可看见，左右两边矩形为ROS node，中间连线上是Topic名称。  

关掉小乌龟运动的那个终端。
![](http://ogd1nxbhk.bkt.clouddn.com/1ab5_turtle_dot.png)

小乌龟运动的node就没有了。

## 心得

这次的配置比起前面的DOL以及后面的那个8G的神奇物种都要简单。

安装过程没有出现任何的问题。

问题就在测试那里。

但是，发现以前的那点解决问题。

学习总是有益的，哪怕当时并不觉得权限改变会在以后用到。但是，这次就真的用到了。







