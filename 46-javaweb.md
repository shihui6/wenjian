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

2020/11/30
###JSP
    *jsp
        概念：jsp的全称是java server pages；java的服务器页面
        作用：jsp主要作用是代替servlet程序回传html页面的数据
                因为servlet程序回传html页面数据是一件非常繁琐的事情。开发成本和维护成本都极高

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
        
