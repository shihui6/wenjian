###nginx
    **应用场景
        1：http服务器。nginx是一个http服务可以独立提供http服务。可以做网页静态服务器
        2：虚拟主机，可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机
        3：反向代理，负载均衡。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况
    

    **nginx在Linux下的安装
        1：环境准备
            1：需要安装gcc的环境
            2：第三方的开发包
                1：PCRE
                    概念：PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括perl兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库

                    下载方式：yum install -y pcre pcre-devel

                    注意点：pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库
                2：zlib
                    概念：zlib库提供了很多中压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库

                    下载方式：yum install -y zlib zlib-devel
                3：OpenSSL
                    概念：OpenSSL是一个强大的安全套接字层密码库，囊括主要的密码算法，常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它的目的使用。nginx不仅支持http协议，还支持https协议(即在ssl协议上传输http)，所以需要在linux安装openssl库

                    下载方式：yum install -y openssl openssl-devel

    
    **nginx静态网站部署
        1：静态网站的部署
            1：方法：将/资料/静态页面/index 目录下的所有内容 上传到服务器的/usr/local/nginx/html 下可访问
            
            2：需要修改Linux系统下的nginx.conf的nginx配置文件，则需要用到工具EditPlus
                操作步骤：[File]->[FTP Upload]->[Settings]->[Add]->Description(写一个描述信息，可随便写)，FTP server(即是Linux下的部署的地址)，username,password->[Advanced Options...]->[encryption(连接方式) 选择sftp]和port端口->[ok]
                然后在Deitplus的操作页面Directory里面找设置的远程连接的目录，就可以进行操作和文件上传

            3：nginx.conf配置信息
                server {
                    listen 80 //端口号没有指定的话，默认80端口
                    server_name localhost; //域名或者ip地址，这里配置localhost意思是代表当前的Linux服务器的ip地址

                    location / {
                        root html    //默认访问资源的目录；(也叫指定访问的映射的目录)，这里是html目录
                        index index.html index.htm  //指定访问的目录下面的默认的首页，这是指定的默认的首页是index.html或者是index.htm
                    }

                    error_page 500 502 503 504 /50x.html; #发生错误时，默认访问的错误页面
                    location = /50x.html {
                        root html;
                    }
                }


        2：配置虚拟主机
            概念：虚拟主机也叫'网站空间',就是把一台运行在互联网上的物理服务器瓜分成多个'虚拟'服务器
            作用：在同一个nginx里面部署多个项目，在配置文件里配置多个server
            用法：上传静态网站(前端打包的文件)
                    将/资料/静态页面/index  上传到服务器的/usr/local/nginx/html下
                    将/资料/静态页面/regist  上传到服务器的/usr/local/nginx/regist下
            

        3：域名
            概念：域名是ip的对应关系，如，我们输入ip地址加端口号访问，但是ip地址太难记，所以就有了域名
                概念理解：一个域名对应一个ip地址，一个ip地址可以被多个域名绑定
            
            域名解析的执行过程:
                当我们在地址栏中输入baidu.com域名之后，它会在本地找hosts文件看有没有相应的ip与baidu.com域名对应，如果有的话就返回ip地址再通过80端口访问百度的服务器；如果hosts文件没有对应的ip，那么需要通过DNS服务器去找对应的域名和ip的对应关系

            通过不同的域名访问的都是80端口，看到不一样的项目


        4：Nginx反向代理与负载均衡
            1：正向代理(联想图理解)
                概念：在客户端(浏览器)配置代理服务器，通过代理服务器进行互联网进行访问。这种代理服务称为正向代理

                概念解释：如果把局域网外的internet想象成一个巨大的资源库，则局域网中的客户端要访问internet，则需要在浏览器配置代理服务器，通过代理服务器去访问网址，然后返回内容。这种代理服务就称为正向代理

            2：反向代理(联想图理解)：
                概念：我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实的IP地址

                    1：配置反向代理---准备工作
                        1:将travel案例部署到tomcat中(ROOT目录)，上传到服务器
                        2：启动TOMCAT，输入网址http://192.168.177.129:8080可以看到网站首页
                    
                    2：配置nginx方向代理
                        1：在nginx主机修改nginx配置文件
                            具体内容：
                                upstream tomcat-travel {
                                    server 192.168.177.129:8080
                                }

                                server {
                                    listen 80;
                                    server_name www.htmtravel.com;

                                    location / {
                                        proxy_pass http://tomcat-travel;
                                        index index.html;
                                    }
                                }

                            浏览器url输入www.htmtravel.com地址，通过nginx反向代理的执行过程：
                                浏览器url输入www.htmtravel.com地址，访问nginx，然后nginx解析对应的proxy_pass，然后会找到短路径tomcat-travel,通过tomcat-travel找到upstream属性对应的名字，然后就找到了对应的ip和端口，然后nginx通过ip和端口访问tomcat，然后tomcat把资源返回给nginx，然后nginx把资源返回给浏览器
            
            3：负载均衡
                1：配置负载均衡的准备工作
                    1：将tomcat复制三份，修改端口分别为8080，8081，8082
                    2：分别启动这三个tomcat服务
                
                2：配置负载均衡
                    upstream tomcat-travel {
                        server 192.168.177.129:8080;
                        server 192.168.177.129:8080;
                        server 192.168.177.129:8080;
                    }

                    server {
                        listen 80;
                        server_name www.htmtravel.com;

                        location / {
                            proxy_pass http://tomcat-travel;
                            index index.html;
                        }
                    }

                    负载均衡的执行流程和上面的配置反向代理是一样的，不同点是，在upstream里面配置了3个server(默认的是1:1:1)，且通过www.htmtravel.com域名访问到的概率是相等的，配置权重在ip地址后加weight=2,然权重变成百分之50  
                                                                                    例如(server 192.168.177.129:8080 weight=2)


    **linux的操作
        查看用nginx部署的项目占用的端口     netstat -ntulp | grep nginx
        查看服务器上所有项目占用的端口      netstat -ntulp


                