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

    **PyCharm交互式开发环境
        在PyCharm中的Python Console，类似于js的浏览器的控制台


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

        2：格式化输出f'{表达式}'
            概念：格式化字符串除了%s，还可以写成f'{表达式}'
            注意点f'{表达式}'是Python3.6中新增的格式化方法

            ```py
                # 用%s做对比
                name = 'TOM'
                print('我的名字是%s' % name)

                # 用f'{}'表达式
                print(f'我的名字是{name}')
            ```

        4：转义字符
            概念：\n：换行功能；
                 \t：制表符功能，一个tab键(4个空格)的距离
            
            事例：
            ```py
                print('hello\nworld')
                # 输出结果 是换行的
                        # hello
                        # world
            ```

        5：print的结束符
            5-1：概念：print的结束符能够格式化输出的不同的展示形式
            5-2：作用：为什么两个print输出是换行输出？原因是：在Python中，print()，默认自带end='\n'这个换行结束符，所以导致每两个print直接会换行展示，用户可以按需求更改结束符
            5-3：规律：在遇到\n的换行符只会在此内容后面换行，前面不换行
            5-4：用法：
                事例：
                ```py
                    print('输出内容',end='\n')
                    print('输出内容',end='....')
                ```

    **输入
        输入语法：input('输入内容')
        输入特点：
            当程序执行到input，等待用户输入，输入完成之后才继续向下执行
            在Python中，input接收用户输入后，一般存储到变量，方便使用
            在Python中，input会把接收到的任意用户输入的数据都当做字符串处理
        用法：事例：
                ```py
                    password = input('请输入您的密码：')
                    print(f'你输入的密码是{password}')
                    type(password)
                ```
                    事例执行机制：程序执行到input时(程序就等待用户输入，当用户执行操作之后，程序才会向下执行)，当用户输入的密码为1111时，此时密码1111会被赋值给password变量，且用type检测的类型是字符串类型

    **基础语法
        1：类型转换
            转换数据类型的作用或必要性
                如果我们想要将input输入的内容(因为input输入的类型都是字符串类型)转换成整型该如何操作
            
            转换数据类型的方法(使用转换数据类型函数即可)
                函数：
                    int(x,[,base])  将x转换为一个整数
                    float(x) 将x转换为一个浮点数
                    str(x) 将对象x转换为字符串
                    eval(str) 用来计算在字符串中的有效Python表达式(字符串里面原本的数据类型)，并返回一个对象
                    tuple(s) 将序列s转换为一个元祖
                    list(s) 将序列s转换为一个列表

                事例：
                ```py
                    str2 = '1.1'
                    eval(str2) #输出结果为1.1浮点数类型
                ```

        2：运算符
            2-1：运算符的分类
                算数运算符
                赋值运算符
                复合赋值运算符
                比较运算符
                逻辑运算符
            
            2-2：算数运算符
                +
                -
                *
                /
                //  整除    9//4输出结果为2
                %   取余    9%4S输出结果为1
                **
                ()
                注意点：混合运算优先级顺序:()高于**高于*高于/ // % 高于 + -

            2-3：赋值运算符(也就是=号)
                2-3-1：单个变量赋值
                    事例：num = 1
                2-3-2：多个变量赋值
                    事例：
                    ```py
                        num1,float1,str1 = 1,1.2,'hello world'
                        print(num1) #输出结果为1
                        print(float1) #输出结果为1.2
                        print(str1) #输出结果为hello world
                    ```
                    注意点：左边变量的个数和右边数据的个数要一一对应

                2-3-3：多变量赋相同的值
                    a = b = 10

            2-4：复合赋值运算符
                +=  加法赋值运算符  c+=a等价于c = c+a
                -=  减法赋值运算符  c-=a等价于c = c-a
                *=  乘法赋值运算符  c*=a等价于c = c*a
                /=  除法赋值运算符  c/=a等价于c = c/a
                //= 整除赋值运算符  c//=a等价于c = c//a
                %=  取余赋值运算符  c%=a等价于c = c%a
                **= 幂赋值运算符  c**=a等价于c = c**a
                注意点：如果复合赋值运算符右边还有表达式，则解析器先算复合赋值运算符右边的表达式，再进行复合赋值运算

            2-5：比较运算符(和js里一样，返回的都是boolean值)
                    概念：一般用来做判断
                    ==
                    !=
                    >
                    >=

            2-6：逻辑运算符
                and     x and y  布尔'与'：如果x为false，x and y 返回 False，否则返回y的值      True and false，返回False
                or      x or y  布尔'或'：如果x为True，它返回 True，否则返回y的值      True or false，返回True
                not     x not y  布尔'非'：如果x为True，返回 False，如果x为False，它返回True     not false，返回True，not True，返回False

                注意点：逻辑运算符两边进行算数运算符最好加上()提升优先级

            2-7：拓展
                概念：数字之间的逻辑运算
                语法：
                    and运算符，只要有一个值为0，则结果为0，否则结果为最后一个非0数字
                    or运算符，只有所有值为0结果才为0，否则结果为第一个非0数字
                    事例：

                    ```py
                        print(0 and 1) #输出结果为0
                        print(2 and 1) #输出结果为1
                        print(2 or 1) #输出结果为2
                        print(0 or 1) #输出结果为1
                    ```

        3：条件语句
            3-1：语法：
                if 条件：
                    条件成立执行的代码1
                    条件成立执行的代码2

                if 条件：
                    条件成立执行的代码1
                    条件成立执行的代码2
                else：
                    条件不成立执行的代码1
                    条件不成立执行的代码2


            3-2：多重判断
                语法：
                    if 条件1:
                        条件1成立执行的代码1
                    elif 条件2:
                        条件2成立执行的代码2
                    ...
                    ...
                    else:
                        以上条件都不成立执行执行的代码

            3-3：注意点：扩展：age >=18 and age <= 60可以简化为 18<=age<=60


            3-4：if嵌套
                语法：
                    if 条件1：
                        条件1成立执行的代码
                        if 条件2：
                            条件2成立执行的代码

            3-5：三目运算符
                概念：三目运算符也叫三元运算符或三元表达式
                语法：条件成立执行的表达式 if 条件 else 条件不成立执行的表达式
                用法：
                    事例
                    ```py
                        a = 1
                        b = 2
                        c = a if a > b else b
                        print(c)
                    ```
                        事例执行机制：三元运算符先执行if后面的条件，条件成立将if前面的a(表达式)赋值给c，条件不成立则将else后面的b赋值给c(表达式)


    **基础语法循环
        循环的作用：让代码更高效的重复执行
        循环分类：在Python中，循环分为while和for两种，最终实现效果相同
        注意点：只要在循环缩进里，就属于这个循环
        死循环：进入死循环时单击PyCharm右侧的红色按钮停止程序停止程序执行

        1:while循环
            语法：
                while 条件：
                        条件成立重复执行的代码1
                        条件成立重复执行的代码2
                        .....
                        .....

            用法：事例

                    ```py
                        i = 0
                        while i < 5:
                            print('python真好')
                            i +=1
                        
                        print('哇哦')
                        # 在while循环体中输出结果是打印五次 python真好
                    ```
                        事例执行机制：执行完while循环体之后，才会执行循环体外部的代码print('哇哦')

                事例：
                    方法一：用条件判断进行计算1-100偶数累加和

                    ```py
                        # 思路：
                        # 1：准备累加的数字 开始1 结束100 增量1
                        # 2：准备保存结果的变量result
                        # 3：循环加法运算 -- 如果是偶数才加法运算 -- 和21取余为0
                        # 4：输出结果
                        i = 0
                        while i <=100:
                            if i%2==0:
                                result +=i
                            i+=1
                        print(result)
                    ```

                    方法二：计数控制器控制累加
                    ```py
                        i = 0
                        while i <= 100:
                            result +=i
                            i +=2
                        print(result)
                    ```

        2：break和continue
            概念：break和continue是循环中满足一定条件退出循环的两种不同方式，也是专门在循环中使用的关键字
            理解：
                break：是终止此循环
                continue：退出当前一次循环继而执行下一次循环代码

        3：while循环嵌套
            语法：while循环里面再有一个while循环

            while循环嵌套执行机制：先由父循环到自循环，把所有的子循环执行完了，才会到父循环那里去，如果父循环条件再成立，还会进入子循环，以此类推，父循环条件不满足了，才会退出整个循环

                事例：打印出正方形的星号
                ```py
                    j = 0
                    while j <= 4:
                        i = 0
                        while i <=4:
                            print('*',end="")
                            i+=1
                        print() #每行结束要换行，这里借助一个空的print，利用print默认结束符换行
                        j+=1
                ```

                事例：99乘法表
                ```py
                    j = 1
                    while j <=6:
                        i = 1
                        while i <=j:
                            print(f'{i}*{j}={i*j}',end='\t')
                            i+=1
                        print()
                        j+=1
                ```

        4:for循环
            语法：
                for 临时变量 in 序列:    (序列相当于js中的数组，字符串，对象等等，可以被遍历的)
                    重复执行的代码1
                    重复执行的代码2
                    .....
                    .....

                事例：
                    ```py
                        str1 = 'woshishhui'
                        for i in str1:
                            print(i)
                    ```

            4-1:for循环配合break和continue
                语法：
                    break在for和while循环中都是退出整个循环
                    continue在for和while循环中都是终止当前循环，但是会继续会执行下一次循环

        5：else
            概念：循环可以和else配合使用，else下方缩进的代码指的是当循环正常结束之后要执行的代码

            5-1：while...else
                语法：
                    while 条件：
                        条件成立重复执行的代码
                    else：
                        循环正常结束之后要执行的代码

            5-2：for...else
                语法：
                    for 临时变量 in 序列：
                            重复执行的代码
                    else：
                        循环正常结束之后执行的代码
            
            5-3：break和continue对else下方代码的影响
                概念：
                    break的情况：else指的是循环正常结束之后要执行的代码，即如果是break终止循环的情况，else下方的缩进代码将不执行
                    continue的情况：continue是退出当前一次循环，继续下次循环，所以该循环在continue控制下是可以正常结束的，当循环结束后，则执行了else缩进的代码

        
    **字符串
        1：认识字符串
            1-1：字符串特征
                    一对引号字符串(单引号，双引号)
                        ```py
                            name1 = 'Tom'
                            name2 = "Rose"
                        ```
                    三引号字符串
                        ```py
                            name3 = '''Tom'''

                            name4 = """Rose"""

                            a = '''i am Tom,
                                    nice to meet you'''

                            b = """i am Tom,
                                    nice to meet you"""
                        ```
                    注意点：三引号和三引号注释符号的区别是：三引号字符串是=后面跟着，三引号注释是直接书写三引号

        2：下标(js里面的字符串操作一样)
            概念：'下标'又叫'索引'
            用法：
                事例(通过索引获取去字符串中对应的字符)
                ```py
                    name = "abcdefg"
                    print(name[0]) #输出a
                    print(name[1]) #输出b
                    print(name[2]) #输出c
                ```

        3：切片(相当于js里面的字符串截取)
            语法：序列[开始位置的下标:结束位置的下标:步长]
                    参数解释：
                        不包含结束位置下标对应的数据，正负数均可
                        步长是选取间隔，正负整数均可，默认步长为1

                    事例：

                    ```py
                        str1 = 'abcdef'
                        print(str1[2:5:1]) #cde
                        print(str1[2:5:2]) #ce
                        print(str1[2:5]) #cde
                        print(str1[:5]) #abcde  --如果不写开始，默认从0开始选取
                        print(str1[2:]) #cdef   --如果不写结束，表示选取到最后
                        print(str1[:]) #abcdef  --如果不写开始和结束，表示选取所有

                        # 负数测试
                        print(str1[::-1]) #fedcba   --如果步长为负数，表示倒叙选取
                        print(str1[-4:-1]) #cde  --下标-1表示最后一个数据，依次向前类推
                        print(str1[-4:-1:-1]) #不能选取出数据：从-4开始到-1结束，选取方向为从左到右，但是-1步长是从右到左选取，所以选取不到数据
                    ```

        4：常用操作方法
            概念：字符串的常用操作方法：查找，修改和判断三大类

            4-1：查找
                概念：字符串查找方法即是查找子串在字符串中的位置或出现的次数
                4-1-1：find()
                        概念：检测某个子串是否包含在这个字符串中，如果在返回这个子串开始位置的下标，否则返回-1
                        语法：字符串序列.find(子串,开始位置下标,结束位置下标)
                        注意点：开始和结束下标可以省略，表示在整个字符串序列中查找
                4-1-2：index()
                        概念：检测某个子串是否包含在这个字符串中，如果在返回这个子串开始位置的下标，否则报错
                        语法：字符串序列.index(子串,开始位置下标,结束位置下标)
                        注意点：开始和结束下标可以省略，表示在整个字符串序列中查找
                4-1-3:count()
                        概念：返回某个子串在字符串中出现的次数
                        语法：字符串序列.count(子串,开始位置下标,结束位置下标)
                        注意点：开始和结束下标可以省略，表示在整个字符串序列中查找
                4-1-4:rfind()
                        概念：和find()功能相同，但查找方向为右侧开始
                4-1-5:rindex()
                        概念：和index()功能相同，但查找放下为右侧开始
                事例：

                    ```py
                        mystr = 'hello world and wo and shi and Python'
                        # find()
                        mystr.find('and') #12
                        mystr.find('and',15,30) #23
                        mystr.find('ands') #-1,ands子串不存在

                        # index()
                        mystr.index('and') #12
                        mystr.index('and',15,20) #23
                        mystr.index('and') #如果index查找子串不存在，会报错

                        # count()
                        mystr.count('and',15,30) #2
                        mystr.count('and') #3
                        mystr.count('ands') #0
                    ```

            4-2:修改
                概念：通过函数的形式修改字符串中的数据 
                4-2-1:replace()
                        概念：替换
                        语法：字符串序列.replace(旧子串,新子串,替换次数)
                                返回值是修改后的字符串，不会改变原有字符串
                        注意点：替换次数如果不写，则替换次数为该子串出现次数

                        ```py
                            mystr = 'hello world and wo and shi and Python'
                            new_str = mystr.replace('and','he')
                            print(new_str) #输出结果为 'hello world he wo he shi he Python'
                        ```
                4-2-2：split()  (跟js的字符串方法split()一样)
                        概念：字符串分割
                        语法：字符串序列.split(分割字符,次数)
                            参数说明：不加参数二(次数)，则全部分割
                        事例：

                        ```py
                            mystr = 'hello world and wo and shi and Python'
                            list = mystr.split('and')
                            print(list) #输出结果Wie ['hello world','wo','shi','Python']
                        ```
                4-2-3：join()  (与js方法不一样)
                        概念：用一个字符或子串合并字符串，即是将多个字符串合并为一个新的字符串
                        语法：字符或子串.join(多字符传组成的序列)
                        事例：

                        ```py
                            list = ['aa','bb','cc']
                            new_str = '-'.join(list)
                            print(new_str) #输出结果为aa-bb-cc
                        ```

                4-2-4：大小写转换修改
                    4-2-4-1：capitalize()
                                    概念：将字符串第一个字符转换成大写
                                    注意点：capitalize()函数转换后，只有字符串第一个字符变成大写，其他字符全变成小写
                    4-2-4-2：title()
                                    概念：将字符串每个单词首字母转换成大写
                    4-2-4-3:lower()
                                    概念：将字符串中大写转小写
                    4-2-4-4:upper()
                                    概念：将字符串中小写转大写

                4-2-5：删除字符串中的空白符
                    4-2-5-1：lstrip()
                                    概念：删除字符串左侧空白字符
                    4-2-5-2：rstrip()
                                    概念：删除字符串右侧空白字符
                    4-2-5-3：strip()
                                    概念：删除字符串俩侧空白字符

                4-2-6：设置字符串左中右对齐
                    4-2-6-1：ljust()
                                概念：返回一个原字符串左对齐，并使用指定字符(默认空格)填充至对应长度的新字符串
                                语法：字符串序列.ljust(长度,填充字符)
                                事例：

                                ```py
                                    str = 'hello'
                                    hello.ljust(10,'.')
                                    # 返回结果为 hello.....   
                                ```
                                    事例解释：设置返回字符长度为10，不足长度10的部分用.号填充

                    4-2-6-2：rjust()
                                概念：返回一个原字符串右对齐，并使用指定字符(默认空格)填充对应长度的新字符串，语法和ljust()相同
                    4-2-6-3：center()
                                概念：返回一个原字符串居中对齐，并使用指定字符(默认空格)天聪至对应长度的新字符串，语法和ljust()相同

                4-2-7：判断
                        概念：所谓判断即是判断真假，返回的结果是布尔数据类型：True或False
                        4-2-7-1：startswith()
                                    概念：检查字符串是否是以指定子串开头，是则返回True，否则返回False。如果设置开始和结束位置下标，则在指定范围内检查
                                    语法：字符串序列.startswith(子串,开始位置下标,结束位置下标)
                        4-2-7-2：endswith()
                                    概念：检查字符串是否以指定子串结尾，是则返回True，否则返回False。如果设置开始和结束位置下标，则在指定范围内检查
                                    语法：字符串序列.endswith(子串,开始位置下标,结束位置下标)


                4-2-8：其他方法
                        4-2-8-1:isalpha()
                                    概念：如果字符串中所有字符都是字母则返回True，否则返回False
                        4-2-8-2：isdigit()
                                    概念：如果字符串只包含数字则返回True否则返回False
                        4-2-8-3：isalnum()
                                    概念：如果字符串至少有一个字符并且所有字符都是字母或数字则返回True，否则返回False
                        4-2-8-4：isspace()
                                    概念：如果字符串中只包含空白，则返回True，否则返回False
        

    **列表
        概念：列表的常用操作：增，删，改，查
        1：查
            用法：通过下标或函数去查找
                事例：(通过下标查找)
                    ```py
                        str = ['Tom','rose','jack']
                        str[0] #返回结果为Tom
                        str[1] #返回结果为rose
                    ```
                
            1-1:函数查找
                index()
                    概念：返回指定数据所在的位置
                    语法：列表序列.index(数据,开始位置下标,结束位置下标)
                    注意点：如果查找的数据不存在则报错
                count()
                    概念：统计指定数据在当前列表中出现的次数
                    用法：事例
                    ```py
                        name_list = ['Tom','Rose','jack']
                        name_list.count('jack')  #返回结果为1
                    ```
                len()
                    概念：访问列表长度，即列表中数据的个数
                    用法：事例
                    ```py
                        name_list = ['Tom','Rose','jack']
                        len(name_list) #返回结果3
                    ```

            1-2：判断是否存在
                1-2-1：in:判断指定数据在某个序列序列，如果在返回True,否则返回False
                    用法：事例
                        ```py
                            name_list = ['Tom','Rose','jack']
                            'jack' in name_list  #返回结果为True
                        ```
                1-2-2：not in ：判断指定数据不再某个列表序列，如果不在返回True，否则返回False

        2：增
            概念：增加指定数据到列表中
            2-1：append()
                    概念：列表结尾追加数据(改变原来列表)(和js里面的append操作数组是一样的) 
                    语法：列表序列.append(数据)
                    用法：事例
                    ```py
                        name_list = ['Tom','jack','rose']
                        name_list.append('shihui')
                        print(name_list)  #输出结果为['Tom','jack','rose','shihui']
                        print(name_list.append(['xiaoming','xiaohong']))  #输出结果为 ['Tom','jack','rose',['xiaoming','xiaohong']]
                    ```
                    注意点：如果append()追加的数据是一个序列，则追加整个序列到列表中

            2-2：extend()
                    概念：列表结尾追加数据，如果数据是一个序列，则将这个序列的数据逐一添加到列表
                    语法：列表序列.extend(数据)
                    用法：事例
                    ```py
                        name_list = ['Tom','jack','rose']
                        name_list.extend('shi')     #输出结果为['Tom','jack','rose','s','h','i']
                        name_list.exntend(['shi','hui'])  #输出结果为['Tom','jack','rose','shi','hui']
                    ```
                    注意点：extend()追加数据是一个序列，把数据序列里面的数据拆开然后逐一追加到列表的结尾

            2-3：insert()
                    概念：向指定位置新增数据
                    语法：列表序列.insert(位置下标,数据)
                    用法：事例
                    ```py
                        name_list = ['Tom','jack','rose']
                        name_list.insert(1,'aa')  #输出结尾为 ['Tom','aa','jack','rose']
                    ```


        3:删除
            3-1：del
                    概念：可以删除列表序列，也可以删除列表中指定项
                    语法：
                        del 目标
                        del(目标)
                    用法：事例：
                        ```py
                            name_list = ['Tom','jack','rose']
                            del name_list[0] #删除列表的第一项
                            del name_list  #删除整个列表
                        ```

            3-2：pop()
                    概念：删除指定下标的数据(默认为最后一个),并返回被删除的数据
                    语法：列表序列.pop(下标)
                
            3-3：remove()
                    概念：删除列表中指定数据
                    语法：列表序列.remove(数据)

            3-4：clear()
                    概念：清空列表  
                    语法：列表序列.clear()

        4：修改
            4-1：修改指定下标数据
                    用法：实例
                    ```py
                        name_list = ['Tom','jack','rose']
                        name_list[0] = 'shi'  #输出结果为 ['shi','jack','rose']
                    ```
            4-2：reverse()
                    概念：逆置列表数据
            4-3：sort()
                    概念：可以是升序或者是降序的排序
                    语法：列表序列.sort(key=None,reverse=False)
                    注意点：reverse表示排序规则，reverse=True降序,reverse=False升序(默认)

        5：复制
            5-1:copy()
                    概念：复制列表数据

    **列表的循环遍历
        1：while循环
        2：for循环
            for...in....

    **列表嵌套
        概念：所谓的列表嵌套指的就是一个列表里面包含了其他的字列表
        用法：
            事例：应用场景：要存储班级一，二，三个班级学生姓名，且每个班级的学生姓名在一个列表
            ```py
                name_list = [['Tom','Lily','Rose'],['张三','lisi'],['xiaoming','xiaohong','xiaolv']]
                print(name_list[0][1])  #输出 Lily
            ```

    **元祖
        概念：一个元祖可以存储多个数据，元祖内的数据是不能修改的
        1:定义元祖
            概念：定义元祖使用小括号，且逗号隔开各个数据，数据可以是不同的数据类型，但最好是相同的数据类型
            用法：
                ```py
                    # 多个数据元祖
                    t1 = (10,20,30)
                    # 单个数据元祖
                    t2 = (10,)
                ```
            注意点：如果定义的元祖只有一个数据，那么这个数据后面也要添加逗号，否则数据类型为唯一的这个数据的数据类型
                事例：
                    ```py
                        t2 = (10,)
                        print(type(t2)) #输出结果为 tuple
                        t3 = (10)
                        print(type(t3)) #输出结果为 int
                    ```

        2：元祖的常见操作
            概念：元祖里的直接数据不支持修改，只支持查找
            2-1：按下标查找数据
                ```py
                    t1 = (10,22,13)
                    print(t1[0]) #输出 10
                ```
            2-2：index()
                    概念：查找某个数据，如果数据存在返回对应的下标，否则报错，语法和列表，字符串的index方法相同
            2-3：count()
                    概念：统计某个数据在当前元祖出现的次数
            2-4：len()
                    概念：统计元祖中数据的个数

            2-5：注意：
                2-5-1：元祖内的直接数据如果修改则立即报错
                2-5-2：但是如果元祖里面有列表，修改列表里面的数据则是支持的 (但是在运用中元祖里面的数据还是不要去修改)
                  事例：
                    ```py
                        t1 = ('aa','bb','cc')
                        t1[0] = 'aaaa'  #这里会报错

                        t2 = ('aa','bb','cc',[10,20,30],'dd')
                        t2[3][0] = 'iiiii' #可以修改元祖里列表的数据
                    ```

    **字典
        概念：字段相当于js里的对象，字典里的数据是以键值对的形式存在的，字典数据和数据顺序没有关系，即字典不支持下标后期无论数据如何变化，只需要按照对应的键的名字查找数据即可
        特点：
            符号为大括号
            数据以键值对形式出现
            各个键值对之间用逗号隔开
        字典的类型：dict
        用法：
            事例创建空字典的两种形式
            ```py
                # 字面量创建
                dict2 = {}
                # 字典函数创建
                dict3 = dict()
            ```
        1：字典常见操作
            1-1：增(js对象用法一样 )
                    用法：字典序列[key] = 值
                注意点：如果key存在则修改这个key对应的值，如果key不存在则新增此键值对

            1-2：删除操作
                    用法：
                        del 删除字典或指定的键值对
                        clear 清空字典
                    事例：

                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        del dict1['name']   #删除字典里面的name键值对
                        del(dict1['name'])  #删除字典里面的name键值对
                        del dict1['names']  #删除不存在的key时报错

                        dict1.clear()  #清空字典操作 返回结果 {}空的字典
                    ```

            1-3:修改
                概念：key存在则修改这个key对应的值，如果key不存在则新增此键值对

            1-4：查找
                注意点：如果当前查找的key存在，则返回对应的值；否则会报错
                    事例：
                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        print(dict1['name'])  #返回对应的值
                        print(dict1['ddd'])  #因为key不存在，所以报错
                    ```
                1-4-1：get()
                        语法：字典序列.get(key,默认值)
                        注意点：如果当前查找的key不存在则返回第二个参数(默认值),如果省略第二个参数，则返回None
                        用法：事例
                            ```py
                                dict1 = {'name':'Tom','age':18,'gender':'男'}
                                print(dict1.get('name'))  #Tom
                                print(dict1.get('id',110))  #110
                                print(dict1.get('id'))  #None
                            ```

                1-4-2:keys()
                        概念：查找字典中所有的key，返回可迭代对象
                        用法：事例
                            ```py
                                dict1 = {'name':'Tom','age':18,'gender':'男'}
                                print(dict1.keys())  #返回  dict_keys(['name','age','gender'])
                            ```

                1-4-3:values()
                        概念：查找字典中所有的值，并返回可迭代对象
                        用法：事例
                            ```py
                                dict1 = {'name':'Tom','age':18,'gender':'男'}
                                print(dict1.values())  #返回dict_values(['Tom', 18, '男'])
                            ```

                1-4-4：items()
                        概念：查找字典中所有的键值对，返回可迭代对象，里面的数据是元祖，元祖数据1是字典的key，元祖数据2是字典key对应的值
                        用法：事例
                            ```py
                                dict1 = {'name':'Tom','age':18,'gender':'男'}
                                print(dict1.items())  #返回 dict_items([('name', 'Tom'), ('age', 18), ('gender', '男')])
                            ```


        2：字典的遍历循环
            2-1：遍历字典的key
                    事例：
                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        for key in dict1.keys():
                            print(key)           #输出结果为 name  age  gender
                    ```

            2-2:遍历字典的value
                    事例;
                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        for value in dict1.values():
                            print(value)            #输出结果为   Tom  18  男

                    ```

            2-3：遍历字典的元素
                    事例:
                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        for item in dict1.items():
                            print(item)             #输出结果为 ('name':'Tom')  ('age':18)  ('gender':'男')
                    ```

            2-4:遍历字典的键值对
                    事例：xxx.items() 返回可迭代对象，内部是元祖，元祖有2个对象
                    ```py
                        dict1 = {'name':'Tom','age':18,'gender':'男'}
                        for key,value in dict1.items():
                            print(f'{key} = {value}') 
                            
                        # 返回结果为
                        # name = Tom
                        # age = 18
                        # gender = 男
                    ```

    **集合
        概念：创建集合使用{}或set()，但是如果要创建空集合只能使用set()，因为{}用来创建空字典
        特点：
            集合没有顺序，所以不支持下标的操作
            去重
        1：集合常见操作
            1-1：集合添加操作
                1-1-1：add()
                        概念：用来给集合增加数据，添加的是单一数据，且去重，如果添加序列会报错
                        用法：事例
                        ```py
                            s1 = {10,20}
                            s1.add(100) 
                            s1.add([10,20,30])  #会报错，因为集合只能加单一数据，添加序列会报错
                        ```
                        注意点：
                            集合有去重功能，如果追加的数据是集合已有数据，则什么事情都不做
                            add()函数集合添加的不是单一数据，会报错

                1-1-2：update()
                        概念：用来给集合增加数据，添加的是序列，如果添加单一数据会报错
                        用法：事例
                        ```py
                            s1 = {10,20,30}
                            s1.update([10,20,30])
                            s1.update(100)  #报错，因为添加的是单一数据
                        ```

            1-2：集合的删除操作
                1-2-1：remove()
                        概念：删除集合中的指定数据，如果数据不存在则报错
                        事例：
                        ```py
                            s1 = {10,20,30,40,50}
                            s1.remove(10)
                            s1.remove(10)  #如果删除这个集合中没有的数据会报错
                            print(s1)
                        ```
                1-2-2：discard()
                        概念：删除集合中的指定数据，如果数据不存在也不会报错
                        事例：
                        ```py
                            s1 = {10,20,30,40,50}
                            s1.discard(10)
                            print(s1)
                        ```
                1-2-3：pop()
                        概念：随机删除某个数据，并返回这个数据
                        事例：
                        ```py
                            s1 = {10,20,30,40,50}
                            del_num = s1.pop()
                            print(del_num)
                        ```
                    
            1-3：集合中的查找数据
                1-3-1：in
                        概念：判断数据在集合序列
                        事例：
                        ```py
                            s1 = {10,20,30,40,50}
                            print(10 in s1)  #返回True
                        ```
                1-3-2：not in
                        概念：判断数据不在集合序列
                        事例：
                        ```py
                            s1 = {10,20,30,40,50}
                            print(10 not in s1) #返回False
                        ```
                    
    
    **公共操作
        1：运算符
            1-1:+
                作用：合并
                支持类型：字符串，列表，元祖
            1-2:*
                作用：复制
                支持类型：字符串，列表，元祖
                事例：
                ```py
                    str = 'a'
                    print(str*5) #输出结果为 aaaaa
                ```
            1-3:in
                作用：元素是否存在
                支持类型：字符串，列表，元祖，字典
            1-4：not in
                作用：元素是否不存在
                支持类型：字符串，列表，元祖，字典
                事例：

                ```py
                    dict1 = {'name':'Python',"age":18}
                    print('name' in dict1)  #True
                    print('name' in dict1.keys())   #True
                    print('name' in dict1.values())     #False
                ```

        2:公共方法
            2-1：len()
                概念：计算容器中的个数
            2-2:del或del()
                概念：删除
            2-3:max()
                概念：返回容器中元素最大值
            2-4:min()
                概念：返回容器中元素最小值
            2-5:range(start,end,step)
                概念：生成从start(不写的start，默认为0)到end的数字(不包含end结束位)，步长为step(省略的话，默认值为1)，供for循环使用
                注意点：
                        如果不写开始，默认从0开始
                        如果不写步长，默认为1
                用法：事例
                    ```py
                        for i in range(1,10,1):
                            print(i)  #输出  1,2,3,4,5,6,7,8,9,10

                        for i in range(1,10,2):
                            print(i)  #输出 1，3，5，7，9

                        for i in range(10):
                            print(i)    #输出 0，,1,2,3,4,5,6,7,8,9,10
                    ```

            2-6:enumerate()
                概念：函数用于将一个可遍历的数据对象(如列表，元祖或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在for循环中
                语法：enumerate(可遍历对象,start=0)
                    参数说明：start参数用来设置遍历数据的下标的起始值，默认为0
                    返回结果是元祖，元祖第一个数据是原迭代对象的数据对应的下标，元祖第二个数据是原迭代对象的数据
                用法：事例
                    ```py
                        list1 = ['a','b','c','d','e']
                        for i in enumerate(list1):
                            print(i)       #返回结果    (0,'a') (1,'b') (2,'c') (3,'d') (4,'e') 

                        for i in enumerate(list1,start=1):
                            print(i)       #返回结果    (1,'a') (2,'b') (3,'c') (4,'d') (5,'e') 
                    ```


        3：容器类型转换
            3-1：tuple()
                概念：将某个序列转换为元祖
                事例：
                ```py
                    list1 = [10,20,30,40,50]
                    s1 = {100,200,300,400}
                    print(tuple(list1))
                    print(tuple(s1))
                ```
            3-2：list()
                概念：转换成列表
                事例：
                ```py
                    s1 = {100,200,300,400}
                    s2 = ('a','b','c','d')
                    print(list(s2))
                    print(list(s1))
                ```
            3-3：set()
                概念：转换成集合,且有去重效果，不支持下标
                事例：
                ```py
                    list1 = [10,20,30,40,50]
                    print(set(list1))
                ```

    
    **推导式
        概念：推导式只包含下列几个：
                                列表推导式
                                字典推导式
                                集合推导式
        1：列表推导式
            概念：列表推导式又叫列表生成式
            作用：用一个表达式创建一个有规律的列表或控制一个有规律的列表
                    说明：有规律的列表就是列表内的数据是有规律的
            用法：
                事例：创建一个0-10的列表
                ```py
                    # 方法一用while循环
                    # 方法二用for循环
                        list1 = []
                        i = 0
                        for i in range(10):
                            list1.append(i)

                        print(list1)
                    # 方法三用列表推导式实现
                        list1 = [i for i in range(10)]
                        print(list1)
                ```
                    事例解释：列表推导式的执行机制：列表里面的for循环左侧的i是返回值，for循环里面的i是正常for循环的i

            1-2：带if的列表推导式
                    用法：
                        事例：创建0-10的偶数的列表
                        ```py
                            方法一：range()步长实现
                                    list1 = [i for i in range(0,10,2)]
                                    print(list1)
                            方法二：for循环加if创建有规律的列表
                                    list2 = []
                                    for i in range(10):
                                        if(i % 2 == 0):
                                            list2.append(i)
                                    print(list2)            
                            方法三：把for循环配合if的代码改写带if的列表推导式(将方法二的for循环结合if语句就行了)
                                    list3 = [i for i in range(10) if i % 2 == 0]
                                    print(list3)
                        ```

            1-3:多个for循环实现列表推导式
                用法：
                    事例：创建列表如：[(1,0),(1,1),(1,2),(2,0),(2,1),(2,2)]

                    ```py
                        # 方法一：用for循环嵌套
                        list1 = []
                        for i in range(1,3):
                            for j in range(3):
                                list.append(i,j)

                        # 方法二：用多个for的列表推导式(将方法一简化写就是方法二)
                        list2 = [(i,j) for i in range(1,3) for j in range(3)]
                        print(list2)
                    ```

        2：字典推导式
            作用：快速合并列表为字典或提取字典中目标数据
            用法：
                事例：创建一个字典：字典key是1-5数字，value是这个数字的2次方
                    ```py
                        dict1 = {i:i**2 for i in range(1,5)}
                        print(dict1)
                    ```

                事例：将两个列表合并为一个字典

                    ```py
                        #情况一： 
                        list1 = ['name','age','gender']
                        list2 = ['Tom',20,'man']
                        dic1 = {list1[i]:list2[i] for i range(len(list1))}
                        
                        # 情况二：
                        list1 = ['name','age','gender','id']
                        list2 = ['Tom',20,'man']
                        dic1 = {list1[i]:list2[i] for i range(len(list1))}  #此时会报错，因为list2取不到3
                    ```
                    事例注意点：
                            如果两个列表数据个数相同，len统计任何一个列表的长度都可以
                            如果两个列表数据个数不同，len统计数据多个列表数据个数会报错，len统计数据少的列表数据个数不会报错

                事例：提取字典中目标数据

                    ```py
                        # 提取电量数量大于等于200的字典数据
                        count = {'MBP':268,'HP':125,'DELL':201,'Lenovo':199,'acer':99}
                        count1 = {key:value for key ,value in count.items() if value >=200}
                        print(count1)
                    ```

        3：集合推导式
            用法：
                事例：创建一个集合，数据为下方列表的2次方
                ```py
                    list1 = [1,2,3,1]
                    set1 = {i**2 for i in list1}
                    print(set1) #打印的结果为{1，4，9} 集合有数据去重功能
                ```
                                

                            
    **函数
        1：函数的作用：
                概念：函数就是将一段具有独立功能的代码块整合到一个整体并命名，在需要的位置调用这个名称即可完成对应的需求
                作用：函数在开发过程中，可以更高效的实现代码重用

        2：使用函数
            2-1：定义函数
                    def 函数名(参数):
                            代码1
                            代码2
                            ...
                            ...

            2-2：调用函数
                    函数名(参数)
            
            2-3：注意点：
                不同的需求，参数可有可无
                在Python中，函数必须先定义后使用

            用法：
                事例：
                ```py
                    # 先定义函数
                    def my_first():
                        print('函数的使用')
                    # 使用函数
                    my_first()
                ```

                事例代码函数执行机制：
                            当调用函数的时候，解释器回到定义函数的地方去执行下方缩进的代码，当这些代码执行完，回到调用函数的地方继续向下执行
                            定义函数的时候，函数体内部缩进的代码并没有执行

        
        3：函数参数的作用
            概念：让函数变的更加灵活(和js里面函数的参数是一样的)

        4：函数返回值的作用
            概念：(和js里函数的返回值是一样的)
            4-1：return
                    作用：
                        负责函数返回值
                        退出当前函数，导致return下方的所有代码(函数内部)不执行，且退出当前函数
            
            用法：事例：

                ```py
                    def sum_num(a,b):
                        return a + b
                        print('我这里不会执行')  #函数体内，return下面的代码不会执行

                    result = sum_num(1,2)
                    print(result)
                ```

        5：函数的说明文档
            5-1：定义函数的说明文档

                ```py
                    def 函数名(参数):
                        """说明文档位置"""
                        代码
                        ......
                        ......
                    
                    # 查看函数的说明文档
                    help(函数名)
                ```
            
            5-2：函数的说明文档的高级使用
                事例：

                ```py
                    def sum(a,b):
                        """
                        求和函数sum
                        :param a: 参数1
                        :param b: 参数2
                        :return: 返回值
                        """
                        return a + b

                    # 查看函数说明文档help(函数名)
                    help(sum)
                ```
            注意点：说明文档的位置是在函数体内，缩进的第一行代码的位置进行书写，也必须写在这个位置

        6:函数嵌套调用
            概念：所谓函数嵌套调用指的是一个函数里面又调用了另外一个函数(和js里的函数嵌套是一样的)
            用法：事例
                ```py
                    def testB():
                        print('B函数开始')
                        print('这是B函数')
                        print('B函数结束')

                    # A函数
                    def testA():
                        print('A函数开始')
                        testB()
                        print('A函数结束')

                    testA()
                ```
        

        7：变量作用域
            概念：变量作用域指的是变量生效的范围，主要分为两类：局部变量和全局变量(和js里的作用域一样)
            7-1：局部变量
                    概念：所谓局部变量是定义在函数体内部的变量，即只在函数体内部生效
                    作用：在函数体内部，临时保存数据，即当函数调用完成后，则销毁局部变量
                    用法：
                        事例

                        ```py
                            def testA():
                                a = 100
                                print(a)
                            testA() # 100
                            print(a) #报错：name 'a'is not defined
                        ```
                            事例解释：变量a是定义在testA函数内部的变量，在函数外部访问则立即报错 

            7-1：全局变量
                    概念：指的是函数体内，外都能生效的变量
                    用法：只要将变量写在全局就可以了(也就是函数外部就行)
                    用法：
                        事例
                        ```py
                            a = 100
                            def testA():
                                print(a)
                            testA() # 100

                            def testB():
                                print(a)
                            testB() #100
                        ```
            7-3:修改全局变量
                    用法：通过global关键字声明a是全局变量，然后改变a即是改变全局变量的a，否则是局部变量(js里面在全局用var或者let定义变量则是全局变量，在函数体内可以访问和修改)
                    事例：

                    ```py
                        a = 100
                        def testA():
                            print(a)


                        def testB():
                            global a  #此处global关键字声明a是全局变量，否则下面给a赋值200，则a是局部的a，外部访问a还是100
                            a = 200
                            print(a)

                        testB() #200
                        testA() #200
                    ```


        8:函数返回值作为参数传递
        9:一个函数有多个返回值
            注意点：
                return a,b 的写法，返回多个数据的时候，默认是元祖类型
                return后面可以连接列表，元祖或字典，以返回多个值

            写法：

            ```py
                # 第一种写法默认返回元祖
                def return_num():
                    return 1,2
                
                print(return_num())  #返回结果为  (1,2) 的元祖

                # 其他写法
                def nums():
                    # return 1,2
                    # return [100,200]
                    # return {'name':'Python','age':18}

                print(nums())  
            ```

        10：函数的参数
                10-1：位置参数(js里面的参数就是通过位置参数来传递的)
                        概念：调用函数时根据函数定义的参数位置来传递参数
                        用法：
                            事例:
                            ```py
                                def user_info(name,age,gender):
                                    print(f'您的名字是{name},年龄是{age}，性别是{gender}')

                                user_info('shi',18,'男')
                            ```
                        注意点：传递和定义参数的顺序及个数必须是一致的

                10-2：关键字参数
                        概念：通过"键=值"形式加以指定。可以让函数更加清晰，容易使用，同时也清除了参数的顺序需求
                        注意点：函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序
                        用法：
                            事例：

                            ```py
                                def user_info(name,age,gender):
                                    print(f'您的名字是{name},年龄是{age}，性别是{gender}')

                                user_info('rose',age = 20,gender='女')  #输出 您的名字是rose,年龄是20，性别是女
                                user_info('小明',gender='男',age=18)    #输出 您的名字是小明,年龄是18，性别是男
                            ```

                10-3：缺省参数(默认参数)
                        概念：缺省参数也叫默认参数，用于定义函数，为参数提供默认值，调用函数时可以不传该默认参数的值
                        注意点：
                            所有位置参数必须出现在默认参数前，包含函数定义和调用
                            函数调用时，如果为缺省参数传值则使用传入的值，否则使用这个默认值
                        用法：
                            事例：

                            ```py
                                def user_info(name,age,gender='男'):
                                    print(f'您的名字是{name},年龄是{age}，性别是{gender}')

                                user_info('Tom',age = 20) #输出  您的名字是Tom,年龄是20，性别是男
                                user_info("rose",age=18,gender='女')  #输出   您的名字是rose,年龄是18，性别是女
                            ```

                10-4：不定长参数(和js里的结构参数类似)
                        概念：不定长参数也叫可变参数。用于不确定调用的时候会传递多少个参数(不传参也可以)的场景。此时，可用包裹位置参数，或者包裹关键字参数，来进行参数传递，会显得非常的方便

                        注意点：不管是包裹位置传递还是包裹关键字传递，都是组包的过程

                        10-4-1：包裹位置传递
                                    概念：接受所有位置参数，返回一个元祖
                                    用法：
                                        事例：

                                        ```py
                                            def user_info(*args):
                                                print(args)

                                            user_info('Tom')    #('Tom',)
                                            user_info('Tom',30)     #('Tom', 30)
                                            user_info('Tom',30,'man')      #('Tom', 30, 'man')
                                            user_info() #()
                                        ```
                                    注意点:传进的所有参数会被args变量收集，它会根据传进参数的位置合并为一个元祖，args是元祖类型，这就是包裹位置传递

                        10-4-2：包裹关键字传递
                                    用法：
                                        事例：
                                        ```py
                                            def user_info(**kwargs):
                                                print(kwargs)

                                            user_info(name='Tom',age=18,id='110') #返回字典{'name': 'Tom', 'age': 18, 'id': '110'}
                                        ```

        11：拆包和交换变量值
            11-1：拆包：元祖(类似于js里的解构)
                    用法：
                        事例：拆包元祖数据
                        ```py
                            def return_num():
                                return 100,200

                            num1,num2 = return_num()

                            print(num1) #100
                            print(num2) #200
                        ```
                
            11-2:拆包：字典
                    用法：
                        事例：拆包字典数据
                        ```py
                            dic2 = {'name':'Tom','age':18}
                            a,b = dic2

                            # 对字典拆包，取出来的是字典的key
                            print(a) #name
                            print(b) #age
                            print(dic2[a]) #Tom
                            print(dic2[b]) #18
                        ```

        12：交换变量的值
            方法一：借助第三个变量存储数据(和js里的变量交换数据一样)
            方法二：和js里数组解构差不多
                    用法：
                        事例：
                        ```py
                            a,b = 1,2
                            a,b = b,a
                            print(a) #2
                            print(b) #1
                        ```

    **引用
        1：了解引用
            概念：在Python中，值是靠引用来传递的；
            1-1：id():
                    概念：id(变量名) 返回这个变量名在内存中的地址，这个地址可以是二进制的，可以是十进制的
                    用法：我们可以用id()来判断两个变量是否为同一个值的引用。我们可以将id值理解为那块内存的地址表示
                事例：

                ```py
                    # 整型int(不可变类型)
                        a = 1
                        b = a
                        print(a)
                        print(id(a))    #8790881854288
                        print(id(b))    #8790881854288  
                                # a和b的id值相同
                        
                        a += 1
                        print(id(a))    #8790881854320
                        # 因为修改了a的数据，内存要开辟另外一份内存存储2，id检测a和b的地址不同

                    # 列表list(可变类型)
                        list1 = [10,20]
                        list2 = list1
                        print(list2)    #[10, 20]
                        print(id(list1))    #30237320
                        print(id(list2))    #30237320

                        # 给list1添加数据，list2也会跟着改变，且list1和list2在改变前后的id都是一样的
                        list1.append(30)    
                        print(list2)        #[10, 20, 30]
                        print(id(list1))    #30237320
                        print(id(list2))    #30237320
                ```
                事例解释：可变类型和不可变类型的原理是：变量对内存数据的引用的方式

        2：引用当做实参
            结论：可以将可变类型和不可变类型当做实参传入

    
    **可变类型和不可变类型总结
        概念：所谓可变类型与不可变类型是指：数据能够直接进行修改，如果能直接修改那么就是可变，否则是不可变

            可变类型：
                列表
                字典
                集合
            不可变类型：
                整型
                浮点型
                字符串
                元祖

    **递归
        应用场景：
            在我们日常开发中，如果要遍历一个文件夹下面所有的文件，通常会使用递归来实现
            在后续的算法课程中，很多算法都离不开递归，例如：快速排序
        特点：
            函数内部自己调用自己
            必须有出口(如果没有出口就是死循环了)
        
        递归的执行机制：重复执行累加并返回结果，在出口处返回

        用法：
            事例：

            ```py
                def sun_number(num):
                    if num == 1:
                        return 1
                    return num + sun_number(num - 1)

                print(sun_number(3))
            ```
            事例解释：如果没有指定递归出口，则会报错，报错内容为超出最大递归深度

    
    **lambda表达式(匿名函数)(也类似于js中箭头函数省略return的写法)
        概念：lambda可以用来简化代码
        特点：同样是占内存的，但lambda所占的内存比函数所占的内存要小
        语法：
            lambda 参数列表：表达式
            语法说明：这里的表达式一定是有返回值的
            语法注意点：
                lambda表达式的参数可有可无，函数的参数在lambda表达式中完全适用
                lambda表达式能接收任何数量的参数但只能返回一个表达式的值
        
        用法：
            事例(函数和lambda表达式对比)

            ```py
                # 在函数中
                def fn1():
                    return 200

                print(fn1)
                print(fn1())

                # 在lambda中
                fn2 = lambda :100

                print(fn2)      #是lambda表达式的内存地址
                print(fn2())
            ```

        事例：
            带有简单参数的lambda和函数两种写法的对比
            

            ```py
                def fn1(a,b):
                    return a + b
                print(fn1(1,2))

                fn2 = lambda a,b:a + b
                print(fn2(2,3))  #返回5
            ```

        1：lambda的参数形式
            1-1：无参数
                事例：
                ```py
                    fn1 = lambda :100
                    print(fn1())
                ```
            1-2：一个参数
                事例：
                ```py
                    fn2 = lambda a: a
                    print(fn2('hello world')) 
                ```
            1-3：默认参数
                事例：
                ```py
                    fn1 = lambda a,b,c=100:a+b+c
                    print(fn1(100,200))  #400
                    print(fn1(100,200,300))  #600
                ```
            1-4：不定长位置可变参数：*args
                事例：
                ```py
                    fn2 = lambda *args:args
                    print(fn2(10,20,30))    #返回元祖 (10, 20, 30)
                ```
                    事例注意点：这里的可变参数传值到lambda之后，返回值为元祖
            1-5:不定长关键字可变参数：**kwargs  (和函数里包裹关键字参数一样)
                事例：
                ```py
                    fn3 = lambda **kwargs:kwargs
                    print(fn3(name='python',age=20))    #返回字典 {'name': 'python', 'age': 20}
                ```
            
        2：lambda的应用
            2-1：带判断的lambda
                事例：(结合三元运算符进行运算)
                ```py
                    fn1 = lambda a,b:a if a > b else b
                    print(fn1(100,200))  #返回 200
                ```
            2-2：列表数据按字典key的值排序
                事例：
                ```py
                    # 1.name key对应的值进行升序排序
                    students = [
                        {'name':'TOM','age':20},
                        {'name':'ROSE','age':19},
                        {'name':'JACK','age':22},
                    ]
                    students.sort(key=lambda x:x['name'])  
                    print(students) #返回 [{'name': 'JACK', 'age': 22}, {'name': 'ROSE', 'age': 19}, {'name': 'TOM', 'age': 20}]
                ```

                事例：
                ```py
                    # name key对应的值进行降序排序
                    students = [
                        {'name':'TOM','age':20},
                        {'name':'ROSE','age':19},
                        {'name':'JACK','age':22},
                    ]
                    students.sort(key=lambda x:x['name'],reverse=True)
                    print(students) #返回 [{'name': 'TOM', 'age': 20}, {'name': 'ROSE', 'age': 19}, {'name': 'JACK', 'age': 22}]
                ```

                事例：
                ```py
                    # age key对应的值进行升序排序
                    students = [
                        {'name':'TOM','age':20},
                        {'name':'ROSE','age':19},
                        {'name':'JACK','age':22},
                    ]
                    students.sort(key=lambda x:x['age'])
                    print(students) #返回 [{'name': 'ROSE', 'age': 19}, {'name': 'TOM', 'age': 20}, {'name': 'JACK', 'age': 22}]
                ```

    
    **高阶函数
        概念：把函数作为参数传入，这样的函数成为高阶函。高阶函数是函数式编程的体现。函数式编程就是指这种高度抽象的编程范式
        用法：
            事例：将内置的球绝对值函数和四舍五入函数当参数传入
            ```py
                def sum_num(a,b,f):
                    return f(a) + f(b)

                print(sum_num(-1,2,abs))    #3
                print(sum_num(1.3,2.9,round)) #4
            ```
            
        1：内置高阶函数
            1-1：map()  (和js里面的map意思差不多，但用法不一样)
                    概念：map(func,lst)，将传入的函数变量func作用到lst变量的每个元素中，并将结果组成新的列表(Python2)/迭代器(Python3)返回
                    用法：事例
                    ```py
                        list1 = [1,2,3,4,5,6]
                        def func(x):
                            return x**2

                        result = map(func,list1)    #在Python中返回的迭代器，通过list()转换成列表
                        print(result)  #返回   <map object at 0x00000000021B4FD0>
                        print(list(result))  #[1, 4, 9, 16, 25, 36]
                    ```
            
            1-2：reduce()  (和js里面的reduce意思差不多，但用法不一样)
                    概念：reduce(func,lst)，其中func必须有两个参数，每次func计算的结果继续和序列的下一个元素做累积计算
                    注意点：reduce()传入的参数func必须接收2个参数
                    用法：
                        事例：计算list1序列中各个数字的累加和

                        ```py
                            import functools
                            list1 = [1,2,3,4,5,6]
                            def func(a,b):
                                return a + b

                            result = functools.reduce(func,list1)
                            print(result) # 返回结果 21
                        ```

            1-3：filter()   (和js里面的filter方法意思一样，但用法有区别)
                    概念：filter(func,lst)函数用于过滤序列，过滤掉不符合条件的元素，返回一个filter对象。如果要转换为列表，可以使用list()来转换
                    用法：
                        事例：

                        ```py
                            list1 = [1,2,3,4,5,6,7,8,9,10]
                            def func(x):
                                return x % 2 == 0
                            result = filter(func,list1)
                            print(result)   #<filter object at 0x0000000002744FD0>
                            print(list(result)) #[2, 4, 6, 8, 10]
                        ```

    **文件操作
        作用：文件操作的作用就是把一些内容(数据)存储存放起来，可以让程序下一次执行的时候直接使用，而不必重新制作一份，省时省力
        1：文件的基本操作
            1-1：打开文件
                    概念：在Python中，使用open函数，可以打开一个已经存在的文件，或者创建一个新文件，语法如下
                    语法：
                        open(name,mode)
                        参数说明：
                                返回值：返回文件对象
                                name：是要打开的目标文件名的字符串(可以包含文件所在的具体路径)
                                mode：设置打开文件的模式(访问模式)：只读，写入，追加等
                                    1：访问模式对文件的影响
                                    2：访问模式对write()的影响
                                    3：访问模式是否可以省略
                                        r：表示只读，如果文件不存在，报错：不支持写入操作。文件指针在开头；这是默认模式
                                        rb：以二进制格式打开一个文件用于只读
                                        r+：可读可写；没有该文件则报错；文件指针在开头，所以能读取出来数据
                                        rb+：以二进制格式打开一个文件用于读写
                                        w：表示只写，如果文件不存在，新建文件：文件指针在开头，执行写入，会覆盖原有内容
                                        wb：以二进制格式打开一个文件只用于写入
                                        w+：可读可写；没有该文件会新建文件；文件指针在开头，用新内容覆盖原有内容
                                        wb+：以二进制格式打开一个文件用于读写
                                        a：表示追加，文件指针在结尾；如果文件不存在，新建文件：在原有内容基础上追加新的内容
                                        ab：以二进制格式打开一个文件用户追加
                                        a+：可读可写；没有该文件会新建文件；文件指针在结尾，无法读取数据(文件指针后面没有数据)
                                        ab+：以二进制格式打开一个文件用于读写
                                        访问模式可以省略，如果省略表示访问模式为r



            1-2：读写等操作
                    read()
                        语法：文件对象.read(num)
                            参数说明：num：表示要从文件中读取的数据的长度(单位是字节)，如果没有传入num，那么就表示读取文件中所有的数据
                        用法：
                            事例：
                            ```py
                                f.open('test.txt','r')
                                f.read(10)
                                # read不写参数表示读取所有
                                # 文件内容如果换行，底层有\n换行符，此换行符占有一个字节
                                f.close()
                            ```

                    readlines()
                        概念：readlines可以按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个列表，其中每一行的数据为一个元素
                        用法：
                            事例：
                            ```py
                                f = open('test.txt')
                                print(f.readlines()) #返回列表['aaa\n', 'bbb\n', 'vvv\n', 'bbb\n', 'ccc']
                                f.close()
                            ```
                    
                    readline()
                        概念：readline()一次读取一行内容，调用多次就读多行
                        用法：
                            事例
                            ```py
                                f = open('test.txt')
                                print(f.readline()) #第一调用readline读取出来的是第一行
                                print(f.readline()) #第二调用readline读取出来的是第二行
                                print(f.readline()) #第三调用readline读取出来的是第三行
                                f.close()
                            ```
            1-3：关闭文件
            注意：
                可以只打开和关闭文件，不进行任何读写操作
                只打开文件，而不关闭文件的话，那么这个文件将会持续占有内存，这样子内存消耗会比较大，内存占有量大的话，风险会比较高。
        

            1-4：seek()
                    作用：用来移动文件指针
                    语法：文件对象.seek(偏移量,起始位置)
                            参数说明：起始位置：
                                              0：文件开头
                                              1：当前位置
                                              2：文件结尾
                    用法：事例

                        ```py
                            f = open('test.txt','r+')
                            # 把文件指针放在开头，且偏移量为2的位置开始读取
                            f.seek(2,0)
                            print(f.read())  #结果为从偏移量为2的位置开始读取：  a
                                                                            # bbb
                                                                            # vvv
                                                                            # bbb
                                                                            # ccc
                            f.close()
                        ```

        2：文件备份
            作用：将要备份的数据写入备份文件
            步骤：
                1：接收用户输入的文件名
                2：规划备份文件名
                3：备份文件写入数据
            
            1：事例：备份文件写入数据 
                1-1：打开源文件 和 备份文件
                1-2：将源文件数据写入备份文件
                1-3：关闭文件

                ```py
                    # 接收用户输入的文件名
                    old_name = input('请输入文件名：')
                    # 规划备份文件名
                    index = old_name.rfind('.')
                    # 过滤掉无效的文件名
                    if index > 0:
                        postfix = old_name[index:]
                    new_name = old_name[:index]+'[备份]'+postfix

                    # 打开 原文件 和 备份文件
                    old_f = open(old_name,'rb')
                    new_f = open(new_name,'wb')
                    # 如果不确定目标文件大小，循环读取写入，当读取出来的数据没有了，终止循环
                    while True:
                        con = old_f.read(1024)
                        if len(con) == 0:
                            break
                        new_f.write(con)

                    # 关闭文件
                    old_f.close()
                    new_f.close()
                ```

        3：文件和文件夹的操作
            概念：在Python中文件和文件夹的操作要借助os模块里面的相关功能
            用法：
                步骤：
                    1：导入os模块
                        import os
                    2：使用os模块相关功能
                        os.函数名()
                        2-1：文件或者文件夹重命名     os.rename(目标文件名/目标文件夹名,新文件名/新文件夹名)
                        2-2：删除文件       os.remove(目标文件名)
                        2-3：创建文件夹     os.midir(文件夹名字/带路径的文件夹的名字)
                        2-4：删除文件夹     os.rmdir(文件夹名字)
                        2-5：获取当前目录   os.getcwd()
                        2-6：改变目录路径   os.chdir(目录)
                        2-7：获取某个文件夹下所有的文件，返回一个列表   os.listdir(目录)
                
                事例：

                ```py
                    import os
                    os.rename('test.txt','test1.txt')  #重命名  将test.txt重命名为test1.txt
                    os.remove('100.txt') #删除文件
                    # os.mkdir() 创建文件夹
                    os.mkdir('10')
                    # os.rmdir() 删除文件夹
                    os.rmdir('10')

                    #os.chdir()  改变目录路径
                    # 需求：在aa文件夹里面创建bb文件夹：1：切换目录到aa，2：创建bb
                    os.midir('aa')      #先创建aa文件夹
                    os.chdir('aa')      #切换目录到aa
                    os.mkdir('bb')      #在aa目录里面创建bb

                    # os.listdir() :获取某个文件夹下所有文件，返回一个列表  默认是当前文件夹
                    os.listdir('aa')    #返回aa文件夹下的所有文件目录并组成列表
                    os.listdir()        #返回当前文件所在的目录下的所在的文件目录所组成的列表

                ```

                





        

        
        







                        
                            




                    













                    
                

                                        













            




