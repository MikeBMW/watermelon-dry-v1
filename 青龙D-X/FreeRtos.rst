FreeeRTOS & SAFERTOS
===================================================================================================

从开源FreeRTOS到功能安全SAFERTOS应用
-----------------------------------------------------------------------------------------

https://www.bilibili.com/video/BV1f44y1t7Fs/?vd_source=2f0ca3a155a84399b09f80867b125658

.. image:: /images/freertos1.png
.. image:: /images/freertos2.png
.. image:: /images/freertos3.png
.. image:: /images/freertos4.png

FreeRTOS-MPU(Memory Protection Unit)
-----------------------------------------------------------------------------------------
MPU(Memory Protection Unit，内存保护单元)在 Cortex-M内核中是可选模块，带MPU的微控制器允许内存映射（包括Flash、RAM和外围设备）细分为若干区域，分别给每个区域分配不同的访问权限。

FreeRTOS-MPU是FreeRTOS针对MPU实现的一个安全版本，支持ARMv7-M(Cortex-M3, Cortex-M4 和 Cortex-M7)和ARMv8-M (Cortex-M23和Cortex-M33) 内核的微控制器。

针对ARMv7-M的FreeRTOS移植存在两个版本，一个支持MPU，一个不支持。针对ARMv8-M只有一个移植版本，通过编译开关控制是否支持MPU。

FreeRTOS通过将任务分为特权和非特权运行模式和限制对RAM、外设、可执行代码、任务堆栈内存的访问，使得应用更健壮和安全。例如，防止代码从RAM中执行可以获得巨大的好处，因为这样做可以防止许多攻击向量，如缓冲区溢出漏洞或加载到RAM中的恶意代码的执行。

使用MPU必然会使应用程序设计更加复杂，首先必须确定MPU的内存区域限制并向RTOS进行描述，其次MPU限制应用程序任务可以做什么和不能做什么。

 
MPU的策略

创建一个将每个任务限制在其自己的内存区域的应用程序可能是最安全的，但它也是设计和实现最复杂的。通常最好使用一个MPU来创建一个伪进程和线程模型——允许线程组共享内存空间。例如，创建一个可被可信的第一方代码访问的内存空间，以及一个仅可被不可信的第三方代码访问的内存空间。

.. image:: /images/freertos5.png
.. image:: /images/freertos6.png
.. image:: /images/freertos7.png
.. image:: /images/freertos8.png
.. image:: /images/freertos9.png

FreeRTOS产品线
-----------------------------------------------------------------------------------------

* 功能安全版本 SAFETRTOS
* 开源中间件 FreeRTOS Plus
* 商业授权版本 OPENRTOS

.. image:: /images/freertos10.png
.. image:: /images/freertos11.png
.. image:: /images/freertos12.png
.. image:: /images/freertos13.png
.. image:: /images/freertos14.png
.. image:: /images/freertos15.png
