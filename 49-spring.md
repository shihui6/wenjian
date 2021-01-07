***框架
    概念：
        半成品软件
        高度抽取可重用代码的一种设计；高度的通用性
    框架的理解：抽取一种高度可重用的；多个可重用模块的集合，形成一个某个领域的整体解决方案；例如：事务控制，强大的servlet，项目中的一些工具等等。

***Spring
    容器：可以管理所有的组件(类)，即框架
    核心点：IOC和AOP


    **IOC
        概念：叫控制反转
            解释：实际上就是指对一个对象的控制权的反转
        
        使用：
            步骤：在idea里面创建一个maven工程->生成的maven工程里的pom.xml配置文件添加spring依赖(即将spring框架导入进maven工程里面)->在生成的resources文件下创建xml配置文件(说明：在pom.xml里面添加spring依赖的时候才可以在resources文件下创建xml配置，否则右击resources时是没有xml该选项的)
        
            事例：通过配置xml文件向spring容器里添加类

                ```xml  
                    <!--    第一种添加属性的方法：构造方法属性注入，本质调用的类的构造方法-->
                        <bean class="org.javaboy.ioc.model.User" id="user">
                            <constructor-arg name="id" value="1" ></constructor-arg>
                            <constructor-arg name="username" value="javaboy"></constructor-arg>
                            <constructor-arg name="address" value="www.shihui.com"></constructor-arg>
                        </bean>
                    <!--第二种添加属性的方法：set方法属性注入，本质调用类的set方法-->
                        <bean class="org.javaboy.ioc.model.User" id="user2">
                            <property name="id" value="2"></property>
                            <property name="address" value="www.waoshihui"></property>
                            <property name="username" value="石惠"></property>
                        </bean>
                ```
                ```java 在java类里通过使用容器的上下文，来获取对象
                    //构造方法的属性注入
                        public void first(){
                            //ClassPathXmlApplicationContext类是容器的上下文
                            //配置文件指的是resources下的applicationContext.xml。目的是：把对象交给容器，以后对象的创建,销毁都有容器来做
                            //执行逻辑：当项目启动，spring容器加载配置文件之后会将配置文件中包含的类，加载到容器中，可以通过容器获取类就可以
                            ClassPathXmlApplicationContext cst = new ClassPathXmlApplicationContext("applicationContext.xml");
                            User user = (User) cst.getBean("user");
                            System.out.println(user);
                        }
                        //set方法的属性注入
                        public void second(){
                            ClassPathXmlApplicationContext cst2 = new ClassPathXmlApplicationContext("applicationContext.xml");
                            User user2 = (User) cst2.getBean("user2");
                            System.out.println(user2);
                        }
                ```

            事例：通过java配置一个类(配置类的作用类似于applicationContext.xml里面的配置类)

                ```java 通过java配置类到spring容器中
                    /**
                    * @configuration注解表示一个java配置类，配置类的作用类似于 applicationContext.xml
                    *
                    * ComponentScan当项目启动的时候，会根据ComponentScan扫描里面的包
                    * 若@ComponentScan(basePackages = "org.javaboy.ioc.service")。
                    *  当项目启动的时候，它会扫描service包下面所有的实体类，如果类上面加了@Service等等，那么该类会被注册到spring容器里
                    *
                    * 若@ComponentScan不指定路径，它会扫描当前配置类所在的包下面的类和包下面的子包
                    */
                    @Configurable
                    @ComponentScan(basePackages = "org.javaboy.ioc.service")
                    public class javaConfig {
                        @Bean
                        SayHello sayHello(){
                            return new SayHello();
                        }
                    }
                ```
                ```java SayHello类里面的信息
                    public class SayHello {
                        public String sayHello(String name) {
                            return "hello"+name;
                        }
                    }
                ```
                ```java 在类上添加Service注解
                    @Service
                    public class UserService {
                        @Autowired
                        UserDao userDao;
                        public List<String> getAllUsers(){
                            String hello = userDao.hello();
                            System.out.println(hello);
                            List<String> users = new ArrayList<String>();
                            for (int i = 0; i < 10; i++) {
                                users.add("shihui"+i);
                            }
                            return users;
                        }
                    }
                ```
                ```java  在java中使用java配置的到spring容器中的类
                    public static void main(String[] args) {
                            AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(javaConfig.class);
                    //        SayHello sayHello = (SayHello) ctx.getBean("sayHello");
                    //        System.out.println(sayHello.sayHello("石惠"));

                            UserService userService = ctx.getBean(UserService.class);
                            List<String> allUsers = userService.getAllUsers();
                            System.out.println(allUsers);

                        }
                ```


            事例：更加复杂的通过java类添加到spring容器中的信息

                ```java LinuxCondition类的内容
                    public class LinuxCondition implements Condition {
                        public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
                            String osname = conditionContext.getEnvironment().getProperty("os.name");
                            return osname.toLowerCase().contains("linux");
                        }
                    }
                ```

                ```java LinuxShowCmd
                    public class LinuxShowCmd implements ShowCmd{
                        public String showCmd() {
                            return "ls";
                        }
                    }
                ```
                ```java ShowCmd接口
                    public interface ShowCmd {
                        String showCmd();
                    }
                ```
                ```java WindowCondition
                    public class WindowCondition implements Condition {
                        public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
                            String osname = conditionContext.getEnvironment().getProperty("os.name");
                            return osname.toLowerCase().contains("win");
                        }
                    }
                ```
                ```java WindowShowCmd
                    public class WindowShowCmd implements ShowCmd{
                        public String showCmd() {
                            return "dir";
                        }
                    }
                ```
                ```java javaConfig配置信息
                    @Configurable       Configurable注解表示:javaConfig是个配置类，该类里面的信息是添加到spring容器里面的
                    public class javaConfig {
                        @Bean("cmd")        Bean注解表示：不添加此注解的内容不会添加到spring容器里
                        @Conditional(WindowCondition.class) Conditional注解表示：通过Bean注解再加上Conditional注解进行条件判断
                        ShowCmd winCmd(){
                            return new WindowShowCmd();
                        }

                        @Bean("cmd")
                        @Conditional(LinuxCondition.class)
                        ShowCmd linuxCmd(){
                            return new LinuxShowCmd();
                        }
                    }
                ```
                ```java Main配置信息的内容
                    public class Main {
                        public static void main(String[] args) {
                            AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(javaConfig.class);
                            //通过容器上下文ctx调用cmd所注释的内容，且判断在window环境下的话，调用winCmd方法
                            ShowCmd cmd = (ShowCmd) ctx.getBean("cmd");
                            String s = cmd.showCmd();
                            System.out.println(s);
                        }
                    }
                ```
    
    **AOP
        概念：面向切面编程，这是对面向对象思想的一种补充
            解释：面向切面编程，就是在程序运行时，不改变程序源码的情况下，动态的增强方法的功能，常见的使用场景非常多:日志，事务，数据库操作。。。。。

            概念                说明

            切点            要添加代码的地方，称作切点
            通知(增强)      通知就是向切点动态添加的代码
            切面            切点+通知
            连接点          切点的定义

        原理：在AOP实际上是基于java动态代理来实现的
            java中动态代理有两种实现方法:jdk,cglib
            
        