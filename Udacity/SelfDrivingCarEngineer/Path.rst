
Path Planning, Concentrations, and Systems
+++++++++++++++++++++++++++++++++++++++++++++
In this term, you'll learn how to plan where the vehicle should go, how the vehicle systems work together to get it there, 
and you'll perform a deep-dive into a concentration of your choice.

Search
=======================

Prediction
=======================

Behavior Planning
=======================

Trajectory Generation
=======================

Path Planning Project
=======================

Coming Up: Electives
=======================

Elective: Advanced Deep Learning
==================================

Fully Convolutional Networks
==================================

Scene Understanding
==================================

Inference performance
==================================

Elective Project: Semantic Segmentation
=========================================

Elective: Functional Safety
=========================================

Functional Safety
=========================================

Autonomous Vehicle Architecture
=========================================

xxx
=========================================

System Integration Project
=========================================
Run your code on Carla, Udaticy's autonomous vehicle!

Getting Started
------------------------------
准备工作

Project Setup
~~~~~~~~~~~~~~~~
* 本项目可以使用Ubuntu 18.04系统, ROS Melodic和Unity仿真器;
* Unity仿真器链接 https://github.com/udacity/CarND-Capstone/releases ，
  使用linux_sys_int.zip，可以和ROS装在同一Ubuntu系统中；
* 项目链接 https://github.com/udacity/CarND-Capstone;
* 如果仿真器在Windows10上，ROS在Ubuntu上，则需要进行端口映射，端口号是4567，
  参考文档链接 https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Port+Forwarding.pdf  
  
  项目启动代码::

      cd ros
      catkin_make
      source devel/setup.sh
      roslaunch launch/styx.launch

Project Overview
------------------------------

System Architecture Diagram
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. image:: /images/系统架构.jpg

Code Structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* 功能
   #. (path_to_project_repo)/ros/src/tl_detector/
   #. (path_to_project_repo)/ros/src/waypoint_updater/
   #. (path_to_project_repo)/ros/src/twist_controller/

* 非功能
   #. (path_to_project_repo)/ros/src/styx/
   #. (path_to_project_repo)/ros/src/styx_msgs/
   #. (path_to_project_repo)/ros/src/waypoint_loader/
   #. (path_to_project_repo)/ros/src/waypoint_follower/