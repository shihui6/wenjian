时间2021、2、3
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
                            DispatcherServlet类似于一个中央处理器的功能
                        -->
                    <servlet>
                        <servlet-name>springDispatcherServlet</servlet-name>
                        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
                        <init-param>
                            <!--
                                contextConfigLocation：指定springmvc配置文件位置；若不指定配置文件位置，则在web->WEB-INF下创建springmvc-servlet.xml书写配置文件
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

时间2021、2、4
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
时间2021、2、5
    *SpringMVC的原生API
    *SpringMVC除了在方法上传入原生的request和session外还能怎样把数据带给页面
        1.可以在方法处传入Map，或者Model或者ModelMap。这些参数里面保存的所有的数据都会放在域中。可以在页面获取
            Map,Model,ModelMap:最终都是BindingAwareModeMap在工作；相当于给BindingAwareModelMap中保存的东西都会被放在请求域中
                解释：因为Map,Model,ModelMap最终都会继承或者实现BindingAwareModeMap
            
            事例：

            ```java 处理请求并将数据放在域中
                @RequestMapping(value = "model01")
                public String Model(Map<String,String> map){
                    map.put("msg","123");
                    return "success";
                }
                @RequestMapping(value = "model02")
                public String Model02(Model model){
                    model.addAttribute("msg","今天天气真好");
                    return "success";
                }
                @RequestMapping(value = "model03")
                public String Model03(ModelMap model){
                    model.addAttribute("msg","今天太阳真好");
                    return "success";
                }
            ```
            ```jsp  获取域中的数据
                ${requestScope.msg}
            ```

        2.方法返回值可以变为ModelAndView类型
            特点：既包含视图信息(页面地址)也包含模型数据(给页面带的数据)
            而且数据是放在请求域中，request,session,application
            事例：

            ```java
                /**
                * 返回值是ModelAndView；可以为页面携带数据
                */
                @RequestMapping(value = "model04")
                public ModelAndView Model04(ModelMap model){
                    ModelAndView mv = new ModelAndView("success");
                    mv.addObject("msg","你好好哟");
                    return mv;
                }
            ```
        
        3.SpringMVC提供了一种可以临时给Session域中保存数据的方式
            使用一个注解:@SessionAttributes(只能标在类上)
            解释：给BindingAwareModelMap或ModelAndView中保存的数据，同时给session中放一份，value指定保存数据时要给session中放的数据的key；若BindingAwareModelMap没有与value指定的key一致就不保存数据
            用法：
                @SessionAttributes(values={"msg"}):
                解释：
                    value={"msg"}:只要保存的是这种key的数据，给Session中放一份
                    types={String.class}:只要保存的是这种类型的数据，给Session中放一份
            但是我们推荐使用原生API，因为@SessionAttributes()可能会引发异常

    *输出参数
        思路：
            1.SpringMVC要封装请求参数的Book对象不应该是自己new出来的，而应该是从数据库中拿到的准备好的对象
            2.再来使用这个对象封装请求参数
        技术点：@ModelAttribute
                用法：方法上的注解
                特点：这个方法就会提前于目标方法先运行；我们可以在这里提前查出数据库中图书的信息
                步骤:
                    1.先将数据从数据库中取出来，保存在book对象中
                    2.将保存的book对象的数据赋值给变量key
                    3.告诉SpringMVC不要new这个book了，用我刚才保存了一个book
                使用：

                ```java 接受页面过来的请求，代码先执行加了注解@ModelAttribute的方法再执行目标方法
                    @RequestMapping(value = "updateBook")
                    public String updateBook(@ModelAttribute("haha")Book book){
                        System.out.println("页面提交过来的图书信息"+book);
                        return "success";
                    }

                    /**
                    * 1.我们可以在这里提前查出数据库中图书的信息
                    * 2.将这个图书信息保存起来(方便下一个方法能使用)
                    */
                    @ModelAttribute
                    public void myModelAttribute(Map<String,Object> map){
                        Book book = new Book("吴承恩", "西游记", 98.9, 98, 10);
                        map.put("haha",book);
                        System.out.println("modelAttribute方法查询了图书并保存起来了"+book);
                    }
                ```
                ```jsp  页面发送请求
                    <form action="updateBook" method="post">
                    书名：西游记<br/>
                    作者：<input type="text" name="author"><br/>
                    价格：<input type="text" name="price"><br/>
                    库存：<input type="text" name="stock"><br/>
                    存量：<input type="text" name="sales"><br/>
                    <input type="submit" name="提交接口">
                    </form>
                ```

-----------------------------------------------------------------------------------------------------------
**搭建开发环境
    通过idea创建项目SpringMVC项目
        步骤：file->java->Module,此页面勾选上web相关的选项->点击+号添加Name:archetypeCatalog,Value:internal(此配置可以加速构建项目)->生成项目之后，不全目录结构：main文件下新建java,resources文件夹，右键点击java文件Mark directory as,选中Sources root(源码的根目录)。右键点击resources文件Mark directory as,选中Resources Root(资源的根目录)
    
    配置依赖：

        ```xml   在pom.xml中引入如下依赖
            <properties>
                <spring.version>5.0.2.RELEASE</spring.version>
            </properties>
            <dependencies>
                <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
                </dependency>
                <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>${spring.version}</version>
                </dependency>
                <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>${spring.version}</version>
                </dependency>
                <dependency>
                <groupId>javax.servlet</groupId>
                <artifactId>servlet-api</artifactId>
                <version>2.5</version>
                <scope>provided</scope>
                </dependency>
                <dependency>
                <groupId>javax.servlet</groupId>
                <artifactId>jsp-api</artifactId>
                <version>2.0</version>
                <scope>provided</scope>
                </dependency>
            </dependencies>
        ```
    配置前端控制器：

        ```xml 在web.xml中进行配置
            <servlet>
                <servlet-name>dispatcherServlet</servlet-name>
                <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
                <init-param>
                    <!--DispatcherServlet帮我们加载springmvc配置文件，加载之后springmvc里面的扫描注解才会生效-->
                    <param-name>contextConfigLocation</param-name>
                    <param-value>classpath:springmvc.xml</param-value>
                </init-param>
                <load-on-startup>1</load-on-startup>
            </servlet>
            <servlet-mapping>
                <servlet-name>dispatcherServlet</servlet-name>
                <url-pattern>/</url-pattern>
            </servlet-mapping>
        ```
    
    右键点击resources，创建springmvc.xml文件

        ```xml  springmvc.xml的内容
        <!--    开启注解扫描-->
            <context:component-scan base-package="cn.itcast"></context:component-scan>
        <!--    视图解析器-->
            <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                <property name="prefix" value="/WEB-INF/pages/"></property>
                <property name="suffix" value=".jsp"></property>
            </bean>
        <!--    开启springmvc框架注解的支持-->
            <mvc:annotation-driven></mvc:annotation-driven>
        ```
    配置Tomcat服务

*流程介绍
    1.启动服务器，加载一些配置文件
        DispatcherServlet对象被创建
        springmvc.xml被加载
        helloController类创建成对象
        并且可以使用springmvc的注解功能了
    2.发送请求，后台处理请求


