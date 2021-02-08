
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

        作用：
            主动的new资源变为被动的接受资源
            只要容器管理的组件，都能使用容器提供强大的功能
    
    DI：(Dependency Injection)依赖注入
        容器能知道哪个组件(类)运行的时候，需要另外一个类(组件);容器通过反射的形式将依赖的组件注入到运行的类中，如：将容器中准备好的BookService对象注入(利用反射给属性赋值)到BookServket中

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

                    

时间2021/1/11
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
                                    person06的bean，可以省略class,因为继承可以用父级的类型
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
                            bean实例默认的创建是按照配置的顺序创建bean的
                        使用：通过depends-on指定创建的先后顺序
                            <bean id="car" class="com.atguigu.bean.Car" depends-on="book,person">
                            意思是：在创建car之前，先创建book再创建person

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
                                        * 即：当创建容器的时候，会自动调用getObject方法，自动调用生成相应的对象
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
                            和jsp页面上的el表达式一样，spEL根据javaBean风格getxxx(),setxxx()方法定义的属性访问对象，完全符合我们熟悉的操作习惯
                        基本语法：SpEL使用#{}作为定界符，所有在大括号中的字符都将被认为是SpEL表达式
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
                            属性base-package：指定扫描的基础包；把基础包及他下面所有的包的所有加了注解的类都会自动扫描进ioc容器中
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
                    概念：使用@Autowired注解实现根据类型自动装配
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

时间2021/1/15
***AOP
    AOP：面向切面的编程
        面向切面编程：
            是基于OOP(面向对象的编程)基础之上新的编程思想
        核心概念：
            指在程序运行期间，将某段代码动态的切入到指定方法的指定位置进行运行的这种编程方式，叫面向切面编程
        理解面向切面编程的一种运用：加日志记录(即在计算机运行计算方法的时候进行日志记录，例如js中的console.log功能)
            1.直接编写在方法内部；
                缺点：这种方法不推荐，修改维护麻烦，因为如果需要改的话要一个个改；是耦合的，我们要做到解耦
            2.我们希望的是：
                业务逻辑(核心模块)；日志模块；在核心功能运行期间，自己动态的加上
                运行的时候，日志功能可以加上
        
        AOP使用场景
            1.AOP加日志保存到数据库
            2.AOP做权限验证
            3.AOP做安全检查
            4.AOP做事务控制

        jdk默认的动态代理：
                可以使用动态代理来将日志代码动态的在目标方法执行前后先进行执行
                动态代理的缺点：
                    1。写起来难
                    2.jdk默认的动态代理，如果目标对象没有实现任何接口，是无法为他创建代理对象的；因为代理对象和被代理对象之间唯一的联系就是接口
                
        Spring动态代理
            概念：Spring实现了AOP功能；底层就是动态代理
            特点：
                1.可以利用Spring一句代码都不写的去创建动态代理；
                2.实现简单，而且没有强制要求目标代理对象实现接口；将某段代码(日志)动态的切入(不把日志代码写死在业务逻辑方法中)到指定方法(加减乘除)的指定位置(方法的开始，结束，异常。。。)进行运行的这种编程方式(Spring简化了面向切面编程)
        Spring的AOP    
            AOP的术语：
                连接点：每一个方法的每一个位置都是一个连接点
                切入点：我们真正需要执行日志记录的地方
                切入点表达式：在众多连接点中选出我们感兴趣的地方；
                    类似于下面例子中的execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int))
                横切关注点：连接每个方法的线
                通知方法：每个方法
                切面类：
        
        基于注解的AOP步骤：
            1.将目标类和切面类都加入到ioc容器中。@component
            2.告诉Spring哪个类是切面类，通过@Aspect注解
            3.在切面类中使用五个通知注解来配置切面中的这些通知方法都何时运行
            4.开启基于注解的AOP功能

            使用步骤：
                1.导包(导入相应的依赖)

                    ```xml
                        <!-- 此依赖是Aspect依赖 -->
                        <dependency>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-starter-aop</artifactId>
                            <version>2.2.2.RELEASE</version>
                        </dependency>
                    ```
                2.写配置
                    1.将目标类和切面类(封装了通知方法(在目标方法执行前后执行的方法))加入到ioc容器中

                        ```xml
                            <context:component-scan  base-package="com.atguigu"></context:component-scan>
                            <!--开启基于注解的AOP功能：aop名称空间-->
                            4.开启基于注解的AOP模式
                            <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
                        ```
                    2.告诉Spring到底哪个类是切面类(加注解@Aspect)

                        ```java
                            @Aspect   //此注解的作用：将此类定性成切面类；这里要加Aspect依赖，可以在maven官方输入Aspect后可找到;
                            @Component  //作用：将LogUtils类添加到容器中
                            public class LogUtils {
                                3.告诉Spring，切面类里面的每一个方法，都是何时何地运行
                                /**
                                * 告诉Spring每个方法都是什么时候运行
                                * @Before：在目标方法之前运行                        前置通知
                                * @After：在目标方法运行结束之后运行                 后置通知
                                * @AfterReturning：在目标方法正常返回之后            返回通知
                                * @AfterThrowing：在目标方法抛出异常之后运行         异常通知
                                * @Around：环绕                                     环绕通知
                                */

                                //在执行目标方法之前运行:需要在前置通知注解里面写切入点表达式
                                    //切入点表达式的作用：执行在哪些方法执行的时候调用该通知方法

                                    细节点3
                                    /**
                                    * 切入点表达式的写法：
                                    *  固定格式：execution(访问权限符 返回值类型 方法全类名(参数列表))
                                    *  通配符：
                                    *      *:
                                    *          1.匹配一个或者多个字符:execution(public int com.atguigu.impl.MyMath*.*(int,int))
                                    *          2.匹配任意一个参数;第一个是int，第二个参数任意类型;(匹配两个参数)
                                    *              execution(public int com.atguigu.impl.MyMathCalcultor.*(int,*))
                                    *          3.权限位置(public)上不能用*；权限位置上不写*，会自动添加上public权限
                                    *      ..:
                                    *          1.匹配任意多个参数，任意类型参数
                                    *              execution(public int com.atguigu.impl.MyMathCalcultor.*(..))
                                    *          2.匹配任意多层路径
                                    *              execution(public int com.atguigu..MyMathCalcultor.*(..))
                                    *
                                    *      &&：要切入的位置满足这两个表达式
                                    *          execution(public int com.atguigu..MyMath*.*(..)&&execution(* *.*(int,double)))
                                    *      ||：满足任意一个表达式即可切入
                                    *          execution(public int com.atguigu..MyMath*.*(..)||execution(* *.*(int,double)))
                                    *      !：只要不是这个位置都切入
                                    *          !execution(public int com.atguigu..MyMath*.*(..)&&execution(* *.*(int,double)))
                                    */

                                    细节点4
                                    /**
                                    * 细节点4：我们可以在通知方法运行的时候，拿到目标方法的详细信息
                                    *  1.只需要为通知方法的参数列表上加上一个参数
                                    *      JoinPoint joinPoint：这个类封装了当前目标方法的详细信息
                                    *  2.告诉Spring哪个参数是用来接收异常 注意点：返回值只有在@AfterReturning，@AfterThrowing才会有
                                    *      throwing="exception"：告诉Spring，exception参数是用来接收异常的
                                    *      returning = "result":告诉Spring，result参数是用来接收返回值的
                                    *  3.Exception exception：指定通知方法可以接受哪些异常；
                                    *
                                    * 解释点：
                                    *      Spring对通知方法的要求不严格
                                    *          唯一的要求就是方法的参数列表一定不能乱写
                                    *          通知方法是Spring利用反射调用的，每次方法调用得确定这个方法的参数表的值
                                    *          参数表上的每一个参数，Spring都得知道是什么；JoinPoint类型参数Spring是认识的，不知道的参数一定要告诉Spring是什么
                                    */

                                    细节点5
                                        抽取可重用的切入点表达式
                                            1.随便声明一个没有实现的返回void的空方法
                                            2.给方法上标注@Pointcut注解
                                        事例：
                                            @Pointcut("execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int))")
                                            public void hhhMypoint(){};
                                            然后在下面的@Before("hhhMypoint()")即可

                                //execution(访问权限 返回值类型 方法签名);解释：方法签名，即是方法的全类名.方法名(*表示在任何方法执行时都执行切面方法)
                                @Before("execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int))")
                                public static void logStart(JoinPoint joinPoint){ //细节点4的使用
                                    String name = joinPoint.getSignature().getName();//获取方法名
                                    Object[] args = joinPoint.getArgs();//获取参数列表
                                    System.out.println(""+name+"方法开始执行"+ Arrays.asList(args)+"");
                                }
                                //在目标方法正常执行完成之后执行
                                @AfterReturning(value = "execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int)))",returning = "result") //returning = "result"此处就是告诉Spring，result是返回值
                                public static void logReturn(JoinPoint joinPoint,Object result){
                                    System.out.println("方法正常执行，计算结果是"+result+"");
                                }
                                //在目标方法结束的时候运行(在finally中执行的代码)
                                @After("execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int)))")
                                public static void logEnd(){
                                    System.out.println("方法执行结束");
                                }
                                //在目标方法出现异常的时候执行
                                @AfterThrowing(value = "execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int)))",throwing = "exception") //throwing = "exception"此处就是告诉Spring，exception是抛异常
                                public static void logYIchang(JoinPoint joinPoint,Exception exception){
                                    System.out.println("方法抛出异常"+exception+"");
                                }

                                ----------------------------------------------------------------------------------------- 
                                环绕通知@Around         
                                /**
                                * Throwable:环绕通知
                                *      环绕是Spring中强大的通知
                                *      环绕的基本原理是：动态代理
                                *
                                * 环绕通知和普通通知的执行顺序
                                *       环绕通知---普通通知---目标方法执行---环绕正常返回/出现异常---环绕后置---普通后置---普通方法返回

                                注意点：使用环绕通知之后，其他普通通知可以不写。一个环绕通知可以取代其他4个普通通知

                                */

                                @Around("execution(public int com.atguigu.impl.MyMathCalcultor.*(int,int)))")
                                public Object myAround(ProceedingJoinPoint pjp) throws Throwable{
                                    Object[] args = pjp.getArgs();//获取参数列表
                                    String name = pjp.getSignature().getName();//获取方法名
                                    Object proceed = null;
                                    try{
                                        System.out.println("【环绕前置通知】"+name+"方法开始");
                                        //这行代码就是利用反射调用目标即可，就是method.invoke(obj,args)
                                        proceed = pjp.proceed(args); //执行方法且返回值
                                        System.out.println("【环绕返回通知】"+name+"方法返回值"+proceed+"");
                                    }catch (Exception e){
                                        System.out.println("【环绕异常通知】"+name+"方法出现异常"+e);
                                    }finally {
                                        System.out.println("【环绕后置通知】"+name+"方法结束");
                                    }
                                    return proceed;
                                }
                            }
                        ```
                    3.告诉Spring，切面类里面的每一个方法，都是何时何地运行(在第二步中)
                    4.开启基于注解的AOP模式(在第一步中)

                3.测试

                    ```java
                        public static void main(String[] args) {
                                ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
                                //从ioc容器中拿到目标对象；注意：如果想要用类型，一定用他的接口类型，不要用他本类
                                    /**
                                    * 原因：AOP的底层是动态代理，而动态代理在创建对象的时候，是创建的代理的对象，而代理对象的类型是com.sun.proxy.$Proxy12
                                    * 本类的对象并没有创建,容器中保存的组件是他的代理对象;$Proxy12。当然不是本类的类型;但是同样的代理对象用到了接口的类型
                                    */
                                Calculator bean =  ioc.getBean(Calculator.class);
                                bean.add(1,2);

                            细节点2
                            //情况二：
                                //如果类没有实现的接口则获取到的bean的类型就是本类类型(即MyMathCalcultor类没有实现Calculator接口)
                                MyMathCalcultor bean1 = ioc.getBean(MyMathCalcultor.class);
                                //运行能成功；因为cglib帮我创建好了代理对象，此bean1的类型是com.atguigu.impl.MyMathCalculator$$EnhancerByCGLIB$$fe279f4;

                            /**
                            * 通知方法的执行顺序
                            *      正常执行：@Before(前置通知)===>@After(后置通知)===>@AfterReturning(正常返回通知)
                            *      异常执行：@Before(前置通知)===>@After(后置通知)===>@AfterThrowing(方法异常通知)
                            */
                            }
                    ```  

            
时间2021/1/20
        注解和配置适合
            异同点：两个方法思路是一样的，步骤也差不多
            注解：快速方便
            配置：功能完善;重要的用配置，不重要的用注解

        
        基于配置的AOP步骤
            1.将目标类和切面类都加入到ioc容器中
            2.告诉Spring哪个类是切面类
            3.在切面类中使用五个通知注解来配置切面中的这些通知方法都何时运行

            事例：

                ```xml
                    <!--基于配置的AOP-->
                        <bean id="myMathCalcultor" class="com.atguigu.impl.MyMathCalcultor"></bean>
                        <bean id="logUtils" class="com.atguigu.uitils.LogUtils"></bean>
                    <!--需要AOP名称空间-->
                    <!-- aop:config属性order可以指定切面的执行顺序
                        <aop:config order="1"></aop:config>
                    -->
                        <aop:config>
                            <!--指定切面:相当于切面加了@Aspect注解-->
                            <aop:aspect ref="logUtils">
                                    <!--配置哪个方法是前置通知：method指定方法名
                                        logStart方法名是切面类里面定义的切面类中的方法，@Before("切入点表达式")是一样的
                                    -->
                                    <aop:before method="logStart" pointcut="execution(* com.atguigu.impl.*.*(..))"></aop:before>
                                    <!--通过配置的方式将切入点表达式封装-->
                                    <aop:pointcut expression="execution(* com.atguigu.impl.*.*(..))" id="mypoint"></aop:pointcut>
                                    <aop:after-returning method="logReturn" pointcut-ref="mypoint" returning="result"></aop:after-returning>
                                    <aop:after-throwing method="logYIchang" pointcut-ref="mypoint" throwing="exception"></aop:after-throwing>
                                    <aop:after method="logEnd" pointcut-ref="mypoint"></aop:after>
                            </aop:aspect>
                        </aop:config>
                    <!-- 
                        切面方法的执行顺序：环绕通知---普通通知---目标方法执行---环绕正常返回/出现异常---环绕后置---普通后置---普通方法返回
                    -->
                ```
----------------------------------------------------------------------------------------------

*spring的ioc的作用：
    只能削减程序的解耦问题

*ioc的概念
    容器：根本上是Map的key=value的键值对
    spring容器ioc的基本原理思想代码：

    ```java
        //定义一个Map，用于存放我们要创建的对象。我们把它称为容器
        private static Map<String,Object> beans;
        static {
            try {
                //实例化对象
                Properties props = new Properties();
                //获取properties文件的流对象
                InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
                props.load(in);
                //实例化容器
                beans = new HashMap<String, Object>()
                //取出配置文件中所有的key
                Enumeration keys = props.keys();
                //遍历枚举
                while (keys.hasMoreElements()){
                    //取出每个key
                    String key = keys.nextElement().toString();
                    //根据key获取value
                    String beanPath = props.getProperty(key);
                    //反射创建对象
                    Object value = Class.forName(beanPath).newInstance();
                    //把key和value存入容器中
                    beans.put(key,value);
                }
            }catch (Exception){
                throw new ExceptionInInitializerError("初始化properties失效");
            }
        }
    ```

*spring工程创建
    file->maven生成maven工程->在pom.xml中加入org.springframework依赖

        ```xml
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>5.0.2.RELEASE</version>
            </dependency>
        ```
    在resources中创建xml配置文件约束文件如下
        作用：将对象的创建交给spring来管理

        ```xml
            <?xml version="1.0" encoding="UTF-8"?>
                <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://www.springframework.org/schema/beans 
                http://www.springframework.org/schema/beans/spring-beans.xsd">
            </beans>
        ```
    使用spring容器
        步骤:
            获取核心容器对象
            根据id获取bean对象
*Spring容器知识点
    ApplicationContext的常用实现类
        ClassPathXmlApplicationContext:（单例对象适用）
                它可以加载类路径下的配置文件，要求配置文件必须在类路径下，不在的话，加载不了(常用)
        FileSystemXmlApplicationContext:（用的时候创建，多例对象适用）
                它可以加载磁盘上任意路径下的配置文件

        AnnotationConfigApplicationContext：它是用于读取注解创建容器的
        
*程序的耦合
    耦合：程序间的依赖关系
        包括：
            类之间的依赖
            方法间的依赖
    解耦：降低程序间的依赖关系
    实际开发中：应该做到：编译器不依赖，运行时才依赖
    解耦的思路:
        第一步：使用反射创建对象，而避免使用new关键字
        第二步：通过读取配置文件来获取要创建的对象全限定类名

*解决耦合所需要的知识点
    Bean：在计算机英语中，有可重用组件的含义
    javaBean：用java语言编写的可重用组件；javabean > 实体类


        

