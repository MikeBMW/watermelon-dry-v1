.. |br| raw:: html

   <br/>
  
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
* 项目链接 https://github.com/udacity/CarND-Capstone
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
    以下是代码简介，能够修改的代码位于 ``(path_to_project_repo)/ros/src/``

* 可变功能
   #. (path_to_project_repo)/ros/src/tl_detector/ |br|
   
      This package contains the traffic light detection node: ``tl_detector.py``. 
      This node takes in data from the ``/image_color``, ``/current_pose``, 
      and ``/base_waypoints`` topics and publishes the locations to stop for red traffic lights to the ``/traffic_waypoint`` topic.

      The ``/current_pose topic`` provides the vehicle's current position, 
      and ``/base_waypoints`` provides a complete list of waypoints the car will be following.

      You will build both a traffic light detection node and a traffic light classification node. 
      Traffic light detection should take place within ``tl_detector.py``, 
      whereas traffic light classification should take place within ``../tl_detector/light_classification_model/tl_classfier.py``.

      .. image:: /images/TrafficLightDetection.jpg

   #. (path_to_project_repo)/ros/src/waypoint_updater/
      
      This package contains the waypoint updater node: ``waypoint_updater.py``. 
      The purpose of this node is to update the target velocity property of each waypoint 
      based on traffic light and obstacle detection data. This node will subscribe to the ``/base_waypoints``, 
      ``/current_pose``, ``/obstacle_waypoint``, and ``/traffic_waypoint`` topics, 
      and publish a list of waypoints ahead of the car with target velocities to the ``/final_waypoints`` topic.

      .. image:: /images/WaypointUpdater.jpg

   #. (path_to_project_repo)/ros/src/twist_controller/

      Carla is equipped with a drive-by-wire (dbw) system, meaning the throttle, brake, and steering have electronic control. 
      This package contains the files that are responsible for control of the vehicle: the node ``dbw_node.py`` 
      and the file ``twist_controller.py``, along with a pid and lowpass filter that you can use in your implementation. 
      The dbw_node subscribes to the ``/current_velocity`` topic along with the ``/twist_cmd`` topic to receive target linear and angular velocities. 
      Additionally, this node will subscribe to ``/vehicle/dbw_enabled``, which indicates if the car is under dbw or driver control. 
      This node will publish throttle, brake, and steering commands to the ``/vehicle/throttle_cmd``, ``/vehicle/brake_cmd``, 
      and ``/vehicle/steering_cmd`` topics.

      .. image:: /images/DBW.jpg

* 固化功能

   In addition to these packages you will find the following, which are not necessary to change for the project. 
   The ``styx`` and ``styx_msgs`` packages are used to provide a link between the simulator and ROS, 
   and to provide custom ROS message types:


   #. (path_to_project_repo)/ros/src/styx/

      A package that contains a server for communicating with the simulator, 
      and a bridge to translate and publish simulator messages to ROS topics.


   #. (path_to_project_repo)/ros/src/styx_msgs/

      A package which includes definitions of the custom ROS message types used in the project.


   #. (path_to_project_repo)/ros/src/waypoint_loader/

      A package which loads the static waypoint data and publishes to ``/base_waypoints``.


   #. (path_to_project_repo)/ros/src/waypoint_follower/

      A package containing code from Autoware(https://github.com/autowarefoundation/autoware) which subscribes to ``/final_waypoints`` 
      and publishes target vehicle linear and angular velocities in the form of twist commands to the ``/twist_cmd topic``.


Waypoint Updater Node
------------------------------

Waypoint Updater Node Overview
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The eventual purpose of this node is to publish a fixed number of waypoints ahead of the vehicle with the correct target velocities, 
depending on traffic lights and obstacles. The goal for the first version of the node should be simply to subscribe to the topics.

* ``/base_waypoints``
* ``/current_pose``

and publish a list of waypoints to

* ``/final_waypoints``

The ``/base_waypoints`` topic publishes a list of all waypoints for the track, 
so this list includes waypoints both before and after the vehicle (note that the publisher for ``/base_waypoints`` publishes only once). 
For this step in the project, 
the list published to ``/final_waypoints`` should include just a fixed number of waypoints currently ahead of the vehicle:

* The first waypoint in the list published to ``/final_waypoints`` should be the first waypoint that is currently ahead of the car.
* The total number of waypoints ahead of the vehicle that should be included in the ``/final_waypoints`` list 
  is provided by the ``LOOKAHEAD_WPS`` variable in ``waypoint_updater.py``.


Waypoint Message Descriptions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section will demonstrate use of ``rostopic` and ``rosmsg`` to learn more about the messages being transmitted on 
``/base_waypoints`` and ``/final_waypoints``. 

From the code in ``waypoint_updater.py``, we can see that both the ``/final_waypoints`` and ``/base_waypoints`` topics have message type Lane. 
You can look at the details about this message type in ``<path_to_project_repo>/ros/src/styx_msgs/msg/``, 
but this can also be done from the command line after launching the ROS project using rostopic and rosmsg as follows:

After opening a new terminal window and sourcing ``devel/setup.bash``, you can see see all topics by executing::

   $ rostopic list

You should see ``/final_waypoints`` among the topics. Executing::

   $ rostopic info /final_waypoints

will provide info about the message type being used in ``/final_waypoints``::

   Type: styx_msgs/Lane

Next executing::

   $ rosmsg info styx_msgs/Lane

provides the following message information::

   std_msgs/Header header
      uint32 seq
      time stamp
      string frame_id
   styx_msgs/Waypoint[] waypoints
      geometry_msgs/PoseStamped pose
         std_msgs/Header header
         uint32 seq
         time stamp
         string frame_id
      geometry_msgs/Pose pose
         geometry_msgs/Point position
            float64 x
            float64 y
            float64 z
         geometry_msgs/Quaternion orientation
            float64 x
            float64 y
            float64 z
            float64 w
      geometry_msgs/TwistStamped twist
         std_msgs/Header header
            uint32 seq
            time stamp
            string frame_id
      geometry_msgs/Twist twist
         geometry_msgs/Vector3 linear
            float64 x
            float64 y
            float64 z
         geometry_msgs/Vector3 angular
            float64 x
            float64 y
            float64 z

twist messages http://docs.ros.org/en/jade/api/geometry_msgs/html/msg/Twist.html


Lane message example
~~~~~~~~~~~~~~~~~~~~~~~~~~
As a use-case example, given a single ``styx_msgs/Lane`` message ``my_lane_msg``, 
you can access the x direction linear velocity of the first waypoint in Python with::

   my_lane_msg[0].twist.twist.linear.x


Topics and message types
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Comparison
   :widths: 20 20 20
   :header-rows: 1

   * - Topic
     - Msg Type
     - Notes
   * - /base_waypoints
     - styx_msgs/Lane	
     - Waypoints as provided by a static .csv file.
   * - /current_pose	
     - geometry_msgs/PoseStamped
     - Current position of the vehicle, provided by the simulator or localization.
   * - /final_waypoints	
     - styx_msgs/Lane	
     - This is a subset of /base_waypoints |br|
       The first waypoint is the one in |br|
       /base_waypoints which is closest to the car.

DBW Node
---------------------------------------------

Traffic Light Detection Node
---------------------------------------------
