示例教程
********

运行示例
========

在了解仿真模型实现的细节之前，可以使用工具包自带的测试用例验证环境部署的正确性，我们先在GUI中快速完成这些操作。

#. 运行MasterSimulatorUI.exe，启动MasterSim主界面，如 :numref:`fig_t_0` 。

.. _fig_t_0:
.. figure:: 0.png
    :scale: 50%

    MasterSim主界面

#. ，如 :numref:`fig_t_1` 。

.. _fig_t_1:
.. figure:: 1.png
    :scale: 50%

    Milestone主界面

#. ，如 :numref:`fig_t_2` 。

.. _fig_t_2:
.. figure:: 2.png
    :scale: 50%

    创建工程   

#. ，如 :numref:`fig_t_3` 。

.. _fig_t_3:
.. figure:: 3.png
    :scale: 50%

    选择模型

#. ，如 :numref:`fig_t_4` 。

.. _fig_t_4:
.. figure:: 4.png
    :scale: 50%

    完成编译工程创建      

#. ，如 :numref:`fig_t_5` 。

.. _fig_t_5:
.. figure:: 5.png
    :scale: 50%

    构建模型

#. ，如 :numref:`fig_t_6` 。

.. _fig_t_6:
.. figure:: 6.png
    :scale: 50%

    查看导出的FMU   

#. ，如 :numref:`fig_t_7` 。

.. _fig_t_7:
.. figure:: 7.png
    :scale: 50%

    打开MasterSim测试工程

#. ，如 :numref:`fig_t_8` 。

.. _fig_t_8:
.. figure:: 8.png
    :scale: 50%

    测试工程默认路径   

#. ，如 :numref:`fig_t_9` 。

.. _fig_t_9:
.. figure:: 9.png
    :scale: 50%

    MasterSim中的模型连接

#. ，如 :numref:`fig_t_10` 。

.. _fig_t_10:
.. figure:: 10.png
    :scale: 50%

    MasterSim中的仿真配置   

#. ，如 :numref:`fig_t_11` 。

.. _fig_t_11:
.. figure:: 11.png
    :scale: 50%

    MasterSim仿真过程监控

#. ，如 :numref:`fig_t_12` 。

.. _fig_t_12:
.. figure:: 12.png
    :scale: 50%

    MasterSim仿真结果路径      

#. ，如 :numref:`fig_t_13` 。

.. _fig_t_13:
.. figure:: 13.png
    :scale: 50%

    Excel中查看结果csv数据文件

#. ，如 :numref:`fig_t_14` 。

.. _fig_t_14:
.. figure:: 14.png
    :scale: 50%

    系统时域响应曲线   

示例模型说明
=============

控制器模型为PD测速反馈控制器，如 :eq:`eq_controller` 。

.. math::
    :label: eq_controller

    F=k_p\left( r_x-x \right) -k_dv

    
被控对象为简单的一维质量块模型，如 :eq:`eq_plant` 。

.. math::
    :label: eq_plant
    
    \left[ \begin{array}{c}
    \dot{x}\\
    \dot{v}\\
    \end{array} \right] =
    \left[ \begin{array}{c}
    v\\
    \frac{F}{m}\\
    \end{array} \right] 

所组成控制系统的原理框图如 :numref:`fig_sys_all` 所示。

.. _fig_sys_all:
.. figure:: sys_all.png
    :scale: 50%

    控制系统框图

系统的传递函数为 :eq:`eq_tf` 。

.. math::
    :label: eq_tf

    \frac{X\left(s\right)}{R\left(s\right)}= \frac{ k_p \frac{1}{s} \frac{\frac{1}{ms}}{1+k_d*\frac{1}{ms}} }{ 1 + k_p \frac{1}{s} \frac{\frac{1}{ms}}{1+k_d*\frac{1}{ms}} }
    = \frac{ \frac{k_p}{ms^2+k_ds} }{ 1 + \frac{k_p}{ms^2+k_ds} }
    = \frac{k_p}{ms^2 + k_ds + k_p}
    = \frac{1}{\frac{ms}{k_p}s^2 + \frac{k_d}{k_p}s + 1}

.. math::
    :label: eq_sys

    D\left(s\right) = \frac{ms}{k_p}s^2 + \frac{k_d}{k_p}s + 1=\frac{1}{\omega_n^2}s^2 + 2\frac{\zeta}{\omega_n}s + 1

可见该反馈控制系统为典型的二阶系统，其特征方程为 :eq:`eq_sys` ，
为使得系统的响应具有较明显的动态过程，选择系统参数使得阻尼比较小且振荡频率为 :math:`0.5 \mathrm{Hz}`
即取 :math:`\zeta=0.2, \omega_n=2\pi \times 0.5` ，则系统的反馈增益为 :math:`k_p = \omega_n^2 m, k_d = 2 \zeta \omega_n m` 。
对于这个简单的模型，当然没有必要分别使用不同的仿真工具对其不同部分进行建模，
但我们为了快速验证系统运行的正确性，可以以此作为测试用例，容易给出在Simulink中的仿真结果与设计的预期相一致，如 :numref:`fig_sys_res` 。

.. _fig_sys_res:
.. figure:: sys_res.png
    :scale: 50%

    预期的系统响应

接下来，将控制器和被控对象分别实现为两个模型，通过接口的连接实现该系统的仿真，则系统的接口关系为 :numref:`fig_sys_sep` 。

.. _fig_sys_sep:
.. figure:: sys_sep.png
    :scale: 50%

    系统的接口关系

代码结构剖析
============

.. toctree::
    
    interface
    controller
    plant
