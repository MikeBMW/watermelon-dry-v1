Test Automation
===================================================================================================
Standards for working with test systems. This includes APIs for programmatic access to sensor and actuator devices, Measurement and Calibration systems, HIL systems, DoE systems and formats for test descriptions.

iLinkRT
-----------------------------------------------------------------------------------------
官网： https://www.asam.net/standards/detail/ilinkrt/

The standard ASAM iLinkRT defines a protocol for high-speed measurement and calibration data exchange in test automation systems. The protocol is typically used to connect test automation systems and automated calibration systems (commonly called "MC-clients") with measurement & calibration servers ("MC-servers") and simulation systems, through which the client applications have access to data on the ECUs under test. The standard allows to setup a flexible and high-performance test system, where test applications and ECUs can be added or removed as needed without major integration efforts. Use cases with simultaneous use of multiple tools are now also supported.

ASAM iLinkRT uses an efficient protocol-based and event-triggered communication method. The transport layer is Ethernet (UDP/IP), which is the communication backbone in today's testing labs. MC-clients can establish multiple point-to-point connections with MC-servers, using standard IPv4 or IPv6 addresses and UDP ports. Each connection consists of two channels: one for sending commands, and one for data acquisition and event handling. The data acquisition channel uses an event driven DAQ mechanism, similar to ASAM MCD-1 XCP, which ensures that the measurement data is transferred with minimum delay. Multi-casting is supported to achieve higher throughput when multiple clients are connected to one server. Calibration data can be up- and downloaded via the command channel. Measurement data objects and calibration data objects are supported, as known from ASAM MCD-2 MC. The data exchange can be configured for physical or hex value data transfer.

Beside measuring and adjusting, the standard is now also supporting recording as well as MC-server configuration and parameterization. The behavior for usage in multi-client multi-server environments is defined. This allows the selection of the required measurement sequences together with a raster reference by each MC-client as well as the complete reconfigurations or query of existing configurations. The MC-Server informs all MC-Clients about status changes in form of events. The support of pre-configurations is standardized. This enables the client application to operate with simple command sequences. State models and sequence diagrams for typical use cases were introduced to facilitate the understanding of the standard content.

ASAM iLinkRT is easy to understand, implementation-independent and, with the release of V3.0.0, independent of ASAP3: The contents of all supplementary ASAP3 commands have been transferred to iLinkRT. Due to the integrated configuration options for multi-client use cases, additional parallel protocols are no longer necessary. Therefore, ASAM iLinkRT will replace ASAP3 from now on.

AVL 实例
-----------------------------------------------------------------------------------------