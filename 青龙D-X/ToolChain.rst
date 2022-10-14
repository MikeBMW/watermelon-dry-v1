数据链路
======================================================================================================

系统方案
------------------------------------------------------------------------------------------------
系统方案包括数据链路架构、功能架构、总线通讯拓扑等


数据链路架构
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
数据链路包括车端、云端、开发端、台架和车辆端、

.. image:: /images/数据链路架构.png




功能架构
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
功能架构包括感知融合、地图定位、规划控制、系统车辆等。

.. image:: /images/功能架构.png


功能分布

.. image:: /images/功能分布.png

感知

.. image:: /images/感知.png

高精地图定位

.. image:: /images/高精地图1.png
.. image:: /images/高精地图2.png
.. image:: /images/高精地图3.png

规划决策

.. image:: /images/规划决策1.png
.. image:: /images/规划决策2.png
.. image:: /images/规划决策3.png


控制

.. image:: /images/控制.png

系统监管

.. image:: /images/系统监管.png

端到端

.. image:: /images/端到端延时.png


总线通讯拓扑
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
拓扑包括DDS、CAN、XCP等通讯协议

.. image:: /images/总线通讯拓扑.png  

.. note:: 
    * 2核 BPU AI引擎,高性能等效算力128 TOPS
    * 八核 Arm® Cortex® -A55 CPU集群
    * CV引擎,双核DSP,双核ISP,强力Codec

开发调试
------------------------------------------------------------------------------------------------

XCP协议
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /images/XCP主从节点.png
.. image:: /images/XCP_Drive.png

车端
------------------------------------------------------------------------------------------------

ROS RVIZ Gazebo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /images/ROS_RVIZ.png
.. image:: /images/ROS_RVIZ_V.png
.. image:: /images/ROS_Gazebo.png


云端
------------------------------------------------------------------------------------------------

A7 数据中心
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

泊车环视激光雷达真值
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
必要性论证


数据链路平台开发责任划分
------------------------------------------------------------------------------------------------

*  数据链路系统是一套工具集，既可以给开发用，也可以给测试用；开发要想好怎么用，测试也要想好怎么用； 
*  尽快搜集 ``规控`` 类需求: 开发要提出XCP对上位机显示的要求, 包括具体的规控参数; --> 李锐 9.30
*  优先考虑 ``规控`` LDW,LKA,AEB等ADAS功能 **虚警** 类测试场景,即长距离测试环境下(至少1000米),系统应不发出报警，并记录系统的任何报警情况； --> 李锐 9.30
*  初步调研 **融合**, **定位**, **诊断**, **高精定位** 等需求；--> TBD
*  尽快搜集 ``泊车建图``, ``泊车定位`` 的测试需求；    --> 侯殿龙  10.15
    * 传感器数据包括:
       #. 前广角相机图像;
       #. 4路环视相机图像;
       #. 车辆报文(包括轮速脉冲和车辆三轴IMU数据)
       #. RTK数据
       #. IMU数据,5个毫米波雷达的点云
    
*  ``感知`` 类需求要考虑地平线J5的视频采集方案, 要与地平线方案保持一致, 方便以后观察内部参数 ;  --> TBD
*  快速建立 ``ROS可视化功能``;  --> 高亮L4  持续进行,与规控需求打和
*  探索ROS集成XCP驱动的方案, 需要NeuSAR支持, 兼容CANape;  --> Neusar   
*  软件架构接口和数据流设计，确保与平台工具适配；  -->高学新   持续进行
*  PSM制定技术平台的开发计划; 明确工作的内容,范围,拆解等; 制定ADS域控并行产品交汇点计划,确保交汇点之后平台工具可用； -->高亮 项目待确认

.. note:: 
   #. L4团队为BYD项目提供可视化工具; -> 高亮
   #. 关于传感输入数据回灌方案,参考J5感知数据采集方案,制定平台统一方案（摄像头、毫米波雷达、激光雷达等）;
   #. 支持SLAM调试参数;
   #. 架构支持接口定义描述,提供元数据;  -> 架构 高学新
   #. 采用ROS2+DDS框架, 将路采数据标准化，制定测试用例; -> 高亮
   #. 将会支持全平台数据可视化(融合、定位、感知等),支持开发人员远程调试; -> 高亮
   #. 泊车规划架构方案，障碍物检测，停车位检测，应用层数据流，具体的数据结构 -> 赵钢平 


元数据
------------------------------------------------------------------------------------------------
元数据（Metadata），又称中介数据、中继数据，为描述数据的数据（data about data），主要是描述数据属性（property）的信息，用来支持如指示存储位置、历史数据、资源查找、文件记录等功能。

数据 ： ``175``

.. image:: /images/元数据.png


总线拓扑
------------------------------------------------------------------------------------------------

.. image:: /images/总线拓扑2.png
    

代码 ROS Client Library
------------------------------------------------------------------------------------------------
rclcomm::

    class rclcomm :public QThread
    {
        Q_OBJECT
    public:
        rclcomm();
        void run() override;
    private:
        void recv_callback(const std_msgs::msg::Int32::SharedPtr msg);
    private:
        rclcpp::Publisher<std_msgs::msg::Int32>::SharedPtr _publisher;
        rclcpp::Subscription<std_msgs::msg::Int32>::SharedPtr _subscription;
        std::shared_ptr<rclcpp::Node> node;
    signals:
        void emitTopicData(QString);
    };

    rclcomm::rclcomm()
    {
        int argc=0;
        char **argv=NULL;
        rclcpp::init(argc,argv);
        node=rclcpp::Node::make_shared("ros2_qt_demo");
        _publisher = node->create_publisher<std_msgs::msg::Int32>("ros2_qt_dmeo_publish",10);
        _subscription = node->create_subscription<std_msgs::msg::Int32>("ros2_qt_dmeo_publish",10,std::bind(&rclcomm::recv_callback,this,std::placeholders::_1));
    }
    void rclcomm::run(){
        std_msgs::msg::Int32 pub_msg;
        pub_msg.data=0;
        rclcpp::WallRate loop_rate(1);
        while (rclcpp::ok()) {
        _publisher->publish(pub_msg);
        pub_msg.data++;
        rclcpp::spin_some(node);
        loop_rate.sleep();
        }
        rclcpp::shutdown();
    }
    void rclcomm::recv_callback(const std_msgs::msg::Int32::SharedPtr msg){
        qDebug()<<msg->data;
        emit emitTopicData("i am listen from topic:" +QString::fromStdString(std::to_string(msg->data)));
    }  

