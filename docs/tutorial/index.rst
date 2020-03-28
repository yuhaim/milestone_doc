案例教程
********

运行案例
========

在工具包根目录下执行"mkdir build" //建立单独的构建目录，名称任意，用于将临时文件与工具分开
在工具包根目录下执行"cd build" //切换到创建的构建目录
在创建的构建目录下执行"cmake .." //在构建目录下，指定代码目录在上层目录，生成编译工程文件（Windows下为MSVC sln，Linux下为Makefile）
在创建的构建目录下执行"cmake --build . " //执行构建，注意"."为当前目录，附加"--config Release"或"--config Debug"参数切换Release版和Debug版，默认为Debug版
所导出的fmu模型在export目录中
依次在不同系统下执行工具包，将model目录（保留已生成的中间文件）或整个工具包复制到其他系统继续构建，将获得同时支持多系统的fmu文件

代码结构剖析
============

.. toctree::
    
    interface
    controller
    plant
