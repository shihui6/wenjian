###mysql数据库
    历史：Mysql由瑞典Mysql AB公司开发，目前食欲Oracle公司
         Mysql是一个开源的关系型数据库管理系统
         Mysql分为社区版和企业版
    
    目录结构介绍
        bin目录，存储可执行文件
        data目录，存储数据文件 (可以把创建的数据库文件，索引文件存储在data目录中)
        docs，文档 
        include目录，存储包含的头文件 
        lib目录，存储库文件 (我们所需要调用的文件)
        share，错误消息和字符集文件 

    关于mysql配置文件my.ini文件的介绍
        [client]   指mysql客户端  
        port       指mysql的端口号
        default-character-set   默认的编码方式改成utf8
        [mysqld]   主要是进行mysql服务器端的配置

    启动和关闭mysql服务
        1-图形化的启动和关闭服务，在计算机管理-> 服务和应用程序 -> 服务  里面进行启动和关闭
        2-使用命令行 
            net start mysql 启动服务
            net stop mysql 停止服务
    查看当前正在使用的数据库
        select database();

    登录mysql客户端
        mysql -uroot -pwossh1423875545 -P3306 -hlocalhost    //-P后面是端口号

    查看mysql版本
        在命令行中 输入mysql -V 

    mysql参数解释  登录指定端口的数据库
        -D , --database=name  打开指定数据库
        --delimiter = name  指定分隔符
        -h, --host=name  服务器名称
        -p , --password 密码
        --prompt=name 设置提示符
        -u , --user=name  用户名
        -V ,--version  输出版本信息并退出

    mysql退出
        mysql>exit;
        mysql>quit;
        mysql> \q

    修改mysql提示符
        第一种：mysql -uroot -pwossh1423875545 --prompt \h
        第二种：只能在登录之后进行修改  mysql>prompt mysql>  

    mysql提示符  (prompt \u@\h \d>)  输出结果为  root@127.0.0.1 test>  可修改提示符的操作
        \D 完整的日期
        \d 当前数据库
        \h 服务器名称
        \u 当前用户

    MySQL语句的规范
        关键字与函数名称全部是大写 (如果写成小写系统也是承认的)
        数据库名称，表名称，字段名称全部小写
        SQL语句必须以分号结尾  (如果不以分号结尾，程序就无法执行完成)


    数据库操作
        1-创建数据库
            CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name  
            解释：{} 指必写项  
                  | 指选择其中的一个 或的意思
                  [] 有或没有都是被允许的
                  IF NOT EXISTS 如果当前创建的数据库存在，会报错，但是加上IF NOT EXISTS 之后就不会报错，只会产生一个警告
                        例子： CREATE DATABASE IF NOT EXISTS T1  如果当前有t1数据库，则会产生警告，不加上if not exists 会报错
        2-查看当前服务器下的数据库列表
            SHOW {DATABASES | SECHEMAS} [LIKE '']
        3-查看数据库创建时候的使用的编码方式是什么
            CREATE SHOW DATABASE T1
        4-创建以gbk的编码方式的数据库
            CREATE DATABASE T2 CHARACTER SET GBK
        5-将数据库的编码方式由原有的GBK改成utf8
            alter {database | schema} [dd_name] [default] character set [=] charset_name
            例子： alter database t2 character set = utf8  就将数据库t2的编码方式改成了utf8了
        6-删除数据库   在删除数据库的情况下一定要保证数据库存在才行
            drop {database | schema} [if exists] db_name


***上个阶段的总结
    mysql默认的端口号是多少 3306
    mysql中的超级用户叫什么 root
    创建数据库 create database
    修改数据库 alter database
    删除数据库 drop database


###数据类型和数据表的操作
        数据类型
            指：列，存储过程参数，表达式和局部变量的数据特征，它决定了数据的存储格式，代表了不同的信息类型
            1-整型
                整型： TINYINT 占1个字节 有符号值：-128到127  无符号值：0到255
                       SMALLINT 占2个字节 有符号值：-32768到32767 无符号值：0到65535
                       MEDIUMINT 占3个字节 有符号值: -8388608到8388607 无符号值：0到16777215
                       INT 占4个字节 有符号值：-2的31次方 到 2的31次方减一  无符号值：0到2的32次方减一
                       BIGINT 占8个字节 有符号值和无符号值都是非常大的
                所以在存储数据的时候根据具体情况进行存储，例子：存储年龄，一般0到100多，所以取 TINYINT 占1个字节就可以，如果选BIGINT ，如果有1千万个用户，那么所占的字节就多很多，所以占的内存就多很多  考虑整数类型存储方式，可以优化性能

            2-浮点型
                FLOAT[(M,D)] 单精度浮点数 存储范围：-3.4028E+38到3.4028E+38  M是数字总位数，D是小数点后面的位数(M>=D)。如果M和D被省略，根据硬件允许的限制来保存值。单精度浮点数精确到大约7位小数位
                DOUBLE[{M,D}]  双精度浮点数
            
            3-日期时间型    存储范围
                YEAR        1970年 - 2069年的一个日期
                TIME        -8385959 - 8385959
                DATE        1000年的1月1号 - 9999年12月31号的一个日期
                DATETIME    1000年的1月1号零点 - 9999年的12月31号的23点59分59秒 
                TIMESTAMP   1970年的1月1号零点起 - 2037年
            
            4-字符型                                存储需求
                CHAR(M)                       M个字节，0<=M<=255
                VARCHAR(M)                    L+1个字节，其中L<=M且0<=65535
                TINYTEXT
                TEXT
                MEDIUMTEXT
                LONGTEXT
                EUNM('value1','value2'...)  1或2个字节， 取决于枚举值的个数(最多65535个)
                SET('value1','value2'...)  1、2、34或者8个字节，取决于set成员的数量(最多64个成员)


        数据表
            指：数据表(称表)是数据库最重要的组成部分之一，是其对象的基础  (数据表是数据库的重要组成部分)
            表中 行为记录，列为字段

            1-打开数据库    use + 数据库名称
            2-创建数据表
                CREATE TABLE [IF NOT EXISTS] table_name (
                column_name data_type,       //column_name 列名称    data_type  数据类型
            )
                    例子：create table tab1(
                        username UARCHAR(20),
                        age TINYNT UNSIGNED,
                        salary FLOAT(8,2) UNSIGNED
                    )

            3-查看数据表  SHOW TABLES [FORM db_name]  [LIKE 'parttern' | WHERE expr]
                    例子：SELECT DATABASE() FROM TEST;
            
            4-查看刚刚创建好了的数据表的结构
                SHOW COLUMNS FROM tbl_name
                    例子：SHOW COLUMNS FROM tab1
            
            5-插入记录
                insert [into] tbl_name [(col_name)] values(val,...)
                    例子：不指定添加的column就需要添加全部的字段信息  insert tab1 values('tom',25,7892)  
                            指定添加的column                        insert tab1(username,salary) values('join',345656)

            6-查看插入记录  查看数据是否插入
                select expr ,...from tbl_name
                    例子：select * from tab1

            7-NULL，字段值可以为空   NOT NULL，字段值禁止为空 
                运用：可以规定字段中的值，可以为空和禁止为空(必填)
                    例子：create table tab1(
                        username VARVHAR(20) NOT NULL,    //username字段禁止为空
                        age TINYINT UNSIGNED NULL           //age字段可以为空
                    );

            8-自动编号 写入字段
                AUTO_INCREMENT
                自动编号，且必须与主键组合使用   但是主键不一定要和AUTO_INCREMENT一起使用   
                默认情况下，起始值为1，每次的增量为1
                    例子：create table tab2(
                        id SMALLINT AUTO_INCREMENT PRIMARY KEY,
                        username VARVHAR(20) NOT NULL
                    );

            9-mysql数据表中的主键约束
                1-每张数据表只能存在一个主键
                2-主键保证记录的唯一性
                3-主键自动为NOT NULL
                    例子：如果创建的表格没有自动递增，且赋值为主键，所以就可以被赋值

            10-唯一约束，保证唯一性 (UNIQUE KEY)    主键约束一张表只能存在一个，唯一约束一张表可以存在多个
                唯一约束可以保证记录的唯一性
                唯一约束的字段可以为空值
                每张数据表可以存在多个唯一约束
                    create table tb5(
                        id SMALLINT AUTO_INCREMENT PRIMARY KEY,
                        username VARCHAR(30) NOT NULL UNIQUE KEY
                    )
            
            11-默认约束
                定义：当插入记录时，如果没有明确为字段赋值，则自动赋予默认值
                    例子：create table tb6(
                        id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
                        username VARCHAR(20) NOT NULL UNIQUE KEY,
                        sex ENUM('1','2','3')  DEFAULT '3'
                    )














    ***为什么net start mysql (net start + 服务名) 显示服务器名无效
        原因：因为net start + 服务名，启动的是win下注册的而服务，此时，系统中并没有注册mysql到服务中心。即当前路径下没有mysql服务
        解决：如何将mysql注册到win服务里面
            1-来到mysql的安装路径下bin
            2-在命令行中输入mysqld --install 成功出现Service successfully install代表你已经安装成功
            3-如果出现 Install/Remove of the Service Denied!表示不成功，通过管理员你的身份运行DOS窗口就可以成功了

    ***登录数据库命令  mysql -u + 用户名 -p 即可登录