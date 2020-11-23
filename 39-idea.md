##idea
    概念：java的集成开发环境
    配置文件说明：bin目录里面idea.exe.vmoptions
                            -Xms150m：修改150的值，提升idea启动的效率
                            -Xmx500m：修改500的值，降低垃圾回收频率，也会提升性能

**创建工程
    在当前的src目录中，先创建package(come.atguigu.java)->在创建java.class文件

模块操作
    创建模块(modules)
        idea里面一个Project(项目)打开一个window窗口，一个项目下面可以分为很多的模块(module),模块之前相互依赖可以相互调用的
        创建模块步骤：new->Module->next->给当前的module起名字然后在当前的项目中生成模块，点开模块里的src->new->创建包package->创建java.class文件

    删除模块(modules)
        比如模块(module2)上右键->Open Module Settings->点击减号,然后返回module2->右键会显示delete选项->点击delete选项即可删除