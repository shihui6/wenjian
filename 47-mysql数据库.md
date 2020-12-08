###数据库的基本概念
    DB：数据库，存储数据的"仓库"。是保存了一系列有组织的数据的容器
    DBMS：数据库管理系统。又称为数据库软件(产品)，用于管理DB中的数据。
    SQL：结构化查询语言：专门用来与DBMS通信的语言(增删改查的语言，发送给数据库管理系统，然后再对数据库进行操作)

    mysql数据库的优点：
        成本低：开放源代码，一般可以免费试用
        性能高：执行很快
        简单：很容易安装和使用

    数据库存储数据的特点：
        1.将数据放到表中，表再放到库中
        2.一个数据库中可以有多个表，每个表都有一个名字，用来表示自己。表明具有唯一性
        3.表具有一些特性，这些特性定义了数据在表中如何存储
        4.表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java中的"属性"
        5.表中的数据是按行存储的，每一行类似于java中的“对象”

    mysql数据库的安装
        安装步骤：
            Custom(自定义安装)->next->install->finish->跳出来一个配置界面，若配置界面被关了，也可以在刚刚mysql安装的目录bin目录下，有一个MysqlLInstanceConfig.exe文件，双击打开即可->next->选择Detailed Configuration(精确配置)->Developer Machine(目前学习阶段只有开发机就够了)->Multifunctional DataBase(多功能存储引擎)->Decision Support(DSS)/OLAP->Manual Selected Default Character Set/Collation，选字符集UTF8->勾选上Include Bin Directory in Windows Path(不钩的话就用不了mysql的命令了)->设置密码,且把Enable root access from remote machines允许远程机访问勾选->Execute执行，成功了
    
        配置文件介绍
            配置文件位置：mysql的安装位置下与bin目录在同一级下的my.ini配置文件打开->可以配置服务端的端口号port；baseDir路径指的是：mysql的安装路径，datadir路径指的是：数据库将数据最终存储在datadir的文件磁盘路径下;character-set-server可修改字符集;default-storage-engine可修改存储引擎
    

    mysql的启动,停止,登录,退出
        mysql服务的启动和停止
            方式一：
                计算机右键点击管理->服务应用程序->双击服务,找到相应的服务，右键点击可以停止或启动服务，右击属性可修改开机自动启动或开机不自动启动
            方式二：
                通过管理员身份运行：
                                    net start 服务名(启动服务)
                                    net stop 服务名(停止服务)
            
        mysql服务的登录和退出
            通过windows自带的客户端登录：
                    mysql [-h主机名 -P端口号] -u用户名 -p密码
            退出：
                exit或ctrl+c

    
###mysql的使用
    mysql的语法规范
        1.不区分大小写，但建议关键字大写，表名，列名小写
        2.每条命令最好用分号结尾
        3.每条命令根据需要，可以进行缩进或换行
        4.注释：
            单行注释：#注释文字
            单行注释：-- 注释文字
            多行注释：/*注释文字*/

    mysql的常见命令：
        >查看当前所有的数据库
                show databases;
        >打开指定的数据库
                user 库名;
        >查看当前库的所有的表
                show tables;
        >查看其它库的所有的表
                show tables from 库名;
        >创建表
                create table 表名(
                    列名 列类型,
                    列名 列类型,
                )
        >查看表结构
                desc 表名;
        >查看服务器的版本
                方式一：登录到mysql服务端
                    select version();
                方式二：没有登录到mysql服务端
                    mysql --version 或 mysql -V

    
    ***SQLyog的使用
        图形化界面中在常见命令下吸入sql语句，并且可以把在常见命令下的sql语句保存成sql文件在本地
        在本地上执行sql脚本文件步骤：
                点击数据库连接对象(root@localhost)右键点击执行sql脚本->选择需要执行的sql文件点击执行即可

        *DQL语言的学习
            1.基础查询
                语法：select 查询列表 from 表名;
                特点：
                    1.查询列表可以死活：表中的字段，常量值，表达式，函数
                    2。查询的结果是一个虚拟的表格

                细节点：(着重号``的知识点)
                    1.sql文件中有很多sql语句，只想执行一个sql语句：选中想要执行的sql语句，点击执行即可
                    2.着重号``,查询表格的时候如select `last_name` from employees;和select last_name from employees;是一样的，``符号可以区分关键字和字段名

                使用： 如：表名是employees 
                    1.查询表中的单个字段
                        select last_name from employees; 
                    2.查询表中的多个字段
                        select last_name,salary,email from employees; 
                    3.查询表中的所有字段
                        方式一：按照原始表的字段顺序展示
                            select * from employees; 
                        方式二：按照自定义的字段顺序展示
                            select 字段名顺序 from employees;

                    4.查询常量值  (单引号的知识点)
                        select 100;
                        select 'join';(sql语法里不区分字符串字符，统一用单引号)
                    5.查询表达式
                        select 100*90;
                    6.查询函数
                        select version();
                    7.起别名： (""的知识点)
                        作用：
                            >便于理解
                            >如果要查询的字段有重名的情况，使用别名可以区分开来
                        方式一：使用as
                                select 100*90 as 结果(结果就是别名)
                                select last_name as 姓,first_name as 名 from employees;
                        方式二：使用空格
                                select last_name 姓,first_name 名 from employees;
                        案例：查询salary，显示结果为out put  
                            select salary as "out put" from employees;(这里用单引号也可以，sql语法建议使用双引号)
                    
                    8.去重：
                        案例：查询员工表中涉及到的所有的部门编号department_id,需要用到distinct
                            select distinct department_id from employees;

                    9.+号的作用：
                        java中的+号：
                            1.运算符：两个操作数都为数值型
                            2。连接符：只要有一个操作数为字符串
                        mysql中的+号
                            仅仅只有一个功能：运算符
                            案例：
                                select 100+90; 两个操作数都为数值型，则做加法运算
                                select '123'+90;只要其中一方为字符型，试图将字符型数值转换成数值型
                                                    如果转换成功，则继续做加法运算
                                select 'join'_90;   如果转换失败，则将字符型数值转换成0
                                select null+100;    只要其中一方为null，则结果为null
                            

                        案例：查询员工名和姓连接成一个字段，并显示姓名，字段的拼接，concat()函数有拼接的作用
                                select concat(last_name,first_name) as 姓名 from employees;

                事例练习：知识点(IFNULL()函数使用，作用：判断字段值是否为null)
                    #1.下面的语句是否可以执行成功
                        SELECT last_name,job_id,salary AS sal FROM employees;
                    #2.找出下面语句的错误
                        SELECT employee_id,last_name,salary*12 AS "ANNUAL SALARY" FROM employees;
                    #3.显示表departments的结构，并查询其中的全部数据
                        DESC departments;
                        SELECT * FROM departments;
                    #4.显示出表employees中的全部job_id不能重复
                        SELECT DISTINCT job_id FROM employees;
                    #5.显示出表employees的全部的列，各个列之间用逗号连接，列头显示成out_put
                        SELECT CONCAT(`first_name`,',',`last_name`,',',`job_id`,',',IFNULL(commission_pct,0)) AS out_put FROM employees;


            2.条件查询
                语法：
                    select  查询列表  from  表名   where  筛选条件
                    条件筛选的分类
                        1.按条件表达式筛选
                            简单条件运算符：> < = != <> >= <=
                        2.按逻辑表达式筛选
                            逻辑运算符： &&     ||      ！
                                        and    or      not   
                            作用：用于连接条件表达式
                                &&和and：两个条件都为true，结果true，反之为false
                                ||或or：只要有一个条件为true，结果为true，反之为false
                                ！或not：如果连接的条件本身为false，结果为true，反之为false  
                        3.模糊查询
                            like
                                特点：
                                    一般和通配符搭配使用
                                        通配符：
                                            %：任意多个字符，也包含0个字符
                                            _：任意单个字符

                            between and(等价于>=某个值<=)
                                注意点：
                                    使用between and可以提高语句的简洁度
                                    包含临界值
                                    两个临界值不要调换顺序
                                    数值类型要要一致

                            in
                                作用：判断某字段的值是否属于in列表中的某一项
                                特点：
                                    使用in提高语句简洁度
                                    in列表的值类型必须一致或兼容

                            is null
                                注意点：
                                    =或<>不能用于判断null的值
                                    is null 或 is not null 可以判断null的值

                            安全等于<=>
                                is null :仅仅可以判断null值，可读性比较高，建议使用
                                <=>：既可以判断null值，又可以判断普通数值，可读性较低

                        使用：
                            1.按条件表达式筛序
                                案例1：查询工资>12000的员工的信息
                                    select * from employees where salary>12000;
                                案例2：查询部门编号不等于90的员工名和部门编号
                                    select last_name,department_id from employees where department_id<>90;

                            2.按逻辑表达式筛选
                                案例1：查询工资z在10000到20000之间的员工名，工资以及奖金
                                    select last_name,salary,commission_pct from employees where salary>=10000 and salary <= 20000;
                                案例2：查询部门编号不是在90到110之间，或者工资高于15000的员工信息
                                    select * from employees where department_id < 90 || department_id > 110 || salary > 15000;

                            3.模糊查询
                                like的案例
                                    案例1：查询员工名中包含字符a的员工信息
                                        select * from employees where last_name like '%a%';
                                    案例2：查询员工中第三个字符为n，第五个字符为l的员工名和工资
                                        select last_name,salary from employees where last_name like '__n_l%';
                                    案例3：查询员工名中第二个字符为_的员工名
                                        select last_name from  employees where last_name like '_\_%';
                                        或者
                                        select last_name from  employees where last_name like '_$_%' escape '$';(escape作用：标识$符号在这里是转义符)

                                between and的案例
                                    案例1：查询员工编号在100到120之间的员工信息
                                        select * from employees where employee_id between 100 and 120;
                                
                                in的案例
                                    案例1：查询员工的工种编号是 IT_PROG，AD_VP,AD_PRES中的一个的员工名和工种编号
                                        select last_name,job_id from employees where job_id in ('IT_PROG','AD_VP','AD_PRES');
                                
                                is null的案例
                                    案例1：查询没有奖金的员工名和奖金率
                                        select last_name,commision_pct from employees where commision_pct is null;
                                        或者
                                        select last_name,commision_pct from employees where commision_pct <=> null;

                                    案例:查询工资为12000的员工信息
                                        select last_name,salary from employees where commision_pct <=> 12000;

                                
            3.排序查询
            4.常见函数
            5.分组函数
            6.分组查询
            7.连接查询
            8.子查询
            9.分页查询
            10.union联合查询
        


