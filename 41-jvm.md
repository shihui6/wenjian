***类的加载，连接与初始化
    注意：
        先编译，再加载，连接与初始化，使用，卸载
        加载了一不定会被初始化，初始化就一定会加载
    *过程
        1-加载：查找并加载类的二进制数据(将class的字节码文件从磁盘加载到内存中去)
        2-连接：
            >验证：确保被加载的类的正确性(确保所加载的字节码文件没有被恶意的篡改，一切都是符合jvm对于字节码文件的要求)
            >准备：为类的静态变量(准备阶段还没有对象)分配内存，并将其初始化为默认值
            >解析：
                第一种理解：把类中的符号引用(间接的引用方式：比如一个类里面的方法引用了另外一个类)转换为直接引用(通过指针直接指到目标的对象的内存位置)
                第二种理解：解析过程就是在类型的常量池中寻找类，接口，字段和方法的符号引用，把这些符号引用替换成直接引用的过程
        3-初始化：为类的静态变量赋予正确的初始值(从上到下按顺序赋值)(java程序对类的主动使用才能初始化类的静态变量，被动使用不能初始化)
        4-使用
        5-卸载
    
    
    **类实例化过程：(类的实例化过程和类的初始化过程相似)
        为新的对象分配内存
        为实例变量赋默认值
        为实例变量赋正确的初始值
        java编译器为它编译的每一个类都至少生成一个实例初始化方法，在java的class文件中，这个实例初始化方法被称为"<init>"。针对源代码中每一个类的构造方法，java编译器都产生一个<init>方法
    
    *类的加载
        概念：
            类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在内存中创建一个java.lang.Class对象(规范并未说明Class对象位于哪里，HotSpot虚拟机将其放在了方法区中)用来封装类在方法区内的数据结构
            
        加载.class文件的方式
            -从本地系统中直接加载
            -通过网络下载.class文件
            -从zip，jar等归档文件中加载.class文件
            -从专有数据库中提取.class文件
            -将java源文件动态编译为.class文件(举例：动态代理)

        类加载器并不需要等到某个类被"首次主动使用"时再加载它
            分析：JVM规范允许类加载器在预料某个类将要被使用时就预先加载它，如果在预先加载的过程中遇到了.class文件缺失或存在错误，类加载器必须在程序首次主动使用该类时才报告错误(LinkageError错误)

            如果这个类一直没有被程序主动使用，那么类加载器就不会报告错误
    
    *类的验证
        概念：类被加载后，就进入连接阶段。连接就是将已经读入到内存的类的二进制数据合并到虚拟机的运行时环境中去
            
    *类的准备
        概念：在准备阶段，java虚拟机为类的静态变量分配内存，并设置默认的初始值。例如对于以下Sample类，在准备阶段，将为int类型的静态变量a分配4个字节的内存空间，并赋予默认值0，为long类型的静态变量b分配8个字节的内存空间，并且赋予默认值0

    *类的初始化
        概念：
            >在初始化阶段，java虚拟机执行类的初始化语句，为类的静态变量赋予初始值。在程序中，静态变量的初始化有两种途径：1：在静态变量的声明处进行初始化；2：在静态代码块中进行初始化

            >静态变量的声明语句，以及静态代码块都被看作类的初始化语句，java虚拟机会按照初始化语句在类文件中的先后顺序来依次执行它们
        
        类的初始化步骤
            >假如这个类还没有被加载和连接，那就先进行加载和连接
            >假如类存在直接父类，并且这个父类还没有被初始化，那就先初始化直接父类
            >假如类中存在初始化语句，那就按照语句出现的先后顺序依次执行这些初始化语句

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
    
    *类的初始化时机
        概念：当java虚拟机初始化一个类时，要求它的所有父类都已经被初始化，但是这条规则并不适用于接口
            在初始化一个类时，并不会先初始化它所实现的接口
            在初始化一个接口时，并不会先初始化它的父接口
            因此，一个父接口并不会因为它的子接口或实现类的初始化而初始化。只有当程序首次使用特定接口的静态变量时，才会导致该接口的初始化
    
    *类加载器
        类加载器的执行原理：
            类加载器用来把类加载到java虚拟机中。从JDK1.2版本开始，类的加载过程采用父亲委托机制，这种机制能更好地保证java平台的安全。在此委托机制中，除了java虚拟机自带的根类加载器以外，其余的类的加载器都有且只有一个父加载器。当java程序请求加载器loader1加载Sample类时，loader1首先委托自己的父加载器去加载Sample类，若父加载器能加载，则由父加载器完成加载任务，否则才由加载器loader1本身加载Sample类。若还加载不了就抛异常

        java虚拟机自带了以下几种加载器
            >根类加载器：该加载器没有父类加载器。它负责加载虚拟机的核心类库.根类加载器从系统属性sun.boot.class.path所指定的目录中加载类库。根类加载器的实现依赖于底层操作系统，属于虚拟机的实现的一部分，它并没有继承java.lang.ClassLoader类。
            >扩展类加载器：它的父类加载器为根类加载器。它从java.ext.dirs系统属性所指定的目录中加载类库，或者从JDK的安装目录的jre\lib\ext子目录(扩展目录)下加载类库，如果把用户创建的JAR文件放在这个目录下，也会自动由扩展类加载器加载。扩展类加载器是纯java类，是java.lang.ClassLoader类的子类
            >系统类加载器：也称为应用类加载器，它的父加载器为扩展类加载器。它从环境变量classpath或者系统属性java.class.path所制定的目录中加载类，它是用户自定义的类加载器的默认父加载器。系统类加载器是纯java类，是java.lang.Classloader类的子类

        几种加载器的关系：表面上看是继承关系，实际上是包含关系：系统加载器包含扩展类加载器，扩展类加载器包含根类加载器
        

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
        事例：关于常量池的理解和助记符

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
        事例：常量在编译期间不确定的情况

            ```java
                /**
                *当一个常量的值并非编译器可以确定的，那么其值就不会被放到调用类的常量池中
                * 这时在程序运行的时，会导致主动使用这个常量所在的类，显然会导致这个类被初始化
                */
                public class test3 {
                    public static void main(String[] args) {
                        System.out.println(Myparents3.str);
                    }
                }
                class Myparents3{
                    public static final String str = UUID.randomUUID().toString();
                    static {
                        System.out.println("myparents static code");
                    }
                }
            ```

        事例：数组创建本质的分析

            ```java
                /*
                    对于数组实例来说，其类型是由JVM在运行期动态生成的，表示[Lcom.shengs.Myparent4
                    这种形式。动态生成的类型，其父类型就是Object

                    对于数组来说，javaDoc经常将构成数组的元素为Component，实际上就是将数组降低一个维度后的类型

                    助记符：
                    anewarray:表示创建一个引用类型的(如类，接口，数组)数组，并将其引用值压入栈顶
                    newarray:表示创建一个指定的原始类型(如int，float,char等)的数组，并将其引用值压入栈顶
                */
                public class Mytest4 {
                    public static void main(String[] args) {
                        Myparent4[] myparent4s = new Myparent4[1];
                        //创建数组执行机制：new并不表示对元素的主动使用，而new仅仅是对该数组进行实例化，这个数组的实例的类型是由虚拟机在运行期动态创建出来的
                        System.out.println(myparent4s.getClass());//getclass()作用：表示获得这个引用所表示的对象，他所从属于的类型

                        int[] ints = new int[1];
                        System.out.println(ints.getClass());

                        char[] chars = new char[1];
                        System.out.println(chars.getClass());

                        boolean[] booleans = new boolean[1];
                        System.out.println(booleans.getClass());

                        short[] shorts = new short[1];
                        System.out.println(shorts.getClass());

                        byte[] bytes = new byte[1];
                        System.out.println(bytes.getClass());
                    }
                }
                class Myparent4{
                    static {
                        System.out.println("Myparent4 static block");
                    }
                }
                /*
                输出结果：
                class [Lcom.shengs.Myparent4;
                class [I
                class [C
                class [Z
                class [S
                class [B
                */
            ```
    

        事例：(接口初始化规则)接口初始化的子接口和父接口初始化的情况

            ```java
                /*
                当一个接口在初始化时，并不要求其父接口都完成初始化
                只有当真正使用到父接口的时候(如引用接口中所定义的常量时)，才会初始化
                */
                public class Mytest5 {
                    public static void main(String[] args) {
                        System.out.println(MyChild1.b);
                    }
                }
                interface Myparent5{
                    public static int a = 5;
                }
                interface MyChild1 extends Myparent5{
                    public static int b = 6;
                }
            ```

        事例：类加载准备阶段和类初始化阶段分析

            ```java
                public class Mytest6 {
                    public static void main(String[] args) {
                        Singal singal = Singal.getInstance();
                    }
                }

                class Singal{
                    public static int count1;
                    private static Singal singal = new Singal();
                    private Singal(){
                        count1++;
                        count2++;//准备阶段的重要意义
                    }
                    public static int count2=0;
                    public static Singal getInstance(){
                        return singal;
                    }
                }
                /*
                    调用Singal.getInstance()静态方法时，属于主动调用，Singal类会初始化，进入准备阶段，给所有的静态变量分配内存并提供默认值
                    初始化阶段，会按顺序的给静态变量赋值
                    注意：初始化只会执行一次
                */
            ```