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


    *路由地址的精确匹配
         /**
        * 外层类加注解@RequestMapping("/haha")，为当前类所有的方法的请求地址指定一个基准路径
        * 访问方式：/haha/handle01
        * method:限定请求方式
        *      GET,POST
        *      method=RequestMethod.POST:只接受这种类型的请求，默认是什么都可以;不是规定的方式就会报错
        * params:规定请求参数
        *      params和headers支持简单的表达式
        *          param1:表示请求必须包含名为param1的请求参数
        *              举例：params={"username"},发送请求的时候必须带上一个名为username的参数，没带都会404
        *          !param1：表示请求不能包含名为param1的请求参数
        *              举例：params={"!username"}，发送请求的时候必须不携带上一个名为username的参数；带了都会404
        *          params !=value1
        *              举例：params={"username!=123"},发送请求的时候，携带的username值必须不是123(不带username或者username不是123)
        *          {"param1=value1","param2"}:请求参数必须包含param1和param2的
        *          {"username!=123","pwd","!age"}:请求参数必须满足以上规则；请求的username不能是123，必须有pwd的值，不能有age
        *headers：规定请求头，和params一样也能写简单的表达式
        *      举例：headers = {"User-Agent=浏览器的信息"}
        *
        *consumes:只接受内容类型是哪种的请求，规定请求头中的Content-Type
        *produces：告诉浏览器返回的内容是什么，给响应头中加上Content-Type:text/html;charset=utf-8
        */

            ```java
                @RequestMapping("/haha")
                @Controller
                public class RequestMappingTestController {
                    @RequestMapping(value = "handle01",method = RequestMethod.POST)
                    public String handle01(){
                        System.out.println("RequestMappingTestController。。。正在被请求");
                        return "success";
                    }
                    // 前端通过ip:端口号/haha/handle03?username=pwd即可访问到该方法
                    @RequestMapping(value = "handle03",params = {"username!=123","pwd","!age"})
                    public String handle03(){
                        System.out.println("handle03.。。请求到了");
                        return "success";
                    }

                //    @RequestMapping(value = "handle04",headers = {"User-Agent=浏览器的信息"})
                //    public String handle04(){
                //        System.out.println("handle04.。。请求到了");
                //        return "success";
                //    }
                }
            ```

    *通配符的使用
        /**
        * URL地址可以写模糊的通配符
        *      ?:能替代任意一个字符
        *          模糊和精确多个匹配情况下，精确优先
        *      *:能替代任意多个字符，和一层路径
        *      **:能替代多层路径
        */

            ```java
                @RequestMapping("haha")
                @Controller
                public class RequestMappintTest {
                    //    使用通配符?  anTest0?,用来匹配一个字符，可以anTest01或者anTest02都可以
                    @RequestMapping("anTest0?")
                    public String antTest01(){
                        System.out.println("进入到antTest01了");
                        return "success";
                    }
                    //    使用通配符*，匹配多个字符，精确优先
                    @RequestMapping("anTest0*")
                    public String antTest02(){
                        System.out.println("进入到antTest02了");
                        return "success";
                    }
                    //    使用通配符*，匹配一层路径，多层路径不支持，精确优先
                    @RequestMapping("/a/*/anTest0*")
                    public String antTest03(){
                        System.out.println("进入到antTest03了");
                        return "success";
                    }
                    //    使用通配符**，匹配多层路径，精确优先
                    @RequestMapping("/a/**/anTest0*")
                    public String antTest04(){
                        System.out.println("进入到antTest04了");
                        return "success";
                    }
                }
            ```
    *路径上的占位符

            ```java
                //路径上可以有占位符，占位符语法：可以在任意路径的地方写一个{变量名}，路径上的占位符只能占一层路径
                //  /user/admin
                @RequestMapping("/user/{id}")
                public String antTest05(@PathVariable("id") String id){
                    System.out.println("进入到antTest05了,并且获得参数为："+id);
                    return "success";
                }
            ```
    
    *Rest
        系统希望以非常简洁的URL地址来发请求，怎样表示对一个资源的增删改查用请求方式来区分
        Rest推荐：url地址这么起名:/资源名/资源标识符
            系统的URL地址就这么来设计即可：简洁的URL提交请求，以请求方式区分对资源操作
                /book/1     :GET-----查询1号图书
                /book/1     :PUT-----更新1号图书
                /book/1     :DELETE-----删除1号图书
                /book/1     :POST-----添加图书
            事例：

                ```html html发送请求  
                    <a href="book/1">查询图书</a></br>
                    <form action="book/1" method="post">
                    <input type="submit" value="添加1号图书">
                    </form>
                ```
                ```java 接受html发送过来的请求
                    @RequestMapping(value = "/book/{bid}",method = RequestMethod.GET)
                    public String getBook(@PathVariable("bid") String id){
                        System.out.println("查询"+id+"号图书成功");
                        return "success";
                    }

                    @RequestMapping(value = "/book/{bid}",method = RequestMethod.POST)
                    public String PostBook(@PathVariable("bid") String id){
                        System.out.println("post到了"+id+"号图书成功");
                        return "success";
                    }
                ```
            
            问题：从页面上只能发起post和get请求，其他请求是发不出来的

        如何从页面中发起PUT，DELETE形式的请求？Spring提供了对Rest风格的支持
            1.SpringMVC中有一个Filter；他可以把普通的请求转化为规定形式的请求；配置这个filter
                首先在web.xml中配置一下filter

                    ```xml
                        <filter>
                            <filter-name>HiddenHttpMethodFilter</filter-name>
                            <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
                        </filter>
                        <filter-mapping>
                            <filter-name>HiddenHttpMethodFilter</filter-name>
                            <url-pattern>/*</url-pattern>
                        </filter-mapping>
                    ```
            2.如何发其他形式的请求如：DELETE,PUT
                按照以下要求:1.创建一个post类型的表单，2.表单项中携带一个_method的参数，3.这个_method对应的值就是要发送请求的方法

                事例：

                    ```html 发送delete和put请求
                        <form action="book/1" method="post">
                        <input name="_method" value="delete">
                        <input type="submit" value="删除1号图书">
                        </form>
                        </br>
                        <form action="book/1" method="post">
                        <input name="_method" value="put">
                        <input type="submit" value="put1号图书">
                        </form>
                    ```
                    ```java 接受delete或put请求
                        @RequestMapping(value = "/book/{bid}",method = RequestMethod.PUT)
                        public String putBook(@PathVariable("bid") String id){
                            System.out.println("PUT到了"+id+"号图书成功");
                            return "success";
                        }

                        @RequestMapping(value = "/book/{bid}",method = RequestMethod.DELETE)
                        public String deleteBook(@PathVariable("bid") String id){
                            System.out.println("DELETE到了"+id+"号图书成功");
                            return "success";
                        }
                    ```
                

    *SpringMVC获取请求带来的各种信息
        1.SpringMVC获取请求参数带来的各种信息
            *      默认方式获取请求参数
            *          直接给方法参数上写一个和请求参数名相同的变量。这个变量就来接受请求参数的值；带了参数：有值；没带：null
            *      @RequestParam:获取请求参数的;参数默认是必须带的
            *              @RequestParam(value = "user",required = false,defaultValue = "shihui")
            *                  意思是：形参username的值就是即：username = request.getParameter("user")
            *                  解释@RequestParam的参数含义：
            *                          value:指定要获取的参数的key
            *                          required：指定这个参数是否是必须的，默认是true
            *                          defaultValue:默认值，没有带默认值是null

                ```java
                    @RequestMapping("/canshu")
                    public String canshu(@RequestParam(value = "user",required = false,defaultValue = "shihui") String username){
                        System.out.println("参数为："+username);
                        return "success";
                    }
                ```
        
        2.获取请求头的参数的信息和cookie的值
            @RequestHeader：获取请求头中的某个key
            *              @RequestHeader("User-Agent") String userAgent
            *                  意思是，形参userAgent的值即是：request.getHeader("User-Agent")
            *                  如果请求头中没有这个值就会报错，但是可以指定属性，让他赋予默认值
            *                  value:定要获取的参数的key
            *                  required:指定这个参数是否是必须的，默认是true
            *                  defaultValue:默认值，没有带默认值是null
            *
            @CookieValue::获取某个cookie

                ```java
                    @RequestMapping("/canshu")
                    public String canshu(@RequestParam(value = "user",required = false) String username,
                                        @RequestHeader("User-Agent") String userAgent,
                                        @CookieValue(value = "JSESSIONID",required = false)String cookieid){
                        System.out.println("参数为："+username);
                        System.out.println("请求头中的值："+userAgent);
                        System.out.println("获取cookie的值："+cookieid);
                        return "success";
                    }
                ```

    *SpringMVC会自动为这个POJO进行赋值,将参数通过javabean进行封装
            特点：
                1.将pojo中的每一个属性，从request参数中尝试获取出来，并封装即可
                2.还可以封装级联属性：属性的属性
                3.请求的参数的参数名和对象中的属性一一对应就行

                    提交数据可能有乱码
                        请求乱码：
                            GET请求：该server.xml;在8080端口处URLEncoding="UTF-8"
                            POST请求:在第一次请求获取参数之前设置：request.setCharacterEncoding("UTF-8")
                        响应乱码:
                            response.setContentType("text/html;charset=utf-8")

                        解决乱码的步骤：

                            ```xml  在web.xml中配置filter
                                <!--配置一个字符编码的Filter；一定要注意，字符编码filter一般都在其他filter之前，否则没法解决问题-->
                                <filter>
                                    <filter-name>CharacterEncodingFilter</filter-name>
                                    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
                                    <!--        encoding：指定解决post请求乱码-->
                                    <init-param>
                                        <param-name>encoding</param-name>
                                        <param-value>UTF-8</param-value>
                                    </init-param>
                                    <!--        forceEncoding：解决响应乱码-->
                                    <init-param>
                                        <param-name>forceEncoding</param-name>
                                        <param-value>true</param-value>
                                    </init-param>
                                </filter>
                                <filter-mapping>
                                    <filter-name>CharacterEncodingFilter</filter-name>
                                    <url-pattern>/*</url-pattern>
                                </filter-mapping>
                            ```

                ```html     前端页面发送请求
                    <form action="book" method="post">
                    书名：<input type="text" name="bookName"><br/>
                    作者：<input type="text" name="author"><br/>
                    价格：<input type="text" name="price"><br/>
                    库存：<input type="text" name="stock"><br/>
                    存量：<input type="text" name="sales"><br/>
                    街道：<input type="text" name="address.dress"><br/>
                    城市：<input type="text" name="address.city"><br/>
                    <input type="submit" name="提交接口">
                    </form>
                ```
                ```java     后端接收请求
                    @RequestMapping("/book")
                    public String Book(Book book){
                        System.out.println("我要保存的图书"+book);
                        return "success";
                    }
                ```
                ```java     封装Book类即POJO数据的类
                    public class Book {
                        private String author;
                        private String bookName;
                        private Double price;
                        private Integer stock;
                        private Integer sales;
                        private Address address;
                    }
                    // 已经省略getter和setter方法，构造函数方法
                ```
                ```java     级联关系的Address的POJO数据的类
                    public class Address {
                        private String dress;
                        private String city;
                    }
                    // 已经省略getter和setter方法，构造函数方法
                ```

    *SpringMVC的原生API
    
