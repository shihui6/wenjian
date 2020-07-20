###nginx
    **nginx
        概念：是一个高性能的http和反向代理服务器，特点是占有内存少，并发能力强，事实上nginx的并发能力确实是同类型的网页服务器中表现较好的；
        特点：
            nginx专为性能优化而开发，性能是其重要的考量，实现上非常注重效率，能经受高负载的考研，有报告表明能支持高达50000个并发连接数
            nginx支持热部署，它的启动特别容易，并且几乎可以做到7*24小时不间断运行，即使运行数个月也不需要重新启动，你还能够在不间断服务的情况下，对软件版本进行升级

        1：作用：
            1-1：反向代理
                概念：我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实的IP地址

            1-2：正向代理
                概念：在客户端(浏览器)配置代理服务器，通过代理服务器进行互联网进行访问。这种代理服务称为正向代理

                    概念解释：如果把局域网外的internet想象成一个巨大的资源库，则局域网中的客户端要访问internet，则需要在浏览器配置代理服务器，通过代理服务器去访问网址，然后返回内容。这种代理服务就称为正向代理

            1-3：负载均衡
                概念：单个服务器解决不了，我们增加服务器的数量，然后通过反向代理服务器将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡

            1-4：动静分离
                概念：为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析(部署到不同的服务器上)，客户端请求资源的时候，先通过ngnix代理服务器，然后代理服务器通过请求判断是静态资源还是动态资源向不同的服务器请求。加快解析速度。降低原来单个服务器的压力

        
        2：nginx在linux系统安装
            

                
###虚拟软件
    概念：虚拟原件是一个可以使你在一台机器上同时运行二个或更多Windows,LIUNX等系统。它可以模拟一个标准PC环境，这个环境和真实的计算机一样，都有芯片组，CPU，内存，显卡，声卡，网咖，软驱，硬盘，光驱，串口，并口，USB控制器等  

    **常用的虚拟原件
            VMware workstation
            VirtualBox

    **Linux安装

    **Linux目录结构
        /：代表根目录
        打开computer->Filesystem->很多常用的子目录和子文件夹
        
        root用户登录的：代表的是管理员
            例如：[root@user ~]:指root用户登录的，user指的是登录的主机名；~:指当前所处的目录，根据不同的用户用户登录代表的含义也不一样，可通过pwd查看当前的目录
        普通用户目录，操作是在home目录里

    **Linux的常见命令
        1：命令行打开步骤：在Linux图形化界面中，右键打开命令行

        2：查看主机的ip地址：ifconfig 查看ip地址(此处的ip作用：通过工具连接到Linux系统中)

        3：切换目录命令
                cd app:切换到app目录
                cd ..：切换到上一层目录
                cd /：切换到系统根目录
                cd ~：切换到用户主目录
                cd -:切换到上一次所在目录

        4：pwd：代表当前目录

        5：touch：创建文件

        6：clear：清屏
        7：ctrl+l：清屏

        8：列出文件列表：ls 或 ll
                概念：ls是一个非常有用的命令，用来显示当前目录下的内容。配合参数使用，以不同的方式显示目录内容
                用法：ls：显示当前目录下的内容，蓝色的表示文件夹，白色的是文件
                    ls -a:显示当前目录下的所有文件，包括隐藏文件
                    ls -l：显示当前目录下的内容,简写(ll)把非隐藏的文件或者文件夹的详细信息显示出来

        9：目录创建和删除
                mkdir：创建目录
                rmdir:删除目录(只能删除空的目录)
                mkdir --help:查看mkdir扩展功能对应的描述
        
        10：浏览文件
            10-1：cat：用于显示文件的内容
                    用法：cat(参数)<文件名>

            10-2：more：显示的内容不会超过一个屏幕的情况，
                    按空格键会显示下一页的数据
                    回车显示下一行的内容
                    按q键退出查看
                    用法：more <文件名>

            10-3：less：跟more差不多，就是多了用鼠标的滚轮查看的效果
                    用法 less <文件名>
            
            10-4：tail：命令是在实际使用过程中使用非常多的一个命令
                    功能是：用于显示文件后几行的内容用法
                    tail -10 /etc/passwd ：查看后10行的数据
                    tail -f catalina.log : 动态查看日志(日志发生变化，会动态展示出来)
                    ctrl+c：结束查看

        11：文件操作
            11-1：cp：copy命令可以将文件从一处复制到另一处
                    用法：cp命令将文件赋值成另外一个文件或复制到某目录时，需要指定源文件名与目标文件名或目录
                    事例：
                        cp a.txt b.txt (将a.txt复制为b.txt)
                        cp a.txt ../    (将a.txt文件复制到上一层目录中)
            
            11-2：mv:移动或者重命名 
                    概念：相当于windows里面的剪切
                    事例：
                        mv a.txt ../ (将a.txt文件移动到上一层目录中)
                        mv a.txt b.txt (将a.txt文件重命名为b.txt)

            11-3：rm：删除文件
                    rm a.txt 删除 a.txt文件，删除需要用户确认(删除文件夹需要加参数：rm -r 文件夹)
                    rm -r a 删除文件夹，需要用户确认
                    rm -f a.txt (不询问，直接删除)
                    rm -rf a 不询问递归删除
                    rm -rf *    删除所有文件
                    rm -rf /*   自杀

            11-4：tar:打包或者压缩
                    概念：tar命令位于/bin目录下，它能够将用户所指定的文件或目录打包成一个文件，但不做压缩，但是通过参数可以增加其功能
                    参数：
                        -c：创建一个新的tar文件
                        -v：显示运行过程中的信息
                        -f：指定文件名
                        -z：调用gzip压缩命令进行压缩·
                        -t：查看压缩文件的内容
                        -x：解压tar文件
                    事例：
                        打包：tar -cvf xxx.tar bbb   (bbb文件打包成xxx.tar)
                        打包并且压缩：tar -zcvf xxx.tar.gz bbb (将bbb文件打包压缩成xxx.tar.gz)
                        解压：
                            tar -xvf xxx.tar   (解压的是没有压缩的文件)
                            tar -zxvf xxx.tar.gz -C /usr/aaa    (解压xxx.tar.gz到/usr/aaa文件夹下；-C指定解压到的目录的路径)

                
            11-5：find：用于查找符合条件的文件
                    事例：
                        find / -name 'ins' (查找文件名称是以ins开头的文件)
                        find / -name 'ins'

            11-6：grep：用于查找文件里符合添加的字符串
                    事例：
                        grep lang anaconda.cfg (在文件anaconda.cfg中查找lang的字符)
                        grep lang anaconda.cfg --color (在文件anaconda.cfg中查找lang的字符且高亮显示lang字符)
                        grep lang anaconda.cfg --color -A1(在文件anaconda.cfg中查找lang的字符且高亮显示lang字符，且向后多显示一行)
                        grep lang anaconda.cfg --color -A1 -B1(在文件anaconda.cfg中查找lang的字符且高亮显示lang字符，且向前和向后各显示一行)

        4：重定向输出>和>>
                >:重定向输出，覆盖原有内容
                >>:重定向输出，有追加功能
                    事例：
                        b.txt > a.txt(将b文件里的内容添加到a文件里面，会覆盖原有的内容)
                        b.txt >> a.txt(将b文件里的内容追加到a文件里面)

        5：系统管理命令
                ps：正在运行的某个进程的状态
                ps -ef 查看所有进程
                ps -ef | grep ssh 查找某一进程
                kill 2868 杀掉2868编号的进程
                kill -9 2868 强制杀死进程

        6：管道 |
                概念：管道是Linux命令中重要的一个概念，其作用是将一个命令的输出用作另一个命令的输入
                用法：
                    事例：
                    ls --help | more  (解释：将ls --help的输出当做more的输入)
                    ps -ef | grep java (解释：查询名称中包含java的进程)
                    ifconfig | more
                    cat index.html | more

    **Vi和Vim编辑器
        1：Vim编辑器(在Linux中编辑文件的工具)
            概念：在Linux下一般使用Vi编辑器来编辑文件(就是改写文件的内容)。Vi既可以查看文件也可以编辑文件。相当于windows下的编辑修改文件
            三种模式：
                命令行模式(切换到命令行模式按esc)
                插入模式，(切换到插入模式按i,o,a键)
                    i：在当前光标位置前插入
                    I：在当前光标行首插入
                    a：在当前光标位置后插入
                    A：在当前光标行尾插入
                    o：在当前光标行之后插入一行
                    O：在当前光标行之前插入一行
                底行模式(切换到底行模式按：(冒号)，)
            Vim编辑器执行机制：
                Vim file(打开文件)
                esc->:q (退出)
                修改文件：输入i进入插入模式才能修改
                保存并退出：esc->:wq 或者不保存强制退出 esc->:q!

            快捷键：
                dd (快速删除一行)
                yy (复制当前行)
                nyy (从当前行向后复制几行)
                p (粘贴)
                R (替换)

            事例：
                Vim <文件名> ：进入文件中，且也可以查看文件的内容并且做出修改

    
    **Linux权限
        1：概念：通过ll命令可查看当前目录下的所有的文件或文件夹的权限，权限分为4租部分代表不同的内容 _ ___ ___ ___ 
            概念详解：
                权限一：文件类型
                            _:表示文件
                            d:表示文件夹
                            l:表示连接
                权限二：当前用户具有该文件的权限
                            r:read读            (r代表的数字是4，意思是在修改权限时刻通过数字4代替r)
                            w:write写           (w代表的数字是2，意思是在修改权限时刻通过数字2代替w)
                            x:excute执行        (x代表的数字是1，意思是在修改权限时刻通过数字1代替x)
                权限三：当前组内其他用户具有该文件的权限
                            r:read读
                            w:write写
                            x:excute执行
                权限四：其他组内的用户具有该文件的权限
                            r:read读
                            w:write写
                            x:excute执行

        2：文件权限管理
            概念：变更文件或目录的权限
            用法：
                第一种用法：chmod 755 a.txt 
                    执行思路：第一种方法是第二种方法的简写，3个数字分别表示权限一二三，把a.txt文件的权限修改成权限二的可读可写可执；权限三：可读可执行；权限四：可读可执行
                第二种用法：chomd u=rwx,g=rx,o=rx a.txt
                    执行思路：u代表权限二；g表示权限三：o表示权限四；把a.txt文件的权限修改成权限二的可读可写可执；权限三：可读可执行；权限四：可读可执行

    **Linux上常用网络操作
        1：主机名的配置
            hostname (作用：查看主机名)
            hostname xxxx (作用：修改主机名只是临时改变 重启后无效)
            如果想要永久生效，可修改/etc/susconfig/network文件里对应的hostname字段对应的值
        
        2：IP地址配置
            ifconfig 作用：查看(修改)ip地址(临时的，重启后无效)
            ifconfig etho 192.168.12.22 修改ip地址，临时的，重启后无效
            如果想要永久生效，修改 /etc/sysconfig/network-scripts/ifcfg-eth0文件
                该文件的内容讲解
                    DEVICE=eth0 //网卡名称
                    BOOTPROTO=static //获取ip的方式 取值为(static/dhcp/bttop/none)
                    HWADDR=D0:0C:29:B5:B2:69 //MAC地址
                    IPADDR=192.168.177.129 //IP地址
                    NETMASK=255.255.255.0 //子网掩码
                    NETWORK=192.168.177.0 //网络地址
                    BROADCAST=192.168.0.255 //广播地址
                    NBOOT=yes //系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备(激活此修改配置)

        3：域名映射
            概念：/etc/hosts文件用于在通过主机名进行访问时做ip地址解析的作用；相当于windows系统的hosts文件功能
        
        4：网络服务管理
            service network status 查看指定服务的状态
            service network stop 停止指定服务
            service network start 启动指定服务
            service nerwork restart 重启指定服务

            service --status-all 查看系统中所有后台服务
            netstart -nltp 查看系统中网络进程的端口监听情况

            防火墙设置
                概念：防火墙根据配置文件 /etc/sysconfig/iptables来控制本机的'出''入'网络访问行为
                service iptables status 查看防火墙状态
                service iptables stop 关闭防火墙
                service iptables start 启动防火墙
                chkconfig iptables off 禁止防火墙自启


    **Linux上软件安装
        1：Linux上的软件安装有以下几种常见方式介绍
            1-1：二进制发布包
                    软件(此软件由软件提供商为具体平台提供)已经针对具体平台编译打包发布，只要解压，修改配置即可；缺点：就是各个平台是不兼容的
                
            1-2：RPM包
                    这种方式可以兼容大多数的linux的平台；软件已经按照redhat的包管理工具规范RPM进行打包发布，需要获取到相应的软件RPM发布包，然后用RPM命令进行安装；缺点：不会安装这个软件所依赖的软件包

            1-3：Yum在线安装
                    软件已经以RPM规范打包，但发布在了网络上的一些服务器上，可用yum在线安装服务器上的rpm软件，并且会自动解决软件安装过程中的库依赖问题
                
            1-4：源码编译安装
                    软件以源码工程的形式发布，需要获取到源码工程后用相应开发工具进行编译打包部署

        2：上传与下载工具介绍
            2-1：FileZilla
                    概念：FileZilla是可以让本机电脑和Linux相互进行上传和下载的工具
                
            2-2：lrzsz
                    概念：我们可以使用yum安装方式安装 yum install lrzsz
                    注意点：
                        必须要有网络
                        要在crt中设置上传和下载的目录
                    用法：
                        上传用 rz
                            实例：命令行中输入rz会自动打开图形化工具，从上传的目录里面选个文件进行上传，然后点添加即可
                            注意点：上传至命令行当前操作的目录中
                        下载用 sz
                            实例：sz <文件> 下载当前命令行操作的目录中的文件，下载到设置的下载的本地目录中

            2-3：sftp(自带的无需下载)
                    用法：
                        使用alt+p组合键打开sftp窗口
                        put命令上传
                            用法实例：
                                put <文件绝对路径>  上传到了当前用户所在的根目录也叫操作目录
                                put h://redis-2.3.4-win32-win64 
                        get命令下载
                            用法实例：
                                get <文件名> 从root目录上，下载到本地的文档目录中

    
    **Linux上安装部署环境以及mysql数据库
        1：在Linux上安装JDK
            JRE的概念：
                JRE是java运行的环境，包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的。
            JDK的概念：
                JDK是java开发工具包，是程序员使用java语言编写java程序所需的开发工具包，是提供给程序员使用的。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析工具：jconsole,jvisualvm等工具软件，还包含了java程序编写所需要的文档和demo。

            JDK和JRE的使用场景：
                如果你需要运行java程序，只需要安装JRE就可以了。如果你需要编写java程序，需要安装JDK。

            在Linux上安装JDK的步骤：
                步骤一：上传JDK到Linux的服务器
                            上传JDK
                            卸载open-JDK(已经安装过的JDK)
                            查看jdk版本
                                java-version
                            查看安装的jdk信息
                                rpm -qa | grep java 
                            卸载jdk
                                rpm -e --nodeps <jdk全名称信息>
                步骤二：在Linux服务器上安装JDK
                            通常将软件安装到 /usr/local
                            直接解压就可以
                                tar -zxvf jdk.tar.gz
                步骤三：配置JDK的环境变量
                            配置环境的目录在 /etc/profile
                            打开环境配置目录 vim /etc/profile
                            在环境配置目录末尾行添加
                                JAVA_HOME=/user/local/jdk/jdk1.7.0_71 (此处是安装jdk的目录，也是添加环境变量最重要的操作)
                                CLASSPATH=.:$JAVA_HOME/lib.tools.jar
                                PATH=$JAVA_HOME/bin:$PATH
                                export JAVA_HOME CLASSPATH PATH
                            报错并退出
                            使更改的配置立即生效 
                                source /etc/profile  (source + 配置环境变量目录)

        2：在Linux上安装Mysql
            步骤：
                通过上传工具上传Mysql的包

                安装Mysql的服务端(server)
                    rpm -ivh <Mysql服务端>
                    安装完之后，可以在/root/.mysql_secret中找到随机的密码(登录时候要用到)

                安装MYsql客户端(client)
                    rpm -ivh <Mysql客户端>
                    
                启动mysql服务
                    service mysql start 
                
                登录mysql
                    mysql -uroot -p密码 (该密码就是刚刚安装服务端之后的提示的随机密码)
                
                重新设置密码(原因：第一次操作mysql时需要重新设置密码)
                    set password = password('root') //设置root为密码

                远程访问mysql设置
                    grant all privileges on *.* to 'root' @'%' identified by 'root' (授权所有的权限给root用户密码identified后面的root 授权操作)
                    flush privileges    (授权之后需要刷新一下)
                    在Linux中很多软件的端口都被'防火墙'限制，我们需要将防火墙关闭
        
        3：在Linux上安装tomcat
            1：步骤：
                Tomcat上传到Linux上

                将上传的tomcat解压

                在tomcat/bin目录下执行startup.sh(注意防火墙)
                        ./startup.sh执行文件
                
                查看tomcat是否安装成功，浏览器访问Linux的ip地址加默认端口号8080

            2：tomcat
                1：概念：tomcat可以认为是HTTP服务器，但通常它仍然会和Nginx配合一起使用
                概念理解：
                    1：动静态资源分离，运用Nginx的反向代理功能分发请求；所有动态资源的请求交给tomcat，而静态资源的请求(例如：图片，视频，css，js文件等)则直接由Nginx配合返回给浏览器，这样能打打减轻tomcat的压力
                        1：静态资源和动态资源的区别(通过静态资源和动态资源的执行机制进行区分)
                            1：在静态web程序中，客户端使用web浏览器经过网络链接到服务器上，使用HTTP协议发送一个请求，告诉服务器我现在需要得到哪个页面，所有的请求交给web服务器，之后web服务器根据用户的需要，从文件系统取出内容。之后通过web服务器返回给客户端，客户端接收到内容之后经过浏览器渲染解析，得到显示效果

                            2：在动态web中，程序依然使用客户端和服务端，客户端依然使用浏览器，通过网络连接到服务器，使用HTTP协议发送请求，现在的所有请求都先经过一个webServer来处理。如果客户端请求的是静态资源(*.htm等)，则将请求直接转交给web服务器，之后web服务器从文件系统中取出内容，发送回客户端浏览器进行解析执行；如果客户端请求的是动态资源(*.jsp,*asp,*.php等)，则先将请求转交给web container(web容器)，在web container中连接数据库，从数据库中取出数据等一系列操作后动态拼凑页面的展示内容，拼凑页面的展示内容后，把所有的展示内容交给web服务器，之后通过web服务器将内容发送回客户端浏览器进行解析执行

                        
                    2：负载均衡，当业务压力增大时，可能一个tomcat的实例不足以处理，那么这时可以启动多个tomcat实例进行水平扩展，而Nginx的负载均衡可以把请求通过算法分发到各个不同的实例进行处理

                2：tomcat和客户端的关系
                    客户端浏览器发送http请求->tomcat服务器->到达tomcat底层的serviet->执行serviet的方法处理请求，然后从数据库返回数据到tomcat服务,然后tomcat服务再将数据返回给客户端

                        2-1：serviet和jsp的关系
                            jsp的概念：JSP全名java Server Pages，java服务器页面，JSP是一种基于文本的程序，其特点就是HTML和java代码共同存在
                                jsp的作用：JSP为了简化servlet的工作出现的替代品，servlet输出HTML非常困难，JSP就是代替Servlet输出HTml的
                            
                            servlet概念：servlet其实就是一个遵循Servlet开发的java类，servlet是由服务器(例如tomcat服务器)调用的，运行在服务器端
                                servlet的作用：可以实现数据的交互功能，动态提供数据

        4：在Linux上安装redis
            1：步骤：
                安装gcc-c++环境(原因：redis是C语言开发的，安装redis需要先将官网下载的源码进行编译，编译依赖gcc环境)
                    yum install gcc-c++
                    输入y确认下载
                    下载完成

                安装redis
                    下载redis
                        wget http://download.redis.io/releases/redis-3.0.4.tar.gz
                    解压
                        tar -xzvf redis-3.0.4.tar.gz
                    编译安装(切换至程序目录，并执行make命令编译)
                        cd redis-3.0.4
                        make(作用：编译；条件是要有gcc的环境)

                    执行安装命令
                        make PREFIX=/usr/local/redis install(解释：安装到/usr/local/redis下的目录；PREFIX指定安装路径)
                        make install 安装完成后，会在/usr/local/redis/bin目录下生成下面几个可执行文件，它们的作用分别是
                                redis-server    (Redis服务器端启动程序)
                                redis-cli       (Redis客户端操作工具，也可以用telnet根据其纯文本协议来操作)
                                redis-benchmark (Redis性能测试工具)
                                redis-check-aof (数据修复工具)
                                redis-check-dump (检查导出工具)

                配置redis
                    复制配置文件redis.conf配置文件到/usr/local/redis/bin目录
                    cd redis-3.0.4
                    cp redis.conf /usr/local/redis/bin

                    启动redis环境的俩种方式：
                        服务端运行命令
                            ./redis-server redis.conf (当前目录下的server文件启动，需要跟上配置文件一起启动)
                        
                        客户端运行命令
                            启动redis-cli文件(执行机制：启动redis-cli客户端之后会连接redis的服务端)
            2：redis
                概念：redis是完全开源免费的，遵守BSD协议，是一个高性能的键值数据库(非关系型数据库)。是NoSql数据库之一，也被人们称之为数据结构服务器




        5：部署步骤：
                通过堡垒机将文件上传到linux服务器上
                将工具连接远程的Linux服务器
                用工具在Linux中安装部署环境
                用工具操作文件，在Linux中启动文件即完成部署




                


                
            






    






            

                