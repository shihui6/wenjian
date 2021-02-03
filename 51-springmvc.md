使用：
    导包
        1.springmvc是spring的web模块；所有模块的运行都是依赖核心模块(ioc模块)
            1.核心模块
                commons-logging-1.1.3.jar
                spring-aop-4.0.0.RELEASE.jar
                spring-beans-4.0.0.RELEASE.jar
                spring-context-4.0.0.RELEASE.jar
                spring-core-4.0.0.RELEASE.jar
                spring-expression-4.0.0.RELEASE.jar
            2.web模块
                spring-web-4.0.0.RELEASE.jar
                spring-webmvc-4.0.0.RELEASE.jar
    写配置  
        1.web.xml可能要写的配置
            配置springmvc的前端控制器，指定springmvc配置文件位置

                ```xml
                        <!--
                            springmvc思想是有一个前端控制器能拦截所有请求，并只能派发
                            这个前端控制器是一个servlet；应该在web.xml中配置这个servlet来拦截所有请求
                        -->
                    <servlet>
                        <servlet-name>springDispatcherServlet</servlet-name>
                        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
                        <init-param>
                            <!--
                                contextConfigLocation：指定springmvc配置文件位置
                            -->
                            <param-name>contextConfigLocation</param-name>
                            <param-value>classpath:springmvc.xml</param-value>
                        </init-param>
                        <!--
                            servlet启动加载，servlet原本是第一次访问创建对象
                            load-on-startup:服务器启动的时候创建对象；值越小优先级越高，越先创建对象
                        -->
                        <load-on-startup>1</load-on-startup>
                    </servlet>
                    <servlet-mapping>
                        <servlet-name>springDispatcherServlet</servlet-name>
                        <!--
                            /*和/都是拦截所有请求；/：会拦截所有请求，但是不会拦截*.jsp;能保证jsp访问正常
                            /*的范围更大，还会拦截到*.jsp这些请求，一旦拦截jsp页面所有的都没法显示了
                        -->
                        <url-pattern>/</url-pattern>
                    </servlet-mapping>
                ```

        2.框架自身的配置

                ```xml  位置：根据全局web.xml的<param-value>classpath:springmvc.xml</param-value>信息
                    <!--扫描包下面所有组件-->
                    <context:component-scan base-package="com.atguigu"></context:component-scan>

                    <!--配置一个视图解析器；能帮我们拼接页面地址-->
                    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                        <property name="prefix" value="/WEB-INF/pages/"></property>
                        <property name="suffix" value=".jsp"></property>
                    </bean>
                ```
        3.根据springmvc.xml中配置的context:component-scan标签扫描com.atguigu包下的所有的类

                ```java
                    /**
                    * 1.告诉SpringMVC这是一个处理器，可以处理请求
                    *      @Controller:标识哪个组件是控制器
                    */
                    @Controller
                    public class MyFirstController {
                        /**
                        *  /代表从当前项目下开始;处理当前项目下的hello请求
                        */
                        @RequestMapping("/hello")
                        public String myfisrtRequest(){
                            System.out.println("请求收到了......正在处理中");
                            //视图解析器自动拼串
                            //<property name="prefix" value="/WEB-INF/pages/"></property>
                            //<property name="suffix" value=".jsp"></property>
                            //   (前缀) /WEB-INF/pages/+返回值(success)+后缀(.jsp)
                            return "success";
                        }
                    }
                ```
    运行流程：
        1.
            1.客户端点击链接会发送http://localhost:8088/javamvcF/hello
            2.来到tomcat服务器
            3.SpringMVC的前端控制器收到所有请求
            4.来看请求地址和@RequestMappint标注的哪个匹配，来找到底使用哪个类的哪个方法来处理请求
            5.前端控制器找到了目标处理器类和目标方法，直接利用反射执行目标方法
            6.方法执行完成以后会有一个返回值；SpringMVC认为这个返回值就是要去的页面地址
            7.拿到这个返回值以后；用视图解析器进行拼串得到完成的页面地址
            8.拿到页面地址，前端控制器帮我们转发到页面
        2.@RequestMappint
            作用：
                就是告诉Springmvc，这个方法是用来处理什么请求的；/可以省略，即使省略了，也会默认从当前项目下开始
        3.如果不指定配置文件位置(springmvc.xml的位置)
            如果不指定也会默认在/WEB-INF下找一个 前端控制器名-servlet.xml，所以我们在/WEB-INF目录下创建相应的xml文件即可
