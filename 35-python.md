###python
    **安装
        地址：https://www.python.org/downloads/release/python-372/
        1：检查python解释器是否安装成功
            在cmd中输入python
        2：退出当前指令
            exit()
        3:进入指定版本:如进入python3的版本
            python3

    **解析器的作用和分类
        作用：运行文件或者运行代码用的
        原因：通过Python写出来的代码计算机直接读取看不懂，通过解析器解析成计算机可识别的代码
        1：Python分类
                Cpython，C语言开发的解释器(官方)，应用广泛的解释器(用的做多的解释器)
                IPython，基于CPython的一种交互式解释器
                其他解释器：
                    PyPy：基于Python，基于Python语言开发的解释器
                    Jython，运行在java平台的解释器，直接把Python代码编译成java字节码执行
                    IronPython，运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码

    **PyCharm
        1：概念：是一种Python IDE(集成开发环境)，带有一整套可以帮助用户在使用Python语言开发时提高其效率的工具
            补充：继承了调用和调试解释器的功能
        2：功能：(PyCharm内部集成的功能)
            Project管理
            智能提示
            语法高亮
            代码跳转
            调试代码
            解释代码(解释器)
            框架和库
            等等

            事例：在pyCharm里编写代码右键点击执行
                执行机制：点击执行，则pyCharm调用操作系统已经安装好的解释器去运行我们的py文件

        
        3：PythonCharm版本
            专业版和社区办，我们一般用社区版，专业版收费，一般的社区办就足够用了
    
        4：下载
            地址：https://www.jetbrains.com/pycharm/download/#section=windows
        
        5：基本使用(创建py文件)
            步骤：打开pyCharm->[create new project]->选择项目根目录和解释器版本->[create] 
        
        6：基本设置
            6-1:界面外观设置：
                6-1-1:修改主题：[file]--[Apperance&Behavior]--[Apperance]
                            Theme：修改工具主题颜色
                            Name：修改主题字体
                            Size：修改主题字号
                6-1-2:修改代码文字格式   [file]--[settings]--[Editor]--[Font]
                            Font：修改字体
                            Size：修改字号
                            Line Spacing：修改行间距
            6-2:修改解释器：[file]--[Settings]--[Project:项目名称]--[Project Interperter]--[设置图标]--[Add]--浏览到目标计时器--[ok]--[ok]

        7:项目管理
            7-1:步骤：[File]--[Open]--浏览器选择目标项目根目录--[ok]--选择打开项目方式
            7-2:打开项目
                This Window:覆盖当前项目，从而打开目标项目
                New Window:在新窗口打开，则打开两次PyCharm，每个PyCharm负责一个项目
                Attach：一个PyCharm中可以打开多个项目
            7-3:关闭项目：
                [file]--[Close Project]/[Close Projects in current window]

        8:注释
            8-1：作用：对代码进行解释说明，方便维护
            8-2：分类：多行注释和单行注释
                8-2-1：单行注释： #注释内容  快捷键ctrl+/
                8-2-2：多行注释：有两种写法

                    ```py
                        """
                            第一行注释
                            第二行注释
                            第三行注释
                        """
 
                        '''
                            注释一
                            注释二
                            注释三
                        '''
                    ```


    **语法学习
        1：变量：
            注意点：不能随便缩进，否则会报错
            1-1：概念：变量就是一个存储的当前数据所在的内存地址的名字
            1-2：读取变量机制：程序通过这个变量名字找到相应的变量对应的数据所在的内存区域
        
            1-3：定义变量
                语法：变量名 = 值
                变量名自定义，要满足标识符的命名规则
                1-3-1：标识符
                    标识符命名规则是Python中定义各种名字的时候的统一规范，具体如下
                        由数字，字母，下划线组成
                        不能数字开头
                        不能使用内置关键字
                        严格区分大小写
                
                1-3-2：命名习惯
                    见名知义
                    大驼峰：如MyName
                    小驼峰：如：myName
                    下划线：如：my_name
                
            1-4：使用变量

                ```py
                    # 定义变量
                    myName = 'Tom'
                    # 使用变量
                    print(myName)
                ```

        2：数据类型
            分类
                数值
                    整数(int)
                    浮点型(float)
                布尔型
                    True
                    False
                str(字符串)
                list(列表)
                tuple(元祖)
                set(集合)
                dict(字典)

            验证Python数据类型
                用法：通过type(数据)
                ```py
                    num = 1
                    type(num)
                ```

    **Debug工具
        概念：Debug工具是PyCharm IDE中集成的用来调试程序的工具，在这里程序员可以查看程序的执行细节和流程或者调解bug
        Debug工具使用步骤
            1：打断点
                断点位置：目标是要调试的代码块的第一行代码即可，即一个断点即可
                打断点的方法：点击目标代码的行号右侧空白位置
            2：Debug调试

    
    **输出
        概念：程序输出一种带格式的数据给用户
        1：格式化输出
            概念：格式化输出要用到的格式化符号
            格式化符号 %s：字符串 %d：有符号的十进制整数 %f：浮点数 %c：字符 %u：无符号十进制整数 %o：八进制整数 %x：十六进制整数等等
            用法：
                注意点：%s可以输出字符串，整数，浮点数
                事例：格式化输出数字

                ```py
                    age = 18
                    print('今年我的年龄是%d岁' % age)
                    # 执行结果为 今年我的年龄是18岁
                ```
                    事例执行机制：意思是用%来填充age这个变量，将age填充到%d的位置

                事例：格式化输出浮点数

                ```py
                    weight = 18.8
                    print('我的体重是%f.2公斤' % weight)
                    # 输出结果为 我的体重是18.80公斤
                ```
                    注意点：格式化输出浮点数时 %.2f 表示小数点后显示的小数位数

                事例：我的学号是001

                ```py
                    stud_id = 1
                    print('我的体重是%3d公斤' % stud_id)
                    # 输出结果为 我的体重是001
                ```
                    注意点：%03d，表示输出的整数显示位数，不足以0补全，超出当前位数则原样输出

                事例：同时输出不同的数据

                ```py
                    # 我的名字是x，今年x岁，体重x公斤
                    name = 'Tom'
                    age = 18
                    weight = 77.7
                    print('我的名字是%s，今年%d岁，体重%.2f公斤' % (name,age+1,weight))
                    # 输出结果 我的名字是Tom，明年年19岁，体重77.70公斤
                ```

        2：格式化输出%f





            




