##java
    1：优势：跨平台是java语言的核心优势，随着互联网的发展而发展，建立了强大的生态体系，目前已经覆盖IT各行业的"第一大语言"
            生态体系的体现：
                        企业级的开发
                        安卓开发
                        大数据
                        云计算
    
    2：java的三个版本：
        javaSE(java Standard Edition) ：java的标准版，定位在个人计算机上的应用
                应用的理解：也就是桌面应用；例如：写个word，qq，浏览器，记事本等等，但是效率不高，还不如用c++去开发这些应用
            
        javaEE(java Enterprise Edition) ：企业版，定位在服务端的应用(也是应用的范围原来越广了)

        javaME(java Micro Editon)：微型版，定位在消费性电子产品的应用上，比如：智能微波炉(基本上都不用了，基本上都是被安卓取代了)

    3：java的特性和优势
        1:跨平台/可移植性：
                    这是java的核心优势。java在设计时就很注重移植和跨平台性。比如:java的int永远都是32位。不像C++可能是16，32，可能是根据编译器厂商规定的变化。这样的话程序的移植就会非常麻烦
        
        2:安全性：
                java适合于网络/分布式环境，为了达到这个目标，在安全性方面投入了很大的精力，使java可以很容易构建防病毒，防篡改的系统
            
        3:面向对象：

        4:简单性：
                java就是c++语法的简化版，我们也可以将java称之为'C++-'.也就是C加加减，指的就是将C++的一些内容去掉；比如：头文件，指针运算，结构，联合，操作符重载，虚基类等等。同时，由于语法基于C语言，因此学习起来完全不费劲儿
        
        5:高性能：
                java最初发展阶段，总是被人诟病'性能低'；客观上，高级语言运行效率总是低于低级语言的这个无法避免。java语言本身发展中通过虚拟机的优化提升了几十倍运行效率。比如，通过JIT(JUST IN TIME)即时编译技术提高运行效率。将一些"热点"字节码编译成本地机器码，并将结果缓存起来，在需要的时候重新调用。这样的话，使java程序的执行效率大大提高，某些代码甚至接待C++的效率

                因此，java低性能的短腿，已经被完全解决了。业界发展商，我们也看到很多C++应用转到Java开发，很多C++程序员转型为java程序员
        6:分布式：
                java是Internet的分布式环境设计的，因此它能够处理TCP/IP协议。事实上，通过URL访问一个网络资源和访问本地文件是一样简单的。java还支持远程方法调用，使程序能够通过网络调用
    
        7:多线程：
                多线程的使用可以带来更好的交互响应和实时行为。java多线程的简单性是java成为主流服务器端开发语言的主要原因之一

        8:健壮性：
                java是一种健壮的语言，吸收了C/C++语言的有点，但去掉了其影响程序健壮性的部分(如：指针，内存的申请与释放等)。java程序不能造成计算机奔溃。即使java程序也可能有错误。如果出现某种出乎意料的事，程序也不会奔溃，而是把该异常抛出，再通过异常处理机制加以处理


    4：java应用程序的运行机制
        概念：计算机高级语言的类型主要有编译型和解释型两种，而java语言是两种类型的结合
        java程序运行机制:
                java源码文件首先通过java编译器，编译成字节码文件，然后把字节码文件给了JRE(或者说是虚拟机)，在JRE中字节码文件通过类装载器，字节码校验器，然后给虚拟机解释器解析。最后在系统平台上跑程序；

                实现跨平台原理：避免java源文件直接跟系统操作平台上运行，而是通过中间件JRE或者JVM虚拟机处理之后，再跟操作系统打交道。这也是实现跨平台的核心机制

    5：JVM，JDK，JRE的作用和区别
        JVM：
            作用：JVM(java Virtual Machine)就是一个虚拟的用于执行bytecode字节码的'虚拟计算机'

        JDK：
            概念：
                JDK是提供给java开发人员使用的，其中包含了java的开发工具，也包含了JRE。所以安装了JDK，就不用在单独安装JRE了
                其中开发工具为：编译工具(javac.exe) 打包工具(jar.exe)等
            作用：JDK(java Development Kit)java开发工具包，包含JRE，以及增加编译器和调试器等用于程序开发的文件；如果我们要开发java程序必须要安装jdk
        
        JRE：
            概念：包含了java虚拟机(JVM)和java程序所需的核心类库等，如果想要运行一个开发好的java程序，计算机中只需要安装JRE即可
            作用：JRE(java Runtime Environment)包含：java虚拟机，库函数，运行java应用程序所必须的文件；如果仅仅是运行java文件那么JRE就够了

        总结：JDK里面包含JRE，JRE里面包含JVM

    6：JDK的下载与安装和目录介绍
        JDK安装之后生成的目录介绍：
                bin目录：是二进制的文件(调编译器，虚拟机等等一些命令)
                lib：库的意思，存放的java的jar包
                src.zip:是jdk源码目录
            
        JDK安装完之后设置环境变量步骤：
                1：打开环境变量->点击系统新建->变量名：JAVA_HOME;变量值：java安装的路径
                2：修改系统环境变量path，在最前面追加%JAVA_HOME%\bin(或者直接添加java文件bin目录所在的文件路径),并以;和原路径分隔

        
    7：java语言特点：
        1：java对大小写敏感，如果出现大小写拼写错误，程序无法运行
        2：关键字public被称作访问修饰符，用于控制程序的其它部分对这段代码的访问级别
        3：关键字class的意思是类。java是面向对象的语言，所有代码必须位于类里面
        4：一个源文件中至多只能声明一个public类，其它类的个数不限，如果源文件中包含一个public类，源文件名必须和其中定义的public的类名相同，且以'java'为扩展名
        5：正确编译后的源文件，会得到相应的字节码文件，编译器为每个类生成独立的字节码文件(编译后一个class类生成一个以.class为扩展名的字节码文件)，且将字节码文件自动命名为类的名字且以'.class'为扩展名
        6：main方法是java应用程序的入口方法，它有固定的书写格式
        7：public static void main(String[] args){...}
        8：在java中，用花括号划分程序的各个部分，任何方法的代码都必须以'{'开始，以"}"结束，由于编译器忽略空格，所以花括号风格不受限
        9：java中每个语句必须以分号结束，回车不是句语句的结束标志，所以一个语句可以跨多行


    
    ***基础语法
        **：注释
            1：单行注释：使用"//"开头，"//"后面的当行内容均为注释

            2：多行注释：以"/*"开头以"*/"结尾，在"/*"和"*/"之间的内容为注释，我们也可以使用多行注释作为行内注释。但是在使用时要注意，多行注释不能嵌套使用

            3：文档注释：以"/**"开头以"*/"结尾，注释中包含一些说明性的文字及一些javaDoc标签(后期写项目时，可以生成项目的API)

        **：标识符
            1：概念：标识符是用来给变量，类，方法以及包进行命名的

            2：标识符遵守的规则：
                1:标识符必须以字母，下滑线，美元符号$开头
                2:标识符其他部分可以是字母，下划线，美元符和数字的任意组合
                3:java标识符大小写敏感，且长度无限制
                4:标识符不可以是java关键字

            3：标识符的使用规范：
                表示类名的标识符：每个单词的首字母大写：如Man，GoodMan
                表示方法和变量的标识符：第一个单词小写，从第二个单词开始首字母大写，我们称之为"驼峰原则",如eatFood()

        **：变量
            1：概念：变量本质上就是一个"可操作的存储空间"，空间位置是确定的，但是里面放置什么值不确定。我们可通过变量名来访问"对应的存储空间"，从而操作这个"存储空间"存储的值

            2：特点：
                1：java是一种强类型语言，每个变量都必须声明其数据类型。变量的数据类型决定了变量占据存储空间的大小。比如：int a = 3;表示a变量的空间大小为4个字节
                2：变量作为程序中最基本的存储单元，其要素包括变量名，变量类型和作用域。变量在使用前必须对其声明，只有在变量声明以后，才能为其分配相应长度的存储空间

            3：分类：(和js里面变量差不多不过声明方式不一样)
                1：局部变量：
                    概念：方法或语句块内部定义的变量。生命周期是从声明位置开始到方法或语句块执行完毕为止
                2：成员变量
                    概念：也叫实例变量；方法外部，类的内部定义的变量。从属于对象，生命周期伴随对象始终
                    特点：成员变量会自动被初始化
                3：静态变量
                    概念：也叫类变量；使用static定义。从属于类，生命周期伴随类始终，从类加载到卸载

        **：常量
            1：概念：常量通常指的是一个固定的值
            2：用法：在java中，主要是利用关键字final来定义一个常量
                  定义常量时用大写字母和下划线
                  事例：

                    ```java
                        final String Name = "石惠"
                    ```

        **：数据类型
            1：基本数据类型：
                    1：数值型：
                        1：整数类型(byte,short,int,long)

                            概念：整型用于表示没有小数部分的数值，它允许是负数。整型的范围与运行java代码的机器无关，这正是java程序具有很强移植能力原因之一。与此相反，c和C++程序需要针对不同的处理器选择最有效的整型
                            
                            具体内容：
                                byte    1字节   表数范围  正负2的8次方之间数字
                                short   2字节   表数范围  正负2的16次方之间数字
                                int     4字节   表数范围  正负2的32次方之间数字
                                long    8字节   表数范围  正负2的64次方之间数字

                                事例：

                                ```java
                                    byte age = 30;
                                    short salary = 30000;   
                                    int population = 20000000;  //整型常量默认值是int类型
                                    long globalPopulation = 7400000000L; 
                                            //不加L的话会报错，因为整型常量默认值是int类型，如果不加的L的话，表示int类型，后面加L表示这是一个long类型的整型，
                                ```
                                    事例注意点：java语言的整型常数默认为int型，声明long型常量可以在后面加L


                        2：浮点类型(float,double)
                            float   4字节       表数范围    -3.403E38 ~ 3.403E38
                            double  8字节       表数范围    -1.798E308 ~ 1.798E308

                            1:概念：float类型又被称为单精度类型，尾数可以精确到7位有效数字，在很多情况下，float类型的精度很难满足需求
                                  double表示这种类型的数值精度约是float类型的两倍，又被称作双精度类型，绝大部分应用程序都采用double类型
                                注意点：浮点型常量默认类型也是double

                            2:注意点：
                                1:float类型的数值有一个后缀F，没有后缀F的浮点数值默认为double类型。也可以在浮点数值后添加后缀D，以明确其为double类型

                                2:浮点型是不精确的，不能比较，比较的返回false；要使用任意精度的整数运算用BigInteger。实现任意精度的浮点运算用BigDecimal

                                事例1：float类型的数值要添加后缀F的注意点

                                ```java
                                    float f = 3.14F;
                                    double d1 = 3.14;
                                    double d1 = 3.14D;
                                ```
                                事例2：浮点型不精确

                                ```java
                                    float f = 0.1f;
                                    double d = 1.0/10;
                                    System.out.println(f==d) //输出结果为false
                                ```

                    2：字符型(char)
                        char    2个字节 

                        概念：char类型用来表示在Unicode编码表中的字符。Unicode编码被设计用来处理各种语言的文字，它占2个字节，可允许有65536(包含了所有的文字和字母)

                        注意点：char类型只能写一个字符(中文，英文，韩文都可以，但是只能写一个)

                        用法：
                            1：在java中使用单引号来表示字符常量。例如'A'是一个字符，它与"A"是不同的，"A"表示含有一个字符的字符串
                            2：Unicode具有从0到65535之间的编码，他们通常用从'\u0000'到'\uFFFF'之间的十六进制值来表示(前缀为u表示Unicode)
                            3：java语言中允许使用转义字符'\'，来将其后的字符转变为其他的含义。通常的转义字符及其含义和转义对应的Unicode值都一一对应

                            例如：'\u0123'：代表的一个字符，底层Unicode数值是0123。
                        事例：

                            ```java
                                public class pr1 {
                                    public static void main(String[] args){
                                        char a = 'T';
                                        char b = '上';
                                        String c = "今天天气真好";
                                        System.out.println(b);
                                        System.out.println(a);
                                        System.out.println(c);
                                        System.out.println(""+'a'+'\n'+'b');
                                    }
                                }

                                /*输出结果
                                    上
                                    T
                                    今天天气真好
                                    a
                                    b
                                */
                            ```

                    3：布尔型(boolean)
                        概念：boolean类型有两个值，true和false，在内存中占一位(不是一个字节)，不可以使用0或非0的整数代替true和false，这点和js语言不同，用的时候必须用true和false来表示布尔型

                        事例：

                            ```java
                                public class pr1 {
                                    public static void main(String[] args){
                                        //测试布尔值
                                        boolean man = true;
                                        if(man){	//不推荐man==true
                                            System.out.println("男性");
                                        }
                                    }
                                }
                            ```
            2：引用数据类型：
                    类(class)
                    接口(interface)
                    数组

                

        **：运算符
            1：算术运算符
                概念：算术运算符中+,-,*,/,%属于二元运算符，二元运算符指的是需要两个操作数才能完成运算的运算符。其中的%是取模运算符，就是我们常说的求余数操作
                1：二元运算符的运算规则：
                    整数运算：
                        1：如果两个操作数有一个为Long，则结果为Long。
                        2：没有Long时，结果为int。即使操作数全为short，byte，结果也是int。
                    浮点运算：
                        1：如果两个操作数有一个为double，则结果为double。
                        2：只有两个操作数都是float，则结才为float。
                    取模运算：
                        1：其操作数可以为浮点数，一般使用整数，结果是"余数"，"余数"符号和左边操作数相同
                        实例：
                            ```java
                                7%3=1
                                -7%3=-1
                                7%-3=1
                            ```

                2：一元运算符 ++与--
                    实例：

                        ```java
                            public class mo {
                                public static void main(String[] args){
                                    //测试自增和自减
                                    int a  = 3;
                                    int b = a++;  //执行完后，b=3.先给b赋值，a再自增
                                    System.out.println(b+a);
                                    
                                    a  = 3;
                                    int  c= ++a;  //执行完后c=5，原因：a先自增，再给c赋值 
                                    System.out.println(b+a);
                                }
                            }
                        ```

            2：赋值运算符及扩展运算符
                概念：+=，-=，*=，/=，%=

            3：关系运算符
                概念：
                    关系运算符用来进行比较运算，关系运算符的结果是布尔值:true/false
                    ==,!=,>,<,>=,<=

            4：逻辑运算符
                概念：逻辑运算的操作数和运算结果都是boolean值
                        短路与      &&      只要有一个为false，则直接返回false
                        短路或      ||      只要有一个为true，则直接返回true
                        逻辑非      !       取反：!false为true，!true为false

                    注意点：短路与和逻辑与的区别，短路与第一个操作数为false，则不在计算后面的操作数了，直接返回false
                    实例：

                    ```java
                        public class luoji {
                            public static void main(String[] args){
                                boolean b1 = true;
                                boolean b2 = false;
                                System.out.println(b1&b2);  //false
                                System.out.println(b1|b2);  //true
                                System.out.println(b1^b2);  //true
                                boolean b3 = 1>2&&2>(3/0);  
                                System.out.println(b3);     //false 
                            }
                        }
                    ```

            5：位运算符
                概念：位运算指的是进行二进制的运算
                    ~       取反
                    &       按位与
                    |       按位或
                    ^       按位异或
                    <<      左移运算符，左移一位相当于乘2
                    >>      右移运算符，右移一位相当于除2取商

                    实例：

                        ```java
                            public class opera4 {
                                public static void main(String[] args){
                                    int a = 3;
                                    int b = 4;
                                    System.out.println(a&b);    //0
                                    System.out.println(a|b);    //7
                                    System.out.println(a^b);    //7
                                    System.out.println(~a);     //-4
                                    
                                    //移位运算
                                    int c = 3 <<2;
                                    int d = 3 >> 2;
                                    System.out.println(c);      //12
                                    System.out.println(d);      //0
                                }
                            }
                        ```

            6：条件运算符(和js里面的三元运算符差不多)
                语法格式：x?y:z
                    语法解释：其中x为boolean类型表达式，先计算x的值，若为true，则整个运算的结果为表达式y的值，否则整个运算结果为表达式z的值

                    事例：

                        ```java
                            public class opera4 {
                                public static void main(String[] args){
                                    //条件运算符
                                    int score = 80;
                                    int x = -100;
                                    String type = score < 60?"不及格":"及格";
                                    System.out.println(type);               //输出及格
                                    System.out.println(x>0?1:(x==0?0:-1));  //输出-1
                                }
                            }
                        ```

            7：字符串连接符
                概念："+"号，就是字符串连接符，运算符两侧的操作数中只要有一个是字符串类型，系统会自动将另一个操作数转换为字符串然后进行连接
                实例：

                    ```java
                        public class opera4 {
                            public static void main(String[] args){
                                String c = "3";
                                int a = 3;
                                int b = 4;
                                char d = 'a';
                                System.out.println(c + a + b);//输出334
                                System.out.println(a + b + c);//输出73，执行机制为：a和b先进行加法运算为7，再和字符串c进行拼接，
                                System.out.println(d + 4);	//输出101，这边d在Unicode是97，97+4 =101
                            }
                        }
                    ```

            8：运算符的优先级
                概念：  括号运算符，一元运算符，位逻辑运算符，递增与递减运算符，。。。。。。
                注意点：逻辑非>逻辑与>逻辑或

            
        **类型转换
            1：自动类型转换
                    概念：自动类型转换指的是容量小的数据类型可以自动转换为容量大的数据类型
                    事例：

                        ```java
                            public class opera4 {
                                public static void main(String[] args){
                                    int a = 324;
                                    long b = a;	//a可以赋值给b，说明a可以自动转换成long类型
                                    double d = b;//long类型可以转换为double类型，但存在精度问题
                                    //a = b;long类型转换为int类型会报错，不能从容量大的类型转换为容量小的类型
                                }
                            }
                        ```
            2：强制类型转换
                概念：强制类型转换，又被称为造型，用于显示的转换一个数值的类型。在有可能丢失信息的情况下进行的转换是通过造型来完成的，但可能造成精度降低或溢出

                语法：(type)var
                    语法解释：运算符"()"中的type表示将值var想要转换成的目标数据类型

                    实例：

                    ```java
                        public class lleiee {
                            public static void main(String[] args){
                                double x = 3.12;
                                int b = (int)x;
                                System.out.println(b); //输出3
                            }
                        }
                    ```           

            3：类型转换注意点
                操作比较大的数时，要留意是否溢出
                命名时不要用小写的l，要用大写的L

                实例：

                    ```java
                        public class lleiee {
                            public static void main(String[] args){
                                int money = 1000000000;
                                int years = 20;
                                int totaone = money*years;//返回的totaone仍然是负数，超过了int的范围
                                System.out.println(totaone);
                                long total = money*((long)years);
                                //返回的total正确，因为先将一个因子变成long，整个表达式发生提升。全部用long来计算
                                System.out.println(total);
                                long total2 = money*30L;
                                System.out.println(total2);
                            }
                        }
                    ```


        **控制语句
            概念：流程控制语句是用来控制程序中各语句执行顺序的语句

            1：顺序结构

            2：选择结构(和js里面的差不多)
                概念："如果...，则..."的逻辑
                1：if单选择结构
                    语法：
                        if(布尔表达式){
                            语句块
                        }
                    注意点：可以不加大括号，但是如果不加大括号只执行第一句代码；

                2：if-else双选择结构
                    语法：
                        if(布尔表达式){
                            语句块1
                        }else{
                            语句块2
                        }

                3：if-else if-else多选择结构
                    语法：
                        if(布尔表达式){
                            语句块1
                        }else if(布尔表达式){
                            语句块2
                        }else{
                            语句块3
                        }

                4：switch结构
                    语法：
                        switch(表达式){
                            case 值1:
                                语句序列1;
                                break;
                            case 值2:
                                语句序列2;
                                break;
                            ........
                            ........
                            default:
                                默认语句；
                                break;
                        }
                        注意点：jdk1.7版本以后case值可以是字符串了，版本以前不可以使用字符串，必须是整数(long类型除外)或者枚举

            3：循环结构
                概念："如果...,则再继续"的逻辑

                1：while循环
                    while(布尔值表达式){
                        循环体;
                    }

                2：do...while循环
                    do{
                        循环体;
                    }while(布尔表达式)
                    
                3：for循环
                    语法：
                        for(初始化,条件判断,i++){
                            循环体;
                        }
                    for循环执行机制：
                                1:执行初始化语句int i=1;
                                2:判断条件
                                3：执行循环体
                                4：步进迭代i++;
                                5:回到第二步继续判断；
                    注意点：初始化变量作用域只能在for循环体内被访问到，在外部不能被访问(类似js里面的let声明)

            4：循环嵌套
                概念：在一个循环语句内部再嵌套一个或多个循环，称为循环嵌套

            5:break和continue
                1：break
                    概念：break用于强行退出循环，不执行循环中剩余的语句

                2：continue
                    概念：continue用来退出本次循环，循环中的剩余语句还是执行

                3：带标签的break和continue
                    概念："标签"是指后面跟一个冒号的标识符，例如:"label"。对java来说唯一用到标签的地方是在循环语句之前。而在循环之前设置标签的唯一理由是：我们希望在循环中嵌套另外一个循环，由于break和continue关键字通常只中断当前循环，但若随同标签使用，它们就会中断到存在标签的地方

                    实例： 打印出101到150之间所有的质数

                        ```java
                            public class Biaoqian {
                                public static void main(String[] args){
                                    outer:for(int i=101;i<150;i++){
                                        for(int j = 2;j < i/2;j++){
                                            if(i % j == 0){
                                                continue outer;
                                            }
                                        }
                                        System.out.print(i + "   ");
                                    }
                                }
                            }
                        ```
                        实例中标签的执行机制：
                            当i%j==0的时候，进入if语句，执行continue outer;不会执行本次循环且会根据标签outer跳转到外部for循环执行程序
                            如果不写标签的outer，当执行到continue语句，会跳到里面的循环不会跳转到外面的循环了


        **方法  (和js里面的函数方法概念差不多)
            概念：方法就是一段用来完成特定功能的代码片段，类似于其他语言的函数
            方法声明格式：
                [修饰符1  修饰符2] 返回值类型   方法名(形式参数列表){
                    java语句;
                }
            方法的调用方式
                对象名.方法名(实参列表)
                方法的详细说明
                    1：形式参数：在方法声明时用于接收外界传入的数据
                    2：实参：调用方法时实际传给方法的数据
                    3：返回值：方法在执行完毕后返回给调用它的环境的数据
                    4：返回值类型：事先约定的返回值的类型，如无返回值，必须显式指定位void

                    注意点：
                        1：return的作用(和js里面的类似)：1结束方法的运行 2：返回值
                        2：实参的数目，数据类型和次序必须和所调用的方法声明的形式参数列表匹配
                        3：java中进行方法调用中传递参数时，遵循值传递的原则(传递的都是数据的副本)
                        4：基本类型传递的是该数据值的copy值
                        5：应用类型传递的是该对象引用的copy值，但指向的是同一个对象
                        6：方法的使用；可以调用当前类的属性或方法
                        7：方法中不可以定义方法

            1：方法的重载
                1：概念，重载的方法，实际是完全不同的独立的方法，只是名称相同而已

                2：构成方法重载的条件：
                        1：不同的含义：形参类型，形参个数，形参顺序不同
                        2：只有返回值不同不构成方法的重载


            2：递归
                1：概念：方法自己调用自己

                2：递归结构包括两个部分：
                        1：定义递归头：解答：什么时候不调用自身方法。如果没有头，将陷入死循环，也就是递归的结束条件
                        2：递归体。解答：什么时候需要调用自身方法
                
                3：递归的缺陷：简单的程序是递归的有点之一。但时递归调用会占用大量的系统堆栈，内存耗用多，在递归调用层次多时速度要比循环慢的多，所以在使用递归时要谨慎

                4：注意点：任何能用递归解决的问题也能使用循环迭代解决
                        在要求高性能的情况下尽量避免使用递归，递归调用既花时间又耗内存
                

        **对象和类  (类和对象的概念和js里面的是类似的)
            1：面向对象和面向过程的区别
                1：都是解决问题的思维方式，都是代码组织的方式
                2：解决简单问题可以使用面向过程
                3：解决复杂问题：宏观上使用面向对象把握，微观处理上仍然是面向过程



            2：注意点：一个java文件可以有很多class类，但是只能有一个public修饰类

                实例：

                    ```java
                        class Computer {
                            String brand;
                        }
                        public class SxtStu {
                            int id;
                            String sname;
                            int age;
                            Computer comp;
                            void study(){
                                System.out.println("我正在学习，使用的电脑是"+comp.brand);
                            }
                            SxtStu(){} //这是构造函数

                            public static void main(String[] args){
                                SxtStu stu1 = new SxtStu();  //stu1的类型是引用类型SxtStu
                                stu1.sname = "石惠";
                                Computer comp1 = new Computer();
                                comp1.brand = "联想";
                                stu1.comp = comp1;
                                stu1.study();   //输出结果：我正在学习，使用的电脑是联想
                            }
                        }
                    ```

            3：内存分析    
                概念：java虚拟机的内存可分为三个区域：栈stack，堆heap，方法区(静态区)method area   (记住那张图)

                java执行程序的机制：javac编译文件成，java + 类名(也就是调用java虚拟机执行这个类，要知道java虚拟机本来就有三个区域)，java执行这个类，将代码加载到内存空间里面执行；接下来就是我知道的在栈，堆，方法区里面执行的过程

                栈的特点：
                    1：栈描述的是方法执行的内存模型。每个方法被调用都会创建一个栈帧(存储局部变量，操作数，方法出口等)
                    2：JVM为每个线程创建一个栈，用于存放该线程执行方法的信息(实际参数，局部变量等)
                    3：栈属于线程私有，不能实现线程间的共享
                    4：栈的存储特性是"先进后出，后进先出"
                    5：栈是由系统自动分配，速度快，栈是一个连续的内存空间

                堆的特点：
                    1：堆用于存储创建好的对象和数组(数组也是对象)
                    2：JVM只有一个堆，被所有线程共享
                    3：堆是一个不连续的内存空间，分配灵活，速度慢
                    堆的运用实例：当类被new的时候，就在堆里产生了一个对象

                方法区(又叫静态区)特点：
                    1：JVM只有一个方法区，被所有线程共享
                    2：方法区实际也是堆，只是用于存储类，常量相关的信息
                    3：用来存放程序中永远是不变或唯一的内容。(类信息 class对象，静态变量，字符串常量等)

            
            4：构造器
                4-1：概念：构造器也叫构造方法(constructor),用于对象的初始化

                4-2：要点：
                    1：通过new关键字调用
                    2：构造器虽然有返回值，但是不能定义返回值类型(返回值的类型肯定是本类)，不能在构造器里使用return返回某个值
                    3：如果我们没有定义构造器，则编译器会自动定义一个无参的构造函数，如果已定义则编译器不会自动添加
                    4：构造器的方法名必须和类名一致

                    事例：

                        ```java
                            class Point{
                                double x,y;
                                public Point(double _x,double _y){
                                    this.x = _x;
                                    this.y = _y;
                                }
                                
                                public double getDistance(Point p){
                                    return Math.sqrt(x-p.x)*(x-p.x)+(y-p.y)*(y-p.y);
                                }
                            }

                            public class Gouzao {
                                
                                public static void main(String[] args){
                                    Point p = new Point(1.0,2.0);
                                    Point origin = new Point(0,0);
                                    System.out.println();
                                    System.out.println(p.getDistance(origin));
                                    
                                }
                            }
                        ```

                4-3：构造方法的重载
                    概念：构造方法的重载和方法的重载是一样的；都是方法名称相同，形参列表不同

                        事例：

                            ```java
                                public class Chongzai {
                                    int id;
                                    String name;
                                    String pwd;
                                    
                                    public Chongzai(){
                                        
                                    }
                                    public Chongzai(int id,String name){
                                        this.id = id;
                                        this.name = name;
                                    }
                                    public Chongzai(int id,String name,String pwd){
                                        this.id = id;
                                        this.name = name;
                                        this.pwd = pwd;
                                    }
                                    public static void main(String[] args){
                                        Chongzai u1 = new Chongzai();
                                        Chongzai u2 = new Chongzai(101,"石惠");
                                        Chongzai u3 = new Chongzai(101,"小石","12331321");
                                    }
                                }
                            ```


        **对象
            1：对象创建的过程
                1：分配对象空间，并将对象成员变量初始化为0或空
                2：执行属性值的显式初始化
                3：执行构造方法
                4：返回对象的地址给相关的变量
            

            2：this
                概念：this的本质就是"创建好的对象的地址"由于在构造方法调用前，对象已经创建。因此，在构造方法中也可以使用this代表"当前对象"

                用法：
                    1：在程序中使用this来指明当前对象：普通方法中，this总是指向调用该方法的对象。构造方法中，this总是指向正要初始化的对象
                    2：使用this关键字调用重载的构造方法，避免相同的初始化代码。但只能在构造方法中用，并且必须位于构造方法的第一句
                    3：this不能用于static方法中

                    实例：

                        ```java
                            public class User {
                                int a,b,c;
                                
                                User(int a,int b){
                                    this.a =a;
                                    this.b =b;
                                }
                                
                                User(int a,int b,int c){
                                    this(a,b);      
                                                    //this调用重载的构造方法，省去了相同的初始化代码，但只能在构造方法中这样用，且必须放在构造方法的第一句
                                    this.c = c;
                                }
                                
                                void eat(){
                                    System.out.println("今天天气真好，出去吃饭");
                                }
                                
                                public static void main(String[] args){
                                    User meat = new User(2,3);
                                    meat.eat();
                                }
                                
                            }
                        ```

            3:static
                概念：
                    静态的，static可以用来修饰：属性，方法，代码块，内部类
                理解static的原理：从静态属性，静态方法和非静态属性，飞静态方法的是否可以被调用，从类和对象的生命周期考虑就行，一目了然

                2：static修饰属性，静态变量(或类变量)
                    1：属性：按是否使用static修饰，分为：静态属性vs非静态属性(实例变量)
                        实例变量：我们创建了类的多个对象，每个对象都独立的拥有一个套类中的非静态属性。当修改其中一个对象中的非静态属性时，不会导致其他对象中同样的属性值的修改

                        静态变量：我们创建了类的多个对象，多个对象共享同一个静态变量。当通过某一个对象修改静态变量时，会导致其他对象调用此惊天变量时，是修改过了的
                    
                    2：static修饰属性的其他说明
                        1：静态属性随着类的加载而加载。可以通过"类.静态变量"的方式进行调用
                        2：静态变量的加载要早于对象的创建(也是对象可以调静态属性的原因)
                        3：由于类只会加载一次，则静态变量在内存中也只会存在一份；存在方法区的静态域中
                        4：         类变量          实例变量
                        类           yes              no
                        对象         yes              yes

                        事例：

                            ```java
                                public class StaticTest{
                                    public static void main(String[] args){
                                        Chinese c1 = new Chinese();
                                        c1.nation = "CHA";

                                        Chinese c2 = new Chinese();
                                        c2.nation = "CHINA";

                                        System.out.println(c1.nation);//输出 "CHINA"

                                    }
                                }
                                class Chinese{
                                    static String nation;
                                }
                            ```
                3：使用static修饰方法，静态方法
                    1：随着类的加载而加载，可以通过"类.静态方法"的方式进行调用
                    2：         静态方法        非静态方法
                        类        yes              no
                        对象      yes              yes

                    3：静态方法中，只能调用静态的方法或属性；
                        非静态方法中，即可以调用非静态的方法或属性，也可以调用静态的方法或属性
                
                4：static注意点
                    1：在静态的方法内，不能使用this关键字，super关键字
                    2：关于静态属性和静态方法的使用，大家都从生命周期的角度去解释

                5：开发中，如何确定一个属性是否要声明为static的？
                        属性可以被多个对象所共享的，不会随着对象的不同而不同的
                   开发中，如何确定一个方法是否要声明为static的？
                        操作静态属性的方法，通常设置为static的
                        工具类中的方法，习惯声明为static的。比如：Math,Arrays,Collections

                
                
                注意点：
                    静态方法和属性可以在非静态方法里面用
                    静态方法里面不可以用非静态属性和方法

                
            4：静态初始化块
                概念：构造方法用于对象的初始化；静态初始化块用于类的初始化操作，在静态初始化块中不能直接访问非static成员

                静态初始化块执行顺序：
                    1：上溯到Object类，先执行Object的静态初始化块，再向下执行子类的静态初始化块，直到我们的类的静态初始化块为止
                    2：构造方法执行顺序和上面顺序一样

                注意点：
                    在类的初始化里不能使用对象的属性
                    先加载静态初始化类再加载构造器
                原理：编译器编译的时候，首先是初始化类，通过类再初始化对象，先有类后有对象。
                
                    事例：

                    ```java
                        public class Jingtai {
                            int id;
                            String name;
                            String pwd;
                            static String company;
                            static {
                                System.out.println("执行类的初始化工作");
                                company = "石氏集团";
                                printCompany();
                            }
                            public static void printCompany(){
                                System.out.println(company);
                            }
                            public static void main(String[] args){
                                
                            }
                        }
                    ```

            5：参数传值机制
                概念：java方法中所有参数都是"值传递"，也就是"传递的是值的副本"。也就是说，我们得到的是"原参数的复印件，而不是原件"。因此，复印件改变不会影响原件

                引用类型参数传值
                    传递的是值的副本。但是引用类型指的是"对象的地址"。因此，副本和原参数都指向了同一个"地址"，改变"副本指向地址对象的值，也意味着原参数指向对象的值也发生了改变"

            
                



        **垃圾回收机制
            1：内存管理
                java的内存管理很大程度指的就是对象的管理，其中包括对象空间的分配和释放
                对象空间的分配：使用new关键字创建对象即可
                对象空间的释放：将对象赋值null即可。垃圾回收器将负责回收所有"不可达"对象的内存空间
            
            2：垃圾回收过程
                任何一种垃圾回收算法一般要做两件基本事情：
                    1：发现无用的对象
                    2：回收无用对象占用的内存空间

            3：垃圾回收相关算法
                1：引用计数法
                    概念：堆中每个对象都有一个引用计数。被引用一次，计数加1，被引用变量值变为null，则计数减1，查到计数为0，则表示变成无用对象。
                    优点：算法简单
                    缺点："循环引用的无用对象"无法被识别
                2：引用可达发(根搜索算法)
                    概念：程序把所有的引用关系看作一张图，从一个节点GC ROOT开始，寻找对应的引用节点，找到这个节点以后，继续寻找这个节点的引用节点，当所有的引用节点寻找完毕之后，剩余的节点则被认为是没有被引用到的节点，即无用节点，则会垃圾回收机制被处理掉

            4：通用的分代垃圾回收机制
                概念：分代垃圾回收机制，是基于这样的一个事实：不同的对象生命周期是不一样的。因此，不同生命周期的对象可以采取不同的回收算法，以便提高回收效率。我们将对象分为三种状态：年轻代，年老代，持久代。JVM将堆内存划分为Eden，Survivor和Tenured/Old空间

                    1：年轻代
                        所有新生成的对象首先都是方在Eden区
                    2：年老代
                        在年轻代中经历了N(默认15)次垃圾回收后仍然存活的对象，就会被放到年老代中。因此可以认为年老代中存放的都是一些生命周期较长的对象。年老代对象越来越多，我们就需要启动Major GC和Full Gc(全量回收)，来一次大扫除，全面清理年轻代区域和年老代区域
                    3：持久代
                        用于存放静态文件，如java类，方法等。持久代对应垃圾回收没有显著影响

                    4：垃圾回收过程
                        1：新创建的对象，会存储在Eden中
                        2：当Eden满了(达到一定比例)不能创建新对象，则触发垃圾回收(Minor GC)，将无用对象清理掉，然后剩余对象复制到某个Survivor中，如S1，同时清空Eden区
                        3：当Eden区再次满了，会将S1中的不能清空的对象存到另外一个Survivor中，如S2，同时将Eden区中的不能清空的对象，也复制到S1中，保证Eden和S1，均被清空
                        4：重复多次(默认15次)Survivor中没有被清理的对象，则会复制到年老代Old(Tenured)中
                        5：当Old区满了，则会触发一个一次完整地垃圾回收机制(FullGC)，之前新生代的垃圾回收称为(minnorGC)

                            说明：
                                Minor GC：用于清理年轻代取余。Eden区满了就会触发一次Minor GC。清理无用对象，将有用对象复制到"Survivor1","Survivor2"区中(这两个区，大小空间也相同，同一时刻Survivor1和Survivor2只有一个在用，一个为空)

                                Major GC：用于清理老年代区域
 
                                Full GC：用于清理年轻代，年老代区域。成本较高，会对系统性能产生影响

                    5：JVM调优的过程中，很大一部分工作就是对于Full GC的调节。有如下原因可能导致Full GC
                            1：年老代(Tenured)被写满
                            2：持久代(Perm)被写满
                            3：System.gc()被显式调用(这里在程序中写System.gc()表示不是调用只是建议GC启动，不是调用GC)
                            4：上一次Gc之后Heap的各域分配策略动态变化


            5：开发中容易造成内存泄露的操作：
                1：创建大量无用对象
                    比如：我们在需要大量拼接字符串时，使用了String而不是StringBuilder
                2：静态集合类的使用
                    像HashMap，Vector，List等的使用最容易出现内存泄露，这些静态变量的生命周期和应用程序一致，所有的对象Object也不能被释放
                3：各种连接对象(IO流对象，数据库连接对象，网络连接对象)未关闭
                    IO流对象，数据库连接对象，网络连接对象等连接对象属于物理连接，和硬盘或者网络连接，不使用的时候一定要关闭
                4：监听器的使用
                    释放对象时，没有删除相应的监听器


        **包(package)
            1：概念：包机制是java中管理类的重要手段。开发中，我们会遇到大量同名的类，通过包我们很容易解决类重名的问题，也可以实现对类的有效管理。包对于类，相当于文件夹对于文件的作用。

            2：package作用：
                通过package实现对类的管理

            3：package的使用有两个要点：
                1：通常是类的第一句非注释性语句
                2：包名：域名倒着写即可，再加上模块名，便于内部管理类

            4：JDK中的主要的包：
                java中的常用包
                    java.lang       包含一些java语言的核心类，如String，Math，Integer，System和Thread，提供常用功能
                    java.awt        包含了构成抽象窗口工具集(abstract window toolkits)的多个类，这些类被用来构建和管理应用程序的图形用户界面
                    java.net        包含执行与网络相关的操作的类
                    java.io         包含能提供多种输入/输出功能的类
                    java.util       包含一些实用工具类，如定义系统特性，使用与日期日历相关的函数

                    其中java.lang包是不需要import引入的，直接可以用

                事例：

                    ```java
                        package cn.sxt.oo;  //是类的第一句非注释性的语句

                        public class User {

                        }
                    ```
                        事例解释：最终会把User类打包到cn.sxt.oo这个包里面

            5：import详解
                1：作用：导入包的作用
                2：注意点：在同一个包里面的类，不需要通过import引入，直接可以使用
                3：用法：
                    事例：

                    ```java
                        package cn.hui; //package cn.hui 的作用是 这个java文件里面的类，最后会放到cn.shi的这个包里
                        // 第一种引入包的方式
                        import cn.shi.User;
                        public class Data {
                            public static void main(String[] args){
                            // 第二种引入包的方式
                            // cn.shi.User user = new cn.shi.User();
                            //user.say();
                            }
                        }
                    ```
                
                4：静态导入
                    概念：静态导入是在JDK1.5新增加的功能，其作用是用于导入指定类的静态属性，这样我们可以直接使用静态属性
                    事例：

                    ```java
                        package cn.hui;
                        //静态导入的方式
                        import static java.lang.Math.*;
                        public class Test {
                            public static void main(String[] args){
                                System.out.println(PI);
                            }
                        }
                    ```


        **继承
            1:概念：
                继承让我们更加容易实现类的扩展。实现代码的重用，不用再重新发明轮子
                从英文字面理解，extends的意思是"扩展"。子类是父类的扩展。(子类继承父类的东西，但是不是所有的东西都可以用)

            2:要点：
                1：父类也称作超类，基类，派生类。
                2：java中只有单继承，没有像C++那样的多继承。多继承会引起混乱，使得继承链过于复杂，系统难于维护
                3：java中类没有多继承，接口有多继承
                4：子类继承父类，可以得到父类的全部属性和方法(除了父类的构造方法)，但不见得可以直接访问(比如，父类私有的属性和方法)
                5：如果定义一个类，没有调用extends，则它的父类默认是：java.lang.Object

            快捷键：可以使用ctrl+T方便的查看类的继承层次结构

                实例：

                    ```java
                        public class extend {
                            public static void main(String[] args){
                                Student stu = new Student("shihui",18,"软件工程");
                                stu.study();
                                stu.rest();
                            }
                        }

                        class Person {
                            String name;
                            int height;
                            public void rest(){
                                System.out.println("休息一会儿");
                            }
                        }

                        class Student extends Person {
                            String major;
                            public void study(){
                                System.out.println("学习两小时");
                            }
                            public Student(String name,int height,String major){
                                this.name = name;
                                this.height = height;
                                this.major = major;
                            }
                            public Student(){
                                
                            }
                        }
                    ```

        
            3:instanceof运算符
                概念：instanceof是二元运算符，左边是对象，右边是类；当对象是右边类或子类所创建对象时，返回true；否则返回false
                作用：判断对象是不是此类的类型

                实例：

                    ```java
                        public class Users {
                            public static void main(String[] args){
                                Student stu = new Student("shihui",18,"软件工程");
                                stu.study();
                                stu.rest();
                                System.out.println(stu instanceof Person);      //返回 true 说明：stu实例对象的类型是 Person
                                System.out.println(stu instanceof Student);     //返回 true 说明：stu实例对象的类型是 Student

                                //事例理解：a instanceof A，a instance B；如果两个返回都为true，那么B肯定是A的父类
                            }
                        }

                        class Person {
                            String name;
                            int height;
                            public void rest(){
                                System.out.println("休息一会儿");
                            }
                        }

                        class Student extends Person {
                            String major;
                            public void study(){
                                System.out.println("学习两小时");
                            }
                            public Student(String name,int height,String major){
                                this.name = name;
                                this.height = height;
                                this.major = major;
                            }
                            public Student(){
                                
                            }
                        }
                    ```

        **对象
            1：方法的重写override
                1：概念：子类重写父类的方法，
                2：方法重写需要符合下面三个要点：
                    1："=="：方法名，形参列表相同
                    2："<="：返回值类型和声明异常类型，子类小于等于父类
                            事例：

                                ```java
                                    public class extend {
                                        public static void main(String[] args){
                                            Horse hose = new Horse();
                                            hose.run();
                                            hose.whoIs().study();   
                                        }
                                    }

                                    class Person {
                                    }
                                    class Student extends Person {
                                        public void study(){
                                            System.out.println("学习两小时");
                                        }
                                    }
                                    class Veche{
                                        public void run(){
                                            System.out.println("跑。。。。");
                                        }
                                        public Person whoIs(){
                                            return new Person();
                                        }
                                    }
                                    class Horse extends Veche {
                                        public void run(){
                                            System.out.println("得加。。。");
                                        }
                                        public Student whoIs(){     //Horse重写父类Veche的whoIs方法，返回值类型必须是小于等于父类的类型
                                            return new Student(); 
                                        }
                                    }
                                ```
                    3：">="：访问权限，子类大于父类

            
            2：==和equals方法
                1：==：
                    概念：代表比较双方是否相同。如果是基本类型则表示值相等，如果是引用类型则表示地址相等即是同一个对象
                
                2：equals
                    概念：Object类中定义有:public boolean equals(Object obj)方法，提供定义"对象内容相等"的逻辑

                    事例：

                        ```java
                            public class Tone {
                                public static void main(String[] args){
                                    Stud stu = new Stud(18,"shihui");
                                    Stud stu1 = new Stud(18,"wodetian");
                                    System.out.println(stu.equals(stu1)); //返回true
                                }
                            }
                            class Stud {
                                int id;
                                String name;
                                public Stud(int id,String name){
                                    this.id = id;
                                    this.name = name;
                                }
                                // 重写Object的equals方法
                                public boolean equals(Object obj){ 
                                    if(this == obj){
                                        return false;
                                    }
                                    if(obj == null){
                                        return false;
                                    }
                                    Stud other = (Stud) obj;
                                    if(id != other.id){
                                        return true;
                                    }
                                    return true;
                                }
                            }
                        ```

            3：super
                概念：super是直接父类对象的引用。可以通过super来访问父类中被子类覆盖的方法或属性

                构造方法调用顺序：
                    构造方法第一句总是：super(...)来调用父类对应的构造方法。所以，流程就是：先向上追溯到Object，然后再依次向下执行类的初始化块和构造方法，直到当前子类为止

                    事例：

                        ```java
                            public class Jcheng {
                                public static void main(String[] args){
                                    new Child().f();
                                }
                            }
                            class Father {
                                public int value;
                                public void f(){
                                    value = 100;
                                    System.out.println(value);
                                }
                            }
                            class Child extends Father {
                                public int value;
                                public void f(){
                                    super.f();          //调用父类对象的普通方法
                                    value = 200;
                                    System.out.println("child里面的"+value);
                                    System.out.println(value);
                                    System.out.println(super.value);    //调用父类对象的成员变量
                                }
                            }
                        ```

            4：多态
                概念：多态指的是同一个方法调用，由于对象不同可能会有不同的行为。现实生活中，同一个方法，具体实现会完全不同

                编译时多态
                    方法重载都是编译时多态。根据实际参数的数据类型，个数和次序，java在编译时能够确定执行重载方法中的哪一个
                    方法覆盖表现出两种多态性，当对象引用本类实例时，为编译时多态，否则为运行时多态。例如，以下声明p，m引用本类实例，调用toString()方法是编译时多态
                        事例：

                        ```java
                            public class Test {
                            
                                public static void main(String[] args) {
                                    Person p = new Person();         //对象引用本类实例
                                    Man m = new Man();               
                                    System.out.println(p.toString());//编译时多态，执行Person类的toString()
                                    System.out.println(m.toString()); //编译时多态，执行Man类的toString()
                                }
                            }
                            
                            class Person{
                                public String toString() {
                                    String name = "Person";
                                    return name;
                                }
                            }
                            
                            class Man extends Person{
                                public String toString(){
                                    String name = "Man";
                                    return name;
                                }
                            }
                        ```
                运行时多态
                    1:当父类引用子类实例时
                        事例：
                        ```java
                            Person p = new Man();   
                            p.toString();
                        ```
                        事例解释运行时多态：java支持运行时多态，意思为p.toString()实际执行p所引用实例toString(),究竟执行Person类还是Man类里的方法，运行时再确定。如果Man类声明了toString()方法，则执行之；否则执行Person类的toString()方法

                        运行机制：程序运行时，java从实例所属的类开始寻找匹配的方法执行，如果当前类中没有匹配的方法，则沿着继承关系逐层向上，依次在父类或各祖先类中寻找匹配方法，直到Object类


                多态的要点：
                    1：多态是方法的多态，不是属性的多态(多态与属性无关)
                    2：多态的存在要有3个必要条件：继承，方法重写，父类引用指向子类对象
                    3：父类引用指向子类对象后，用该父类引用调用子类重写的方法，此时多态就出现了

                    实例：

                        ```java
                            public class Duo {
                                public static void main(String[] args){
                                    Animal a = new Animal();
                                    animalCry(a);   //叫了一声
                                    Dog d = new Dog();
                                    animalCry(d);//汪汪汪
                                }
                                static void animalCry(Animal a){        //父类引用类型指向子类对象
                                    a.shout();
                                }
                            }
                            class Animal {
                                public void shout(){
                                    System.out.println("叫了一声");
                                }
                            }
                            class Dog extends Animal{           //继承，方法重写
                                public void shout(){
                                    System.out.println("汪汪汪");
                                }
                            }
                            class Cat extends Animal{
                                public void shout(){
                                    System.out.println("喵喵喵");
                                }
                            }
                        ```
                        事例理解：
                            1：当调用子父类同名参数的方法时，实际执行的是子类重写父类的方法
                            2：有了对象的多态性以后，我们在编译期，只能调用父类中声明的方法，但在运行期，我们实际执行的是子类重写父类的方法；总结：编译，看左边；运行看右边

                        多态以及对象转型的核心理解：Animal d = new Dog() Animal是个引用类，相当于图纸，Animal中有的d对象才能调用；



            5：对象的转型
                概念：
                    1：父类引用指向子类对象，我们称这个过程为向上转型，属于自动类型转换
                    2：向上转型后的父类引用变量只能调用它编译类型的方法，不能调用运行时类型的方法。这时，我们就需要进行类型的强制转换，我们称之为向下转型

                    实例：

                        ```java
                            public class Duo {
                                public static void main(String[] args){
                                    Animal a = new Dog();	//自动向上转型
                                    a.shout();  //汪汪汪
                                    Dog d = (Dog)a;		//强制转型
                                    d.seeDoor();	//看门。。
                                }
                            }
                            class Animal {
                                public void shout(){
                                    System.out.println("叫了一声");
                                }
                            }
                            class Dog extends Animal{
                                public void shout(){
                                    System.out.println("汪汪汪");
                                }
                                public void seeDoor(){
                                    System.out.println("看门。。");
                                }
                            }
                        ```
                    事例注意点：父类引用子类对象，对象调用父类里面没有的方法会报错。

            6：final关键字
                作用：
                    1：修饰变量：被他修饰的变量不可改变。一旦赋了初值，就不能被重新赋值
                        final int Max_speed = 120
                    2：修饰方法：该方法不可被子类重写。但是可以被重载
                        final void study(){}
                    3：修饰类：修饰的类不能被继承。比如Math，String
                        final class A {}

            
            7：抽象方法和抽象类
                抽象方法
                    概念：使用abstract修饰的方法，没有方法体，只有声明。定义的是一种"规范"，就是告诉子类必须要给抽象方法提供具体实现
                
                抽象类
                    概念：包含抽象方法的类就是抽象类。通过abstract方法定义规范，然后要求子类必须定义具体实现。通过抽象类，我们就可以做到严格限制子类的设计，使子类之间更加通用

                    抽象类特点：
                        1：有抽象方法的类只能定义成抽象类
                        2：抽象类不能实例化，即不能用new来实例化抽象类
                        3：抽象类可以包含属性，方法，构造方法。但是构造方法不能用来new实例，只能用来被子类调用
                        4：抽象类只能用来被继承
                        5：抽象方法必须被子类实现
                        6：abstract不能用来修饰私有方法，静态方法，final的方法，final类

                    实例：

                        ```java
                            public abstract class Animal {
                                //抽象方法的特点：没有实现，子类必须实现
                                abstract public	void shout(); 
                                public void run(){
                                    System.out.println("快跑");
                                }
                                public static void main(String[] args){
                                    Animal d = new Dog();
                                    d.shout();
                                    d.run();
                                }
                            }
                            class Dog extends Animal{
                                @Override
                                public void shout() {
                                    System.out.println("汪汪汪");
                                }
                            }
                        ```

            8:接口
                1：概念：
                    1：接口只定以规范，全面地实现了：规范和具体实现分离
                    2：抽象类还提供某些具体实现，接口不提供任何实现，接口中所有方法都是抽象方法。接口是完全面向规范，规定了一批类具有的公共方法规范
                    3：接口是契约是规范，必须遵守

                2：声明格式
                    [访问修饰符] interface 接口名 [extends 父接口1，父接口2...] {
                        常量定义;
                        方法定义;
                    }

                3：定义接口的详细说明：
                    1：访问修饰符：只能是public或默认
                    2：接口名：和类名采用相同命名机制
                    3：extends：接口可以多继承
                    4：常量：接口中的属性只能是常量，总是：public static final修饰。不写也是
                    5：方法：接口中的方法只能是：public abstract。省略的话，也是public abstract

                4：要点：
                    1：子类通过implements来实现接口中的规范
                    2：接口不能创建实例，但是可用于声明引用变量类型
                    3：一个类实现了接口，必须实现接口中所有的方法，并且这些方法只能是public的
                    4：JDK1.7之前，接口只能包含静态常量，抽象方法，不能有普通属性，构造方法，普通方法
                    5：JDK1.8后，接口中包含普通的静态方法

                    实例：

                        ```java
                            public class Shixian {
                                public static void main(String[] args){
                                    Volant f = new Angel(); 
                                    //要是把f实例对象的类型写成Volant，编译器就会把f当成volant，而volant里面只有一个方法，所以在这里只能调用flay方法，虽然在Angel里面有Helpother方法，但是编译器并不知道。而f里面有关于接口Volant关于fly方法的实现，所以会调用Angel里面的fly方法。类似于子类继承父类，子类重写父类的方法，调用时会调用子类的方法。若调用的方法只有父类里有，则就会调用父类里的方法
                                    f.fly(); //输出飞起来了
                                }
                            }
                            interface Volant {
                                void fly();
                                int FLY_HEIGHT = 1000;
                            }
                            interface Honest {
                                void Helpother();
                            }
                            class Angel implements Volant,Honest{
                                @Override
                                public void Helpother() {
                                    // TODO Auto-generated method stub
                                    System.out.println("帮助别人");
                                }
                                @Override
                                public void fly() {
                                    // TODO Auto-generated method stub
                                    System.out.println("飞起来了");
                                }
                            }
                            class GoodMan implements Honest {
                                @Override
                                public void Helpother() {
                                    // TODO Auto-generated method stub
                                    System.out.println("好人帮助别人");
                                }
                            }
                        ```

                5：接口的多继承
                    概念：接口完全支持多继承。和类的继承类似，子接口扩展某个父接口，将会获得父接口中所定义的一切
                    用法：
                        事例：

                            ```java
                                interface A{
                                    void testa();
                                }
                                interface B{
                                    void testb();
                                }
                                interface C extends A,B{
                                    void testc();
                                }

                                class myDuo implements C {
                                    @Override
                                    public void testa() {
                                        // TODO Auto-generated method stub
                                    }
                                    @Override
                                    public void testb() {
                                        // TODO Auto-generated method stub
                                    }
                                    @Override
                                    public void testc() {
                                        // TODO Auto-generated method stub
                                    }
                                }
                            ```

        **封装
            1：封装的具体优点：
                1：提高代码的安全性
                2：提高代码的复用性
                3："高内聚"：封装细节，便于修改内部代码，提高可维护性
                4："低耦合"：简化外部调用，便于调用者使用，便于扩展和写作

            2：访问控制符：
                private         表示私有，只有自己类能访问
                default         表示没有修饰符修饰，只有同一个包的类能访问
                protected       表示可以被同一个包的类以及其它包中的子类访问
                                    (解释：其它包中的子类是指：在其它包中创建该类的子类，可以使用该类的内部的protected控制的属性或方法)
                public          表示可以被该项目的所有包中的所有类访问

            3：封装时类的属性的处理(类的访问控制符的使用)
                1:一般使用private访问权限
                2：提供相应的get/set方法来访问相关属性，这些方法通常是public修饰的，以提供对属性的赋值与读取操作
                3：一些只用于本类的辅助性方法可以用private修饰，希望其他类调用的方法用public修饰


        **数组
            1:概念：
                数组是相同类型数据的有序集合。数组描述的是相同类型的若干数据，按照一定的先后次序排列组合而成。其中，每个数据称作一个元素，每个元素可以通过一个索引(小标)来访问

                数组属于引用类型，数组也是对象，数组中的每个元素相当于该对象的成员变量

            2:特点：
                1：长度是确定的。数组一旦被创建，它的大小就是不可以改变的
                2：其元素必须是相同类型，不允许出现混合类型
                3：数组类型可以是任何数据类型，包括基本数据类型和引用数据类型

            3:数组声明方式有两种
                1： type[] arr_name;(推荐使用这种方式)
                2： type   arr_name[];
                实例：  

                ```java
                    int[] arr01; //声明
                    arr01 = new int[]{1,2,3,4} //初始化
                    String[] arr02;
                ```

                注意点：
                    1：声明的时候并没有实例化任何对象，只有在实例化数组对象时，JVM才分配空间，这时才与长度有关
                    2：声明一个数组的时候并没有数组真正被创建
                    3：构造一个数组，必须指定长度


            4:初始化方式
                1：静态初始化
                    解释：初始化的时候直接赋值(数组的初始化和数组元素的赋值操作同时进行)
                    实例：

                        ```java
                            int[] id = new int[]{1,2,3,4,5};
                            int[] a = {1,2,3};
                            User[] b = {new User(12,"是"),new User(11,"会")};
                        ```
                2：动态初始化
                    解释：先分配空间(数组长度)，然后一个个给数组进行赋值(数组的初始化和数组元素的赋值操作分开进行)
                    事例：
                        ```java
                            int[] a = new int[2];
                            a1[0] = 1;
                            a1[1] = 2
                        ```
                3：默认初始化
                    解释：初始化数组长度之后，默认给数组的元素进行赋值。赋值的规则和成员变量默认规则一致
                    事例：
                        ```java
                            int[] a1 = new int[2]; //默认给数组a1赋值0
                        ```

                    
            5:用法：
                实例：

                    ```java
                        public class Shuzu {
                            public static void main(String[] args){
                                int[] arr01 = new int[10];	//数组内每个元素的类型是int
                                String[] arr02 = new String[5];	//数组内每个元素的类型为 String
                                User[] arr03 =new User[3]; 
                                //为User类型指的是，数组内部元素为对象的引用类型，存的是对象的地址，且应用类型为User
                                for(int i =0;i <10;i++){
                                    arr01[i] = 10*i;
                                }
                                for(int i =0;i<10;i++){
                                    System.out.println(arr01[i]);
                                }
                                
                                arr03[0] = new User(10,"石惠");
                                arr03[1] = new User(11,"石惠h");
                                arr03[2] = new User(12,"石惠shihui");
                                for(int i=0;i<3;i++){
                                    System.out.println(arr03[i].getName());
                                }
                            }
                        }
                        class User{
                            private String name;
                            public User(int id,String name){
                                this.name = name;
                            }
                            public String getName(){
                                return this.name;
                            }
                        }
                    ```

            
            6：for-each循环
                概念：增强for循环for-each是JDK1.5新增的功能，专门用于读取数组或集合中所有的元素，即对数组进行遍历
                事例：

                    ```java
                        for(int m:a){} //意思是循环a数组，把每个元素取出来之后放到m变量里面
                    ```
            
            7：数组的拷贝
                1:概念：System类里也包含了一个static void arraycopy(object src,int srcpos,object dest,int destpos,int length)方法，该方法可以将src数组里的元素值付给dest数组的元素，其中srcpos指定从src数组的第几个元素开始赋值，length参数指定将src数组的多少个元素赋给dest数组的元素

                2:用法
                    事例：将s1的数组拷贝到s2里面

                        ```java
                            public static void testBasic1(){
                                String[] s1 = {"aa","bb","cc","dd","ee"};
                                String[] s2 = new String[10];
                                System.arraycopy(s1, 2, s2, 6, 3);
                                for(int i=0;i<s2.length;i++){
                                    System.out.println(i+"数组"+s2[i]);
                                }
                            }
                        ```

                    事例：从数组中删除某个元素(本质上还是数组的拷贝)

                        ```java
                            public static void testBasic2(){
                                    String[] s1 = {"aa","bb","cc","dd","ee"};
                                    String[] s2 = new String[5];
                                    System.arraycopy(s1, 3, s1, 2, 2);
                                    s1[s1.length-1] = null;
                                    for(int i=0;i<s1.length;i++){
                                        System.out.println(s1[i]);
                                    }
                                }
                        ```

                    事例：数组的扩容(本质上是：先定义一个更大的数组，然后将原数组内容原封不动拷贝到新数组中)

                        ```java
                            public static void extendRange(){
                                    String[] s1 = {"aa","bb","cc","dd"};
                                    String[] s2 = new String[s1.length+10];
                                    System.arraycopy(s1, 0, s2, 0, s1.length);
                                    for(String item:s2){
                                        System.out.println(item);
                                    }
                                }
                        ```
            
            8:java.util.Array类
                概念：JDK提供的java.util.Arrays类，包含了常用的数组操作，方便我们日常开发。Arrays类包含了：排序，查找，填充，打印内容等常见的操作

                用法：
                    事例：

                        ```java
                            public static void testArr(){
                                    int[] a = {10,11,22,44,14,12,55};
                                    System.out.println(Arrays.toString(a));//描述数组内容的字符串，不用此方法则返回对数组内存地址的引用
                                    Arrays.sort(a);	//排序
                                    System.out.println(Arrays.toString(a));
                                    System.out.println(Arrays.binarySearch(a,44));//查找指定的元素,查找不到返回负数
                                }
                        ```


            9：多维数组
                概念：多维数组可以看成以数组为元素的数组。可以有二维，三维，甚至更多维数组，但是实际上开发中用的非常少。最多到二维数组(学习容器后，我们一般使用容器，二维数组用的都很少)

                用法：
                    实例：

                    ```java
                        public static void main(String[] args){
                                int[][] a = new int[3][];	//创建多维数组
                                a[0] = new int[]{02,30};	//给数组的第一项里面添加一个数组
                                a[1] = new int[]{10,30,80};
                                a[2] = new int[]{50,60};
                                for(int[] temp:a){
                                    System.out.println(Arrays.toString(temp));
                                }
                            }
                    ```

            10：冒泡排序算法
                概念：冒泡排序重复访问要排序的数列，一次比价两个元素，如果他们的顺序错误就把他们交换过来，这样越大的元素会经过交换慢慢到数列的顶端

                冒泡排序算法的执行机制：
                    1：比较相邻的元素。如果第一个比第二个大，就交换他们两
                    2：对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数
                    3：针对所有的元素重复以上的步骤，除了最后一个
                    4：持续每次对越来越少的元素重复上面步骤，知道没有任何一对数字需要比较

                用法：
                    实例：

                        ```java
                            public static void main(String[] args){
                                    int[] values = {3,1,2,6,5,4,7,9,8,0};
                                    int temp = 0;
                                    for(int i=0;i<values.length-1;i++){
                                        for(int j =0;j<values.length-1-i;j++){
                                            if(values[j] > values[j+1]){
                                                temp = values[j];
                                                values[j] = values[j+1];
                                                values[j+1] = temp;
                                            }
                                        }
                                    }
                                    System.out.println(Arrays.toString(values));
                                }
                        ```

            11：二分法查找
                二分法执行机制：
                    1：二分法查找，基本思想是数组中的元素从小到大有序的存放在数组中，首先将给定值key与数组中间位置上元素的key进行比较，如果相等，则检索成功
                    2：否则，若key小，则在数组前半部分继续进行二分法检索
                    3：若key大，则在数组后半部分中继续进行二分法检索；
                    4：这样，经过一次比较就缩小一半的检索区间，如此进行下去，知道检索成功或检索失败

                用法：
                    实例：

                        ```java
                            public static int myBubble(int[] arr,int value){
                                    int low = 0;
                                    int high = arr.length-1;
                                    while(low <=high){
                                        int mid = (low+high)/2;
                                        if(value == arr[mid]){
                                            return mid;
                                        }
                                        if(value > arr[mid]){
                                            low = mid +1;
                                        }
                                        if(value < arr[mid]){
                                            high = mid -1;
                                        }
                                    }
                                    return -1;
                                }
                        ```




        **类
            1：内部类  
                1:在java中内部类主要分为成员内部类(非静态内部类，静态内部类)，匿名内部类，局部内部类

                2:注意点：
                    成员内部类可以使用private,default,protected,public任意进行修饰
                    编译生成的内部类文件 外部类$内部类.class
                
                3:非静态内部类注意点：
                    1：非静态内部类必须寄存在一个外部类对象里。因此，如果有一个非静态内部类对象那么一定存在对应的外部类对象。非静态内部类对象单独属于外部类的某个对象
                    2：非静态内部类可以直接访问外部类的成员，但是外部类不能直接访问非静态内部类成员
                    3：非静态内部类不能有静态方法，静态属性和静态初始化块
                    4：外部类的静态方法，静态代码块不能访问非静态内部类，包括不能使用非静态内部类定义变量，创建实例

                    实例：

                        ```java
                            public class TestInnerclass {
                                public static void main(String[] args){
                                    //创建内部类对象
                                    //原因：普通的非静态内部类对象依托于外部类的对象，所以创建内部类对象，要先创建外部类对象。如下
                                    Outer.Inner inner = new Outer().new Inner();
                                    inner.show();
                                }
                            }
                            class Outer{
                                private int age = 10;
                                public void testOuter(){}
                                
                                class Inner {
                                    int age = 20;
                                    public void show(){
                                        int age = 30;
                                        //内部类访问外部类的属性的方法
                                        System.out.println("外部类的成员变量:"+Outer.this.age);
                                        System.out.println("内部类的成员变量:"+this.age);
                                        System.out.println("字节的变量:"+age);
                                    }
                                }
                            }
                        ```

                4:静态内部类
                    定义方式
                        static class ClassName {}
                    使用要点：
                        1：当一个静态内部类对象存在，并不一定存在对应的外部类对象。因此，静态内部类的实例方法不能直接访问外部类的实例方法
                        2：静态内部类看做外部类的一个静态成员。因此，外部类的方法中可以通过："静态内部类.名字"的方式访问静态内部类的静态成员，通过new静态内部类()访问静态内部类的实例

                        实例：

                            ```java
                                public class TestStaticinner {
                                    public static void main(String[] args){
                                        //静态内部类创建对象的方式
                                        Outer2.Inner2 inner = new Outer2.Inner2();
                                    }
                                }
                                class Outer2{
                                    static class Inner2{
                                        
                                    }
                                }
                            ```
                    
                
                5:匿名内部类
                    概念：适合哪种只需要使用一次的类。比如，键盘监听操作等等

                    语法：
                        new 父类构造器(实参类表) \实现接口() {}

                        实例：

                            ```java
                                public class TestNiming {
                                    public static void test01(AA a){
                                        a.aa();
                                    }
                                    public static void main(String[] args){
                                        TestNiming.test01(new AA(){//使用匿名内部类实现类体，实现接口
                                            @Override
                                            public void aa() {
                                                // TODO Auto-generated method stub
                                                System.out.println("匿名函数被调用");
                                            }
                                        });
                                    }
                                }
                                interface AA {
                                    void aa();
                                }
                            ```

                6:局部内部类
                    概念：类方法中的类

        
        **String类和常量池 
                    
            1：概念：
                1：String类又称不可变字符序列
                2：java字符串就是Unicode字符序列，例如字符串"java"就是4个Unicode字符"j","a","v","a"组成的
                3：java没有内置的字符串类型，而是在标准的java类库中提供了一个预定义的类String，每个用双引号括起来的字符串都是String类的一个实例
                4：String属于引用数据类型
                5：声明String类型时，使用一对"",
                6:String可以和8种基本数据类型变量做运算，且运算只能是连接运算：+，运算结果任然为String类型

            2；全局字符串常量池
                全局字符串常量池中存放的内容是类加载完成后存放到String Pool中的，在每个VM中只有一份，存放的是字符串常量的引用值(在堆中生成字符串对象实例)

            3：class文件常量池
                class常量池是在编译的时候每个class都有的，在编译阶段，存放的是常量(文本字符串，final常量等)和符号引用

            4：运行时常量池
                运行时常量池是在类加载完成之后，将每个class常量池中的符号引用值存放到运行时常量池中，也就是说，每个class都有一个运行时常量池，类在解析之后，将符号引用替换成直接引用，与全局常量池中的引用保持一致
            
                实例：

                    ```java
                        String str1 = "gaoqi";
                        String str2 = "gaoqi";
                        String str3 = new String("gaoqi");
                        System.out.println(str1 == str3)//返回false，str1引用指向"gaoqi"字符串常量池，str3指向新建的对象，两个地址不一样所以false
                        System.out.println(str1.equals(str3)) //返回true,因为equals直接比较内容
                    ```

            5：String的API
                    事例：

                        ```java
                            String s2 = " How are you! ";
                            String s3 = "shihui"
                            s = s2.trim();  //trim方法去除字符串守卫的空格。
                            s = s2.toLowerCase(); //toLowerCase转小写
                            s = s2.toUpperCase(); //toUpperCase转大写
                            s = s2.subString(4,7); //subString提取字符串：下标[4,7)不包括7
                            s = s2.startsWith("How"); //startsWith是否以How开头
                            s = s2.endsWith("How"); //endsWith是否以you结尾
                            s = s2.charAt(6); //charAt提取下标为6的字符
                            s = s2.length(); //length字符串长度
                            s = s2.equals(s3); //equals比较两个字符串是否相等
                            s = s2.equalsIgnoreCase(s3); //equalsIgnoreCase比较两个字符串是否相等(忽略大小写)
                            s = s2.indexOf("you"); //indexOf字符串s1中是否包含you
                            s = s2.replace('','&'); //replace将s2中的空格替换成&
                        ```

            6：StringBuilder和StringBuffer
                概念：
                    StringBuilder和StringBuffer是可变字符序列(在堆中的原来的对象空间里修改)，而String类是不可变字符序列(堆中原来的对象中修改不了，修改完之后，赋值给新创建的对象)

                    StringBuilder线程不安全，效率高(一般使用它)；StringBuffer线程安全，效率低

                用法：
                    事例：简单的使用

                    ```java
                        public static void main(String[] args){
                                StringBuilder sb = new StringBuilder("abcdefg");
                                System.out.println(Integer.toHexString(sb.hashCode())); //15db9742
                                System.out.println(sb); //abcdefg
                                sb.setCharAt(2,'M');
                                System.out.println(Integer.toHexString(sb.hashCode())); //15db9742
                                System.out.println(sb); //abMdefg
                            }
                    ```
                        事例说明：说明了，StringBuilder是可变字符序列，修改的是字符本身

                
                使用不可变字符序列和可变字符序列使用时的性能测试
                    事例：

                    ```java
                        public static void main(String[] args){
                                //使用String进行字符串拼接
                                String str1 = "";
                                //本质上使用StringBuilder拼接，但是每次循环都会生成一个StringBuilder对象
                                long num1 = Runtime.getRuntime().freeMemory();//获取系统剩余内存空间
                                long time1 = System.currentTimeMillis(); //获取系统的当前时间
                                for(int i=0;i<5000;i++){
                                    str1 = str1 + i;
                                }
                                long num2 = Runtime.getRuntime().freeMemory();
                                long time2 = System.currentTimeMillis();
                                System.out.println("String占用内存"+ (num2-num1));      //String占用内存12406960
                                System.out.println("String占用时间"+ (time2-time1));    //String占用时间81毫秒
                                
                                StringBuilder sb1 = new StringBuilder("");
                                long num3 = Runtime.getRuntime().freeMemory();
                                long time3 = System.currentTimeMillis();
                                for(int i=0;i<5000;i++){
                                    sb1.append(i);
                                }
                                long num4 = Runtime.getRuntime().freeMemory();
                                long time4 = System.currentTimeMillis();
                                System.out.println("StringBuilder占用内存"+ (num4-num3));      //Stringbuilder占用内存0
                                System.out.println("StringBuilder占用时间"+ (time4-time3));     //StringBUilder占用时间1毫秒
                            }
                    ```
                        测试结果：结果为Stringbuilder可变字符序列的性能比不可变字符的性能高很多。



        **包装类
            概念：java是面向对象的语言，但并不是"纯面向对象"的，因为我们经常用到的基本数据类型就不是对象。但是我们在实际应用中经常需要将基本数据类型转化成对象，以便于操作，所以产生了包装类，包装类的实质还是由基本数据类型组成的。比如：将基本数据类型存储到Object[]数组或集合中的操作等等

            理解包装类：方法中比如say(Object e);这里的e参数就不可以用int类型因为int的祖父类就是他自己，但是可以用Integer类型，Integer最终类指向Object

            包装类均位于java.lang包，八种包装类和基本类型的对应关系如下：
                            基本数据类型        包装类
                                byte            Byte
                                boolean         Boolean
                                short           Short
                                char            Charcter
                                int             Integer
                                long            Long
                                float           Float
                                double          Double

                用法：
                    实例：

                    ```java
                        public static void main(String[] args){
                                //基本数据类型转成包装类对象
                                Integer a = new Integer(3);
                                Integer b = Integer.valueOf(30);
                                //把包装类对象转成基本数据
                                int c = b.intValue();
                                double d = b.doubleValue();
                                //把字符串转成包装类对象
                                Integer e = new Integer("99999");
                                Integer f = Integer.parseInt("999888");
                                //把包装类对象转成字符串
                                String str = f.toString();
                                //常见的常量
                                System.out.println("int类型最大的整数"+Integer.MAX_VALUE);
                            }
                    ```

            2:自动装箱和拆箱
                1：自动装箱
                    概念：基本类型的数据处于需要对象的环境中时，会自动转为"对象"
                    实例：我们以Integer为例：在JDK1.5以前，这样的代码Integer i=5是错误的，必须要通过Integer i=new Integer(5)这样的语句来实现基本数据类型转换成包装类的过程；而在JDK1.5以后，java提供了自动装箱的功能，因此只需要Integer i=5这样的语句就能实现基本数据类型转换成包装类，这是因为JVM为我们执行了Integer.valueOf(5)这样的操作，这就是java的自动装箱

                2：自动拆箱
                    概念：每当需要一个值时，对象会自动转成基本数据类型，没必要再去显示调用intValue()
                    实现原理和自动拆箱的一样的，通过编译器进行转换(或者说是JVM执行了转换操作)

            3:包装类自动缓存
                用法
                    实例：

                    ```java
                        //缓存[-128,127]之间的数字，实际就是系统初始的时候，创建了[-128,127]之间的一个缓存数组
                        Integer in1 = 123;
                        Integer in2 = 123;
                        System.out.println(in1 == in2);		//true 因为123在缓存范围内
                        System.out.println(in1.equals(in2) );
                        Integer in3 = 1234;
                        Integer in4 = 1234;
                        System.out.println(in3 == in4);	//false 因为1234不在缓存范围内
                        System.out.println(in3.equals(in4));
                    ```
                    实例自动缓存原理：当我么你调用valueOf()的时候，首先检查是否在[-128,127]之间，如果在这个范围内则直接从缓存数组中拿出已经建好的对象如果不在这个范围，则创建新的Integer对象


        **时间相关类
            1：DateFormate类的作用：
                把时间对象转化成指定格式的字符串。反之，把指定格式的字符串转化成时间对象
                DateFormate是一个抽象类，一般使用它的子类SimpleDateFormate类来实现

                用法：
                    事例：测试时间对象和字符串之间的互相转换

                    ```java
                        public static void main(String[] args) throws ParseException{
                                Date d = new Date();
                                System.out.println(d);
                                System.out.println(d.getTime());	//转成毫秒
                                
                                //遇到日期处理，使用Canlendar类
                                
                                //把时间对象按照指定的格式，转成相应格式的字符串
                                DateFormat df = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
                                String str = df.format(d.getTime());
                                System.out.println(str);	//2020-08-03 02:22:38
                                
                                //把指定格式的字符串按照"格式字符串指定的格式"转成相应的时间对象
                                DateFormat df2 = new SimpleDateFormat("yyyy年MM月dd日 hh时mm分ss秒");
                                Date date = df2.parse("1983年05月10日 10时45分59秒");
                                System.out.println(date);	//Tue May 10 10:45:59 CST 1983
                                
                                //测试其他的格式字符：比如：利用D，获得本时间对象是所处年份的第几天
                                DateFormat df3 = new SimpleDateFormat("D");
                                String str3 = df3.format(new Date());
                                System.out.println(str3);
                            }
                    ```
            
            2：Canlendar日历类
                概念：
                    Canlendar类是一个抽象类，为我们提供了关于日期计算的相关功能，比如：年，月，日，时，分，秒的展示和计算
                    GregorianCalendar是Calendar的一个具体子类，提供了世界上大多数国家/地区使用的标准日历系统

                用法：
                    事例

                    ```java
                        public static void main(String[] args){
                                //获取日期的相关元素
                                Calendar calendar = new GregorianCalendar(2020,7,3,22,10,50);
                                int year = calendar.get(Calendar.YEAR);
                                int month = calendar.get(Calendar.MONTH);
                                int day = calendar.get(Calendar.DATE); //几号
                                int weekday = calendar.get(Calendar.DAY_OF_WEEK);//星期几，1：星期日，2：星期一。。。7星期六
                                System.out.println(year);	
                                System.out.println(month);	//0-11表示对应的月份
                                System.out.println(weekday);//表示星期几
                                System.out.println(day);
                                
                                
                                //设置日期的相关元素
                                Calendar c2 = new GregorianCalendar();//不传参数，默认设置当前日期
                                c2.set(Calendar.YEAR, 2090);
                                System.out.println(c2); //输出c2的日历的有关的对象的信息
                                
                                //日期的计算
                                Calendar c3 = new GregorianCalendar();
                                c3.add(Calendar.DATE, 100);	//往后一百天
                                c3.add(Calendar.YEAR, 100); //往后一百年
                                System.out.println(c3);
                                
                                //日期对象和时间对象的转化
                                Date d4 = c3.getTime();
                                Calendar c4 = new GregorianCalendar();
                                c4.setTime(new Date());
                                System.out.println(c4);
                                
                                printCalendar(c4);
                            }
                            
                            //疯转方法转换：将日期转成要想的时间格式
                            public static void printCalendar(Calendar c){
                                //打印：1918年10月10日 11:23:45 周三
                                int year = c.get(Calendar.YEAR);
                                int month = c.get(Calendar.MONTH)+1;
                                int date = c.get(Calendar.DAY_OF_MONTH);    //当月的第几天
                                int dayweek = c.get(Calendar.DAY_OF_WEEK)-1;//当月周几
                                String dayweek2 = dayweek==0?"日":dayweek+"";
                                
                                int hour = c.get(Calendar.HOUR);
                                int minute = c.get(Calendar.MINUTE);
                                int second = c.get(Calendar.SECOND);
                                
                                System.out.println(year+"年"+month+"月"+date+"日");
                            }
                    ```


        **File类
            概念：java.io.File类：代表文件和目录。在开发中，读取文件，生成文件，删除文件，修改文件的属性时经常会用到
            用法：
                事例：

                ```java
                    public static void main(String[] args) throws IOException{
                            File f = new File("dd.txt");
                            f.createNewFile();
                            System.out.println(System.getProperty("user.dir"));//当前所在的绝对路径
                            System.out.println("File是否存在"+f.exists());//File是否存在true
                            System.out.println("File是否是目录"+f.isDirectory());//File是否是目录false
                            System.out.println("File是否是文件"+f.isFile());//File是否是文件true
                            System.out.println("File最后修改时间"+new Date(f.lastModified()));//File最后修改时间Mon Aug 03 16:44:09 CST 2020
                            System.out.println("File的大小"+f.length());//File的大小0
                            System.out.println("File的文件名"+f.getName());//File的文件名dd.txt
                            System.out.println("File的目录路径"+f.getAbsolutePath());//File的目录路径C:\javalianxi\Duixiang\dd.txt


                            File f2 = new File("c:/学习/话语/大陆");
                            //boolean flag = f2.mkdir();//目录结构中只要有一个不存在，则不会创建整个目录树
                            boolean flag = f2.mkdirs();//目录结构中有一个不存在也没关系；创建整个目录树
                            System.out.println(flag);
                        }
                ```

                实例：递归遍历目录结构和树状展现

                    ```java
                        public static void main(String[] args){
                                //使用递归算法打印目录树
                                File f = new File("C:\\M-mind\\x-mind");
                                printFile(f,0);
                                
                            }
                            static void printFile(File file,int level){
                                for(int i=0;i<level;i++){
                                    System.out.print("-");
                                }
                                System.out.println(file.getName());//打印文件名字
                                if(file.isDirectory()){//判断是否是目录
                                    File[] files = file.listFiles();//返回目录下面所有的子目录和子文件
                                    for(File temp:files){
                                        printFile(temp,level+1);
                                    }
                                }
                            }
                    ```


        **枚举类型
            枚举应用场景：
                有必要定义一组常量的时候，可以使用枚举
                不要使用枚举的高级特性，枚举就是用来方便的，不要搞的太复杂
            语法格式：
                enum 枚举名{
                    枚举体 (常量列表)
                }
            实例：
                ```java
                    enum Season{
                        SPRING,SUMMER,AUTUMN,WINDER
                    }
                ```
                实例说明：所有的枚举类型隐性地继承自java.lang.Enum。枚举实质上还是类，而每个被枚举的成员实质就是一个枚举类型的实例，他们默认都是public static final修饰的。可以直接通过枚举类型名使用它们

            实例：

                ```java
                    public static void main(String[] args){
                            System.out.println(Season.SPRING);
                            Season e = Season.AUTUMN;
                            switch(e){
                                case WINDER:
                                    System.out.println("休息的季节");
                                    break;
                                case SPRING:
                                    System.out.println("播种的季节");
                                    break;
                                case SUMMER:
                                    System.out.println("大太阳的季节");
                                    break;
                                case AUTUMN:
                                    System.out.println("收获的季节");
                                    break;
                            }
                        }
                        enum Season{
                            SPRING,SUMMER,AUTUMN,WINDER
                        }
                        enum Week{
                            SUNDAY,WENSDAY,TESDAY,FRIDAY,SARTDAY
                        }
                ```


        **异常
            概念：软件程序运行过程中，非常可能遇到刚刚提到的这些问题，我们称之为异常，英文是：Exception,意思是例外
            异常机制本质：就是当程序出现错误，程序安全退出机制
            java是采用面向对象的方式来处理异常的。处理过程：
                1：抛出异常：在执行一个方法时，如果发生异常，则这个方法生成代表该异常的一个对象，停止当前执行路径，并把异常对象提交给JRE
                2：捕获异常：JRE得到该异常后，寻找相应的代码来处理该异常。JRE在方法的调用栈中查找，从生成异常的方法开始回溯，直到找到相应的异常处理代码为止
            
            异常分类：
                不同的类型的异常分别用不同的java类表示，所有异常的根类为java.lang.Throwable，Throwable下面又派生出了两个子类：Error和Exception。
                运行时异常和已检查异常

            1：运行时异常(RuntimeException)
                特点：以下都是运行时异常情况，编译时可以通过的异常，不会抛出异常；运行时会报异常

                    ```java
                        int a = o;
                        System.out.println(1/a);//这个地方会抛出RuntimeException运行时异常
                    ```
                    1:NullPointerExceptioin空指针异常
                        解决空指针异常，通常是增加非空判断

                            ```java
                                String str = null;
                                str.length(); //会报NullPointerExceptioin空指针异常
                                if(str !=null){//处理NullPointerExceptioin空指针异常的方式
                                    System.out.println(str.length());
                                }
                            ```
                    2:ClassCastException异常

                            ```java
                                Animal d = new Dog();
                                Cat c = (Cat)d; //报ClassCastException异常,因为Dog不可以转换成Cat
                                //处理ClassCastException异常的方式
                                if(d instanceof Cat){
                                    Cat d = (Cat)d;
                                }
                                class Animal {}
                                class Dog extends Animal{}
                                class Cat extends Animal{}
                            ```
                    3:ArrayIndexOutOfBoundsException异常

                            ```java
                                int[] arr = new int[5];
                                int a2 = 5;
                                arr[a2];//会报ArrayIndexOutOfBoundsException异常，也就是数组越界异常的错误
                            ```
                    4:NumberFormatException异常

                            ```java
                                public class Test7 {
                                    public static void main(String[] args){
                                        String str = "1234abcd";
                                        System.out.println(Interger.parseInt(str)); //转化为整数，但时str里面有字母，所以转换不成功，抛出NumberFormatException异常
                                    }
                                }
                            ```
                运行是异常总结：以上都是运行时异常处理可以通过自己逻辑代码规避的


            2:已检查异常(Checked Exception)
                概念：所有不是RuntimeException的异常，统称为Checked Exception，又被称为"已检查异常"，如IOException,SQLException等以及用户自定义的Exception异常。这类异常在编译时就必须做出处理，否则无法通过编译

                处理异常的2种方式：Try/catch捕获异常，使用"throws"声明抛出异常

                方式一：try-catch

                    处理异常的过程：捕获异常时通过3个关键字来实现的：try-catch-finally。用try来执行一段程序，如果出现异常，系统抛出异常，可以通过它的类型来捕捉(catch)并处理它，最后一步是通过finally语句为异常处理提供一个统一的出口，finally所指定的代码都要被执行(catch语句可有多条；finally语句最多只能有一条，根据自己的需要可有可无)

                    用法：
                        事例：

                        ```java
                            public static void main(String[] args){
                                FileReader reader = null;
                                try{
                                    reader = new FileReader("d:/b.txt"); //1   
                                    //执行步骤：读取不到d盘下的b.txt文件,下面的就不执行，会直接执行catch 打印step2，然后走finally
                                    System.out.println("step1");
                                    char c1 = (char)reader.read();
                                    System.out.println(c1);
                                }catch(FileNotFoundException e){ //2   //子类异常    子类异常要在父类异常前面，否则子类异常触发不了
                                    System.out.println("step2");
                                    e.printStackTrace();
                                }catch(IOException e){  //父类异常
                                    e.printStackTrace();
                                }finally{//3
                                    System.out.println("step3");
                                    try{
                                        if(reader !=null){
                                            reader.close()
                                        }
                                    }catch(IOException e){
                                        e.printStackTrace();
                                    }
                                }
                            }
                        ```
                            事例执行机制：读取不到d盘下的b.txt文件,下面的就不执行所以step1也就不会打印，然后执行catch步骤2打印step2，然后走finally步骤3

                方式二：throws
                    场景：
                        1：当CheckedException产生时，不一定立刻处理它，可以再把异常throws出去
                        2：在方法中使用try-catch-finally是由这个方法来处理异常。但是在一些情况下，当前方法并不需要处理发生的异常，而是向上传递给调用它的方法处理
                        3：如果一个方法中可能产生某种异常，但是并不能确定如何处理这种异常，则应根据异常规范在方法的首部声明该方法可能抛出的异常
                        4：如果一个方法抛出多个已检查异常，就必须在方法的首部列出所有的异常，之间以逗号隔开

                    说明：throws抛出异常，谁调用谁处理该异常。

                    用法：
                        事例：

                        ```java
                            public static void main(String[] args) /*throws IOException */{ 
                                //处理异常的第一种方式：方法readMyFile()将异常抛出，我们接受处理异常
                                try{
                                    readMyFile()
                                }catch(IOException e){
                                    e.printStackTrace();
                                }
                                //第二种方法：直接调用readMyFile(),在main里面也将异常抛出，给JRE处理
                            }
                            public static void readMyFile() throws IOException {    //将异常抛给谁调用readMyFile()，就将IOException异常给谁
                                FileReader reader = null;
                                reader = new FileReader("d:/b.txt");
                                char c1 = (char)reader.read();
                                System.out.println(c1);
                                if(reader !=null){
                                    reader.close()
                                }
                            }
                        ```

            3：自定义异常
                用法
                    事例：

                    ```java
                        public class Test04 {
                            public static void main(String[] args){
                                Person p = new Person();
                                p.setAge(-10);
                            }
                        }
                        class Person {
                            int age;
                            public int getAge() {
                                return age;
                            }
                            public void setAge(int age) {
                                if(age < 0){
                                    throw new IllealAge("年龄不能为负数");  
                                    //条件不满足时，throw抛出自定义的异常类IllealAge,在控制台中也会报错
                                }
                                this.age = age;
                            }
                        }
                        class IllealAge extends RuntimeException{
                            public IllealAge(){
                                
                            }
                            public IllealAge(String msg){
                                super(msg);
                            }
                        }
                    ```

            使用异常机制建议：
                1：要避免使用异常处理代替错误处理，这样会降低程序的清晰性，并且效率低下
                2：处理异常不可以代替简单测试，只在异常情况下使用异常机制
                3：不要进行小粒度的异常处理，应该将整个任务包装在一个try语句块中
                4：异常往往在高层处理


        ##容器
            概念：
                是个对象，用来装其他对象的对象
                在容器中Collection是父接口,list和set是子接口
            数组：
                数组就是一种容器，可以在其中放置对象或基本数据类型
                数组的优势：是一种简单的线性序列，可以快速地访问数组元素，效率高。如果从效率和类型检查的角度讲，数组是最好的
                数组的劣势：不灵活。容量需要事先定义好，不能随着需求的变化而扩容。比如：我们在一个用户管理中，要把今天注册的所有用户取出来，那么这样的用户有多少个？我们在写程序时就无法确定。因此，在这里就不能使用数组

            泛型：
                概念：泛型是JDK1.5以后增加的，它可以帮助我们建立类型安全的集合
                本质：就是"数据类型的参数化"。我们可以把"泛型"理解为数据类型的一个占位符(形式参数)，即告诉编译器，在调用泛型时必须传入实际类型
                用法：我们可以在类的声明处增加泛型列表，如：<T,E,V>，此处，字符可以是任何标识符，一般采用这3个字母

                事例：

                    ```java
                        public class TESTGeneric {
                            public static void main(String[] args){
                                MyCollection<String> mc = new MyCollection<String>/*new 类名<这里泛型实参可传可不传>*/();//传入的实参
                                mc.set("今天好开心", 0);
                                String b = mc.get(0);
                                System.out.println(b);
                            }
                        }
                        class MyCollection<E>{  //<E>相当于定了形参
                            Object[] objs = new Object[5];
                            public void set(E obj,int index){
                                objs[index] = obj;
                            }
                            public E get(int index){
                                return (E)objs[index]; //解释：Object的类型强制转成String，因为Object类是所有类型的父类
                            }
                        }
                    ```

            容器中使用泛型
                操作多个list交集并集
                事例：

                ```java
                    public static void test01(){
                            List<String> list01 = new ArrayList<>();
                            list01.add("aa");
                            list01.add("bb");
                            list01.add("cc");
                            List<String> list02 = new ArrayList<>();
                            list02.add("aa");
                            list02.add("dd");
                            list02.add("ee");
                            //list01.addAll(list02);	//将list02里面的元素全部添加到list01里输出list01结果为[aa, bb, cc, aa, dd, ee]
                            //list01.removeAll(list02); //将相同的部分去掉输出list01结果为[bb, cc]
                            //list01.retainAll(list02);  //只输出相同部分，输出list01结果为[aa]
                            //list01.containsAll(list02);//list01里面是否包含list02里面所有的元素，是返回true，否返回false
                            System.out.println(list01);
                        }
                ```

            **List:
                概念：
                    list是有序，可重复的容器
                    有序：List中每个元素都有索引标记。可以根据元素的索引标记(在List中的位置)访问元素，从而精确控制这些元素
                    可重复：List允许加入重复的元素。更确切地讲，List通常允许满足e1.equals(e2)的元素重复加入容器

                List接口常用的实现类有3个：ArrayList,LinkedList和Vector

                建议：
                    需要线程安全时，用Vector
                    不存在线程安全问题时，并且查找较多用ArrayList(一般使用它)
                    不存在线程安全问题时，增加或删除元素较多用LinkedList

                事例：

                ```java
                    public static void test02(){
                            List<String> list = new ArrayList<>();
                            list.add("A");
                            list.add("B");
                            list.add("C");
                            list.add("D");
                            list.add(2, "石惠");//在2位置插入新的字符串"石惠"，后面的内容会依次往后移动，输出结果[A, B, 石惠, C, D]
                            list.remove(2);//把2位置处对应的内容删掉，输出结果[A, B, C, D]
                            list.set(2, "石老二");//将2位置处的内容替换成"石老二"
                            list.indexOf("B");//返回指定的位置第一次出现的位置，不存在返回-1
                            list.lastIndexOf("B");//返回最后出现的指定元素的位置，否则返回-1
                            System.out.println(list);
                        }
                ```

                ArrayList
                    特点：
                        ArrayList底层是用数组实现的存储。特点：查询效率高，增删效率低，线程不安全
                        数组长度是有限的，而ArrayList是可以存放任意数量的对象，长度不受限制
                
                LinkedList
                    特点：增加或删除的效率非常高

                Vector
                    特点：Vector底层是用数组实现的List，相关的方法都加上了同步检查，因此"线程安全，效率低"。比如，indexOf方法就增加了synchronized同步标记

                    
            **map
                概念：map就是用来存储"键(key)-值(value)对"的。Map类中存储的"键值对"通过键来标识，所以"键对象"不能重复,如果键重复的话，新的覆盖旧的

                Map接口中实现类有HashMap,treeMap,HashTable,Properties等

                Map接口中常用的方法
                    object put(object key,Object value)     存放键值对
                    Object get(Object key)                  通过键对象查找得到值对象
                    Object remove(Object key)               删除键对象对应的键值对
                    boolean containsKey(object key)         Map容器中是否包含键对象对应的键值对
                    boolean containsvalue(Object value)     Map容器中是否包含值对象对应的键值对
                    int size()                              包含键值对的数量
                    boolean isEmpty()                       Map是否为空
                    void putAll(Map t)                      将t的所有键值对存放到本map对象
                    void clear()                            清空本map对象所有键值对

                事例：简单的例子

                    ```java
                        public class TestMap {
                            public static void main(String[] args){
                                Map<Integer,String> m1 = new HashMap<>();
                                m1.put(1, "one");
                                m1.put(2, "two");
                                m1.put(3, "three");
                                m1.put(4, "four");
                                System.out.println(m1.get(1)); //one
                                System.out.println(m1.size()); //4
                                System.out.println(m1.isEmpty());//false
                                System.out.println(m1.containsKey(2));//true
                            }
                        }
                    ```

                事例：键值对：键是Interger类型，值是Object类型

                    ```java
                        public class TestMap {
                            public static void main(String[] args){
                                Employee e1 = new Employee(1001,"石惠",50000);
                                Employee e2 = new Employee(1002,"侠士",60000);
                                Employee e3 = new Employee(1003,"老式",70000);
                                Map<Integer,Employee> m1 = new HashMap<>();
                                m1.put(1, e1);
                                m1.put(2, e2);
                                m1.put(3, e3);
                                System.out.println(m1);
                                //{1=id:1001name:石惠薪水50000.0, 2=id:1002name:侠士薪水60000.0, 3=id:1003name:老式薪水70000.0}
                            }
                        }

                        class Employee{
                            int id;
                            String ename;
                            double sarlay;
                            public Employee(int id, String ename, double sarlay) {
                                super();
                                this.id = id;
                                this.ename = ename;
                                this.sarlay = sarlay;
                            }
                            public String toString(){
                                return "id:"+id+"name:"+ename+"薪水"+sarlay;
                            }
                            public int getId() {
                                return id;
                            }
                            public void setId(int id) {
                                this.id = id;
                            }
                            public String getEname() {
                                return ename;
                            }
                            public void setEname(String ename) {
                                this.ename = ename;
                            }
                            public double getSarlay() {
                                return sarlay;
                            }
                            public void setSarlay(double sarlay) {
                                this.sarlay = sarlay;
                            }
                        }
                    ```

                
                HashMap底层实现采用了哈希表，这是一种非常重要的数据结构，哈希表的基本结构就是"数组+链表"

                    知识点介绍：
                        数据接口中由数组和链表和哈希表来实现对数据的存储，他们各有特点
                        1：数组：占用空间连续。寻址容易，查询速度快。但是，增加和删除效率非常低
                        2：链表：占用空间不连续。寻址困难，查询速度慢。但是，增加和删除效率非常高。
                        3：哈希表：哈希表结合了数组和链表的有点(即查询快，增删效率也高)，本质就是"数组+链表"
                    
                    HashMap存储数据put(key,value)的原理：
                        1：获得key对象的hashcode，首先调用key对象的hashcode()方法，获得hashcode
                        2：根据hashcode计算出hash值(要求在[0,数组长度-1区间)。通过将hash值把数据存储到对应的数组的位置，(数组中的每一项都是链表)

                    HashMap取数据get的原理：
                        1：获得key的hashcode，通过hash()散列算法得到hash值，进入定位到数组的位置
                        2：在链表上挨个比较key对象。调用equals()方法，将key对象和链表上所有节点的key对象进行比较，直到碰到返回true的节点对象为止
                        3：返回equals()为true的节点对象的value对象

                
                TreeMap
                    概念：
                        TreeMap和HashMap实现了同样的接口Map，因此，用法对于调用者来说没有区别。
                        HashMap效率高于TreeMap，在需要排序的Map时(key或者value需要排序的时候)才选用TreeMap

                    事例：

                        ```java
                            public class TestTree {
                                public static void main(String[] args){
                                    Map<Integer,String> treemap1 = new TreeMap<>();
                                    treemap1.put(20,"aa");
                                    treemap1.put(2,"VV");
                                    treemap1.put(6,"CC");
                                    //按照key递增的方式排序
                                    for(Integer key:treemap1.keySet()){
                                        System.out.println(key);
                                    }
                                    Map<Emp,String> treemap2 = new TreeMap<>();
                                    treemap2.put(new Emp(100, "张三",50000),"张三是个好小伙子");
                                    treemap2.put(new Emp(200, "李四",5000),"张三是个好小伙子");
                                    treemap2.put(new Emp(30, "王五",6000),"张三是个好小伙子");
                                    treemap2.put(new Emp(3, "金六福",6000),"张三是个好小伙子");
                                    for(Emp key:treemap2.keySet()){
                                        System.out.println(key);
                                        //输出结果
                                        // id:200,name:李四,salary:5000.0
                                        // id:3,name:金六福,salary:6000.0
                                        // id:30,name:王五,salary:6000.0
                                        // id:100,name:张三,salary:50000.0
                                    }
                                }
                            }
                            class Emp implements Comparable<Emp>{ //实现Comparable接口，方便排序
                                int id;
                                String name;
                                double salary;
                                public Emp(int id, String name, double salary) {
                                    super();
                                    this.id = id;
                                    this.name = name;
                                    this.salary = salary;
                                }
                                public String toString(){
                                    return "id:"+id+",name:"+name+",salary:"+salary;
                                }
                                @Override
                                public int compareTo(Emp o) {
                                    //TreeMap排序实现原理： Emp实例对象通过实现接口Comparable的CompareTo比较方法来进行排序
                                    //负数：小于0；等于：整数；正数：大于
                                    if(this.salary > o.salary){
                                        return 1;
                                    }else if(this.salary < o.salary){
                                        return -1;
                                    }else{
                                        if(this.id>o.id){
                                            return 1;
                                        }else if(this.id < o.id){
                                            return -1;
                                        }else{
                                            return 0;
                                        }
                                    }
                                }
                            }
                        ```

                HashMap和HashTable的区别
                    1：HashMap：线程不安全，效率高。允许key或value为null
                    2：HashTable：线程安全，效率低。不允许key或value为null

            

            **set：
                概念：Set接口继承自Collection，Set接口中没有新增方法，方法和Collection保持完全一致。我们在前面通过List学习的方法，在Set中仍然适用。因此，学习Set的使用将没有任何难度

                特点：无序，不可重复。无序指Set中的元素没有索引，我们只能遍历查找；不可重复指的是不允许加入重复的元素。更确切地讲，新元素如果和Set中某个元素通过equals()方法对比为true，则不能加入；甚至，Set中也只能发乳一个null元素，不能多个

                Set常用的实现类有：HashSet，TreeSet等。我们一般使用HashSet

                HashSet
                    事例：

                        ```java
                            public class TestHashSet {
                                public static void main(String[] args){
                                    Set<String> set1 = new HashSet<>();
                                    set1.add("aa");
                                    set1.add("bb");
                                    set1.add("cc");
                                    System.out.println(set1);//[aa, bb, cc]
                                    Set<String> set2 = new HashSet<>();
                                    set2.add("小时");
                                    set2.addAll(set1);
                                    System.out.println(set2); //[aa, bb, cc, 小时]
                                }
                            }
                        ```

                    事例：hashSet源码实现

                        ```java
                            public class SxtHashSet {
                                private static final Object PRESENT = new Object();
                                HashMap map;
                                public SxtHashSet() {
                                    super();
                                    map = new HashMap();
                                }
                                public int size(){
                                    return map.size();
                                }
                                public String toString(){
                                    StringBuilder s = new StringBuilder();
                                    s.append("[");
                                    for(Object key:map.keySet()){
                                        s.append(key + ",");
                                    }
                                    s.setCharAt(s.length()-1, ']');
                                    return s.toString();
                                }
                                public void add(Object o){
                                    map.put(o, PRESENT);
                                }
                                public static void main(String[] args){
                                    SxtHashSet  set = new SxtHashSet();
                                    set.add("aa");
                                    set.add("bb");
                                    set.add("cc");
                                    System.out.println(set);//[aa,bb,cc]
                                }
                            }
                        ```

                
                TreeSet的使用和实现
                    概念：TreeSet底层实际是用TreeMap实现的，内部维持了一个简化版的TreeMap 通过key来存储Set的元素。TreeSet内部需要对存储的元素进行排序，因此，我们对应的类需要实现Comparable接口。这样，才能根据compareTo()方法比较对象之间的大小，才能进行内部排序

                    原理实现：TreeSet的排序的实现原理和TreeMap的实现原理是一样的，可以参考TreeMap的代码

                    事例：TreeSet简单的使用

                        ```java
                            public class TestTreeSet {
                                public static void main(String[] args){
                                    Set<Integer> set = new TreeSet<>();
                                    set.add(300);
                                    set.add(200);
                                    set.add(600);
                                    for(Integer key:set){
                                        System.out.println(key);
                                        //按照顺序输出200，300，600
                                    }
                                }
                            }
                        ```

            
            **Iterator迭代器
                概念：迭代器为我们提供了统一的遍历容器(List/Set/Map>)的方式

                使用Iterator遍历List
                    事例：

                    ```java
                        public static void testIteratorList(){
                                List<String> list = new ArrayList<>();
                                list.add("aa");
                                list.add("bb");
                                list.add("cc");
                                //使用iterator遍历List
                                for(Iterator<String> iter = list.iterator();iter.hasNext();){
                                    String temp = iter.next();//遍历输出当前值，并且将浮标移动到下一位进行遍历
                                    System.out.println(temp);
                                }
                            }
                    ```

                使用Iterator遍历Set
                    事例：

                        ```java
                            public static void testIteratorSet(){
                                    Set<String> set = new HashSet<>();
                                    set.add("aa");
                                    set.add("bb");
                                    set.add("cc");
                                    //使用iterator遍历List
                                    for(Iterator<String> iter = set.iterator();iter.hasNext();){
                                        String temp = iter.next();//遍历输出当前值，并且将浮标移动到下一位进行遍历
                                        System.out.println(temp);
                                    }
                                }
                        ```

                使用Iterator遍历Map
                    事例：

                        ```java
                            public static void main(String[] args){
                                    Map<Integer,String> map = new HashMap<>();
                                    map.put(6,"小时");
                                    map.put(3,"小石");
                                    map.put(5,"石惠");
                                    Set<Integer> ss = map.keySet();
                                    for(Iterator<Integer> iterator = ss.iterator();iterator.hasNext();){
                                        Integer key = iterator.next();
                                        System.out.println(key+"---"+map.get(key));
                                    }
                                }
                        ```

            理解Set<Integer> ss = map.keySet()，调用方法后返回类名<泛型>的形式
                事例理解：

                    ```java
                        class my {
                            public static List<Integer> say(){
                                List<Integer> list = new ArrayList<Integer>();
                                list.add(1);
                                return list;
                            }
                        } 

                        List<Integer> say = my.say(); //调用方法之后返回say，say就是关于List大类，泛型是Integer的类型
                    ```


            **Collections工具类
                概念：类java.util.Collections提供了对Set,List,Map进行排序，查找元素的辅助方法
                区别：Collection是接口，Collections是工具类
                常见的方法：
                    1：void sort(list) //对List容器内的元素进行排序，排序的规则是按照升序排列
                    2：void shuffle(list) //对List容器内的元素进行随机排序
                    3：void reverse(list) //对List容器内的元素进行逆续排列
                    4：void fill(List,Object) //用一个特定的对象重写整个List容器
                    5：int binarySearch(List,Object) //对于顺序的List容器，采用折半查找的方法查找特定的对象

                    事例：

                    ```java
                        public static void main(String[] args){
                                List<String> list = new ArrayList<>();
                                for(int i =0;i<5;i++){
                                    list.add("小石"+i);
                                }
                                System.out.println(list);
                                Collections.shuffle(list);//随机排列
                                System.out.println(list);
                                Collections.reverse(list);//逆序
                                System.out.println(list);
                                Collections.sort(list);//正序排序
                                System.out.println(list);
                                System.out.println(Collections.binarySearch(list, "小石0"));//折半法查找元素，list中无查找元素则返回负数，存在返回相应index
                                Collections.fill(list,"hello");//将list中的所有元素换成hello
                                System.out.println(list);
                            }
                    ```


            **存储表格数据
                理解：存储表格数据的核心思想是对象关系映射(ORM思想)

                事例：每一行数据使用一个map，整个表格使用一个List(将数据存储在对象中)

                    ```java
                        public static void main(String[] args){
                                Map<String,Object> m = new HashMap<>();//将每行数据存储在Map对象中
                                m.put("id", 1001);
                                m.put("name", "张三");
                                m.put("薪水", 20000);
                                m.put("入职日期", "2018.5.5");
                                
                                Map<String,Object> m1 = new HashMap<>();
                                m1.put("id", 1002);
                                m1.put("name", "李四");
                                m1.put("薪水", 2000);
                                m1.put("入职日期", "2014.5.4");
                                
                                Map<String,Object> m2 = new HashMap<>();
                                m2.put("id", 1003);
                                m2.put("name", "王五");
                                m2.put("薪水", 5000);
                                m2.put("入职日期", "2019.5.10");
                                
                                List<Map<String,Object>> table1 = new ArrayList<>();//将每一行map对象，存储在List中
                                table1.add(m);
                                table1.add(m1);
                                table1.add(m2);
                                for(Map<String,Object> row:table1){
                                    Set<String> keyset = row.keySet();
                                    for(String key:keyset){
                                        System.out.print(key+"---"+row.get(key)+"\t");
                                    }
                                    System.out.println();
                                }
                            }       //输出结果：
                                    // name---张三	薪水---20000	id---1001	入职日期---2018.5.5	
                                    // name---李四	薪水---2000	id---1002	入职日期---2014.5.4	
                                    // name---王五	薪水---5000	id---1003	入职日期---2019.5.10
                    ```
                
                事例：每一行数据使用javabean对象存储，多行使用放到map或list中
                    javaBean的，java类中符合javabean的标准如下：
                        类是公共的
                        有一个无参的公共构造器
                        有属性，且有对应的get，set方法


                    ```java
                        public class TestStore {
                            public static void main(String[] args){
                                Users user = new Users(1001,"张三",20000,"2018.5.5");
                                Users user1 = new Users(1001,"张三",20000,"2018.5.5");
                                Users user2 = new Users(1001,"张三",20000,"2018.5.5");
                            
                                List<Users> list = new ArrayList<>();
                                list.add(user);
                                list.add(user1);
                                list.add(user2);
                                for(Users m:list){
                                    System.out.println(m);
                                }
                            }
                        }
                        class Users{
                            private int id;
                            private String name;
                            private double salary;
                            private String handate;
                            
                            public Users(int id, String name, double salary, String handate) {
                                super();
                                this.id = id;
                                this.name = name;
                                this.salary = salary;
                                this.handate = handate;
                            }
                            public String toString(){
                                return "id:"+id+"name:"+name+"salary:"+salary+"handate:"+handate;
                            }
                        }
                    ```



        ##IO
            概念：IO也就是流，流动，流向，从一端移动到另一端。流是一个抽象，动态的概念，是一连串连续动态的数据集合
            流分类
                方向划分：
                    输入流：数据源到程序(InputStream,Reader读进来)
                    输出流：程序到目的地(OutputStream,Write写出去)
                功能划分：
                    节点流：可以直接从数据源或目的地读写数据
                    处理流：也叫包装流，不直接连接到数据源或目的地，是其他流进行封装。目的主要是简化操作和提高性能
                    节点流和处理流的关系：
                        1：节点流处于io操作的第一线，所有操作必须通过他们进行
                        2：处理流可以对其他流进行处理(提高效率或操作灵活性)
                数据划分：
                    字节流：按照字节读取数据(InputStream,OutputStream)(字节流是计算机直接能看懂的数据)
                    字符流：按照字符读取数据(Reader,Write)，因为文件编码的不同，从而有了对字符进行高效操作的字符流对象。(字符流是人类能看懂的数据)
                        字符流原理：底层还是基于字节流操作，自动搜索指定的码表

            IO流操作文件的原理：
                java虚拟机跟操作系统os进行交互，操作系统来操作硬盘上的文件(java是不可以直接跟硬盘进行交互操作的)

            IO相关API
                事例：名称或路径

                    ```java
                        public static void main(String[] args){
                                File src = new File("C:/javalianxi/IOliu/src/img.jpg");
                                System.out.println("名称："+src.getName());//img.jpg
                                System.out.println("路径："+src.getPath());
                                //写的相对路径则返回相对路径，绝对路径则返回绝对路径；例子返回：C:\javalianxi\IOliu\src\img.jpg
                                System.out.println("绝对路径："+src.getAbsolutePath());//绝对路径   C:\javalianxi\IOliu\src\img.jpg
                                System.out.println("父路径："+src.getParent());//返回父路径，没有则返回null  C:\javalianxi\IOliu\src
                                System.out.println("父对象："+src.getParentFile().getName());//返回父路径的文件名称  src
                            }
                    ```
                
                事例：文件状态
                    1：不存在：exists
                    2：存在：文件 isFile
                            文件夹：isDirctory
                    
        
        ##线程
            程序，进程，线程的基本概念
                1：程序：是为完成特定任务，用某种语言编写的一组指令集合。即指一段静态代码，静态对象
                2：进程：是程序的一次执行过程，或是正在运行的一个程序。是一个动态的过程：有它自身的产生，存在和消亡的过程，也叫生命周期
                        如：运行中的qq，运行中的mp3播放器
                        程序时静态的，进行是动态的
                        进程作为资源分配的单位，系统在运行时会为每个进程分配不同的内存区域
                3：线程：进程可进一步细化为线程，是一个程序内部的一条执行路径。
                        若一个进程同一时间并行执行多个线程。就是支持多线程的
                        线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器，线程切换的开销小
                        一个进程中的多个线程共享相同的内存单元/内存地址空间->它们从同一堆中分配对象，可以访问相同的变量和对象。这就使得线程间通信更简便，搞笑。但多个线程操作共享的系统资源可能就会带来安全隐患

            单核CPU和多核CPU的理解
                1：单核CPU，其实是一种假的多线程，因为在一个时间单元内，也只能执行一个线程的任务。平时看到的单核CPU也能有多个程序在运行，是因为CPU的主频比较高，处理程序的比较快，多个应用在短时间内切换着被执行，但是同一个时间段只执行一个程序。外部看着就像执行多程序一样。
                2：如果是多核的话，才能更好的发挥多线程的效率。(现在的服务器都是多核的)
                3：一个java应用程序java.exe，其实至少有三个线程：main()主线程，gc()垃圾回收线程，异常处理线程。当然如果发生异常，会影响主线程
            
            并行和并发概念
                并行：多个CPU同时执行多个任务，比如，多个人同时做不同的事儿。
                并发：一个CPU(采用时间片)通知执行多个任务。比如：秒杀，多个人同时做同一件事儿

            多线程的优点
                提高应用程序的响应。对图形化界面更有意义，可增强用户体验
                提高计算机系统CPU的利用率
                改善程序结构。将即长又复杂的进程分为多个线程，独立运行，利于理解和修改

            何时需要多线程
                程序需要同时执行两个或多个任务
                程序需要实现一些需要等待的任务时，如用户输入，文件读写操作，网络操作，搜索，图片加载等
                需要后台运行的程序时；如垃圾回收机制

            概念：
                1：线程就是独立的执行路径
                2：在程序运行时，即使没有自己创建线程，后台也会存在多个线程，如垃圾回收机制线程，主线程
                3：main()称之为主线程，为系统的入口点，用于执行整个程序
                4：在一个进程中，如果开辟了多个线程，线程的运行由调度器安排调度，调度器是与操作系统紧密相关的，先后顺序是不能人为干预的
                5：对同一份资源操作时，会存在资源抢夺的问题，需要加入并发控制
                6：线程会带来额外的开销，如cpu调度时间，并发控制开销
                7：每个线程在自己的工作内存交互，加载和存储主内存控制不当会造成数据不一致

            创建线程
                创建线程的方法：
                    1：继承Thread类
                        Thread中的常用方法：
                            1：start():启动当前线程;调用当前线程的run()
                            2：run()：通常需要重写Thread类中的此方法，将创建的线程要执行的操作声明在此方法中
                            3：currentThread()：静态方法，返回执行当前代码的线程
                            4：getName()：获取当前线程的名字
                            5：setName()：设置当前线程的名字
                            6：yield()：释放当前CPU的执行权；可能下一秒又分到了CPU的执行权
                            7：join():在线程a中调用线程b的join()，此时线程a就进入阻塞状态，直到线程b完全执行完以后，线程a才结束阻塞状态，然后等待CPU给a线程分配资源
                            8：stop：已过时。当执行此方法时，强制结束当前线程
                            9：sleep(Long millitime)：让当前线程"睡眠"指定的millitime毫秒。在指定的millitime毫秒时间内，当前线程是阻塞状态，阻塞完毕之后，等待CPU分配资源
                            10：isAlive()：判断当前线程是否存活

                        步骤：
                            1：创建一个集成于Thread类的子类
                            2：重写Thread类的run()方法(run方法中是要执行的操作)
                            3：创建Thread类的子类的对象
                            4：通过此对象调用start()方法

                        事例：

                            ```java
                                public class TestTrhead {
                                    public static void main(String[] args){
                                        //创建Thread类的子类的对象
                                        MyThread t1 = new MyThread();
                                        t1.start();//通过此对象调用start()：1：启动当前线程，2：调用当前线程的run()
                                                    //不可以让已经start()的线程去执行。会报Illegal ThreadStateException

                                        // 再启动一个线程
                                        MyThread t2 = new MyThread()
                                            t2.setName("线程一") //给线程t2命名
                                        t2.start();

                                            // 给主线程命名
                                            Thread.currentThread().setName("主线程")
                                        
                                        //下面的操作任然是在main线程中执行的
                                        System.out.println("hello");
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(i+"*****");
                                            }
                                        }
                                    }
                                }
                                //常见一个继承与Thread类的子类
                                class MyThread extends Thread {
                                    //重写Thread类的run()
                                    public void run(){
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(i);
                                            }
                                        }
                                    }
                                }
                            ```
                        事例：创建Thread的匿名子类

                            ```java
                                public static void main(String[] args){
                                        //创建Thread类的匿名子类的方式
                                        //理解匿名概念：因为这里子类没有名，所以拿Thread这个父类来充当的，后面的大括号里写的是匿名子类的方法体
                                        new Thread(){
                                            public void run(){
                                                for(int i=0;i<100;i++){
                                                    if(i%2==0){
                                                        System.out.println(Thread.currentThread().getName()+":"+i);
                                                    }
                                                }
                                            }
                                        }.start();
                                        new Thread(){
                                            public void run(){
                                                for(int i=0;i<100;i++){
                                                    if(i%2==0){
                                                        System.out.println(Thread.currentThread().getName()+":"+i);
                                                    }
                                                }
                                            }
                                        }.start();
                                    }
                            ```
                        
                        线程的调度：
                            概念：高优先级的线程抢占CPU
                            用法：
                                线程的优先级：
                                        MAX_PRIORITY:10
                                        MIN_PRIORITY:1
                                        NORM_PRIORITY:5
                                设置当前线程的优先级：
                                    getpriority()：获取线程的优先级
                                    setpriority(int p)：设置线程的优先级
                                    说明：高优先级的线程要抢占低优先级线程的CPU的执行权。但是只是从概率上讲，高优先级的线程高概率的情况下被执行。并不意味着只有当高优先级的线程执行完以后，低优先级的线程才执行
                                事例：

                                    ```java
                                        public class TestTrhead {
                                            public static void main(String[] args){
                                                MyThread t2 = new MyThread();
                                                t2.setName("线程一"); //给线程t2命名
                                                //给t2线程设置优先级
                                                //先设置优先级再掉线程
                                                t2.setPriority(Thread.MAX_PRIORITY);
                                                t2.start();
                                                // 给主线程命名
                                                Thread.currentThread().setName("主线程");
                                                //给主线程设置优先级
                                                Thread.currentThread().setPriority(Thread.MIN_PRIORITY);
                                                //下面的操作任然是在main线程中执行的
                                                System.out.println("hello");
                                                for(int i=0;i<100;i++){
                                                    if(i%2==0){
                                                        System.out.println(Thread.currentThread().getName()+":"+Thread.currentThread().getPriority()+"--"+i);
                                                    }
                                                }
                                            }
                                        }
                                        //常见一个继承与Thread类的子类
                                        class MyThread extends Thread {
                                            //重写Thread类的run()
                                            public void run(){
                                                for(int i=0;i<100;i++){
                                                    if(i%2==0){
                                                        System.out.println(Thread.currentThread().getName()+":"+Thread.currentThread().getPriority()+"--"+i);
                                                    }
                                                }
                                            }
                                        }
                                    ```

                    2：实现Runnable接口
                        步骤：
                            1：创建一个实现了Runnable接口的类
                            2：实现类去实现Runnable中的抽象方法：run()
                            3：创建实现类的对象
                            4：将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
                            5：通过Thread类的对象调用start()
                        
                        特点：共享数据特点
                        
                        方法：Thread里的方法，在Runnable中也可以用
                        事例：

                            ```java
                                public class TestRunnable {
                                    public static void main(String[] args){
                                        //创建实现类的对象
                                        MThread m = new MThread();
                                        //将此对象作为参数传递到Thread类的构造器中，创建Thread了类的对象
                                        Thread t1 = new Thread(m);
                                        //通过Thread类的对象调用start();启动线程，调用当前线程的run()->调用了Runnable类型的target的run方法
                                        t1.start();
                                        
                                        //再启动一个线程
                                        Thread t2 = new Thread(m);
                                        t2.start();
                                    }
                                }
                                //1创建一个实现Runnable接口的类
                                class MThread implements Runnable{
                                    @Override
                                    //实现类去实现Runnable中的抽象方法:run()
                                    public void run() {
                                        for(int  i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(Thread.currentThread().getName()+"--"+i);
                                            }
                                        }
                                    }
                                }
                            ```
                        **说明：比较创建线程的两种方式;
                                        开发中：优先选择，实现Runnable接口的方式
                                        原因：1：实现的方式没有类的单继承性的局限性
                                              2：实现的方式更适合来处理多个想成的数据共享的情况
                                        
                                    联系：public class Thread implements Runnable
                                    相同点：两种方式都需要重写run()，将线程要执行的逻辑声明在run()中

                    4：实现Callable接口
            

            线程的同步
                为什么要用线程的同步：
                    多个线程执行的不确定性引起执行结果的不稳定
                    多个线程对账本的共享，会造成操作的不完整性，会破话数据
                    通过线程的同步可以解决这个安全问题
                
                



                    








            


        


        

                
            
                    

                












                

            

            
            

            

                



            


        







        



