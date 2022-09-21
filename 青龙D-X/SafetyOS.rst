
操作系统功能安全
===================================================


X9U AB
-------------------------------------------

领导指示
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* QNX开发和Linux开发团队要分开进行;
* 按照功能安全设定,把功能安全有关的部署到QNX中, 更安全一些, 比单独Linux 安全;最好AB面都是QNX系统;
* 如果其它供应商用TDA4+QNX,那一定比目前单Linux的方案更安全;有竞争对手就会很麻烦，这是方案差距；
* 大疆正在做TDA4+QNX移植,大疆已经充分认识到QNX在商业上的杀伤力;花钱就可以升级为QNX safety,设计到成本;百度用Linux;
* QNX在X9U B面 AP2部署一个QNX(杜), 有技术上博弈的机会,QNX冗余备份,安全方案可以说的通；
* 全QNX方案要论证一下; 开发上没有太大区别,有应用开发区别（管控），配置有区别；根据成本区别配置；
* 如果只有Linux,用户还同意增加成本,那目前单Linux方案无解;
* QNX提问,QM ASIL2区别;如何部署X9U等;
* 商业策略,不要因为Linux方便而留下软肋;如果被商业竞争对手利用则会发生重大损失；
* 功能安全,支持AUTOSAR是商业上的重要杀手;
* 风险要深挖，稳定性,接口定义，要重视问题的提出(切换时间8~10秒)，系统架构，平台化开发,关键技术升级，珍惜上升迭代机会;


参考方案
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
J5 Enhanced Linux

.. image:: /images/J5_Linux.png

QNX vs Linux

.. image:: /images/qnxLinux.png

QNX特征

* QNX(Quick UNIX) 是类UNIX系统, **微内核** ;
* QNX中的每个进程都是 **隔离** 的:驱动、栈、OS服务、应用;
* 所有进程都可以应用相同API:POSIX PSE54 ,C++11/14/17,简化嵌入式开发流程;
*  **进程间通信** 是QNX内核的核心功能;
* 用户可以通过开发定制化的应用程序增强系统功能；
* 用户程序与系统程序通过进程间通信进行协作构成一个有机的整体；
* 操作系统以一种 **扁平化** 的结构组织；
* 操作系统通过路径管理器等系统服务支持用户服务的 **动态加入** ;
  
Linux特征

* 宏内核，分层架构；
* 支持对称多处理(SMP)机制;
* 内核可以抢占，需要配置抢占式内核调度器(Preempt RT Scheduling);
* 成熟的生态环境，尤其是图形操作类库；
* 支持动态加载内核模块。


QNX资料
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. image:: /images/QNX功能安全与非功能安全区别.png
.. image:: /images/QNX功能安全产品全景.png
.. image:: /images/QNX座舱智驾一体化.png
.. image:: /images/QNX软件平台全景.png

X9U BSP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. image:: /images/X9U_BSP.png


X9U功能及硬件接口
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
X9U 功能分布（待更新）

.. image:: /images/X9U.png

SSA SSB硬件接口定义见下图

.. image:: /images/X9U硬件.png

X9U SSA

* 2 x CAN MRR Radar
* 3 x GPIO_TC397
* SPI_TC397
* SEM_FAULT
* DDR 4G
* EMMC 64G
* QSPI 8M Norflash
* AP1 RGMII ETH SW
* SMI ETH SW
* **Mipi CSI J5**
* **PCIE J5**
* **2 x GPIO J5**
* Power
* 2 x GPIO USS 
* 2 x SPI USS
* GPI PPS
* USB1
* JTAG
* Boot pin
* 3 x UART debug
* 2 x UART to SSA 

X9U SSB

* SPI_TC397
* 3 x GPIO_TC397
* SEM_FAULT
* AP1 RGMII ETH SW
* 3 x CAN MRR Radar
* DDR 4G
* EMMC 64G
* QSPI 8M Norflash
* GP1
* GP2
* **SPI IMU**
* **UART IMU**
* **3 x GPIO IMU**
* **UART F9K**
* GPI PPS
* USB1
* JTAG
* Boot pin
* 3 x UART debug
* 2 x UART to SSA 

QNX切换计划
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. image:: /images/总线通讯拓扑.png

* 切换时间 v0.8?
* 开发资源 ADAS? Neusar?
* 能力建设 多种配置方案 提高市场竞争力
* 操作系统算力评估,架构设计
* 驱动移植,硬件I/O匹配,确保极简输入输出,纯计算类；
* 算力负荷计算，功能分配不能到极限
* 功能安全
* 外设跑不起来
* QNX专人开发方案动起来,组织队伍,方案操作POC 概念设计；底层对一下；项目对接; 资源和时间；影响项目交付为主线；
* 要用公司的公共资源, Neusar,项目，公共资源;QNX资源

双操作系统配置方案 QNX vs Linux

.. image:: /images/OS配置.png
.. image:: /images/os配置计划.png


QNX 驱动移植
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
移植其他系统驱动程序到QNX操作系统,应遵循如下几个步骤：

* 从其他系统的驱动程序中提炼出对该设备的所有基础操作的函数代码。以串口驱动为例，如对串口的初始化操作、数据发送、数据接收、配置串口参数等操作的代码应该都是与操作系统无关的（唯一相关的地方可能就是将物理基地址映射成虚拟基地址）；
* 创建一个资源管理器的框架,设置路径和设备名,并在io_func中关联所有需要用到的IO操作函数;
* 通过IO操作函数,将前面的两部分关联到一起;
* 整个驱动程序中的操作，例如对接收到的命令行参数的解析处理等。



