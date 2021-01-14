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

        
-----------------------------------------------------------------------------------------------------------------------

尚硅谷
时间2021/1/8

***IOC
    概念：Inversion(反转) of control，控制反转
        概念解释：
                控制：指的是资源的获取方式
                        主动式：(要什么资源都自己创建)
                            BookServlet{
                                BookService = new BookService();//简单对象的创建
                                AirPlane ap = new AirPlane();//复杂对象的创建时比较庞大的工程
                            }
                        被动式：指的是资源的获取不是我们自己创建，而是交给一个容器来创建和设置
                            BookServlet{
                                BookService bs;
                                public void test01(){
                                    bs.checkout();
                                }
                            }
    容器：
        管理所有的组件(组件指:有功能的类)；假设,BookServlet受容器管理，BookService也受容器管理，容器可以自动的探查出哪些组件(类)需要用到另一个组件(类);容器帮我们创建BookService对象，并把BookService对象赋值过去

        主动的new资源变为被动的接受资源
        只要容器管理的组件，都能使用容器提供强大的功能
    
    DI：(Dependency Injection)依赖注入
        容器能知道哪个组件(类)运行的时候，需要另外一个类(组件);容器通过反射的形式，如：将容器中准备好的BookService对象注入(利用反射给属性赋值)到BookServket中

    使用：
        容器的使用步骤：
            idea通过maven创建项目->在pom.xml中引入spring依赖(即将spring框架导入进maven工程里面)->在目录resources下创建xml配置文件(在pom.xml里面添加spring依赖的时候才可以在resources文件下创建xml配置，否则右击resources时是没有xml该选项的)
        
        细节点：
            1.ApplicationContext(IOC容器的接口)
            2.给容器中注册一个组件；我们也从容器中按照id拿到这个组件的对象？
                组件的创建工作，是容器完成的
                Person对象是什么时候创建好了呢？
                容器中对象的创建在容器创建完成的时候就已经创建好了。
            3.同一个组件在ioc容器中是单实例的；容器启动完成都已经创建准备好了。(单实例解释:一个组件类，只创建一次实例对象)
            4.容器中如果没有这个组件，当获取组件的时候，会报异常
            5.ioc容器在创建这个组件对象的时候,(property)会利用setter方法为javaBean的属性进行赋值
            6.javaBean的属性名是由什么决定的？getter/setter方法是属性名
            7.内部bean是不能用id获取的，可以通过name获取


            事例：通过配置xml文件向spring容器里添加类

                    ```xml
                        <!-- 注册一个Person对象，Spring会自动创建这个Person对象 -->
                        <!-- 
                            一个Bean标签可以注册一个组件(对象，类)
                            class：写要注册的组件的全类名
                            id：这个对象的唯一标识
                        -->
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <!-- 使用property标签为Person对象的属性赋值
                            name=""lastName"(指定属性名)
                            value="张三"(给指定的属性名赋值)
                            -->
                            <properrty name="lastName" value="张三" />
                            <properrty name="age" value="18" />
                            <properrty name="email" value="zhangsan@atgugu.com" />
                            <properrty name="gender" value="男" />
                        </bean>
                    ```
                    ```java     从容器中拿到这个组件
                        //ApplicationContext：代表IOC容器
                        //ClassPathXmlApplicationContext：当前应用的xml配置文件(ioc容器的配置文件)在ClassPath(类路径)下
                        //FileSystemXmlApplicationContext：ioc容器的配置文件放在磁盘路径下(参数用的绝对路径)
                        //根据spring的配置文件(指的是ioc.xml配置文件)得到ioc容器对象
                        ApplicationContext ioc = new ClassPathXmlApplicationContext("ioc.xml"); //这一步：创建容器
                        //容器帮我们创建好了对象，我们可以直接拿到
                        Person bean = (Person)ioc.getBean("person01");
                        Person bean2 = (Person)ioc.getBean("person01");
                        bean == bean2；//true 表示同一个组件在容器中只有一份
                    ```

        *使用情景
            1.根据bean的类型从IOC容器中获取bean实例
                ioc.getBean(Person.class)：如果ioc容器中这个类型的bean有多个，用类型查找就会报错
                ioc.getBean("person1",Person.class)：如果ioc容器中这个类型的bean有多个，用指定id和类型的查找是可行的
            2.通过构造器为bean的属性赋值
                方式一：
                    <bean id="person01" class="com.atguigu.bean.Person">
                        <constructor-arg name="email" value="xiaoming@atguigu.com"></constructor-arg>
                        <constructor-arg name="gender" value="男"></constructor-arg>
                        <constructor-arg name="age" value="18"></constructor-arg>
                    </bean>
                方式二：
                    <bean id="person02" class="com.atguigu.bean.Person">
                        <constructor-arg value="xiaoming@atguigu.com"></constructor-arg>
                        <constructor-arg value="男"></constructor-arg>
                        <constructor-arg value="18"></constructor-arg>
                    </bean>
                    注意点：必须按照javabean中有参构造器参数属性的顺序书写。也可通过index指定为指定索引
                方式三：
                    <bean id="person03" class="com.atguigu.bean.Person">
                        <constructor-arg value="xiaoming@atguigu.com"></constructor-arg>
                        <constructor-arg value="男"></constructor-arg>
                        <constructor-arg value="18" index="2" type="java.lang.Integer"></constructor-arg>
                    </bean>
                    注意点：重载的情况下type可以指定参数类型

            3.正确的为各种属性赋值
                情况一：赋null空值
                    <bean id="person01" class="com.atguigu.bean.Person">
                        <property name="lastName">
                            <!-- 进行复杂的复制 -->
                            <null />  //即给lastName属性名赋值null空
                        </property>
                    </bean>
                情况二：引用类型赋值(引用外部bean，引用内部bean)
                    方式一：引用外部bean
                        <bean id="car01" class="com.atguigu.bean.car">
                            <property name="carName" value="宝马"></property>
                            <property name="color" value="绿色"></property>
                            <property name="price" value="3000"></property>
                        </bean>
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <!-- ref：代表引用外面的一个值，等值引用 -->
                            <property name="car" ref="car01"></property>
                        </bean>

                        使用容器中的引用类型
                            Person bean = (Person)ioc.getBean("person01");
                            System.out.println("person的car"+bean.getCar());
                            Car bean2 = (Car)ioc.getBean("caro1");
                            System.out.println(bean2 == bean.getCar()); //true，表明指向的是容器里面的同一个对象

                    方式二：引用内部bean
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="car">
                                <bean class="com.atguigu.bean.car">
                                    <property name="carName" value="宝马"></property>
                                    <property name="color" value="绿色"></property>
                                    <property name="price" value="3000"></property>
                                </bean>
                            </property>
                        </bean>

                        使用容器中的对象
                            Person person01 = (Person)ioc.getbean("person01")
                            Car car = person01.getCar();

                情况三：集合类型赋值(List,Map,Properties)
                    util名称空间创建集合类型的bean

                    1.给类型List赋值
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="bookName" value="东游记"></property>
                        </bean>
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="books">
                                <!-- list标签表示(意思是创建对象)：book = new ArrayList<Book>() -->
                                <list>
                                    <!-- list标签体中添加每一个元素 -->
                                    <bean class="com.atguigu.bean.Book" p:bookName="西游记"></bean>
                                    <!-- 引用外部一个元素 -->
                                    <ref bean="book01"/>
                                </list>
                            </property>
                        </bean>
                    
                    2.给Map类型赋值
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="maps">
                                <!-- maps = new LinkedHashMap<>() -->
                                <map>
                                    <!-- 一个entry代表一个键值对 -->
                                    <entry key="key01" value="张三"></entry>
                                    <entry key="key02" value="18"></entry>
                                    <entry key="key01">
                                        <bean class="com.atguigu.bean.Car">
                                            <property name="carName" value="宝马"></property>
                                        </bean>
                                    </entry>
                                </map>
                            </property>
                        </bean>

                    3.给Properties类型赋值
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="properties">
                                <!-- properties = new Properties() -->
                                <props>
                                    <!-- k=v都是String；值直接写在标签体中 -->
                                    <prop key="username">root</prop>
                                    <prop key="password">123456</prop>
                                </props>
                            </property>
                        </bean>

                    4.util名称空间创建集合类型的bean
                        util命名空间的作用：公共的部分方便别人引用(相当于函数封装的单独的功能模块)

                        <!-- 相当于new LinkedHashMap<>() -->
                        <util:map id="myMap">
                            <!-- 添加元素 -->
                            <entry key="key01" value="张三"></entry>
                            <entry key="key02" value="18"></entry>
                            <entry key="key01">
                                <bean class="com.atguigu.bean.Car">
                                    <property name="carName" value="宝马"></property>
                                </bean>
                            </entry>
                        </util>
                        <!-- 使用命名空间util -->
                        <bean id="person01" class="com.atguigu.bean.Person">
                            <property name="maps" ref="myMap"></property>
                        </bean>

                        可以在java类中直接获取命名空间里的内容
                        Object bean = ioc.getBean("myMap");

                    

时间2021/11/1
                    5.级联属性

                        ```xml  
                            <!--级联属性赋值，级联属性的概念：属性的属性-->
                            <!-- 级联属性的作用：可以修改属性的值，注意：原来的bean的值可能会被修改   -->
                            <bean id="person04" class="com.atguigu.bean.Person">
                                <!--为car赋值的时候。改变car的价格-->
                                <property name="car" ref="car01"></property>
                                <!--修改car中的price为90000-->
                                <property name="car.price" value="900000"></property>
                            </bean>
                        ```
                        ```java     在xml中配置的级联属性在java类中的使用
                            public static void main(String[] args) {
                                    ApplicationContext ioc = new ClassPathXmlApplicationContext("springconfigContext.xml");
                                    Object car01 = ioc.getBean("car01");
                                    System.out.println("容器中的car"+car01);
                                    Object person04 = ioc.getBean("person04");
                                    System.out.println("person中的car"+person04);
                                }
                        ```

                    6.通过继承实现bean配置信息的重用

                        ```xml  重用在xml中配置的bean
                            <bean id="person05" class="com.atguigu.bean.Person">
                                <property name="lastName" value="张三"></property>
                                <property name="age" value="18"></property>
                                <property name="gender" value="男"></property>
                                <property name="email" value="zhangsan@atguigu.com"></property>
                            </bean>
                            <!--parent:指定当前bean的配置信息继承于哪个bean-->
                            <!-- 注意点：
                                    person06的bean，可以省略class
                                    person06继承于person05，把需要修改的属性值在person06中通过property写上即可，如这里的lastName属性
                             -->
                            <bean id="person06" parent="person05">
                                <property name="lastName" value="李四"></property>
                            </bean>
                        ```
                        ```java   
                            public static void main(String[] args) {
                                    ApplicationContext ioc = new ClassPathXmlApplicationContext("springconfigContext.xml");
                                    Object person06 = ioc.getBean("person06");
                                    System.out.println(person06);
                                }
                        ```

                    7.通过abstract属性创建一个模板bean

                        ```xml
                            <!--abstract="true"，指定这个bean的配置是一个抽象的，不能获取他的实例，只能被别人用来继承  -->
                            <bean id="person05" class="com.atguigu.bean.Person" abstract="true">
                                <property name="lastName" value="张三"></property>
                                <property name="age" value="18"></property>
                                <property name="gender" value="男"></property>
                                <property name="email" value="zhangsan@atguigu.com"></property>
                            </bean>
                        ```

                    8。bean之间的依赖
                        概念：
                            只是改变创建顺序，没有真实的依赖关系
                            原来bean实例的创建是按照配置的顺序创建bean的
                        使用：通过depends-on指定创建的先后顺序
                            <bean id="car" class="com.atguigu.bean.Car" depends-on="book,person">
                            意思是：在创建car之前，先创建book在创建person

                    9.bean作用域，分别创建单实例和多实例的bean
                        bean的作用域：指定bean是否单实例，默认是单实例的；即通过属性scope指定单实例或多实例

                        ```xml
                            prototype:多实例的
                                1.容器启动默认不会去创建多实例bean
                                2.获取的时候创建这个bean
                                3.每次获取都会创建一个新的对象
                            singleton:单实例的：默认的
                                1.在容器启动完成之前就已经创建好对象，保存在容器中了
                                2.任何获取都是获取之前创建好的那个对象，即单实例对象只创建一次
                            <bean id="car" class="com.atguigu.bean.Car" scope="prototype">
                        ```

                    10.静态工厂与实例工厂
                        1.静态工厂使用：

                            ```xml
                                    <!--    
                                    工厂模式
                                            概念：工厂帮我们创建对象；有一个专门帮我们创建对象的类，这个类就是工厂
                                            下面是根据：AirPlane ap = AirPlaneFactory.getAirPlane(String jzName);来执行的
                                            分类：
                                                静态工厂：工厂本身不用创建对象；通过静态方法调用，对象 = 工厂类.工厂方法名()
                                                实例工厂：工厂本身需要创建对象
                                                        工厂类 工厂对象 = new 工厂类();
                                                        工厂对象.getAirPlane("张三")
                                    -->
                                    <!--    
                                        1.静态工厂(不需要创建工厂本身)
                                            静态工厂在xml中的配置
                                                在类中指定哪个方法是工厂方法
                                                class：指定静态工厂全类名
                                                factory-method:指定工厂方法
                                                constructor-arg：此标签可以为方法传参
                                    -->
                                <bean id="airplane" class="com.atguigu.bean.AirPlaneStaticFactory"
                                    factory-method="getAirPlane">
                                    <!--可以为方法指定参数-->
                                    <constructor-arg value="李四"></constructor-arg>
                                </bean>

                                在java中的执行机制：在java中启动容器的时候，此时AirPlaneStaticFactory不实例化，而AirPlaneStaticFactory里面的静态方法getAirPlane会被调用
                            ```
                        
                        2.实例工厂使用：

                            ```xml
                                <!-- 实例工厂，先创建工厂实例 -->
                                <bean id="airPlaneInstance" class="com.atguigu.bean.AirPlaneInstanceFactory"></bean>
                                <!--  factory-bean:指定当前对象创建使用哪个工厂
                                        1.先配置出实例工厂对象
                                        2.配置我们要创建的AirPlane使用哪个工厂创建
                                            1.factory-bean:指定使用哪个工厂实例
                                            2.factory-method:使用哪个工厂方法
                                -->
                                <bean id="airplane2" class="com.atguigu.bean.Airplane"
                                    factory-bean="airPlaneInstance"
                                    factory-method="getAirPlane"
                                >
                                    <constructor-arg value="王五"></constructor-arg>
                                    
                                </bean>
                            ```
                            ```java
                                ApplicationContext ioc = new ClassPathXmlApplicationContext("springconfigContext.xml");
                                Object airplane2 = ioc.getBean("airplane2");
                                    //airplane2不是容器通过反射创建的，而是通过工厂的factory-bean和factory-method属性帮我们创建的这个对象。
                                System.out.println(airplane2);
                            ```

                        3.Spring的工厂->FactoryBean的工厂
                            概念：是Spring规定的一个接口
                            作用：只要是这个接口的实现类，Spring都认为是一个工厂

                            用法：
                                步骤：
                                    1.编写一个FactoryBean的实现类，这个实现类就是一个工厂
                                    2.在Spring配置文件中注册
                                
                                特点：
                                    1.ioc容器启动的时候不会创建工厂实例
                                    2.FactoryBean；获取的时候才创建对象，不管是否是单实例，FoctoryBean工厂，在容器启动的时候都不会创建工厂实例

                                ```java     MyFactoryBeanImple实现FactoryBean接口的类都是工厂类
                                    /**
                                    * 实现了FactoryBean接口的类是Spring可以认识的工厂类
                                    * Spring会自动的调用工厂方法创建实例
                                    */

                                    public class MyFactoryBeanImple implements FactoryBean<Book> {
                                        /**
                                        * getObject就是工厂方法，自动会调动
                                        * 返回创建的对象
                                        *
                                        * 即：当创建容器的时候，会自动嗲用getObject方法，自动调用生成相应的对象
                                        */
                                        public Book getObject() throws Exception {
                                            System.out.println("Book对象创建完成了");
                                            Book book = new Book();
                                            book.setBookName(UUID.randomUUID().toString());
                                            return book;
                                        }

                                        /**getObjectType的作用：
                                        * 返回创建的对象的类型
                                        * Spring会自动调用这个方法来确认创建的对象是什么类型
                                        */
                                        public Class<?> getObjectType() {
                                            return Book.class;
                                        }

                                        /**
                                        * 作用：用来判断是单例吗？
                                        * 返回false表明不是单例
                                        * 返回true表明是单例
                                        */
                                        public boolean isSingleton() {
                                            return false;
                                        }
                                    }
                                ```
                                ```xml  FactoryBean工厂类写完之后，需要在xml配置文件中配置一下
                                    <bean id="MyFactoryBeanImple" class="com.atguigu.bean.MyFactoryBeanImple"></bean>
                                ```
                                ```java     在java中使用spring的工厂
                                    public static void main(String[] args) {
                                        ApplicationContext ioc = new ClassPathXmlApplicationContext("springconfigContext.xml");
                                        Object MyFactoryBeanImple = ioc.getBean("MyFactoryBeanImple");
                                        System.out.println(MyFactoryBeanImple);
                                    }
                                ```


时间2020/1/13

                    11.创建都有生命周期方法的bean
                        生命周期：概念：bean的创建到销毁
                        ioc容器中注册的bean
                            1.单例bean，容器启动的时候就会创建好，容器关闭也会销毁创建的bean
                                容器启动(构造器)调用初始化方法->(容器关闭)调用bean的销毁方法
                            2.多实例bean，获取的时候才创建
                                获取bean(构造方法)调用初始化方法-->容器关闭不会调用bean的销毁方法

                        使用：

                            ```java
                                /*
                                    在类里面添加需要在xml配置中用到的销毁方法和初始化方法
                                */
                                public class Book {
                                    public void Myinit(){
                                        System.out.println("book创建了");
                                    }
                                    public void myDestory(){
                                        System.out.println("book销毁了");
                                    }
                                }
                            ```
                            ```xml  在xml配置中添加容器的初始化和销毁时调用类中的方法
                                <bean id="book002" class="com.atguigu.bean.Book"
                                    destroy-method="myDestory"
                                    init-method="Myinit"
                                ></bean>
                            ```
                            ```java 测试容器的创建和销毁
                                public static void main(String[] args) {
                                        //容器的创建
                                        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("springconfigContext.xml");
                                        Object MyFactoryBeanImple = ioc.getBean("book002");
                                        //容器的销毁
                                        ioc.close();
                                    }
                            ```


                    12.测试bean的后置处理器
                        概念：Spring有一个接口(BeanPostProcessor)为后置处理器，可以在bean的初始化前后调用方法
                        使用：
                            步骤：
                                1.编写后置处理器的实现类
                                2.将后置处理器注册在配置文件中

                    
                    13.引用外部属性文件
                        通过引入外部数据库连接池

                    14.基于xml的自动装配(自定义类型自动赋值)
                            javaBean是基本类型
                            自定义类型的属性是一个对象，这个对象在容器中可能存在

                            使用：

                                ```xml  在容器中配置自动装配
                                    <bean id="car" class="com.atguigu.bean.Car">
                                        <property name="carName" value="宝马"></property>
                                        <property name="color" value="白色"></property>
                                    </bean>
                                    <!--
                                        为Person里面的自定义类型的属性赋值
                                        property：即通过property标签进行属性赋值的方法叫：手动赋值
                                        自动赋值(自动装配)
                                            特点：自动赋值仅限于对自定义类型(Person类里面引用了Car类，这里的Car类就是自定义类型)的属性有效；对基本类型(Integer,String等等)是没办法赋值的
                                            方式：给bean添加属性autowire就可以自动赋值

                                            自动装配
                                                autowire="default/no";默认不自动装配；不自动为car属性赋值

                                                按照某种规则自动装配
                                                    autowire="byName"：按照名字
                                                        以属性名作为id去容器中找到这个组件，给他赋值；如果找不到就装配null
                                                        解释：以属性名为id意思是以Person类里面的自定义属性名作为id去查找
                                                    autowire="byType"：按照类型
                                                        以属性的类型作为查找依据去容器中找到这个组件，即是找class；如果容器中有多个这个类型的组件，会报错；如果没找到装配null
                                                    autowire="constructor"：按照构造函数
                                                        按照构造器进行赋值；
                                                            步骤：
                                                                1.先按照有参构造器的类型进行装配(成功)；没有就直接为组件装配null
                                                                2.如果按照类型找到了多个；参数的名作为id继续匹配；找到就装配，找不到就null
                                                                3。不管怎么样都不会报错
                                    -->
                                    <bean id="person" class="com.atguigu.bean.Person" autowire="byName">
                                        <!--<property name="car" ref="car"></property>  这里是手动赋值-->
                                    </bean>
                                ```

                    15。SpEL表达式(Spring Expression Language)
                        概念：
                            Spring表达式语言
                            和jsp页面上的el表达式一样，spEL根据javaBean风格getxxx(),setxxx()方法定义的属性访问对象图，完全符合我们熟悉的操作习惯
                        基本语法：SpEL使用#(...)作为定界符，所有在大括号中的字符都将被认为是SpEL表达式
                        使用：

                            ```xml  SPEL表达式在容器中的使用
                                <!--
                                在SpEL中使用字面量
                                引用其他bean
                                引用其他bean的某个属性值
                                调用非静态方法
                                调用静态方法
                                使用运算符；
                                -->
                                <bean id="person04" class="com.atguigu.bean.Person">
                                    <!--字面量:${}；#{}-->
                                    <property name="age" value="#{123456}"></property>
                                    <!--引用其他bean的某个属性值-->
                                    <property name="lastName" value="#{person.lastName}"></property>
                                    <!--引用其他bean，即和ref的功能一样-->
                                    <property name="car" value="#{car}"></property>
                                    <!--调用非静态方法
                                        语法：#{T(全类名).静态方法名()}
                                    -->
                                    <property name="email" value="#{T(java.util.UUID).randomUUID().toString()}"></property>
                                    <!--调用静态方法-->
                                    <property name="gender" value="#{person.getLastName()}"></property>
                                </bean>
                            ```

        *注解
            概念：
                通过给bean上添加某些注解，可以快速的将bean加入到ioc容器中
                某个类上添加任何一个注解都能快速的将这个组件加入到ioc容器的管理中
                Spring注解分类：
                    @Controller：控制器；我们推荐给控制器层(servlet包下的这些组件加这个注解)
                    @Service：业务逻辑；我们推荐业务逻辑层的组件添加这个注解；
                    @Repository：给数据库层的组件添加这个注解
                    @Component：给不属于以上几层的组件添加这个注解
            
            注意点：
                注解可以随便加；Spring底层不会去验证你加的这个注解；我们推荐各自层加各自的注解
                使用注解加入到容器中的组件，和使用配置加入到容器中的组件行为都是默认一样的
                    1.组件的id。默认就是组件的类名首字母小写；也可修改组件id的名字，如@Service("修改之后的名字")
                    2.组件的作用域，默认就是单例的
            
            使用：使用注解将组件快速的加入到容器中需要几步
                1.给要添加的组件上标四个注解的任何一个
                2.在xml配置中通过属性base-package告诉Spring要自动扫描加了注解的组件；需要依赖context名称空间
                        context名称空间解释：   
                            需要在xml中用到标签context:component-scan
                            context:component-scan：自动扫描组件
                            base-package：指定扫描的基础包；把基础包及他下面所有的包的所有加了注解的类都会自动扫描进ioc容器中
                            <context:component-scan base-package="com.atguigu"></context:component-scan>
                3.一定要导入aop包，因为aop包是支持加注解模式的


                事例：

                    ```xml 先在xml中配置
                        <!-- 
                            context:component-scan标签的作用是：将base-package属性指定的包里面的类，添加到容器中
                        -->
                        <context:component-scan  base-package="com.atguigu"></context:component-scan>
                    ```
                    ```java 在com.atguigu.Dao包里面创建DaoService类，并且添加注解@Service，意思是：将类DaoService添加到容器当中了
                        //这些注解相当于之前xml标签里的属性，如Scope注解可以是该组件变成多实例
                        @Service        //将该组件标记为可添加到容器中的类
                        @Scope(value="prototype") //通过此注解可以改组件成为多实例
                        public class DaoService {
                        }
                    ```
                    ```java   使用容器中的类
                        public static void main(String[] args) {
                                ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
                                DaoService bean1 = (DaoService) ioc.getBean("daoService");
                                DaoService bean2 = (DaoService) ioc.getBean("daoService");
                                System.out.println(bean1 == bean2);
                            }
                    ```


            有了注解为什么要学bean配置呢？
                因为：我们使用的依赖包如：java.Math里的包我们就不可以用注解将它添加到容器中，但是可以用bean配置；

            
            1.使用context:exclude-filter
                作用：指定扫描的时候可以排除一些不要的组件
                使用：

                    ```xml
                        <!-- 
                            type="annotation" ：指定排除规则；按照注解进行排除。标注了指定不要注解的组件
                                    expression="类的全类名":
                            type="assignable"：指定排除某个具体的类，按照类排除
                                    expression="类的全类名"：
                        -->
                        <context:component-scan  base-package="com.atguigu">
                            <context:exclude-filter type="assignable" expression=""></context:exclude-filter>
                        </context:component-scan>
                    ```
                    
            2.使用context:include-filter
                作用：只扫描进入哪些组件；默认都是全部扫描进来(要使用这个标签，必须要禁掉默认的过滤规则才行：use-default-filters="false")
                使用：

                    ```xml
                        <context:component-scan  base-package="com.atguigu" use-default-filters="false">
                            <!-- 指定只扫描哪些组件 -->
                            <context:include-filter type="assignable" expression=""></context:include-filter>
                        </context:component-scan>
                    ```

时间2021/1/14
            3.DI(依赖注入)
                1.自动装配注解
                    概念：使用@Autowired注解实现根据类型实现自动装配
                        概念解释：@Autowired底层是通过xml里的自动装配属性进行操作，即给类中用到的自定义属性自动赋值
                                @Autowired原理：
                                    1.先按照类型去容器中找到对应的组件；即bookService = ioc.getBean(BookService.class)
                                        1.找到一个就赋值
                                        2.没找到：抛异常
                                        3.找到多个(例如：BookService的子类是BookServiceExt,通过BookService类找的话(ioc.getBean(BookService.class)，会报错，因为子类BookServiceExt的类型也可以是BookService，因此遇到同样类型的两个类时要通过id来进行区分，既ioc.getBean("指定id",BookService.class))
                                            1.按照变量名作为id继续匹配(默认id为类名首字母小写，即BookService(bookService))，找到就匹配上;
                                                1.匹配上
                                                2.没有匹配上就报错，没有匹配上：
                                                    原因：因为我们按照变量名作为id继续匹配的
                                                    可以使用@Qualifier("bookService")注解指定一个新的id
                                                        1.找到匹配
                                                        2.找不到，报错
                    
                    注意点：
                        通过@Autowired注解的依赖或者自定义属性的类，需要在容器中能够找到
                        只要通过@Autowired标注的自动装配的属性默认是一定要装配上的，否则就会报错；所以需要设置找不到就赋值null
                            @Autowired(required=false)
                    

                    自动装配的注解有：
                        @Autowired,@Resource;都是自动装配的意思
                            @Autowired：最强大，Spring自己的注解
                            @Resource：是java自己的标注
                            两者的区别：@Resource：扩展性更强；因为是java的标准。如果切换成另外一个容器框架，@Resource还可以使用。@Autowired只能在Spring容器中使用
                        
                    使用：

                        ```xml  先在xml容器中配置，那些包里面的类需要添加到容器中
                            <context:component-scan  base-package="com.atguigu"></context:component-scan>
                        ```
                        ```java  将相应包里需要添加到容器中的类，添加上注解，并且在类中使用依赖注入
                            //DaoService类中的内容
                            @Service
                            public class DaoService {
                                public void daosay(){
                                    System.out.println("这里是DaoService类");
                                }
                            }

                            //Service类中的内容
                            @Service
                            public class Serivce {
                                @Qualifier("bookService") //此时就不用daoService了就用bookService作为id进行查找
                                @Autowired
                                private DaoService daoService;

                                /*
                                    方法上有@Autowired的话，有以下几种特点
                                        1.这个方法也会在bean创建的时候自动运行
                                        2.这个方法上的每一个参数都会自动注入值，并且每个参数可以用注解@Qualifier重新指定id名
                                */
                                @Autowired(required=false)
                                public void SayHello(Book bookDao,@Qualifier("bookService")BookService bookService){
                                    System.out.println("service被调用了");
                                    daoService.daosay();
                                }
                            }
                        ```
                        ```java 测试使用依赖注入
                            public static void main(String[] args) {
                                    ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
                                    Serivce bean1 = (Serivce) ioc.getBean("serivce");
                                    bean1.SayHello();
                                }
                        ```

                2.泛型依赖注入

                        ```java 创建各种类
                            public class Book {
                            }
                            public class User {
                            }

                            --------------------------------------------------
                            //抽象类BaseDao
                            public abstract class BaseDao<T> {
                                public abstract void save();
                            }

                            //类BookDao
                            @Repository
                            public class BookDao extends BaseDao<Book>{
                                public void save() {
                                    System.out.println("是Bookdao保存图书");
                                }
                            }

                            //类UserDao
                            @Repository
                            public class UserDao extends BaseDao<User>{
                                public void save() {
                                    System.out.println("保存用户Userdao");
                                }
                            }

                            ------------------------------------------------------
                            //类BaseService<T>可以不写注解的原因：因为BookService和UserService两个类有注解，并且都继承了BaseService<T>泛型类，相当于把父类BaseService<T>里的内容都继承过来了，毕竟BookService和UserService都有注解，所以正常运行
                            //类BaseService
                            public class BaseService<T> {   
                                @Autowired
                                BaseDao<T> baseDao;
                                public void save(){
                                    baseDao.save();
                                }
                            }

                            //类BookService
                            @Service
                            public class BookService  extends BaseService<Book>{
                            //    @Autowired
                            //    BookDao bookDao;
                            //    public void save(){
                            //        bookDao.save();
                            //    }
                            }

                            //类UserService
                            @Service
                            public class UserService extends BaseService<User>{
                            //    @Autowired
                            //    UserDao userDao;
                            //    public void save(){
                            //        userDao.save();
                            //    }
                            }
                            -----------------------------------------------------------

                            //测试类
                            public class Test {
                                public static void main(String[] args) {
                                    ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
                                    BookService bookService = ioc.getBean(BookService.class);
                                    UserService userService = ioc.getBean(UserService.class);
                                    bookService.save();
                                    userService.save();
                                }
                            }
                            //执行流程：从容器中获取bookService实例，bookService.save()方法调用，bookService调用父类BaseService<Book>里面的save方法，执行到BaseDao<Book>的时候，会从容器中根据类型找BaseDao<Book>的组件，通过注解@Repository注册到容器中的组件是BookDao且类型也是BaseDao<Book>，最后BookDao的组件实例调用相应的方法。
                        ```

***AOP
