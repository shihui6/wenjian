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
            SHOW CREATE  DATABASE T1
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
               重要知识点： 查看当前正在使用的数据库
                            select database();
            2-创建数据表    (创建表字段后面加类型顺序  type null key default extra)
                CREATE TABLE [IF NOT EXISTS] table_name (
                column_name data_type,       //column_name 列名称    data_type  数据类型
            )
                    例子：create table tab1(
                        username UARCHAR(20),
                        age TINYNT UNSIGNED,
                        salary FLOAT(8,2) UNSIGNED
                    )

            3-查看数据表  SHOW TABLES [FROM db_name]  [LIKE 'parttern' | WHERE expr]   查看这个数据库中有什么表
                    例子：show tables  查看这个数据库中的表
            
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

            6-删除数据表  
                drop table + 表名

            7-NULL，字段值可以为空   NOT NULL，字段值禁止为空 
                运用：可以规定字段中的值，可以为空和禁止为空(必填)
                    例子：create table tab1(
                        username VARCHAR(20) NOT NULL,    //username字段禁止为空
                        age TINYINT UNSIGNED NULL           //age字段可以为空
                    );

            8-自动编号 写入字段
                AUTO_INCREMENT
                自动编号，且必须与主键组合使用   但是主键不一定要和AUTO_INCREMENT一起使用   
                默认情况下，起始值为1，每次的增量为1
                    例子：create table tab2(
                        id SMALLINT AUTO_INCREMENT PRIMARY KEY,
                        username VARCHAR(20) NOT NULL
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


***mysql外键约束的要求和解析
        约束：
            1-约束保证数据的完整性和一致性
            2-约束分为表级约束和列级约束  (约束只针对某一个字段称之为列级约束；约束针对俩个及俩个以上则是表级约束)
            3-约束类型包括
                    not null  (非空约束)
                    primary key(主键约束)
                    unique key (唯一约束)
                    default    (默认约束)
                    foreign key(外键约束)

                外键约束的知识点
                    定义：保持数据一致性，完整性。实现一对一或一对多的关系
                外键约束的要求：
                    1-父表(子表所参照的表)和子表(具有外键列的表)必须使用相同的存储引擎，而且禁止使用临时表
                    2-数据表的存储引擎只能为InnoDB
                    3-外键列(加过foreign key关键词的那一列)和参照列(外键列所参照的列)必须具有相似的数据类型。其中数字的长度或是否有符号位必须相同；而字符的长度则可以不同
                    4-外键列(外键列没有索引,mysql不会创建索引)和参照列(不创建索引，mysql则会自动创建索引)必须创建索引。如果外键列不存在索引的话，mysql将自动创建索引
                如何编辑数据表的默认存储引擎
                    1-编辑mysql配置文件
                    2-找到default-storage-engine=INNODB  (找到default-storage-engine把引擎改为INNODB)
                    3-重新启动mysql
                如何满足外键列和参照列具有相似的数据类型
                    查看数据表的存储引擎 和 数据表的编码格式
                        show create table + 表名
                            例子：show create table tab1
                    1-先创建俩张数据表 (父表和子表，并进行外键约束)
                        1-1父表：create table provincess(
                            id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
                            pname VARCHAR(20) NOT NULL
                        );
                        1-2子表：(子表创建外键约束要参照父表的字段)
                            create table usr(
                                id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
                                username VARCHAR(10) NOT NULL,
                                pid SMALLINT UNSIGNED,          //pid是外键列，必须和参照列(父表里的)具有相似的数据类型
                                FOREIGN KEY(pid) REFERENCES provincess(id)   //定义外键列为pid  参照provincess表中的id(参照列)   
                            )
                                解释索引：父表provincess已经定义了主键id，所以自动生成索引；子表的pid为外键列参照父表id系统也会自动生成索引
                        1-3查看表中创建的索引
                            show index from + 表名\G;   \\ 反斜杠G是查找索引以列表的形式展示
                    
                    2-外键约束的参照操作  (如果要在表中插入记录必须现在父表中插入记录，才能在字表中插入记录，因为子表参照的是父表中的信息)
                        1-CASCADE:从父表删除或更新且自动删除或更新子表中的匹配的行
                                例子： create table usr{
                                    id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
                                    username VARCHAR(10) NOT NULL,
                                    pid SMALLINT UNSIGNED, 
                                    FOREIGN KEY(pid) REFERENCES provincess(id) ON DELETE CASCADE  //在外键约束的时候进行ON DELETE 定义外键约束的参照操作
                                }

                                删除表中的一条记录 delete from 表名 where 条件
                                            例子  delete from usr where id = 1
                        2-SET NULL:从父表删除或更新行，并设置子表中的外键列为NULL。如果使用该选项，必须保证子表列没有指定的NOT NULL
                        3-RESTRICT::拒绝对父表的删除或更新操作
                        4-NO ACTION:标准SQL的关键字，在mysql中与restrict相同


***mysql修改数据表
    1-添加/删除列
            添加单列  ALTER TABLE tbl_name(表名) ADD [COLUMN] col_name(列名称) column_definition(列定义) [FIRST | AFTER col_name]
                    例子：ALTER TABLE tab1 ADD password VARCHAR(32) NOT NULL AGTER username  在username这一列后面添加 password字段(一列)
                        ALTER TABLE tab1 ADD password varchar(32) NOT NULL   tab1表的最后面添加password字段(一列)

            添加多列 ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition,...)

            删除列(可以删除一列，也可以删除多列) ALTER TABLE tbl_name DROP [COLUMN] col_name
                    例子：ALTER TABLE tab1 DROP password,DROP age    删除多列
    2-添加主键约束
            ALTER TABLE tbl_name ADD [CONSTRAINT[symbol]] PRIMARY KEY [index_type] (index_col_name)
                例子：ALTER TABLE tab1 ADD CONSTRAINT PK_user_id PRIMARY KEY (id)   //将id列改成主键约束
    3-添加唯一约束
            ALTER TABLE tbl_name ADD [CONSTRAINT[symbol]] UNIQUE [INDEX|KEY] [index_name] [index_type] (index_col_name,...)
                例子：alter table tab1 unique (username)
    4-添加外键约束 (所添加的外键约束必须符合添加外键的条件)
            ALTER TABLE tbl_name ADD [CONSTRAINT[symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition
            例子：给表二的pid参照表一id添加外键约束
                alter table tab2 ADD FOREIGN KEY (pid) reference tab1 (id)
    5-添加/删除默认约束
            ALTER TABLE tbl_name ALTER [COLUMN] col_name {SET DEFAULT literal | DROP DEFAULT}
                例子：alter table tab1 alter age set default 12   //添加默认值
                     alter table tab1 alter age drop default       //删除默认值
    6-删除主键约束
            ALTER TABLE tbl_name DROP PRIMARY KEY
                alter table tab1 drop primary key
            ALTER TABLE tbl_name DROP {INDEX|KEY} index_name    (首先通过show index from tab1\G  查看表中的索引)
                alter table tab1 drop index username
    7-删除外键约束
            ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol  (首先查看表的外键约束 show create table tab1 查看出外键约束 fk_symbol位置填写外键约束的内容)

                







    ***为什么net start mysql (net start + 服务名) 显示服务器名无效
        原因：因为net start + 服务名，启动的是win下注册的而服务，此时，系统中并没有注册mysql到服务中心。即当前路径下没有mysql服务
        解决：如何将mysql注册到win服务里面
            1-来到mysql的安装路径下bin
            2-在命令行中输入mysqld --install 成功出现Service successfully install代表你已经安装成功
            3-如果出现 Install/Remove of the Service Denied!表示不成功，通过管理员你的身份运行DOS窗口就可以成功了

    ***登录数据库命令  mysql -u + 用户名 -p 即可登录