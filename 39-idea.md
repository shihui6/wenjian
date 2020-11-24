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

常用设置
    设置鼠标悬停文本提示：
        File->settings->Editor->Appearance->找到选项show quick documentation on mouse move勾选上就可以了
    自动导入包
        File->settings->Editor->General->Auto import页面勾选insert imports on paste:All；Add unambiguous imports on the fly:自动导入不明确结构；Optimize imports on the fly:自动帮我们优化导入的包；这俩个都勾选上
    
    设置显示行号和方法间的分隔符
        File->settings->Editor->General->Apperance页面勾选出show Line numbers；show method separators
    
    忽略大小写提示
        File->settings->Editor->General->Code Complation页面将case sensitive completions勾选None
    
    设置取消单行显示tabs的操作(java文件多的时候，在显示栏换行显示不会隐藏了)
        File->settings->Editor->General->Editor Tabs页面，将show tabs in single row取消勾选即可
    
    设置默认的字体，字体大小，字体行间距
        File->settings->Editor->Font页面选即可
    
    修改代码中注释的字体颜色
        File->settings->Editor->Color Scheme->Language Defaults页，comments进行注释颜色的选择

    设置项目文件编码
        File->settings->Editor->File Encodings页；可以修改项目文件编码
    
    设置自动编译
        File->settings->Build,Execution,Deployment->Compiler页，勾选Build project automatically和Compole independent modules in parallel


快捷键设置
    import快捷键的jar包
        File->import file location然后导入快捷键的jar包即可(快捷键jar包在百度云盘里)
    
    快捷键的使用：
        1.执行(run)  alt+r
        2 提示补全      alt+/
        3 单行注释      ctrl+/
        4 多行注释      ctrl+shift+/
        5 向下复制一行  ctrl+alt+down
        6 删除一行或选中行  ctrl+d
        7 向下移动行    alt+down
        8 向上移动行    alt+up
        9 向下开始新的一行   shift+enter
        10 向上开始新的一行     ctrl+shift+enter
        11 如何查看源码     ctrl+选中指定的结构 或 ctrl+shift+t
        12 万能解错/生成返回值变量  alt+enter
        13 退回到前一个编辑的页面   alt+left
        14 进入到下一个编辑的页面   alt+right
        15 查看继承关系     F4

        16 格式化代码       ctrl+shift+F
        17 提示方法参数类型     ctrl+alt+/
        18 复制代码     ctrl+c
        19 撤销     ctrl+z
        20 反撤销   ctrl+y
        21 剪切     ctrl+x
        22 粘贴     ctrl+v
        23 保存     ctrl+s
        24 全选     ctrl+a
        25 选中行数，整体往后移动       tab
        26 选中行数，整体往前移动       shift+tab
        27 查看类的结构     ctrl+o
        28 重构：修改变量名与方法       alt+shift+r
        29 大写转小写/小写转大写        ctrl+shift+y
        30 生成构造/get/set/toString     alt+shift+s


        31 查看文档说明     F2
        32 收起所有的方法   alt+shift+c
        33 打开所有方法     alt+shift+x
        34 打开代码所在硬盘文件夹       ctrl+shift+x
        35 生成try-catch等      alt+shift+z
        36 局部变量抽取为成员变量   alt+shift+f
        37 查找/替换    ctrl+f
        38 查找(全局)   ctrl+h
        39 查找文件     快速双击shift
        40 查看类的继承结构图   ctrl+shift+u
        41 查看方法的多层重写结构       ctrl+alt+h
        42 添加到收藏       ctrl+alt+f
        43 抽取方法     alt+shift+m
        44 打开最近修改的文件   ctrl+E
        45 关闭当前打开的代码栏     ctrl+w
        46 关闭打开的所有代码栏     ctrl+shift+w
        47 快速搜索类中的错误       ctrl+shift+q
        48 查找方法在哪里被调用     ctrl+shift+h


代码模板
    File->settings->Editor->Live Templates/Postfix Completion(Live Templates可以修改模板，Postfix Completion不可修改模板)
    模板一：psvm
    模板二：sout
            变形：soutp/soutm/soutv/xxx.sout
    模板三：fori(for循环缩写的)
            变形：iter(增强for循环)
            变形：itar(普通for循环)
    模板四：list.for(遍历list数组增强的)
            变形list.fori(普通list的for循环从头到尾)
            变形list.forr(普通list的for循环从尾到头)
    模板五：ifn(判断是否为null)
            变形:inn(判断是否为null)
    模板六：prsf(可生成private static final)
            变形：psf(public static final)
            变形：psfi(public static final int ...)
            变形：psfs(public static final String ...)


在idea里面添加tomcat服务
    run->edit Configurations->Tomcat Server->Local页，进行操作：Application server:选择Tomcat文件导入;创建javaEE项目：new Module->web Application等即可，然后在local页的Deployment中导入要部署在tomcat上的项目

在idea中关联数据库

idea里面设置git

idea断点调试
    常用断点调试快捷键
        step over 进入下一步，如果当前行断点是一个方法，则不进入当前方法体内
        step into 进入下一步，如果当前行断点是一个方法，则进入当前方法体内
        force step int 进入下一步，如果当前行断点是一个方法，则进入当前方法体内
        step out 跳出
        resume program 恢复程序运行，但如果该断点下面代码还有断点则停在下一个断点上
        stop 停止
        mute breakpoints 点中，使得所有的断点失效
        view breakpoints 查看所有断点

    条件断点
        说明：调试的时候，在循环里增加条件判断，可以极大的提高效率
        具体操作：在断点处右击调出条件断点。可以在满足某个条件下，实施断点


idea中配置maven
    File->settings->Build,Execution,Deployment->Build Tools->Maven页，Maven home directory设置idea绑定的maven版本；User settings files配置文件(文件里面记录了很多重要的信息，如相应的依赖的文件下载的地址)；Local repository(本地仓库的设置)

    创建maven构建的模板：New Module->Spring Initializr-> next,next即可

其他设置
    生成javadoc文档
        功能：针对某个功能，文件，dome都可以生成javadoc文档
        步骤：Tools->Generate javaDoc Scope页，指名需要输出的内容，输出的位置；Locale:zh_CN(生成的语言设置成中文);Other command line arguments(设置编码集)：-encoding UTF-8 -charset UTF-8
    
    清除缓存操作

    idea插件下载
        步骤File->settings->Plugins页对相应插件进行下载，

