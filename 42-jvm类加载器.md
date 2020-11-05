###类加载器
    *数组的类加载器
        概念：
            数组的类加载器是组成数组元素的类加载器，但是数组对应的class对象并不是类加载器加载的，是由jvm动态创建的
            如果数组中的类型是原生类型，那么数组类是没有类加载器的
        
        实例：

            ```java
                public static void main(String[] args) {
                    String[] strings = new String[2];
                    System.out.println(strings.getClass().getClassLoader());

                    test15[] mytest15 = new test15[2];
                    System.out.println(mytest15.getClass().getClassLoader());

                    int[] ints = new int[2];
                    System.out.println(ints.getClass().getClassLoader());
                }
                // 输出结果
                // null     这个输出结果指的是：根类加载器
                // sun.misc.Launcher$AppClassLoader@18b4aac2
                // null     这个输出结果指的是：如果数组中的类型是原生类型，那么数组类是没有类加载器的
            ```
    
    *类加载器的命名空间
        概念：
            >每个类加载器都有自己的命名空间，命名空间由该加载器及所有父类加载器所加载的类组成
                重要说明：(意思是由子加载器所在加载的类的对象是能够看到的父加载器所加载的类的对象，而由父类加载器所加载的类的对象是看不到的子加载器所加载的类的对象)
            >在同一个命名空间中，不会出现类的完整名字(包括类的包名)相同的两个类
            >在不同的命名空间中，有可能出现类的完整名字(包括类的包名)相同的两个类

        不同类加载器的命名空间的关系
            >同一个命名空间内的类是相关可见的
            >子加载器的命名空间包含所有父加载器的命名空间。因此由子加载器加载的类能看见父加载器加载的类。例如：系统类加载器加载的类能看见根类加载器加载的类
            >有父加载器加载的类不能看见子加载器加载的类
            >如果两个加载器之间没有直接或间接的父子关系，那么它们各自加载的类相互不可见

    *类的卸载
        概念：
            >当sample类被加载，连接和初始化后，它的生命周期就开始了。当代表sample类的Class对象不再被引用，即不可触及时，Class对象就会结束生命周期，sample类在方法区内的数据也会被卸载，从而结束sample类的生命周期。

            >当一个类何时结束生命周期，取决于代表它的Class对象何时结束生命周期
        
        注意点：
            >由java虚拟机自带的类加载器所加载的类，在虚拟机的生命周期中，始终不会被卸载。前面已经介绍过，java虚拟机自带的类加载器包括根类加载器，扩展类加载器和系统类加载器。java虚拟机本身会始终引用这些类加载器，而这些类加载器则会始终引用它们所加载的类的Class对象，因此这些Class对象始终是可触及的

            >由用户自定义的类加载器所加载的类是可以被卸载的
        
        工具：
            jvm参数：-XX:+TraceClassUnloading  作用：用来追踪类被卸载的信息；需要在项目中配置一下即可

    *类加载器的双亲委托模型的好处
        1.可以确保java核心库的类型安全：所有的java应用都至少会引用java.lang.Object类，也就是说在运行期，java.lang.Object这个类会被加载到java虚拟机中；如果这个加载过程是由java应用自己的类加载器所完成，那么很可能就会在JVM中存在多个版本的java.lang.Object类，而且这些类之间还是不兼容的，相互不可见(正式命名空间在发挥作用)
            借助于双亲委托机制，java核心类库中的类的加载工作都是由启动类加载器来统一完成，从而确保了java应用所使用的都是同一个版本的java核心类库，他们之间是相互兼容的
        2.可以确保java核心类库提供的类不会被自定义的类所替代
        3.不同的类加载器可以为相同名称(binary name)的类创建额外的命名空间。相同名称的类可以并存在java虚拟机中，只需要用不同的类加载器来加载他们即可。不同类加载器所加载的类之间是不兼容的，这就相当于在java虚拟机内部创建了一个又一个相互隔离的java类空间，这类技术在很多框架中都得到了实际应用。

        事例：触发类的卸载的事例

            ```java
                test16 loader1 = new test16("loader1");
                loader1.setPath("C:\\java\\");
                Class<?> clazz = loader1.loadClass("com.shengs.java.test9");
                System.out.println("加载class文件的地址值"+clazz.hashCode());
                Object object = clazz.newInstance();
                System.out.println(object);
                loader1 = null;
                clazz = null;
                object = null;
                //被加载的test9的类要想被卸载，那么需要将类的引用指向其他地方，即所加载的类才能被卸载
                System.gc(); //这句代码作用会让程序执行垃圾回收
            ```

        事例：自定定义的类加载器加载类

            ```java
                public class test16 extends ClassLoader {
                    private String classLoaderName;
                    private String path;
                    private final String fileExtension = ".class";
                    public test16(String classLoaderName){
                        super();//将系统类加载器当做该类的加载器的父类加载器
                        this.classLoaderName = classLoaderName;
                    }
                    public test16(ClassLoader parent ,String classLoaderName){
                        super(parent);//显示指定该类加载器的父类加载器
                        this.classLoaderName = classLoaderName;
                    }
                    public void setPath(String path){
                        this.path = path;
                    }
                    protected Class<?> findClass(String className) throws ClassNotFoundException {
                        System.out.println("自己定义的加载器被加载啦");
                        byte[] data = this.loadClassData(className);
                        return this.defineClass(className,data,0,data.length);
                    }
                    //通过流读取.class文件的操作，就已经将.class文件读取到内存当中了
                    private byte[] loadClassData(String className){
                        InputStream is = null;
                        byte[] data = null;
                        ByteArrayOutputStream boas = null;
                        try{
                            className = className.replace(".","/");
                            is = new FileInputStream(new File(this.path + className + this.fileExtension));
                            boas = new ByteArrayOutputStream();
                            int ch = 0;
                            while (-1 !=(ch = is.read())){
                                boas.write(ch);
                            }
                            data = boas.toByteArray();
                        }catch (Exception ex){
                            ex.printStackTrace();
                        }finally {
                            try{
                                is.close();
                                boas.close();
                            }catch (Exception ex){
                                ex.printStackTrace();
                            }
                        }
                        return data;
                    }

                    public static void main(String[] args) throws Exception {
                        test16 loader1 = new test16("loader1");
                        loader1.setPath("C:\\java\\");
                        Class<?> clazz = loader1.loadClass("com.shengs.java.test9");
                        System.out.println("加载class文件的地址值"+clazz.hashCode());
                        Object object = clazz.newInstance();
                        System.out.println(object);

                //        test16 loader2 = new test16("loader2");
                        test16 loader2 = new test16(loader1,"loader2");//将loader1作为loader2的父类加载器，
                        loader2.setPath("C:\\java\\");
                        Class<?> clazz2 = loader2.loadClass("com.shengs.java.test9");
                        System.out.println("加载class文件的地址值"+clazz2.hashCode());
                        Object object2 = clazz2.newInstance();
                        System.out.println(object2);

                        test16 loader3 = new test16(loader2,"loader3");
                        loader2.setPath("C:\\java\\");
                        Class<?> clazz3 = loader3.loadClass("com.shengs.java.test9");
                        System.out.println("加载class文件的地址值"+clazz3.hashCode());
                        Object object3 = clazz3.newInstance();
                        System.out.println(object3);
                    }

                }
                /**
                * (1)第一种情况：通过自己定义的加载器进行加载的结果
                * 加载class文件的地址值1956725890
                * com.shengs.java.test9@1540e19d
                * ---------------------------------
                * 加载class文件的地址值2133927002
                * com.shengs.java.test9@6d6f6e28
                *
                * 出现两个不同的class文件地址值的原因：loader1和loader2本身是两个不同的对象，分别构成了两个不同的命名空间，所以就可以将相同的类加载多次(在每个命名空间里面只会加载一次)
                *
                *
                * (2)第二种情况：
                * 输出结果：
                * 加载class文件的地址值460141958
                * com.shengs.java.test9@4554617c
                *
                * 加载class文件的地址值460141958
                * com.shengs.java.test9@74a14482
                *
                * 出现加载的class文件地址相同的原因：
                * 自己定义的加载器委托父加载器在当前的项目中的classpath先进行加载
                *  loader1委托系统类加载器加载的test9，
                *  loader2也是委托系统类加载器加载的test9
                *  而loader1和loader2的系统类加载器指的是同一个类加载器(同一个类加载器也是同一个类的命名空间)，所以会得到相同的class对象
                */


                /**
                * 现象：loader1作为loader2的父加载器，loader2作为loader3的父加载器，类加载器加载类的时候会委托父加载器进行加载；
                * 输出结果：他们的hashcode地址值是一样的，说明他们是由一个加载器加载的
                * 加载class文件的地址值1956725890
                * com.shengs.java.test9@1540e19d
                * 加载class文件的地址值1956725890
                * com.shengs.java.test9@677327b6
                * 加载class文件的地址值1956725890
                * com.shengs.java.test9@14ae5a5
                */
            ```

        事例：类里面调用类，类的加载器加载类的情况

            ```java
                //mysample类内容
                public class mysample {
                    public mysample(){
                        System.out.println("mysample is loaded by:"+ this.getClass().getClassLoader());
                        new mycat();
                    }
                }
                //mycat类的内容
                public class mycat {
                    public mycat(){
                        System.out.println("mycat is loaded by:"+ this.getClass().getClassLoader());
                    }
                }
                //test17里面的内容
                public static void main(String[] args) throws Exception {
                    test16 loader1 = new test16("loader1");
                    Class<?> clazz = loader1.loadClass("com.shengs.java.mysample");//加载mysample类到内存当中
                    System.out.println("class地址"+clazz.hashCode());
                    //通过反射的方式创建clazz对象所对应的mysample类的实例
                    //Object object = clazz.newInstance();//调用的mysample类对应的无参数的构造函数方法
                    //如果注释掉该行，那么并不会实例化mysample对象，即mysample构造方法不会被调用，
                    // 因此不会实例化mycat对象，即没有对mycat进行主动使用，这里就不会加载mycat Class
                }
            ```

        事例:类加载器加载规则的事例

            ```java
                //mysample类内容
                public class mysample {
                    public mysample(){
                        System.out.println("mysample is loaded by:"+ this.getClass().getClassLoader());
                        new mycat();
                    }
                }
                //mycat类的内容
                public class mycat {
                    public mycat(){
                        System.out.println("mycat is loaded by:"+ this.getClass().getClassLoader());
                    }
                }
                public class test17_1 {
                    public static void main(String[] args) throws Exception {
                        test16 loader1 = new test16("loader1");
                        loader1.setPath("C:\\java\\");
                        Class<?> clazz = loader1.loadClass("com.shengs.java.mysample");//加载mysample类到内存当中
                        System.out.println("class地址"+clazz.hashCode());
                        //通过反射的方式创建clazz对象所对应的mysample类的实例
                        Object object = clazz.newInstance();//调用的mysample类对应的无参数的构造函数方法
                        //如果注释掉该行，那么并不会实例化mysample对象，即mysample构造方法不会被调用，
                        // 因此不会实例化mycat对象，即没有对mycat进行主动使用，这里就不会加载mycat Class
                    }
                }
                /**条件：mysample类里调用mycat类
                * 情况一：系统文件mysample和mycat的class文件都在
                * 加载器加载执行思路：mysample和mycat两个类都是由系统类加载器加载的
                *
                * 情况二:系统文件mysample的class文件保留，mycat的class文件删除
                * 加载器加载执行思路：mysample有系统类加载器加载，
                * 然后由加载mysample类的系统类加载器的去加载mycat类，根据加载器的加载规则依次向上传递，然后向下加载。发现没有可加载mycat类的加载器，会报错
                *
                * 情况三：系统文件mycat的class文件保留，mysample的class文件删除
                * 加载器加载执行思路：首先根据加载器的加载规则mysample类只能由test16加载器去加载，加载mysample类的test16加载器会尝试去加载mycat，
                * 但是依据加载规则会委托父类加载器加载，最终由系统类加载器加载完成；
                *
                */
            ```
        
        事例：命名空间事例说明

            ```java
                /**
                * 关于命名空间的重要说明
                * 1.子加载器所加载的类能够访问到父加载器所加载的类
                * 2.父加载器所加载的类无法访问到子加载器所加载的类
                * 
                * 事例：在mysample类中调用mycat类，mysample类由应用类加载器加载，mycat类由test16加载器加载
                *  结果：输出报错，因为在父加载器所加载的类无法访问到子加载器所加载的类
                * 
                * 事例：在mycat类中调用mysample类，mysample类由应用类加载器加载，mycat类由test16加载器加载
                *  结果：有结果输出：因为子加载器所加载的类能够访问到父加载器所加载的类
                */
            ```
        
        事例：测试不同的类加载器，从哪个目录下加载类的

            ```java
                public class test18 {
                    public static void main(String[] args) {
                        System.out.println(System.getProperty("sun.boot.class.path"));
                        System.out.println(System.getProperty("java.ext.dirs"));
                        System.out.println(System.getProperty("java.class.path"));
                    }
                }
                /**
                输出结果：输出启动类加载器，扩展类加载器，应用类加载器分别是从哪些目录下加载class文件
                执行思路：类加载器根据这些属性("sun.boot.class.path","java.ext.dirs","java.class.path")所对应的目录加载类
                */

                //改变扩展类加载加载的目录,让扩展类加载器加载当前目录，并且运行test9文件
                //扩展类加载器的特殊性：只加载jar包里面的内容。
                java -Djava.ext.dirs=./ com.shengs.java.test9
            ```

        事例：将文件添加到启动类加载器所加载的目录中，测试用启动类加载器加载mysample类

            ```java
                public class test18_1 {
                    public static void main(String[] args) throws Exception {
                        test16 loader1 = new test16("loader1");
                        loader1.setPath("C:\\java\\");
                        Class<?> clazz = loader1.loadClass("com.shengs.java.mysample");
                        System.out.println("clazz:"+ clazz.hashCode());
                        System.out.println("clazz有加载器加载:"+ clazz.getClassLoader());
                    }
                }

                /**
                * 输出结果
                * clazz:1163157884
                * clazz有加载器加载:null,说明改类是由启动类加载的
                */
            ```

        事例：类加载器委托系统类加载器加载，加载出来的类是同一个class的字节码文件

            ```java
                public static void main(String[] args) throws Exception {
                    test16 loader1 = new test16("loader1");
                    test16 loader2 = new test16("loader2");

                    Class<?> clazz1 = loader1.loadClass("com.shengs.java.myperson");
                    Class<?> clazz2 = loader2.loadClass("com.shengs.java.myperson");
                    //通过loader1加载myperson类，会在方法区内(内存中)生成一个clazz1对象代表myperson的类型
                    //clazz1的输出结果：class com.shengs.java.myperson
                    //System.out.println(clazz1); //com.shengs.java.myperson
                    //System.out.println(myperson.class); //com.shengs.java.myperson

                    System.out.println(clazz1 == clazz2);
                    //输出结果true：根本原因：loader1加载器加载myperson类委托应用类加载器去加载，
                    // 然后loader2加载器加载myPerson类也委托应用类加载器去加载，并且在内存中缓存起来
                    //然而应用类加载器发现已经加载过了myperson类，会从缓存中直接返回。
                    // 所以clazz1和clazz2在内存中指向的类都是同一个.相当于把clazz1的结果直接返回给clazz2

                    Object object1 = clazz1.newInstance();
                    Object object2 = clazz2.newInstance();

                    Method method = clazz1.getMethod("setMyPerson",Object.class);
                    method.invoke(object1,object2);
                }
            ```

        事例：相互不可见的命名空间，虽然加载的相同的类，但是相互之间不相等也不可访问

            ```java
                public static void main(String[] args) throws Exception {
                    test16 loader1 = new test16("loader1");
                    test16 loader2 = new test16("loader2");
                    loader1.setPath("C:\\java\\");
                    loader2.setPath("C:\\java\\");
                    Class<?> clazz1 = loader1.loadClass("com.shengs.java.myperson");
                    Class<?> clazz2 = loader2.loadClass("com.shengs.java.myperson");

                    System.out.println(clazz1 == clazz2);
                    //输出结果false
                    //原因：loader1类加载器也就是myperson的定义类加载器；loader2类加载器也是myperson的定义类加载器；
                    // 从双亲委托机制上看，loader1和loader2之间直没有任何关系的；因此它们会在jvm的内存中形成两个命名空间，
                    // 所以loader1将myperson的字节码文件加载到loader1的命名空间，loader2将myperson的字节码文件加载到laoder2的命名空间，
                    // 所以clazz1和clazz2之间相互是不可见的

                    Object object1 = clazz1.newInstance();
                    Object object2 = clazz2.newInstance();

                    Method method = clazz1.getMethod("setMyPerson",Object.class);
                    method.invoke(object1,object2);
                    //这里会抛出异常：java.lang.ClassCastException: com.shengs.java.myperson cannot be cast to com.shengs.java.myperson
                    //原因：在jvm中object1和object2这两个对象在相互不同的命名空间中，是没办法相互访问的，所以在object1中不能使用object2
                }
            ```

        事例：启动类加载器与其他类加载器的关系与类的命名空间

            ```java
                /**
                *在运行期，一个java类是由该类的完全限定名(binary name,二进制名)和用于加载该类的定义类加载器所共同决定的。
                * 如果同样名字(即相同的完全限定名)的类是由两个不同的加载器所加载，那么这些类就是不同的，即便.class文件的字节码完全一样，并且从相同的位置加载亦如此
                */

                /*
                    在Oracle的Hotspot实现中，系统属性sun.boot.class.path如果修改错了，则运行会出错，提示如下错误信息
                    Error occurred during initialization of VM
                    java/lang/NoClassDefFoundError:java/lang/Object
                    原因：把启动类加载器修改成自定义的类加载器，则会导致自定义的类加载器在当前下找不到java/lang/Object文件，所以会报错
                */
                public class test23 {
                    public static void main(String[] args) {
                        System.out.println(System.getProperty("sun.boot.class.path"));
                        System.out.println(System.getProperty("java.ext.dirs"));
                        System.out.println(System.getProperty("java.class.path"));
                        /*
                            内建于JVM中的启动类加载器会加载java.lang.ClassLoader以及其它的java平台类，
                            当JVM启动时，一块特殊的机器码会运行(即c++代码)，它会加载扩展类加载器与系统类加载器
                            这块特殊的机器码叫做启动类加载器
                            
                            启动类加载器并不是java类，而其它的加载器则都是java类
                            启动类加载器是特定于平台的机器指令，它负责开启整个加载过程。
                            
                            所有类加载器(除了启动类加载器)都被实现为java类。不过，总归要有一个组件来加载第一个java类加载器，从而让整个过程能够顺利进行下去，
                            加载第一个纯java类加载器就是启动类加载器的职责
                            
                            启动类加载器还会负责加载供JRE正常运行所需要的基本组件(JRE环境),这包括java.util与java.lang包中的类等等
                            
                        */


                        //扩展类加载器与系统类加载器也是由启动类加载的。扩展类加载器与系统类加载器都在Launcher公开类里面，
                        //根据类的加载器规则：会用相同的类加载器尝试加载这个类里面的内部类
                        System.out.println(Launcher.class.getClassLoader());
                    }
                }
            ```

    

