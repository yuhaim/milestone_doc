FMI接口及其实现
**********************

为避免污染程序目录结构，支持shadow构建，运行cmake，生成编译工程。
程序将完成结构多层次递归展开，fmu结构代码生成，xml描述文件生成。
程序完成fmu链接库构建，打包及测试流程。 如 :numref:`fig_flow` 。

.. _fig_flow:
.. figure:: milestone_flow.png
    :scale: 20%

    模型代码模板运行流程

