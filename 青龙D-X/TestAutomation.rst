Test Automation
===================================================================================================
Standards for working with test systems. This includes APIs for programmatic access to sensor and actuator devices, Measurement and Calibration systems, HIL systems, DoE systems and formats for test descriptions.

iLinkRT
-----------------------------------------------------------------------------------------
官网： https://www.asam.net/standards/detail/ilinkrt/

The standard ASAM iLinkRT defines a protocol for **high-speed** measurement and calibration data **exchange** in test automation systems. The protocol is typically used to connect test automation systems and automated calibration systems (commonly called "MC-clients") with measurement & calibration servers ("MC-servers") and simulation systems, through which the client applications have **access to data** on the ECUs under test. The standard allows to setup a flexible and **high-performance test system** , where test applications and ECUs can be added or removed as needed without major integration efforts. Use cases with simultaneous use of multiple tools are now also supported.

ASAM iLinkRT uses an efficient protocol-based and event-triggered communication method. The transport layer is Ethernet (UDP/IP), which is the communication backbone in today's testing labs. MC-clients can establish **multiple point-to-point connections** with MC-servers, using standard IPv4 or IPv6 addresses and UDP ports. Each connection consists of two channels: one for sending commands, and one for data acquisition and event handling. The data acquisition channel uses an event driven DAQ mechanism, similar to ASAM MCD-1 XCP, which ensures that the measurement data is transferred with minimum delay. Multi-casting is supported to achieve higher throughput when multiple clients are connected to one server. Calibration data can be up- and downloaded via the command channel. Measurement data objects and calibration data objects are supported, as known from ASAM MCD-2 MC. The data exchange can be configured for physical or hex value data transfer.

Beside measuring and adjusting, the standard is now also supporting recording as well as MC-server configuration and parameterization. The behavior for usage in multi-client multi-server environments is defined. This allows the selection of the required measurement sequences together with a raster reference by each MC-client as well as the complete reconfigurations or query of existing configurations. The MC-Server informs all MC-Clients about status changes in form of events. The support of pre-configurations is standardized. This enables the client application to operate with simple command sequences. State models and sequence diagrams for typical use cases were introduced to facilitate the understanding of the standard content.

ASAM iLinkRT is easy to understand, implementation-independent and, with the release of V3.0.0, independent of ASAP3: The contents of all supplementary ASAP3 commands have been transferred to iLinkRT. Due to the integrated configuration options for multi-client use cases, additional parallel protocols are no longer necessary. Therefore, ASAM iLinkRT will replace ASAP3 from now on.

AVL 实例
-----------------------------------------------------------------------------------------

问题: How to do this faster?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* 30000 labels in a modern ECU
* more than 200 ECU channels are measured continuously on the testbed
* more than 10 parallel modified maps during an automated procedure
* 300 ms to set a map with ASAP3

答案是, 使用商用或睿驰自研的符合 iLinkRT 标准的工具。

iLinkRTwas developed jointly by AVL List GmbH and ETAS GmbH specificallyfor the application case of a fast ECU access with INCA-MCE. iLinkRTprietary, open and performance-optimized protocol. It is based on the automo-tive standard XCP-on-Ethernet, whereby only commands for the measurementand calibration privilege are used.TMiLinkRTallows fast measurement and calibration privileges as well as simpleimplementation for migrating existing test benches up to INCA-MCE.

.. image:: /images/iLinkRT_1.png
.. image:: /images/iLinkRT_2.png

ETAS ETK / FETK / XETK ECU Interfaces 实例
-----------------------------------------------------------------------------------------

.. image:: /images/INCA_ETK_1.png

FETK 能同时支持 8万个 label

.. image:: /images/INCA_FETK_2.png
.. image:: /images/INCA_FETK_3.png
.. image:: /images/INCA_FETK_4.png
.. image:: /images/INCA_FETK_5.png
.. image:: /images/INCA_FETK_6.png
.. image:: /images/INCA_FETK_7.png
.. image:: /images/INCA_FETK_8.png

ASAM 标准
-----------------------------------------------------------------------------------------
.. image:: /images/iLinkRT_3.png

https://www.asam.net/index.php?eID=dumpFile&t=f&f=5046&token=7ae2818f11157c9b061b2756b8b22ef934a50081

XCP协议
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /images/XCP主从节点.png
.. image:: /images/XCP_Drive.png

决策项
-----------------------------------------------------------------------------------------

#. NeuSAR 开发 高性能标定协议;  -> NeuSAR负责(待定)
#.  移植岚图TDA4 XCP驱动到BYD X9U上, v0.7组入 11月30日

Aurora Gigabit Trace (AGBT) support
-----------------------------------------------------------------------------------------
In depth real-time debugging requires close interaction with the processor. Tracing shall provide a chronological picture of a system's inner workings - before or after a critical event - mainly to help analyzing a faulty program.

http://www.dism.co.kr/2016/ETAS/newsletter/02/images/Measure_all_an_big_data_analytics.pdf

.. image:: /images/INCA_FETK_9.png
.. image:: /images/INCA_FETK_10.png
.. image:: /images/INCA_FETK_11.png
.. image:: /images/INCA_FETK_12.png
.. image:: /images/INCA_FETK_15.png

AURIX™ TC39x Emulation devices
-----------------------------------------------------------------------------------------
Debug, Trace, Measure and Calibrate
Aurora Gigabit Trace (AGBT) support

.. image:: /images/INCA_FETK_13.png

ARM  Embedded Trace Buffer
-----------------------------------------------------------------------------------------

https://developer.arm.com/documentation/ddi0242/b/introduction/about-the-embedded-trace-buffer

Providing an on-chip buffer enables the trace data generated by the ETM (at the system clock rate) to be read by the debugger at a reduced clock rate. This removes the requirement for high-speed pads for the trace data.

This buffered data can also be accessed through an AHB slave-based memory-mapped peripheral included as part of the ETB. This enables software running on the processor to read the trace data generated by the ETM.

.. image:: /images/INCA_FETK_14.png