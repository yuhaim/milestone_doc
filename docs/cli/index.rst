命令行操作
**************************

添加模型
========
复制model内部的模型目录结构（内部sources文件夹为必须），实现与模型文件夹同名称的.h及.cpp模型代码文件
若增加新的模型间接口，在model/interface.h中定义接口数据结构体
在顶层CMakeLists.txt中"foreach (MODEL_NAME controller plant plant_1) # add model to this list"语句处，将新的模型添加在列表中
重新执行上述构建操作，系统将执行增量构建

