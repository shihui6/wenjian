###Tomcat
    ***介绍
        web资源的分类
            web资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源
                静态资源：html,css,js,txt,mp4视频，jpg图片
                动态资源：jsp页面，Servlet程序、
        
        常用的web服务器
            Tomcat：由Apache组织提供的一种web服务器，提供对jsp和servlet的支持。它是一种轻量级的javaweb容器(服务器)，也是当前应用最广泛的javaweb服务器(免费)
            Resin：是一个非常流行的服务器，对servlet和jsp提供了良好的支持，性能也比较优良，resin自身采用了java语言开发(收费，应用比较多)


        Tomcat服务器和servlet版本的需要一一对应的关系，如：Tomcat服务器版本高于servlet版本互相不支持
            Servlet程序从2.5版本是现在市面使用最多的版本(xml配置)
            到了servlet3.0之后。就是注解版本的servlet使用

    ***Tomcat的使用
        *安装
            找到你需要的Tomcat版本对应的zip压缩包，解压到需要安装的目录即可
        
        *目录介绍
            bin     专门用来存放Tomcat服务器的可执行程序
            conf    专门用来存放Tomcat服务器的配置文件
            lib     专门用来存放Tomcat服务器的jar包
            logs    专门用来存放Tomcat服务器运行时输出的日记信息
            temp    专门用来存放Tomcat运行时产生的临时数据
            webapps 专门用来存放部署的Web工程
            work    是Tomcat工作时的目录，用来存放Tomcat运行时jsp翻译为Servlet的源码，和session钝化(将对象序列化写到磁盘上)的目录

        
        *启动Tomcat服务器
            方式一：
                找到Tomcat目录下的bin目录下的startup.bat文件，双击，就可以启动Tomcat服务器
                如何测试Tomcat服务器启动是否成功？
                    打开浏览器，在浏览器地址栏中输入以下地址测试：http:localhost:8080
            
                常见的启动失败的情况有，双击startup.bat文件，就会出现一个小黑窗口一闪而过，这个时候，失败的原因基本上都是因为没有配置好JAVA_HOME环境变量(配置的是jdk的环境变量，只需要配置到jdk的安装目录即可。不需要带上bin目录)

            方式二：
                在tomcat的bin目录下命令行输入catalina run即可

        *Tomcat服务器的停止
            方式一：点击tomcat服务窗口关闭
            方式二：命令行ctrl+c
            方式三：找到tomcat的bin目录下的shutdown.bat双击，即可停止tomcat服务

        *修改Tomcat的端口号
            mysql默认的端口号是：3306
            Tomcat默认的端口号是：8080
            修改tomcat的步骤：
                找到Tomcat目录下的conf目录，找到server.xml配置，找到Connector标签，修改port属性值为你需要的端口号

            HTTP协议默认的端口号是80，若写80端口，浏览器会自动抹去，写和不写都一样

        *部署web工程到Tomcat中
            第一种方法：
                只需要把web工程的目录拷贝到tomcat的webapps目录下即可
                如何访问：如，需要访问部署在tomcat上的book工程，则服务地址+tomcat在服务器上的的端口号+book
                            http://localhost:8080/book/index.html(即访问的是tomcat上book工程)
            第二种方法：
                找到tomcat下的conf目录\Catalina\localhost\下，创建如下的配置文件,abc.xml写入下面的内容
                    <Context path="/abc" docBase="E:\book" />
                        Context表示一个工程上下文
                        path表示工程的访问路径:/abc
                        docBase表示你的工程目录在哪里(通过docBase映射到相关目录下)

                    第二种方式的访问路径如：http://localhost:80/abc(表示访问的是磁盘E:/book目录)

            第三种方式
                当我们在浏览器地址栏中输入访问地址如下
                    http://ip:port/ ===>没有工程名的时候，默认访问的是webapps->ROOT工程里面的内容
                当我们在浏览器中输入的访问地址如下
                    httP://ip:port/ ===>没有资源名，默认访问index.html页面


        *IDEA中如何创建动态web工程
            1.new->Module->Java Enterprise页，选择应用的服务器，一定要勾选Web Application;然后生成的web项目
                生成的web项目的目录介绍
                    src目录存放自己编写的java源代码
                    web目录专门用来存放web工程的资源文件；比如：html页面，css文件，js文件等等
                    web->WEB-INF目录是一个受服务器保护的目录，浏览器无法直接访问到此目录的内容
                    web->WEB-INF->web.xml它是整个动态web工程的配置部署描述文件可以在这里配置很多web工程组件
                            比如：Servlet程序，Filter过滤器，Listener监听器，Session超时......等等
                    web->WEB-INF->lib目录用来存放第三方的jar包(IDEA需要自己配置导入)
                            为什么需要配置导入的jar
                                因为复制粘贴进来的jar包，在项目里不可使用，必须将jar包添加到项目的类库中才可以
                            jar添加到类库的方式
                                方式一
                                    将lib里面的jar包导入到当前类库，点击右键Add as Library就可以将jar包添加到当前类库
                                方式二
                                    打开项目结构菜单操作界面，添加一个自己的类库File->Project Structure->Libraries->java
                                        然后选择项目里类库需要的jar包(前提要将这些需要添加的jar包复制到项目里，才可以选择)，点击确定
                                        然后选择这些jar生成的类库可以使用到的模块
                                        选择Artifacts选项，将类库，添加到打包部署中；操作Artifacts->fix即可

            2.在idea上创建web项目，通过tomcat运行起来的执行机制：
                通过idea启动项目的时候，Idea会整合Tomcat之后。Tomcat被拷贝一些副本(放在对应的目录里)，此副本里配置了访问web项目的信息。
                同时idea也会把web项目的web目录拷贝副本(放在对应的目录)把web目录的副本所在路径放在被拷贝的Tomcat的副本里的xml的配置信息的docBase里，通过路由即可以访问到web项目。
                通过Idea启动之后，通过路由即可访问

        *在idea中启动部署web模板


###Servlet技术
    ***Servlet
        *什么是Servlet
            1.Servlet是javaEE规范之一。规范就是接口
            2。Servlet是javaweb三大组件之一。三大组件分别是：Servlet程序，Filter过滤器，Listener监听器
            3.Servlet是运行在服务器上的一个java小程序，它可以接受客户端发送过来的请求，并响应数据给客户端


        *手动实现Servlet程序
            1.编写一个类去实现Servlet接口
            2。实现service方法，处理请求，并相应数据
            3.到web.xml中去配置Servlet程序的访问地址
                配置文件

                    ```xml
                        <?xml version="1.0" encoding="UTF-8"?>
                        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                                version="4.0">

                            <!--servlet标签给Tomcat配置Servlet程序-->
                            <servlet>
                                <!--servlet-name标签 Servlet程序起一个别名(一般是类名)-->
                                <servlet-name>Helloservlet</servlet-name>
                                <!--servlet-class是Servlet程序的全类名  -->
                                <servlet-class>com.atguigu.servlet.Helloservlet</servlet-class>
                                <init-param>
                                    <param-name>username</param-name>
                                    <param-value>root</param-value>
                                </init-param>
                            </servlet>
                            <servlet-mapping>
                                <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
                                <servlet-name>Helloservlet</servlet-name>
                                <!--url-pattern标签配置访问地址
                                    /斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径(就是在tomcat里面的那个访问路径)
                                    /hello 表示地址为：http://ip:port/工程名/hello(该地址访问对应的helloservlet这个类)
                                -->
                                <url-pattern>/hello</url-pattern>
                            </servlet-mapping>
                        </web-app>
                    ```
                说明：为什么输入hello之后就可以访问到配置的Helloservlet的类
                    执行流程：
                        如：
                        当输入http://localhost:8088/06_servlet；访问的是8088端口下的06_servlet这个web项目
                        当输入http://localhost:8088/06_servlet/hello;当地址后添加/hello的时候，程序会首先到配置文件web.xml里面找有没有/hello这个路径，若有就找该路径对应的<serlet-name>标签对应的类名，通过类名和标签<servlet-class>找到对应要访问的类的位置

        
        *Servlet的生命周期
            1.执行Servlet构造方法
            2.执行init初始化方法
                第一，二步，是在第一次访问的时候创建Servlet程序会调用
            3.执行service方法
                第三步，每次访问都会调用
            4.执行destroy销毁方法
                第四步，在web工程停止的时候调用

        *实现Servlet程序
            方式一
                通过继承HttpServlet实现Servlet程序
                    一般在实际项目开发中，都是使用继承HttpServlet类的方式去实现Servlet程序
                        1.编写一个类去继承HttpServlet类
                        2.根据业务需要重写doGet或doPost方法
                        3.到web.xml中配置Servlet程序的访问地址

            方式二
                通过idea快速生成
                    步骤：在package包下，new->Create new Servlet，然后就可以快速生成相应的配置和class类了

        *ServletConfig类
            概念：ServletConfig类是Servlet程序的配置信息类
                    Servlet程序和ServletConfig对象都是由Tomcat负责创建
                    Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时，就创建一个对应的ServletConfig对象
            作用：
                1.可以获取servlet程序的别名servlet-name(配置文件xml的配置信息)的值
                2.获取初始化参数init-param(配置文件xml的配置信息)
                3.获取ServletContext对象

                事例：实现Servlet接口的时候生成的int方法形参类型是ServletConfig,里面写的

                    ```java
                        public void init(ServletConfig servletConfig) throws ServletException {
                        //1.可以获取servlet程序的别名servlet-name(在配置xml文件里)
                            System.out.println("Helloservlet程序别名"+servletConfig.getServletName());
                        //2.获取初始化参数init-param(在配置xml文件里)
                            System.out.println("初始化参数username的值"+servletConfig.getInitParameter("username"));
                        //3.获取ServletContext对象
                            System.out.println(servletConfig.getServletContext());
                        }
                    ```

时间2020/11/27
        *ServletContext类
            什么是ServletContext
                1.ServletContext是一个接口，它表示Servlet上下文对象
                2.一个web工程，只有一个ServletContext对象
                3.ServletContext对象是一个域对象
                    什么是域对象？
                        域对象，是一个像Map一样存取数据的对象，叫域对象
                        这里的域指的是存取数据的操作范围，整个web工程
                4.ServletContext是在web工程部署启动的时候创建。在web工程停止的时候销毁

            ServletContext的作用：
                1.获取web.xml中配置的上下文参数context-param
                2.获取当前的工程路径，格式：/工程路径
                3.获取工程部署后在服务器硬盘商的绝对路径
                4.像Map一样存取数据

            ServletContext类的使用
                事例：在Servlet类中的使用

                    ```java
                            protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        //        1.获取web.xml中配置的上下文参数context-param
                                /*
                                    前提要在xml配置文件里添加context-param标签内容
                                    如：<context-param>
                                            <param-name>username</param-name>
                                            <param-value>selectContext</param-value>
                                        </context-param>
                                    context-param当前工程下所有的类都可以访问
                                */
                                ServletContext context = getServletConfig().getServletContext();
                                String username = context.getInitParameter("username");
                                System.out.println("context-param参数的username的值"+username); //输出结果selectContext
                        //        2.获取当前的工程路径，格式：/工程路径
                                System.out.println("当前工程路径"+context.getContextPath());
                                    //输出结果 /06_servlet
                        //        3.获取工程部署后在服务器硬盘商的绝对路径
                                /*
                                    "/"斜杠被服务器解析成地址为http://ip:port/工程名/  映射到IDEA代码的web目录
                                */
                                System.out.println("工程部署的路径"+context.getRealPath("/"));
                                System.out.println("工程下css的绝对路径"+context.getRealPath("/css"));
                                    //输出结果为C:\javaceshi\out\artifacts\06_servlet_war_exploded\
                        //        4.像Map一样存取数据
                                    // //获取ServletContext对象
                                    //         ServletContext servletContext = getServletContext();
                                    //         servletContext.setAttribute("key1","value1");
                                    //         System.out.println(servletContext.getAttribute("key1"));
                            }
                    ```

        *Http协议
            什么是http协议
                什么是协议：协议是指双方，或多方，相互约定号，大家都需要遵守的规则，叫协议
                所谓http协议，就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫http协议
                http协议中的数据又叫报文


        *HttpServletRequest类
            作用：每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中。然后传递到service方法(doGet和doPost)中给我们使用。我们可以通过HttpServletRequest对象，获取到所有请求信息

            1.常用的方法：
                getRequestURI()             获取请求的资源路径
                getRequestURL()             获取请求的统一资源定位符(绝对路径)
                getRemoteHost()             获取客户端的IP地址
                getHeader()                 获取请求头
                getParameter()              获取请求的参数
                getParameterValues()        获取请求的参数(多个值的时候使用)
                getMethod()                 获取请求的方式GET或POST
                setAttribute(key,value)     设置域数据
                getAttribute(key)           获取域数据
                getRequestDispatcher()      获取请求转发对象

                >事例：API的使用

                    ```java
                        public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                        //        getRequestURI()             获取请求的资源路径
                                System.out.println("url=>"+req.getRequestURI());
                        //        getRequestURL()             获取请求的统一资源定位符(绝对路径)
                                System.out.println("URL=>"+req.getRequestURL());
                        //        getRemoteHost()             获取客户端的IP地址
                                /*
                                    在idea中，使用localhost访问时，得到的客户端ip地址是===》127.0.0.1
                                    在idea中，使用127.0.0.1访问时，得到的客户端ip地址是===》127.0.0.1
                                    在idea中，使用 真实ip 访问时，得到的客户端ip地址是===》真实的客户端的IP地址
                                */
                                System.out.println("客户端的ip地址=>"+req.getRemoteHost());
                        //        getHeader()                 获取请求头
                                System.out.println("获取请求头的User-Agent=>"+req.getHeader("User-Agent"));
                        //        getMethod()                 获取请求的方式GET或POST
                                System.out.println("请求的方式=>"+req.getMethod());

                                //设置请求体的字符集为UTF-8，从而解决post请求的中文乱码的问题
                                //setCharacterEncoding要在获取请求参数之前调用才有效，否则后面获取的中文参数会有乱码
                                req.setCharacterEncoding("UTF-8");

                        //        获取请求的参数
                                //获取一个参数对应一个值
                                //String username = req.getParameter("username");
                                //获取一个参数对应多个值,返回的是个数组
                        //        String[] hobbies = req.getParameterValues("hobby");
                            }
                    ```

            
                >事例:请求转发的使用getRequestDispatcher()
                    请求转发的特点：
                        1.浏览器地址栏没有变化
                        2.他们是一次请求
                        3.他们共享Request域中的数据
                        4.可以转发到WEB-INF目录下
                            注意点：平时放在web工程下，且非WEB-INF目录下的页面通过地址可以访问
                        5.是否可以访问工程以外的资源(不可以)

                    ```java 下面是Servlet1类中的代码
                        public class Servlet1 extends HttpServlet {
                            public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                                //获取请求的参数(办事的材料)查看
                                String username = req.getParameter("username");
                                System.out.println("在Servlet1中查看的参数"+username);

                                req.setAttribute("key1","我的天哪，太好看了吧");

                                /*
                                    请求转发必须要以斜杠开头，/ 斜杠表示地址为:http://ip:port/工程名/,映射到IDEA代码的web目录
                                */
                                RequestDispatcher requestDispatcher = req.getRequestDispatcher("/Servlet2");
                                //走向Servlet2  就会执行Servlet2的程序
                                requestDispatcher.forward(req,resp);
                            }
                        }
                    ```

                    ```java 下面是Servlet2中的代码
                        public class Servlet2 extends HttpServlet {
                            public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                                //获取请求的参数(办事的材料)查看
                                String username = req.getParameter("username");
                                System.out.println("在Servlet1中查看的参数"+username);
                                //查看 柜台1 是否有
                                Object key1 = req.getAttribute("key1");
                                System.out.println("柜台1是否有章"+key1);
                                //处理自己的业务
                                System.out.println("servlet2处理自己的业务");
                            }
                        }
                    ```

                >事例：base标签的作用：可以设置当前页面中所有相对路径工作时，参照哪个路径来进行跳转
                        路由地址跳转相对路径的工作原理：
                            当我们点击a标签进行跳转的时候，浏览器地址栏中的地址是：http://localhost:8080/07_servlet/a/b/c.html
                            跳转回的a标签路径是:../../index.html
                            所有相对路径在工作时候都会参照当前浏览器地址栏中的地址来进行跳转
                            参照浏览器地址栏中的地址得到的地址是http://localhost:8080/07_servlet/index.html
                        
                        当在html中设置了base标签，并赋了url地址，那么相对路径就会屏蔽浏览器地址栏中的地址，而会参照base的地址来进行跳转


        *HttpServletResponse类
            作用：
                HttpServletResponse类和HttpServletRequest类一样。每次请求进来，Tomcat服务器都会创建一个Response对象传递给Servlet程序去使用。HttpServletRequest表示请求过来的信息，HttpServletResponse表示所有响应的信息

                我们如果需要设置返回给客户端你的信息，都可以通过HttpServletResponse对象来进行设置

            怎么用：
                字节流      getOutputStream()       常用于下载(传递二进制数据)
                字符流      getWrite()              常用于回传字符串(常用)
                注意：两个流同时只能使用一个，使用了字节流，就不能再使用字符流，反之亦然，否则就会报错

                事例：通过流将信息写入到浏览器，并设置浏览器使用UTF-8字符集

                    ```java
                        public class Servlet2 extends HttpServlet {
                            public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                                //设置服务器的和客户端使用UTF-8字符集
                                    // 方法一：
                                        // //设置服务器字符集UTF-8
                                        // resp.setCharacterEncoding("UTF-8");
                                        // //通过响应头，设置浏览器也使用UTF-8字符集
                                        // resp.setHeader("Content-Type","text/html;charset=UTF-8");
                                    //方法二：
                                        //此方法同时设置服务器和客户端都使用UTF-8字符集，还设置了响应头
                                        //此方法一定要在获取流对象之前调用才有效，否则无效
                                        resp.getContentType("text/html;charset=UTF-8");

                                //获取字符流对象
                                PrintWriter writer = resp.getWriter();
                                writer.writer("今天天气真好")
                            }
                        }
                    ```

            1.请求重定向
                使用：在response1中，跳转到response2里

                    ```java
                        public class Servlet2 extends HttpServlet {
                            public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                            // 重定向方法一
                                // //设置响应状态码302，表示重定向(做个标记)
                                // resp.setStatus(302);
                                // //设置响应头，说明新的地址在哪里
                                // resp.setHeader("Location","http://localhost:8080/07_servlet/response2");
                            
                            // 重定向方法二,一行代码搞定
                                resp.sendRedirect("http://localhost:8080/07_servlet/response2");
                            }
                    ```
                    执行机制：通过http://localhost:8080/07_servlet/response1，访问Servlet2类的时候，设置响应状态码302，响应头的地址，浏览器就会自动重定向跳转到http://localhost:8080/07_servlet/response2地址。

                特点：
                    1.浏览器地址栏会发生变化
                    2。两次请求
                    3.不共享Request域中的数据
                        (因为浏览器请求一次服务器，服务器只创建一个Request对象，重定向则是浏览器请求了两次，创建了两个Request对象，所以数据不共享)
                    4.不能访问WEB-INF下的资源
                    5.可以访问工程外的资源

时间2020/11/30
###JSP
    **jsp
        1.概念：jsp的全称是java server pages；java的服务器页面
        2.作用：jsp主要作用是代替servlet程序回传html页面的数据
                因为servlet程序回传html页面数据是一件非常繁琐的事情。开发成本和维护成本都极高

        3.使用：
            如何创建jsp页面
                在创建的javaweb工程下，在web目录下new->jsp/jspx创建文件名即可
            如何访问：
                jsp页面和html页面一样，都是存放在web目录下。访问也跟访问html页面一样
                比如：在web目录下有如下的文件
                    web目录：
                        a.html页面      访问地址=====> http://ip:port/工程路径/a.html
                        b.jsp页面      访问地址=====> http://ip:port/工程路径/b.jsp

            本质：
                jsp页面本质上是Servlet程序
                当我们第一次访问jsp页面的时候。Tomcat服务器会帮我们把jsp页面翻译成为一个java源文件。并且对它进行编译成为.class字节码程序。我们打开java源文件不难发现里面的内容

                    ```java
                        public final class a_jsp extends org.apache.jasper.runtime.HttpJspBase
                    ```
                    ```java
                        public abstract class HttpJspBase extends HttpServlet implements HttpJspPage{}
                    ```
                上面是.class文件中的源代码的一分部
                我们跟踪源代码发现，HttpJspBase类。它直接地继承了HttpServlet类。也就是说。jsp翻译出来的java类，它间接的继承了HttpServlet了。也就是说，翻译出来的是一个Servlet程序

                总结：通过翻译出来的代码，不难发现jsp就是Servlet程序；可以观察翻译出来的Servlet程序的源代码，不难发现。其底层实现，也是通过输出流。把html页面数据回传给客户端的

                事例：本质通过输出流将html页面回传给客户端的

                    ```java
                        PrintWriter writer = resp.getWriter();
                        writer.writer("<html lang=\"en\">\r\n")
                        writer.writer("<head>\r\n")
                        writer.writer("</head>\r\n")
                        writer.writer("</html>\r\n")
                    ```

时间2020/12/01
        *jsp的三种语法
            1.jsp头部的page指令
                概念：jsp的page指令可以修改jsp页面中一些重要的属性，或者行为
                    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
                    >language属性           表示jsp翻译后是什么语言文件。暂时只支持java
                    >contentType属性        表示jsp返回的数据类型是什么。也是源码中response.setContentType()参数值
                    >pageEncoding属性       表示当前jsp页面文件本身的字符集
                    >import属性             跟java源码中一样。用于导包，导类
                    ---------------------------两个属性是给out输出流使用------------------------------
                    >autoFlush属性          设置当out输出流缓冲区满了之后，是否自动刷新缓冲区。默认值是true
                    >buffer属性             设置out缓冲区的大小。默认是8kb
                    ---------------------------两个属性是给out输出流使用------------------------------
                    >errorPage属性          设置当jsp页面运行时出错，自动跳转去的错误页面路径
                        事例：当此页面发生错误时，设置errorPage跳转到b.jsp页面
                        ```jsp
                            <%@ page contentType="text/html;charset=UTF-8"
                                    errorPage="/b.jsp"
                                    language="java" %>
                            <%--
                                errorPage表示错误后自动跳转的路径
                                这个路径一般都是以斜杠开头，它表示请求地址为http://ip:port/工程路径/
                                映射到代码的web目录
                            --%>
                            <html>
                            <head>
                                <title>Title</title>
                            </head>
                            <body>
                            这是首个jsp页面
                                <%
                                    int i = 12 /0; //这行代码会报错，会导致此页面运行出错，会自动跳转到设置的错误页面的路径b.jsp
                                %>
                            </body>
                            </html>
                        ```
                    >isErrorPage属性        设置当前jsp页面是否是错误信息页面。默认false。如果是true可以获取异常信息
                    >session属性            设置访问当前jsp页面，是否会创建HttpSession对象。默认是true
                    >extends属性            设置jsp翻译出来的java类默认继承谁
        

            2.jsp的常用脚本
                1.jsp中常用脚本(极少用)
                    1.声明脚本
                        声明脚本的格式：<%! 声明java代码 %>
                        作用：可以给jsp翻译出来的java类定义属性和方法甚至是静态代码块。内部类等

                        事例：在jsp页面中操作：声明类属性|声明static静态代码块|声明类方法|声明内部类

                            ```java
                                <%-- 1声明类属性--%>
                                <%!
                                    private Integer id;
                                    private String name;
                                    private static Map<String,Object> map;
                                %>
                                <%-- 2.声明static静态代码块--%>
                                <%!
                                    static {
                                        map = new HashMap<String,Object>();
                                        map.put("key1","value1");
                                        map.put("key2","value2");
                                        map.put("key3","value3");
                                    }
                                %>
                                <%-- 3.声明类方法--%>
                                <%!
                                    public int abc(){
                                        return 12;
                                    }
                                %>
                                <%--4.声明内部类--%>
                                <%!
                                    public static class A{
                                        private Integer id = 12;
                                        private String abc = "abc";
                                    }
                                %>
                            ```

                2.表达式脚本(经常用到)
                    表达式脚本的格式是：<%=表达式%>
                    作用：在jsp页面上输出数据
                    表达式脚本的特点：
                        1.所有的表达式脚本都会被翻译到_jspService()方法中
                        2.表达式脚本都会被翻译成为out.print()输出到页面上
                        3.由于表达式脚本翻译的内容都在_jspService()方法中。所以在_jspService()方法中的对象都可以直接使用
                        4.表达式脚本中的表达式不能以分号结束

                    事例：在jsp页面中操作：输出整型|输出浮点型|输出字符串|输出对象

                        ```java
                            <%=12%>
                            <%=12.12%>
                            <%="我是字符串"%>
                            <%=map%>
                            <%=request.getParameter("username")%>
                        ```

                3.代码脚本
                    代码脚本的格式是：<% java语句 %>
                    代码脚本的作用是：可以在jsp页面中，编写我们自己需要的功能(写的是java语句)
                    代码脚本的特点：
                        1.代码脚本翻译之后都在_jspService方法中
                        2.代码脚本由于翻译到_jspService()方法中，所以在_jspService()方法中的现有对象都可以直接使用
                        3.还可以由多个代码脚本块组合完成一个完整的java语句
                        4.代码脚本还可以和表达式脚本一起组合使用，在jsp页面上输出数据

                    事例：关于代码脚本的联系

                        ```java
                            // 1.代码脚本if...else...语句
                            <%
                                int i = 13;
                                if(i == 13){
                                    System.out.println("今天天气真好");
                                }else{
                                    System.out.println("哈哈哈哈哈哈哈");
                                }
                            %>
                            // 2.代码脚本---for循环语句
                            // 可以由多个代码脚本块，组合成一个完整的代码脚本
                            <%
                                for(int j = 0;j<10;j++){
                            %>
                            // 代码脚本和表达式脚本组合使用
                                <%=j%><br>
                            <%
                                }
                            %>

                            // 3.翻译后java文件中_jspService方法内的代码都可以写
                            <%
                                String username = request.getParameter("username");
                            %>
                            <%="参数名username的值是"+username%>
                        ```

            3.jsp中的三种注释
                1.jsp中的3中注释(了解即可)
                    >html注释
                        <!-- 这是html注释 -->
                        html注释会被翻译到java源代码中。在_jspService方法里，以out.writer输出到客户端
                    >java注释
                        <%
                            //当行java注释
                            /*多行java注释*/
                        %>
                        java注释会被翻译到java源代码中
                    >jsp注释
                        <%-- 这是jsp注释 --%>
                        jsp注释可以注释掉jsp页面中所有代码


        *jsp的九大内置对象
            概念：jsp中的内置对象，是指Tomcat在翻译jsp页面成为Servlet源代码后，内部提供的九大对象。叫内置对象
            九大内置对象分别是：
                    request             请求对象
                    response            响应对象
                    pageContext         jsp的上下文对象
                    session             会话对象
                    application         ServletContext对象
                    config              ServletConfig对象
                    out                 jsp输出流对象
                    page                指向当前jsp的对象
                    exception           异常对象

                
                四大域对象分别是:
                    pageContext         (PageContextimpl类)         当前jsp页面范围内有效
                    request             (HttpServletRequest类)      一次请求内有效
                    session             (HttpSession类)             一个会话范围内有效(打开浏览器访问服务器，直到关闭浏览器)
                    application         (ServletContext类)          整个web工程范围内都有效(只要web工程不停止，数据都在)

                    域对象特点：
                        域对象可以像Map一样存取数据的对象。四个域对象功能一样。不同的是他们对数据的存取范围

                    四个域对象使用的优先顺序
                        四个域对象在使用的时候，优先顺序分别是，他们从小到大的范围的顺序
                            pageContext===>request===>session===>application
                    
                    事例：测试四个域对象的使用范围

                        ```jsp      这是scope页面
                            <h1>这是scope页面</h1>
                            <%
                                //往四个域中都分别保存了数据
                                pageContext.setAttribute("key","pageContext");
                                request.setAttribute("key","request");
                                session.setAttribute("key","session");
                                application.setAttribute("key","application");
                            %>
                            pageContext域是否有值：<%=pageContext.getAttribute("key")%>
                            request域是否有值：<%=request.getAttribute("key")%>
                            session域是否有值：<%=session.getAttribute("key")%>
                            application域是否有值：<%=application.getAttribute("key")%>

                            <%
                                request.getRequestDispatcher("/scope2.jsp").forward(request,response);
                            %>
                        ```

                        ```jsp      scope2.jsp页面
                            <h1>scope2.jsp页面</h1>
                            pageContext域是否有值：<%=pageContext.getAttribute("key")%><br>
                            request域是否有值：<%=request.getAttribute("key")%><br>
                            session域是否有值：<%=session.getAttribute("key")%><br>
                            application域是否有值：<%=application.getAttribute("key")%><br>
                        ```


        *jsp中的out输出和response.getWriter输出的区别
            1.概念：
                response中表示响应，经常用于设置返回给客户端的内容(输出)
                out也是给用户做输出使用的
            2.jsp中的输出操作的执行流程
                当jsp页面中所有代码执行完成后会做一下两个操作：
                    1.执行out.flush()操作，会把out缓冲区中的数据追加写入到response缓冲区末尾
                    2.会执行response的刷新操作。把全部数据写给客户端
                总结：由于jsp翻译之后，底层源码都是使用out来进行输出的，所以一般情况下。我们在jsp页面中统一使用out来进行输出。避免打乱页面输出内容的顺序

            3.out.write()和out.print()的对比
                out.write() 输出字符串没有问题
                out.print() 输出任意数据都没有问题(内部实现是转换成字符串后调用的write输出)
                    深入源码，浅出结论：在jsp页面中，可以统一使用out.print()来进行输出

        
        *jsp常用标签
            1.静态包含(常用)
                事例：静态包含的事例

                    ```jsp
                            头部信息<br>
                            主体内容<br>
                            <%@ include file="/include/footer.jsp" %>
                        <%--    
                            <%@ 
                            include file="/include/footer.jsp" 
                            file 属性指定你要包含的jsp页面的路径
                                地址中第一个斜杠 /  表示为http://ip:port/工程路径/     映射到代码的web目录
                                
                                静态包含的特点：
                                    1.静态包含不会翻译被包含的jsp页面
                                    2.静态包含其实是把被包含的jsp页面的代码拷贝到包含的位置执行输出
                            %>
                        --%>
                    ```

            2.动态包含(不常用)
                事例：动态包含的事例

                    ```jsp  main.jsp页面的内容
                        <%--
                            <jsp:include page="/include/footer.jsp"></jsp:include>  这是动态包含
                            page属性指定你要包含的jsp页面的路径
                            动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置

                            动态包含的特点：
                                    1.动态包含会把包含的jsp页面也翻译成为java代码
                                    2。动态包含底层代码使用如下代码去调用被包含的jsp页面执行输出
                                        JspRuntimeLibrary.include(request, response, "/include/footer.jsp",out,false)
                                    3.动态包含，还可以传递参数
                        --%>
                        <jsp:include page="/include/footer.jsp">
                            <jsp:param name="username" value="root"></jsp:param>
                            <jsp:param name="password" value="bbj"></jsp:param>
                        </jsp:include>
                    ```

                    ```jsp  footer.jsp页面的内容
                        脚页信息<br>
                        获取的username的值=<%=request.getParameter("username")%> 
                    ```
                
            3.请求转发
                事例：请求转发

                    ```jsp
                        <jsp:forward page="/scope2.jsp"></jsp:forward>
                        <%--  
                            <jsp:forward page="/scope2.jsp"></jsp:forward>是请求转发标签，它的功能就是请求转发
                            page属性设置请求转发的路径
                            功能和request.getRequestDispatcher("/scope2.jsp").forward(request,response)一样
                        --%>
                    ```
            
        *请求转发的使用
            事例：将数据从java代码中转发到jsp页面中

            ```java     SearchServlet.java页面
                public class SearchServlet extends HttpServlet {
                    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                        //获取请求的参数
                        //发sql语句查询学生信息
                            //使用for循环生成查询到的数据做模拟
                        List<Student> student = new ArrayList<Student>();
                        for(int i=0;i<10;i++){
                            int t = i + 1;
                            student.add(new Student(t,"name"+t,18+t,"phone"+t));
                        }
                        //保存数据到request域中
                        request.setAttribute("student",student);
                        //请求转发到showStudent.jsp页面
                        request.getRequestDispatcher("/test/showStudent.jsp").forward(request,response);
                    }
                }
            ```

            ```jsp  showStudent.jsp页面
                <%
                    List<Student> studentList = (List<Student>)request.getAttribute("student");
                %>
                <table>
                    <tr>
                        <td>编号</td>
                        <td>姓名</td>
                        <td>年龄</td>
                        <td>电话</td>
                        <td>操作</td>
                    </tr>
                    <% for(Student student:studentList){ %>
                        <tr>
                            <td><%=student.getId()%></td>
                            <td><%=student.getName()%></td>
                            <td><%=student.getAge()%></td>
                            <td><%=student.getPhone()%></td>
                            <td>删除，修改</td>
                        </tr>
                    <% }%>
                </table>
            ```

        *Listener监听器
            概念：
                Listener监听器它是javaweb的三大组件之一。javaweb的三大组件分别是：Servlet程序，Filter过滤器，Listener监听器
                Listener它是javeEE的规范，就是接口
            作用：监听某种事物的变化。然后通过回调函数，反馈给客户(程序)去做一些相应的处理

            ServletContextListener监听器
                ServletContextListener它可以监听ServletContext对象的创建和销毁
                ServletContext对象在web工程启动的时候创建，在web工程停止的时候销毁
                监听到创建和销毁之后都会分别调用ServletContextListener监听器的方法反馈
                    两个方法分别是:
                    public interface ServletContextListener extends EventListener{
                        /*
                            在ServletContext对象创建之后马上调用，做初始化
                        */
                        public void contextInitialized(ServletContextEvent sec)

                        /*
                            在ServletContext对象销毁之后调用
                        */
                        public void contextDestroyed(ServletContextEvent sec)
                    }
                使用：
                    创建类实现ServletContextListener监听器，要在xml里面配置一下

                    ```xml
                        <listener>
                            <listener-class>监听器类的位置</listener-class>
                        </listener>
                    ```

时间2020/12/02

    
    **EL表达式
        概念：EL表达式的全称是：Expression Language。是表达式语言
        作用：EL表达式主要是代替jsp页面中的表达式脚本在jsp页面中进行数据的输出。因为EL表达式在输出数据的时候，要比jsp的表达式脚本简洁很多
        事例：EL表达式和jsp表达式的对比

            ```jsp
                <%
                    request.setAttribute("key","值");
                %>
                表达式脚本输出key的值：<%=request.getAttribute("key")%><br>
                EL表达式输出key的值是：${key}
            ```

        1.EL表达式搜索域数据的顺序
            EL表达式的作用：EL表达式主要是在jsp页面中输出数据；主要是输出域对象中的数据
            EL表达式在域中的作用范围：当四个域中都有相同的key的数据的时候，EL表达式会按照四个域从大到小的顺序进行搜索，找到就输出
            事例：EL表达式在四个不同的域中的作用

                ```jsp
                    <%
                        pageContext.setAttribute("key","pageContext");
                        request.setAttribute("key","request");
                        session.setAttribute("key","session");
                        application.setAttribute("key","application");
                    %>
                    ${key}
                ```

            事例：EL表达式输出javaBean的普通属性，数组属性。List集合属性，map集合属性

                ```jsp      输出Person类中普通属性，数组属性。list集合属性和map集合属性
                    <%
                        pageContext.setAttribute("key",person); //key对应的是一个person对象，存入域对象
                    %>
                    输出Person：${p}
                    输出Person的name属性：${p.name}
                    输出Person的phones属性值：${p.phones}
                    输出Person的phones单个属性的值：${p.phones[0]}
                    输出Person的cities集合中的元素值：${p.cities}
                    输出Person的List集合中个别元素值：${p.List[2]}
                    输出Person的Map集合：${p.Map}
                    输出Person的Map集合中某个key的值：${p.Map.key3}
                    输出Person的age属性：${p.age}
                ```
                    EL表达式输出本质：当使用EL表达式输出属性值的时候，不是直接找对应的该属性，而是找该属性对应的get方法
                        如：${p.age}:寻找对应的age属性的getAge方法，而不是直接找person对应的age属性

        2.EL表达式的运算
            1.三种运算
                >关系运算符
                    ==  !=  < > <= >=
                    如：${5==5};返回值是布尔类型的值
                >逻辑运算符
                    &&  ||  !
                    如：${12==12 && 12>11};返回值是布尔类型的值
                >算数运算
                    +   -   *   /   %
                    返回的是，运算之后的结果

            2.empty运算
                empty运算可以判断一个数据是否为空，如果为空，则输出true，不为空输出false
                    以下几种情况为空：
                        >值为null值的时候，为空
                        >值为空串的时候，为空
                        >值是Object类型数组，长度为零的时候
                        >list集合，元素个数为零
                        >map集合，元素个数为零

                事例：empty运算的使用

                    ```jsp
                        ${empty emptyNull}  //判断emptyNull是否为空；是空返回true，不为空返回false
                    ```
            
            3.三元运算
                格式：表达式1?表达式2:表达式3
                    如果表达式1的值为true，返回表达式2的值，如果表达式1的值为假，返回表达式3的值
                事例：在EL表达式中三元运算的使用

                    ```jsp
                        ${12 == 12?"今天天气真好":"今天完美"}
                    ```

            4.点运算和[]运算
                类似于js里面的访问对象属性.或者[]方式

            5.EL表达式的11个隐含对象
                概念：EL表达式中11个隐含对象，是EL表达式中自己定义的，可以直接使用

                变量                    类型                        作用
                pageContext         PageContextImpl             它可以获取jsp中的九大内置对象

                PageScope           Map<String,Object>          它可以获取pageContext域中的数据
                requestScope        Map<String,Object>          它可以获取Request域中的数据
                sessionScope        Map<String,Object>          它可以获取session域中的数据    
                applicationScope    Map<String,Object>          它可以获取ServletContext域中的数据    

                param               Map<String,String>          它可以获取请求参数的值
                paramValues         Map<String,String[]>        它可以获取请求参数的值，获取多个值的时候使用

                header              Map<String,String>          它可以获取请求头信息
                headerValues        Map<String,String[]>        它可以获取请求头信息,获取多个值的时候使用

                cookie              Map<String,Cookie>          它可以获取当前请求的Cookie信息

                initParam           Map<String,String>          它可以获取在web.xml中配置的<context-param>上下文参数


                1.EL获取四个特定域中的属性
                    PageScope           ==========          pageContext域
                    requestScope        ==========          Request域
                    sessionScope        ==========          Session域
                    applicationScope    ==========          ServletContext域

                    事例：选择性的从四个域中输出相同的key值

                        ```jsp
                            <%
                                pageContext.setAttribute("key","pageContext");
                                request.setAttribute("key","request");
                                session.setAttribute("key","session");
                                application.setAttribute("key","application");
                            %>
                            ${pageScope.key}        //指定输出pageContext的key值
                            ${requestScope.key}     //指定输出Request的key值
                            ${sessionScope.key}     //指定输出Session的key值
                        ```

                2.EL表达式pageContext的使用

                    ```jsp
                        <%--
                            request.getScheme()     它可以获取请求的协议
                            request.getServerName() 获取请求的服务器ip或域名
                            request.getServerPort() 获取请求的服务器端口号
                            request.getContextPath()获取当前工程路径
                            request.getMethod()     获取请求的方式
                            request.getRemoteHost   获取客户端的ip地址
                            session.getId()         获取会话的唯一标识
                        --%>
                        上面是request对象的使用，对应下面EL表达式通过pageContext获取request的使用
                        ----------------------------------------------------------------
                                                                                                输出结果
                            1.协议                ${pageContext.request.scheme}                 http
                            2.服务器ip            ${pageContext.request.serverName}             localhost
                            3.服务器端口          ${pageContext.request.serverPort}              8088
                            4.获取工程路径        ${pageContext.request.contextPath}            /09_ELbiao
                            5.获取请求方式        ${pageContext.request.method}                 GET
                            6.获取客户端ip地址       ${pageContext.request.remoteHost}          127.0.0.1
                            7.获取会话的id编号         ${pageContext.session.id}                3B4BF8CE006BEC9AAD631C407A8C1197
                    ```

                3.其他EL表达式隐含对象的实例
                    实例：其他EL表达式隐含对象

                    ```JSP
                        param               Map<String,String>          它可以获取请求参数的值
                        paramValues         Map<String,String[]>        它可以获取请求参数的值，获取多个值的时候使用

                        输出请求参数username的值：${param.username}
                        --------------------------------------------------------------------------------------

                        header              Map<String,String>          它可以获取请求头信息
                        headerValues        Map<String,String[]>        它可以获取请求头信息,获取多个值的时候使用

                        输出请求头的User-Agent的值：${header['User-Agent']}
                        --------------------------------------------------------------------------------------

                        cookie              Map<String,Cookie>          它可以获取当前请求的Cookie信息

                        获取cookie的名称        ${cookie.JSESSIONID.name}
                        获取cookie的值          ${cookie.JSESSIONID.value}
                        --------------------------------------------------------------------------------------

                        initParam           Map<String,String>          它可以获取在web.xml中配置的<context-param>上下文参数

                        获取配置文件xml的上下文参数    ${initParam.username}
                    ```

    
    **JSTL
        概念：JSTL标签库 全称是指JSP Standard Tag Library JSP标准标签库。是一个不断完善的开放源代码的JSP标签库
        作用：标签库是为了替换jsp中的代码脚本。这样使得整个jsp页面变得更加简洁
        使用：
            使用步骤：
                    1.先导入JSTL标签库的jar包
                        taglibs-standard-impl-1.2.1.jar
                        taglibs-standard-spec-1.2.1.jar
                    2.使用taglib指令引入标签库

        *核心库使用
            1.<c:set />
                作用：set标签可以往域中保存数据
                事例：set标签的使用
                        <%--
                            <c:set />
                                作用：set标签可以往域中保存数据
                                    JSTL的set标签  和 域对象.setAttribute(key,value) 一样
                                scope属性设置保存到哪个域
                                    page表示PageContext域(默认值)
                                    request表示Request域
                                    session表示Session域
                                    application表示ServletContext域
                                var属性设置key
                                value属性设置值
                        --%>

                    ```jsp
                        <c:set scope="request" var="key" value="天气真好" />
                        输出JSTL保存的值：${requestScope.key}

                        
                    ```

            2.<c:if />
                作用：if标签用来做if判断
                事例：if标签的使用
                        if标签用来做if判断
                        test属性表示判断的条件(使用EL表达式输出)

                    ```jsp
                        <c:if test="${12 == 12}">
                            <h1>今天天气真好</h1>     //if语句成立，则显示中间的内容
                        </c:if>
                        
                    ```

            3.<c:choose><c:when><c:otherwise>标签
                作用：多路判断。跟switch...case...default非常接近
                事例：<c:choose><c:when><c:otherwise>标签的使用
                        <%--
                            choose标签开始选择判断
                            when标签表示每一种判断情况
                                test属性表示当前这种判断情况
                            otherwise标签表示剩下的情况

                            <c:choose><c:when><c:otherwise>标签使用时注意点
                                1.标签里不能使用html注释，要使用jsp注释
                                2。when标签的父标签一定要是choose标签
                        --%>

                    ```jsp
                        <%
                            request.setAttribute("height",179);
                        %>
                        <c:choose>
                            <c:when test="${requestScope.height > 190}">
                                <h2>小巨人</h2>
                            </c:when>
                            <c:when test="${requestScope.height > 178}">
                                <h2>还可以</h2>
                            </c:when>
                            <c:otherwise>
                                <h2>接下来的身高就比较矮了</h2>
                            </c:otherwise>
                        </c:choose>
                        
                    ```


            4.<c:forEach />
                作用：遍历输出
                    事例：遍历1-10
                            <%--
                                遍历1到10，输出
                                    begin属性设置开始的索引
                                    end属性设置结束的索引
                                    step属性表示遍历的步长值
                                    varStatus属性表示当前遍历到的数据的状态
                                    var属性表示循环的变量(也是当前正在遍历到的数据)
                                    就像forEach标签写成for(int i=1;i<10;i++)
                            --%>

                        ```jsp
                            <c:forEach begin="1" end="10" var="i">
                                <h1>${i}</h1>
                            </c:forEach>
                            
                        ```

                    事例：遍历Object数组
                            <%--
                                遍历Object数组
                                    就像for(Object item:arr)
                                    标签属性：
                                        items表示遍历的数据源(遍历的集合)
                                        var表示当前遍历到的数据
                            --%>

                        ```jsp
                            <%
                                request.setAttribute("arr",new String[]{"1111111","2222222","666666666"});
                            %>
                            <c:forEach items="${requestScope.arr}" var="item">
                                ${item} <br>
                            </c:forEach>
                        ```

                    事例：遍历Map集合(用法和遍历Object数组一样)

                    事例：遍历List集合(用法和遍历Object数组一样)

                    说明：varStatus属性的介绍

                        ```java
                            public interface LoopTagStatus{
                                public Object getCurrent();     //表示获取当前遍历到的数据
                                public int getIndex();          //表示获取遍历的索引
                                public int getCount();          //表示遍历的个数
                                public boolean isFirst();       //表示当前遍历的数据是否是第一条
                                public boolean isLast();        //表示当前遍历的数据是否是最后一条
                                public Integer getBegin();      //获取begin，end，step属性值
                                public Integer getEnd();
                                public Integer getStep();
                            }
                        ```
                        ```jsp  varStatus属性使用
                            <%
                                request.setAttribute("arr",new String[]{"1111111","2222222","666666666"});
                            %>
                            <c:forEach items="${requestScope.arr}" var="item" varStatus="status">
                                ${status.last}
                            </c:forEach>
                        ```

###文件的上传和下载
    **文件的上传
        *编写向服务器上传文件的jsp代码，且编写服务器接收的步骤
            1.要有一个form标签，method="post"请求
            2.form标签的encType属性值必须为multipart/form-data值
            3.在from标签中使用input type="file"添加上传的文件
            4.编写服务器代码(Servlet程序)接收，处理上传的数据

            encType=mutipart/form-data表示提交的数据，以多段(每一个表单项一个数据段)的形式进行拼接，然后以二进制流的形式发送给服务器

            事例：通过form表单向服务器上传图片和用户名字段

                ```jsp  前端上传用户信息的页面
                    <form action="http://localhost:8088/09_ELbiao/uploadServlet" method="post" enctype="multipart/form-data">
                        用户名：<input type="text" name="username"><br>
                        头像：<input type="file" name="photo"><br>
                        <input type="submit" value="上传">
                    </form>
                ```
                ```java 服务器通过流的形式接收前端发过来的数据
                    public class UploadServlet extends HttpServlet {
                        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                            ServletInputStream inputStream = request.getInputStream();
                            byte[] buffer = new byte[1024];
                            int read = inputStream.read(buffer);
                            System.out.println(new String(buffer,0,read));
                        }
                    }
                ```

        
        *commons-fileupload.jar常用API介绍说明
            commons-fileupload.jar需要依赖commons-io.jar这个包，所以两个包我们都需要引入
            使用：
                第一步：就是需要导入两个jar包
                    commons-fileupload.jar
                    commons-io.jar
                
            commons-fileupload.jar和commons-io.jar包中类的说明
                ServletFileUpload类，用于解析上传的数据
                    boolean SerletFileUpload.isMultipartContent(HttpServletRequest request)
                        判断当前上传的数据格式是否是多段的格式
                    
                FileItem类，表示每一个表单项
                    public List<FileItem> parseRequest(HttpServletRequest request)
                        解析上传的数据
                    boolean FileItem.isFormField()
                        判断当前这个表单项，是否是普通的表单项。还是上传的文件类型
                        true表示普通类型的表单项
                        false表示的是上传文件的类型
                    String FileItem.getFieldName()
                        获取表单项的name属性值
                    String FileItem.getString()
                        获取当前表单项的值
                    String FileItem.getName()
                        获取上传的文件名
                    void FileItem.write(file)
                        将上传的文件写到参数file所指向的磁盘位置


            事例：服务器处理客户端上传文件的步骤

                ```java
                    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                            //1.先判断上传的数据是否是多段数据(只有多段的数据，才是文件上传的)
                            if(ServletFileUpload.isMultipartContent(request)){
                                //创建FileItemFactory工厂实现类
                                FileItemFactory fileItemFactory = new DiskFileItemFactory();
                                //创建用于解析上传数据的工具类ServletFileUpLoad类
                                ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
                                try {
                                    //解析上传的数据，得到每一个表单项FileItem
                                    List<FileItem> list = servletFileUpload.parseRequest(request);
                                    //循环判断，每一个表单项，是普通类型，还是上传的文件
                                    for(FileItem fileItem:list){
                                        if(fileItem.isFormField()){
                                            //普通表单项
                                            System.out.println("表单项的name属性值"+fileItem.getFieldName());
                                            //参数UTF-8,解决乱码问题
                                            System.out.println("表单项的value属性值"+fileItem.getString("UTF-8"));
                                        }else{
                                            //上传的文件
                                            System.out.println("表单项的name属性值"+fileItem.getFieldName());
                                            System.out.println("上传的文件名"+fileItem.getName());
                                            fileItem.write(new File("C:\\study\\" + fileItem.getName()));
                                        }
                                    }
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                ```

时间2020/12/03

    **文件下载
        事例：文件下载的事例

            ```java
                protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                    //1.获取要下载的文件名
                    String downLoadFileName = "penguins.jpg";
                    //2。读取要下载的文件的内容(通过ServletContext对象可以读取)
                    ServletContext servletContext = getServletContext();
                    //获取要下载的文件类型
                    String mimeType = servletContext.getMimeType("/file/" + downLoadFileName);
                    System.out.println("文件的类型"+mimeType);
                    //4.在回传前，通过响应头告诉客户端返回的数据类型
                    response.setContentType(mimeType);
                    //5.还要告诉客户端收到的数据是用于下载使用(还是使用响应头)
                    //Content-Disposition响应头，表示收到的数据怎么处理
                    //attachment表示附件，表示要下载
                    //filename=表示指定下载的文件名
                    if(request.getHeader("User-Agent").contains("Firefox")){
                        //如果是火狐浏览器使用Base64编码
                        response.setHeader("Content-Disposition","attachment;filename==?UTF-8?B?"+new BASE64Encoder().encode("好天气.jpg".getBytes("UTF-8"))+"?=");
                    }else{
                        //url编码是把汉子转换为%xx%xx的格式，转化为16进制，因为浏览器本身是不识别汉子的，所以需要使用url编码将汉子转为字符码
                        response.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode("好天气.jpg","UTF-8"));
                    }
                    //通过流读取服务器端的数据
                    InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downLoadFileName);
                    System.out.println("resourceAsStream = " + resourceAsStream);
                    //获取响应的输出流
                    OutputStream outputStream = response.getOutputStream();
                    System.out.println("outputStream = " + outputStream);
                    //3.把下载的文件内容回传给客户端
                    //读取输入流中全部的数据，复制给输出流，输出给客户端
                    IOUtils.copy(resourceAsStream,outputStream);
                }
            ```


###Cookie和Session
    **Cookie
        概念：
            Cookie是服务器通知客户端保存键值对的一种技术
            客户端有了Cookie后，每次请求都发送给服务器
            每个Cookie的大小不能超过4kb

        1.如何创建Cookie
            事例：在服务器端创建cookie并发给客户端

                ```java
                    public void createCookie(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException{
                        //解决post请求中的中文乱码问题
                        //一定要在获取请求参数之前调用才有效
                        req.setCharacterEncoding("UTF-8");
                        //解决响应中文乱码问题
                        res.setContentType("text/html;charset=UTF-8");
                        //1.创建cookie对象
                        Cookie cookie = new Cookie("key1","value1");
                        //2.通知客户端保存cookie
                        res.addCookie(cookie);
                        res.getWriter().write("Cookie创建成功");

                    }
                ```

        2.服务器如何获取cookie
                服务器获取客户端的cookie只需要一行代码:req.getCookie(),即可获得cookie[]的数组
                事例：服务器获取客户端的cookie的操作

                    ```java
                        public void getCookie(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException {
                            Cookie[] cookies = req.getCookies();
                            for (Cookie cookie:cookies){
                                //getName方法返回Cookie的key
                                //getValue方法返回Cookie的value值
                                res.getWriter().write("Cookiename"+cookie.getName()+"cookievalue"+cookie.getValue()+"<br/>");
                            }
                        }
                    ```

        3.Cookie值的修改
            方案一：
                1.先创建一个要修改的同名的Cookie对象
                2。在构造器，同时赋予新的Cookie值
                3.调用response.addCookie(Cookie)

                    事例：修改Cookie的值

                    ```java
                        public void updataCookie(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException{
                        //        1.先创建一个要修改的同名的Cookie对象
                        //        2。在构造器，同时赋予新的Cookie值
                                //浏览器接受到cookie时的执行机制，同名就修改，不同名则创建
                                Cookie cookie = new Cookie("key1", "xindevalue");
                        //        3.调用response.addCookie(Cookie)
                                res.addCookie(cookie);
                                res.getWriter().write("key1的cookie值已经修改好了");

                            }
                    ```

            方案二：
                1.先查找到需要修改的Cookie对象
                    Cookie cookie = CookieUtils.findCookie("key2",req.getCookies())
                2。调用setValue()方法赋予新的Cookie值
                    if(cookie != null){
                        cookie.setValue("newValue2")
                    }
                3.调用response.addCookie()通知客户端保存修改
                    res.addCookie(cookie)
            

        4.Cookie生命控制
            概念：Cookie的生命控制指的是如何管理Cookie什么时候被销毁(删除)
            使用到的方法：
                setMaxAge()
                    整数：表示在指定的秒数后过期
                    负数：表示浏览器一关，Cookie就会被删除(默认值-1)
                    零：表示马上删除
                
                事例：设置Cookie存活时间：指定时间删除，浏览器一关就删除，马上删除

                ```java
                    public void defaultCookie(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException{
                        Cookie cookie = new Cookie("keyshi", "hhhhh");
                        cookie.setMaxAge(0);
                        res.addCookie(cookie);
                    }
                ```

        5.Cookie有效路径Path的设置
            具体内容：
                Cookie的path属性可以有效的过滤哪些Cookie可以发送给服务器。哪些不发
                path属性是通过请求的地址来进行有效的过滤

                CookieA     path=/工程路径
                CookieB     path=/工程路径/abc

                请求地址如下：
                    http://ip:port/工程路径/a.html
                        CookieA 发送
                        CookieB 不发送
                    http://ip:port/工程路径/abc/a.html
                        CookieA 发送
                        CookieB 发送

            事例：创建带有path路径cookie，客户端通过请求地址访问

                ```java
                    public void testPath(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException{
                        Cookie cookie = new Cookie("path", "path1");
                        //getContextPath()===>得到工程路径
                        cookie.setPath(req.getContextPath()+"/abc");//通过请求地址：http://ip:port/工程路径/abc,才能访问到此cookie
                        res.addCookie(cookie);
                    }
                ```

    
    **Session
        概念：
            1.Session就是一个接口
            2.session就是会话。它是用来维护一个客户端和服务器之间关联的一种技术
            3.每个客户端都有自己的一个Session会话
            4.Session会话中，我们经常用来保存用户名登录之后的信息

        创建Session和获取Session
            创建和获取Session。他们的API是一样的
            request.getSession()
                第一次调用时：创建Session会话
                之后调用都是：获取前面创建好的Session会话对象
            isNew():判断到底是不是刚创建出来的
                true：表示刚创建
                false：表示获取之前创建

            每个会话都有一个身份证号。也就是ID值。而且这个ID是唯一的
                getID()得到Session的会话的id值

            事例：创建获取session，获取session唯一标识

            ```java
                public void createorgetsession(HttpServletRequest req,HttpServletResponse res) throws ServletException,IOException{
                        //创建和获取session会话对象
                        HttpSession session = req.getSession();
                        //判断当前session会话，是否是新创建出来的
                        boolean isNew = session.isNew();
                        //获取session会话的唯一标识id
                        String id = session.getId();
                    }
            ```

        Session域中的数据的存取
            方法：
                存：req.getSession().setAttribute("key1","value1")
                取：req.getSession().getAttribute("key1")

        
        1.Session生命周期控制
            生命周期控制的方法：
                public void setMaxInactiveInterval(int interval)
                    功能：设置Session的超时时间(以秒为单位)，超过指定的时长，Session就会被销毁
                    值为整数的时候，设定Session的超时时长
                    值为负数表示永不超时
                public int getMaxInactiveInterval()
                    功能：获取Session的超时时长
                public void invalidate()    
                    功能：让当前Session会话马上超时

            Session默认的超时时长是多少？
                session的超时指的是：客户端两次请求的最大间隔时长
                Session默认的超时时间长为30分钟；因为在Tomcat服务器的配置文件web.xml中默认有以下的配置，它表示配置了当前Tomcat服务器下所有的Session超时配置默认时长为：30分钟
                    <session-config>
                        <session-timeout>30</session-timeout>
                    </session-config>

                若你希望你的web工程，默认的session的超时时长为其他时长。可以在自己的web.xml配置文件中做以上配置。就可以修改你的web工程所有Session的默认超时时长

                若你想修改个别Session的超时时长。就可以使用上面的API。setMaxInactiveInterval(int interval)来进行单独的设置。
                    session.setMaxInactiveInterval(int interval)单独设置超时时长


        2.客户端和Session之间技术关联
            本质：Session技术，底层其实是基于Cookie技术来实现的(Cookie每次请求都会携带JSESSIONID,其实是session的id)

            
###Filter过滤器
    **Filter过滤器
        概念：
            Filter过滤器它是Javaweb的三大组件之一。三大组件分别是：Servlet程序，Listener监听器，Filter过滤器
            它是javeEE的规范。也就是接口
        作用：拦截请求，过滤响应
            拦截请求的常见的应用场景：
                1.权限检查
                2。日记操作
                3.事务管理。。。等等
         