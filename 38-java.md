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


##基本语法(尚学堂)
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

##数组(尚学堂)
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

    
##面向对象编程(尚学堂)
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

        
        4：构造器(硅谷)
            4-1：概念：构造器也叫构造方法(constructor),用于对象的初始化

            4-2：作用
                    >创建对象：new + 构造器：例如Person p = new Person();这里new后面加的其实是类里面的构造器
                    >初始化对象的属性

                
            4-3：说明：
                1：如果没有显示的定义类的构造器的话，则系统默认提供一个空参构造器
                2：定义构造器的格式：权限修饰符 类名(形参列表){}
                3:一个类中定义了多个构造器，彼此构成重载
                4：一旦我们显示的定义了类的构造器之后，系统就不在提供默认的空参构造器
                5：一个类中，至少会有一个构造器

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
        

        2：this(硅谷)
            概念：this的本质就是"创建好的对象的地址"由于在构造方法调用前，对象已经创建。因此，在构造方法中也可以使用this代表"当前对象"

            用法：
                1：在程序中使用this来指明当前对象：普通方法中，this总是指向调用该方法的对象。构造方法中，this总是指向正要初始化的对象
                2：this不能用于static方法中
                3：this指当前对象或当前正在创建的对象(特指构造方法)
                4：在类的方法中，我们可以使用"this.属性"或"this.方法"的方式。调用当前正在创建对象的属性或方法，但是通常情况下，我们都选中省略"this."。特殊情况下，如果方法的形参和类的属性同名时，我们必须显式的使用"this.变量"的方式，表明此变量是属性，而非形参
                5：this调用构造器
                    >我们在类的构造器中，可以显示的使用"this(形参列表)"的方式，调用本类中指定的其他构造器
                    >构造器中不能通过"this(形参列表)"方式调用自己
                    >如果一个类中有n个构造器，则最多有n-1构造器使用了"this(形参列表)"
                    >规定："this(形参列表)"必须声明在当前构造器的首行
                    >构造器内部，最多只能声明一个"this(形参列表)"，用来调用其他的构造器

                

                实例：

                    ```java
                        public class User {
                            int a,b,c;
                            
                            User(int a,int b){
                                this.a =a;   //添加this表明this.a的a是属性a，不加this的话，会被认为是形参a；不重名的话，可以不需要加this。
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

        3:static(硅谷)
            概念：
                静态的，static可以用来修饰：属性，方法，代码块，内部类
            *理解static的原理：从静态属性，静态方法和非静态属性，非静态方法的是否可以被调用，从类和对象的生命周期考虑就行，一目了然

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
                注意点的理解，可以从类和对象的生命周期考虑

            
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

        3：super(硅谷)
            super关键字的使用
                1：super理解为，父类的
                2：super可以用来调用，属性，方法，构造器
                3：super的使用
                    >我们可以在子类的方法或构造器中，通过使用"super.属性"或"super.方法"的方式。显示的调用父类中声明的属性或方法。但是，通常情况下，我们习惯省略"super."
                    >我们在子类中调用没有的属性或方法时，但是父类中有，那就会自动调用父类中的属性或方法
                    >特殊情况，当子类和父类中定义了同名的属性时，我们要想在子类中调用父类中声明的属性或方法，则必须显式的使用"super.属性/方法"的方式，表明调用的是父类中声明的属性或方法
                
                4：super调用构造器
                    >我们可以在子类的构造器中显示的使用"super(形参列表)"的方式，调用父类中声明的指定的构造器
                    >"super(形参列表)"的使用，必须声明在子类构造器的首行
                    >我们在类的构造器中，针对于"super(形参列表)"或"this(形参列表)"，只能二选一，不能同时出现
                    >在构造器的首行，没有显示的声明"super(形参列表)"或"this(形参列表)",则默认调用的是父类的空参构造器：super()
                    >在类的多个构造器中，至少有一个类的构造器使用了"super(形参列表)"，调用父类中的构造器

            子类对象实例化的全过程
                1：从结果上来看：(继承性)
                        子类继承父类以后，就获取了父类中声明的属性或方法
                        创建子类的对象，在堆空间中，就会加载所有父类中声明的属性，子类就可以直接用

                2：从过程上看
                        当我们通过子类的构造器创建子类对象时，我们一定会直接或间接的调用其父类的构造器，进而调用父类的构造器，直到调用了java.lang.object类中空参的构造器位置。正因为加载过所有的父类的结构，所以才可以看到内存中父类的结构，子类对象才可以考虑进行调动

                3：明确：虽然创建子类对象时，调用了父类的构造器，但是自始至终在堆中就创建了一个对象(可以理解为调用父类构造器就是让父类生成隐式的对象，然后将属性或方法都加载到子类当中)；即为new的子类对象


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

        4：多态(硅谷)
            1：理解多态性：可以理解为一个事物的多种形态
            2：何为多态性
                    对象的多态性，父类的引用指向子类的对象(或子类的对象付给父类的引用)
            3：多态的使用
                    有了对象的多态性以后，我们在编译期，只能调用父类中声明的方法，但在运行期，我们实际上执行的是子类重写父类的方法
                    总结：编译，看左边，运行，看右边

            4：多态性的使用前提
                    >要有类的继承关系
                    >要有方法的重写

            5：对象的多态性，只适用于方法，不适用于属性 (对于属性来说：编译和运行都看左边)

            
            6：从编译和运行的角度看
                    在方法调用之前，编译器就已经确定了所要调用的方法，这称为"早绑定"或"静态绑定"
                    对于多态而言，只有等到方法调用的那一刻，解释运行期才会确定所要调用的具体方法，这称为"晚绑定"或"动态绑定"
                
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
                        2：有了对象的多态性以后，我们在编译期，只能调用父类中声明的方法，但在运行期，我们实际执行的是子类重写父类的方法；
                        总结：编译，看左边；运行看右边

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

        6：final关键字(硅谷)
            作用：
                1：final修饰属性：此时的"变量"就被称为是一个常量；底层原理：类生成对象添加到堆中的时候，通过final修饰的属性必须要赋值。
                        >被他修饰的变量不可改变。一旦赋了初始值，就不能被重新赋值
                        >final修饰属性，可以考虑赋值的位置有，显示初始化，代码块中初始化，构造器中初始化   
                        >注意：方法中初始化不可取
                        >static final 用来修饰属性，则为全局常量(即为同一个类，new出来的对象通过static final修饰的属性都不变)


                    final int Max_speed = 120

                  final修饰局部变量
                        尤其是使用final来修饰形参时，表明此形参是一个常量。当我们调用此方法时，给常量赋一个实参，一旦赋值以后，就只能在方法体内使用此形参，但不能进行重新赋值
                        public void show(){
                            final int MAX_SLEEP = 100;
                        }
                        public void show(final int age){}

                2：修饰方法：该方法不可被子类重写。但是可以被重载
                    final void study(){}
                3：修饰类：修饰的类不能被继承。比如Math，String
                    final class A {}

        
        


##面向对象编程
    **抽象类和抽象方法(硅谷)
        abstract关键字的使用
            1：abstract可以用来修饰的结构：类，方法
            2：abstract修饰类：则为抽象类
                >此类不能实例化(但是子类可以调用父类的构造器，所以才能调用父类的非静态方法)
                >抽象类中一定有构造器，便于子类对象实例化的时候调用(涉及到子类对象实例化的全过程)
                >开发中，都会提供抽象类的子类，让子类对象实例化
            3：abstract修饰方法
                >抽象方法只有方法的声明，没有方法体
                >包含抽象方法的类一定是一个抽象类。反之，抽象类中是可以没有抽象方法的
                >若子类重写了父类中(不止是直接父类)的所有的抽象方法后，子类才可以实例化
                 若子类没有重写父类中(不止是直接父类)的所有的抽象方法，则子类也是一个抽象类，则需要abstract修饰一下
        
        抽象类应用
            抽象类是用来模型化那些父类无法确定全部实现，而是由子类提供具体实现的对象的类

        abstract使用上的注意点
            1：abstract不能用来修饰：属性，构造方法等接口
            2：abstract不能用来修饰私有方法，静态方法,final的方法，final的类

        多态的应用：模板方法设计模式
            抽象类体现的就是一种模板设计的模式，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展，改造，但子类总体上会保留抽象类的行为方式

            实例理解：    
                >当功能内部一部分实现是确定的，一部分实现是不确定的。这时可以把不确定的部分暴露出去，让子类实现
                >换句话说，在软件开发中实现一个算法时，整体步骤很固定，通用，这些步骤已经在父类中写好了。但是某些部分易变，易变部分可以抽象出来，供不同子类实现。这就是一种模板模式
        
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
    
    **匿名对象
        使用：
            1：理解：我们创建的对象，没有显式的赋给一个变量名。即为匿名对象
            2：特征：匿名对象只能调用一次
            3：使用：如下

                ```java
                    public class NIming {
                    public static void main(String[] args) {
                        new Phone().sendEmil();//匿名对象的使用
                        PhoneMail phoneMail = new PhoneMail();
                        phoneMail.show(new Phone());//匿名对象的使用，是将Phone对象赋值给了phoneMain的show方法的形参phone，phone在栈中，new Phone()对象地址指向形参phone
                    }
                    }
                    class Phone{
                        double price;
                        public void sendEmil(){
                            System.out.println("用手机发送邮件");
                        }
                        public void showPrice(){
                            System.out.println(price);
                        }
                    }
                    class PhoneMail{
                        public void show(Phone phone){
                            phone.sendEmil();
                            phone.showPrice();
                        }
                    }
                ```

    **方法  (和js里面的函数方法概念差不多)(硅谷)
        1：方法的重载
            1：概念:在同一个类中，允许存在一个以上的同名方法，只要他们的参数个数或者参数类型不同即可

            2：构成方法重载的条件：
                    1：不同的含义：形参类型，形参个数，形参顺序不同
                    2：只有返回值不同不构成方法的重载

        2：方法参数的值传递机制
            底层原理：方法中的局部变量存放在栈中(方法执行完之后，从栈中出去被销毁，方法中相应的局部变量也会被销毁)
                      对象存放在堆中
                      方法区存放常量池和静态域
            
            1：值传递机制：
                如果参数是基本数据类型，此时实参赋给形参的是实参真实存储的数据值
                如果参数是引用数据类型，此时实参赋给形参的是实参存储数据的地址值

            

        3：递归
            1：概念：方法自己调用自己

            2：递归结构包括两个部分：
                    1：定义递归头：解答：什么时候不调用自身方法。如果没有头，将陷入死循环，也就是递归的结束条件
                    2：递归体。解答：什么时候需要调用自身方法
            
            3：递归的缺陷：简单的程序是递归的有点之一。但是递归调用会占用大量的系统堆栈，内存耗用多，在递归调用层次多时速度要比循环慢的多，所以在使用递归时要谨慎

            4：注意点：任何能用递归解决的问题也能使用循环迭代解决
                    在要求高性能的情况下尽量避免使用递归，递归调用既花时间又耗内存


    **接口(硅谷)
        **接口的使用
            1：如何定义接口中的成员
                JDK7以前，只能定义全局常量和抽象方法
                    全局常量：public static final的，书写时候可以省略不写，省略的时候自动加上的
                    抽象方法：public abstract的，可以省略，不写的时候，会自动加上
                
                JDK8：除了定义全局常量和抽象方法之外，还可以定义静态方法，默认方法(略)
            
            2：接口中不能定义构造器，意味着接口不能被实例化

            3：java开发中，接口通过让类去实现(implements)的方式来使用
                如果实现类覆盖了接口中所有的抽象方法，则此实现类就可以实例化
                如果实现类没有覆盖接口中所有的抽象方法，则此实现类仍为一个抽象类

            4：java类可以实现多个接口--->弥补了java单继承性的局限性
                    格式：class AA extends BB implements CC,DD,EE

            5:接口与接口之间可以继承，而且可以多继承

            6：接口的具体使用，体现多态性
                应用场景：
                    安全代理：屏蔽对真实角色的直接访问
                    远程代理：通过代理类处理远程方法调用
                    延迟加载：先加载轻量级的代理对象，真正需要再加载真实对象
                分类：
                    静态代理(静态定义代理类)，下面的事例就是静态代理
                    动态代理(动态生成代理类)
                事例：

                    ```java
                        public class TestInterface {
                            public static void main(String[] args){
                                Computer c = new Computer();
                                //1创建了接口的非匿名实现类的非匿名对象
                                Flash f = new Flash();
                                c.transferDate(f)
                                //2创建了接口的非匿名实现类的匿名对象
                                c.transferDate(new Flash())
                                //3：创建了接口的匿名实现类的非匿名对象
                                USB p = new USB(){
                                    public void start(){
                                        System.out.println("手机开始工作了");
                                    }
                                    public void stop(){
                                        System.out.println("手机结束工作了");
                                    }
                                }
                                c.transferDate(p)
                                //4：创建了接口的匿名实现类的匿名对象
                                c.transferDate(new USB(){
                                    public void start(){
                                        System.out.println("MP3开始工作了");
                                    }
                                    public void stop(){
                                        System.out.println("MP3结束工作了");
                                    }
                                })

                            }
                        }
                        class Computer {
                            public void transferDate(USB usb){ //这边接口上说明了多态性
                                usb.start();
                                usb.stop()
                            }
                        }
                        interface USB {
                            //也可以定义常量：定义长，宽，最大最小传输速度等
                            void start();
                            void stop();
                        }
                        class Flash implements USB{
                            public void start(){
                                System.out.println("U盘开始工作了");
                            }
                            public void stop(){
                                System.out.println("U盘结束工作了");
                            }
                        }
                        class Printer implements USB{
                            public void start(){
                                System.out.println("打印机开始工作了");
                            }
                            public void stop(){
                                System.out.println("打印机结束工作了");
                            }
                        }
                    ```
            7：接口的代理模式
                概念：代理模式是java开发中使用较多的一种设计模式。代理设计就是为其他对象提供一种代理以控制对这个对象的访问

                事例：

                    ```java
                        public class TestProxy {
                            public static void main(String[] args){
                                Server server = new Server();
                                ProxyServer proxyServer = new ProxyServer(server);
                                proxyServer.browse();
                            }
                        }
                        interface NetWork {
                            void browse();
                        }
                        //被代理类
                        class Server implements NetWork {
                            public void browse{
                                System.out.println("真实的服务器访问网络");
                            }
                        }
                        //代理类
                        class ProxyServer implements NetWork{
                            private NetWork work;

                            public ProxyServer(NetWork work){
                                this.work = work;
                            }
                            public void check(){
                                System.out.println("联网之前的检查工作");
                            }
                            public void browse(){
                                check();
                                work.browse();
                            }
                        }
                    ```

            8：JDK8字后增加的知识点
                知识点1：接口中定义的静态方法，只能通过接口类调用
                
                知识点2：通过实现类的对象，可以调用接口中的默认方法

                        如果实现类重写了接口中的默认方法，调用时，任然调用的是重写以后的方法

                知识点3：如果子类(或实现类)继承的父类和实现的接口中声明了同名同参数的默认方法，那么子类在没有重写此方法的情况下，默认调用的是父类中的同名同参数的方法

                知识点4：如果实现类实现了多个接口，而这个接口中定义了同名同参数的默认方法，那么在实现类没有重写此方法的情况下，会报错--->接口冲突；这就必须在实现类中重写此方法

                知识点5：如何在子类(或实现类)的方法中调用父类，接口中被重写的方法
                    事例：

                        ```java
                            class sub extends SuperClass implements ComparaA,ComparaB{
                                public void methods3(){
                                    System.out.println("sub中的methods3");
                                }
                                public void mymethods(){
                                    methods3();//调用自己定义的重写的方法
                                    super.methods3();//调用父类中的声明的方法
                                    //调用接口中的默认方法
                                    ComparaA.super.methods3();
                                    ComparaB.super.methods3();
                                }
                            }
                        ```



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

    **封装(以下是尚学堂)
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

##异常处理(硅谷)
    **异常
        概念：其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理
            例如：空指针访问
                 试图读取不存在的文件
                 网络连接中断
                 数组角标越界

        处理异常机制本质：就是当程序出现错误，程序安全退出机制

        异常体系结构
            java.lang.Throwable
                --->java.lang.Error：一般不便携针对性的代码进行处理
                --->java.lang.Exception:可以进行异常处理
                        --->编译时异常(checked)
                            --->IOException
                                --->FileNotFountException
                            --->ClassNotFoundException
                        --->运行时异常(unchecked)
                            --->NullPointerException  空指针异常
                            --->ArrayIndexOutOfBoundsException  数组越界异常
                            --->ClassCastException     类型转换异常(本身不是这个类型的，强制转换发生的异常)
                            --->NumberFormatException  数值类型转换异常
                            --->InputMismatchException  输入时异常
                            --->ArithmeticException   算数的异常

        异常处理机制：
            异常处理机制：是将异常处理的程序代码集中在一起，与正常的程序代码分开，使得程序简洁，优雅，便于维护
        
        java异常处理的方式
            方式一：
                try-catch-finally(将异常处理掉了，代码还可以往下面执行)
                说明：
                    1：finally是可选的
                    2：使用try将可能出现异常代码包装起来，在执行过程中，一旦出现异常，就会生成一个对应异常类的对象，根据此对象的类型，去catch中进行匹配
                    3：一旦try中的异常对象匹配到某一个catch时，就进入catch中进行异常处理。一旦处理完成，就跳出当前的try-catch结构(在没有finally的情况)，继续执行其后的代码
                    4：catch中的异常类型如果没有子父类关系，则谁声明在上，谁声明在下无所谓
                        catch中的异常类型如果满足子父类关系，则要求子类一定要声明在父类的上面，否则，报错(因为如果父类声明在上，下面的子类永远无法到达)
                    5：常用的异常对象处理的方式 String e.getMessage()  printStackTrace()
                    6：在try结构中声明的变量，再出try结构以后，就不能再被调用

                体会：
                    1：使用try-catch-finally处理编译时异常，使得程序在编译时就不再报错，但是运行时仍可能报错。相当于我们使用try-catch-finally将一个编译时可能出现的异常，延迟到运行时出现
                    2：开发中，由于运行时异常比较常见，所以我们通常就不针对运行时编写try-catch-finally了。针对编译时异常，我们说一定要考虑异常的处理，通常使用try-catch-finally来处理异常
                      
                    
                try-catch-finally中finally的使用
                        1：finally是可选的
                        2：finally中声明的是一定会被执行的代码。即使catch中又出现异常，try中有return语句，catch中有return语句的情况
                        3：想数据库连接，输入输出流，网络编程Socket等资源，JVM是不能自动的回收的，我们需要自己手动的进行资源的释放。此时的资源释放，就需要声明在finally中
            方式二：
                throw + 异常类型(将异常抛出去，由接受的对象处理异常，最终还是通过try-catch处理掉的)
                    1："throw + 异常类型"写在方法的声明处。指明此方法执行时，可能会抛出的异常类型。一旦当方法执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象满足thorws后异常类型时，就会被抛出。异常代码后续的代码，就不再执行

                抛出异常的规则一：

                体会：try-catch-finally：真正的将异常处理掉了
                      throws的方式只是将异常抛给了方法的调用者。并没有将异常真正的处理掉

            
            开发中如何选择使用try-catch-finally，还是使用throws？
                >如果父类中被重写的方法没有throws方式处理异常，则子类重写的方法也不能使用thorws，意味着如果子类重写父类的方法中有异常，必须使用try-catch-finally方式处理。
                >执行的方法a中，先后又调用了另外的几个方法，这几个方法是递进关系执行的。我们建议这几个方法使用throws的方式进行处理。而执行方法a可以考虑使用try-catch-finally方式进行处理

            总结：运行时抛出异常，我们还是要去通过修改代码手动修改bug

                

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
        



##枚举类&注解
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
        
        



        


        


        


        


        



        
                    



        
        

            
                
                



                    








            


        


        

                
            
                    

                












                

            

            
            

            

                



            


        







        



