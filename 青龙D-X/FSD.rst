FSD
===================================================================================================

多层ResNet
-----------------------------------------------------------------------------------------
多层ResNet,生成多层feature特征。底层是高分辨率/低通道，高层是低分辨率/多通道；多feature意味着可以生成高概率，高准确率的待决策项，取决于联系，取决于逻辑，取决于对危险情况的经验。

.. image:: /images/fsd1.png

面向特征数据流的开发模式
-----------------------------------------------------------------------------------------
* 我们需要大量的数据，来训练整个网络。面向数据流的架构是唯一选择。
* FPN(Feature Pyramid Networks) 特征金字塔

.. image:: /images/fsd2.png

深层网络 浅层架构
-----------------------------------------------------------------------------------------
* 深层网络是为了多层语义，提高识别效率，以数据为导向，硬件固化，不在软件层面实现，应用现有模型；
* 浅层架构是为了提高执行效率，保证功能安全；工业互联网，只有4层，不允许多层；


色彩空间
-----------------------------------------------------------------------------------------

.. image:: /images/fsd3.png

架构 autoware AD stack  Autoware.AI Autoware.AUto
-----------------------------------------------------------------------------------------

.. image:: /images/fsd4.png
.. image:: /images/fsd5.png
.. image:: /images/fsd6.png
.. image:: /images/fsd7.png
.. image:: /images/fsd8.png
.. image:: /images/fsd9.png
.. image:: /images/fsd10.png

* plan 上面的独立模块中还应该有产品模块定义，类似ACC，NDP
* 应该是独立的ODD运行设计域

.. image:: /images/fsd11.png
.. image:: /images/fsd12.png

.. image:: /images/fsd16.png
.. image:: /images/fsd17.png
.. image:: /images/fsd18.png
.. image:: /images/fsd19.png
.. image:: /images/fsd20.png


https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/


.. image:: /images/fsd21.png
.. image:: /images/fsd22.png
.. image:: /images/fsd23.png
.. image:: /images/fsd24.png
.. image:: /images/fsd25.png



https://discourse-cloud-file-uploads.s3.dualstack.us-west-2.amazonaws.com/business7/uploads/ros/original/2X/e/efa5f71e9551a73b8a76bb34c02522ccae502835.pdf


https://gitlab.com/autowarefoundation/autoware-foundation/-/wikis/ASWG-AVP-planning-minutes-20191210#autoware-avp-architecture


根据不同的安全等级进行SOC功能分配，397是最实时功能
-----------------------------------------------------------------------------------------

.. image:: /images/fsd13.png

TIER 4
-----------------------------------------------------------------------------------------

https://tier4.jp/en/products/


.. image:: /images/fsd14.png
.. image:: /images/fsd15.png

