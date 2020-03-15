S-Function接口及其实现
*************************

生成fmu的过程中，在模型的sources目录中也生成了支持Simulink导入的S-Function接口代码
若要生成模型的S-Function模块，在顶层SFcnLists.m中"model_list = {'controller', 'plant'}; % add model to this list"语句处，将新的模型添加在列表中
启动MATLAB，将工作路径切换至工具包根目录，运行SFcnLists.m脚本，将执行S-Function的代码生成和模块构建
保存获得的Simulink模型模块，以及工作空间中的数据总线定义，分发模型时还需要附加\*.mexw32/\*.mexw64二进制文件，以及模型所需的数据文件
