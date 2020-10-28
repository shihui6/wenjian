***类的加载，连接与初始化
    *过程
        1-加载：查找并加载类的二进制数据(将class的字节码文件从磁盘加载到内存中去)
        2-连接：
            >验证：确保被加载的类的正确性(确保所加载的字节码文件没有被恶意的篡改，一切都是符合jvm对于字节码文件的要求)
            >准备：为类的静态变量(准备阶段还没有对象)分配内存，并将其初始化为默认值
            >解析：把类中的符号引用(间接的引用方式：比如一个类里面的方法引用了另外一个类)转换为直接引用(通过指针直接指到目标的对象的内存位置)
        3-初始化：为类的静态变量赋予正确的初始值(java程序对类的主动使用才能初始化类的静态变量，被动使用不能初始化)
        4-使用
        5-卸载
    
    *类的加载
        概念：
            类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在内存中创建一个java.lang.Class对象(规范并未说明Class对象位于哪里，HotSpot虚拟机将其放在了方法区中)用来封装类在方法区内的数据结构
            
        加载.class文件的方式
            -从本地系统中直接加载
            -通过网络下载.class文件
            -从zip，jar等归档文件中加载.class文件
            -从专有数据库中提取.class文件
            -将java源文件动态编译为.class文件(举例：动态代理)
    
    *类初始化条件
        所有的java虚拟机实现必须在每个类或接口被java程序"首次主动使用"(必须是主动使用且只首次第一次)时才初始化他们

    *java程序对了类的使用方式可分为两种
        -主动使用(七种情况)
            >创建类的实例
            >访问某个类或接口的静态变量，或者对该静态变量赋值
            >调用类的静态方法(第二种情况和第三种情况可以说是一种情况)
                拓展：助记符：getstatic(jvm字节码通过getstatic助记符指令进行访问或获取类静态变量的操作),putstatic,invokestatic
            >反射
            >初始化一个类的子类(初始化一个子类，当初始化子类的时候，也会对父类进行初始化)
            >java虚拟机启动时被标明为启动类的类(包含了main方法的类)
            >jdk1.7开始提供的动态语言支持
        -被动使用
    
    
        事例：

            ```java
                /**jvm参数：
                * -XX:+TraceClassLoading 用来追踪类的加载信息并打印出来；需要在项目中配置一下即可
                * -XX:+<option>,表示开启option选项
                * -XX:-<option>,表示关闭option选项
                * -XX:<option>=<value>,表示将option选项的值设置为value
                */
                public class mytest1 {
                    public static void main(String[] args) {
                        System.out.println(myChild.str);
                    }
                }

                class Myparent{
                    public static String str = "hello world";
                    static {
                        System.out.println("myparents static block");
                    }
                }

                class myChild extends Myparent{
                    public static String str2 = "wello";
                    static {
                        System.out.println("mychilds static block");
                    }
                }
                //输出结果 
                    // myparents static block
                    // hello world
            ```
        事例：关于常量在内存中执行的流程

            ```java
                /**
                * 常量在编译阶段会存入到调用这个常量的方法所在的类的常量池中
                * 本质上，调用类(mytest2)并没有直接引用到定义常量的类(Myparents)，因此并不会触发
                * 定义常量的类的初始化
                * 注意：这里指的是将常量存放到了mytest2的常量池中，之后mytest2与Myparents就没有任何联系了
                * 甚至，我们可以将Myparents的class文件删除
                */

                /*反编译之后(jvm字节码通过getstatic助记符指令进行访问或获取类静态变量的操作)
                    助记符的执行环境：在类的方法中执行的
                    助记符：
                    ldc表示将int，float或是String类型的常量值从常量池中推送至栈顶
                    bipush表示将单字节(-128~127)的常量值推送至栈顶
                    sipush表示将一个短整型常量值(-32768~32767)推送至栈顶
                    iconst_1表示将int类型的1推送至栈顶 (iconst_1 ~ iconst_5)
                */
                public class mytest2 {
                    public static void main(String[] args) {
                        System.out.println(Myparents.str);
                    }
                }
                class Myparents{
                    public static final String str = "hello world";
                    public static final short s = 127;
                    public static final int i = 128;
                    public static final int m = 6;
                    static {
                        System.out.println("myparents static block");
                    }
                }
            ```