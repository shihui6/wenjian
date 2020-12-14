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

###重点：
    sql语句的执行顺序
    执行顺序：from--where--group by--having--select--order by
 
        from:需要从哪个数据表检索数据 
        where:过滤表中数据的条件 
        group by:如何将上面过滤出的数据分组 
        having:对上面已经分组的数据进行过滤的条件  
        select:查看结果集中的哪个列，或列的计算结果 
        order by :按照什么样的顺序来查看返回的数据 


    或者
        1、from子句组装来自不同数据源的数据； 
        2、where子句基于指定的条件对记录行进行筛选；
        3、group by子句将数据划分为多个分组； 
        4、使用聚集函数进行计算； 
        5、使用having子句筛选分组； 
        6、计算所有的表达式； 
        7、使用order by对结果集进行排序。

    
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
                use 库名;
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
        图形化界面中在常见命令下导入sql语句，并且可以把在常见命令下的sql语句保存成sql文件到本地
        在数据库连接对象上(这里指本地)执行sql脚本文件步骤：
                点击数据库连接对象(root@localhost)右键点击执行sql脚本->选择需要执行的sql文件点击执行即可

        *DQL语言的学习
            1.基础查询
                语法：select 查询列表 from 表名;
                特点：
                    1.查询列表可以是：表中的字段，常量值，表达式，函数
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
                                select 'join'+90;   如果转换失败，则将字符型数值转换成0
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
                                案例1：查询工资在10000到20000之间的员工名，工资以及奖金
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
                                        select last_name from  employees where last_name like '_$_%' escape '$';(escape作用：$符号通过escape处理成了转义符)

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

时间：2020/12/09
                                
            3.排序查询
                语法：select 查询列表 from 表 [where 筛选条件] order by 排序列表 [asc|desc]
                特点：
                    1.asc代表的是升序，desc代表的是降序，如果不写，默认是升序
                    2.order by子句中可以支持单个字段，多个字段，表达式，函数，别名
                    3.order by子句一般是放在查询语句的最后面，limit子句除外
                使用：
                    案例：查询员工信息，要求工资从高到底排序
                        select * from employees order by salary desc;
                    案例：查询员工信息，要求工资从低到高排序
                        select * from employees order by salary;
                        或者
                        select * from employees order by salary asc;
                    
                    案例2：查询部门编号>=90的员工信息，按入职时间的先后进行排序【添加筛选条件】
                        select * from employees where department_id>=90 order by hiredate asc;

                    案例3：按年薪的高低显示员工的信息和年薪【按表达式排序】
                        select *,salary*12*(1+ifnull(commission_pct,0)) 年薪 
                        from employees 
                        order by salary*12*(1+ifnull(commission_pct,0)) desc;
                    
                    案例4：按年薪的高低显示员工的信息和 年薪【按别名排序】
                        select *,salary*12*(1+ifnull(commission_pct,0)) 年薪 
                        from employees 
                        order by 年薪 desc;

                    案例5：按姓名的长度显示员工的姓名和工资【按函数排序】
                        select last_name,salary
                        from employees
                        order by length(last_name) desc;

                    案例6：查询员工信息，要求先按工资升序，再按员工编号降序【按多个字段排序】
                        select *
                        from employees
                        order by salary asc,employees_id desc;


            4.常见函数
                概念：类似于java的方法，将一组逻辑语句封装在方法体重，对外暴露方法名
                好处：1.隐藏了实现细节，2.提高代码的重用性
                使用：select 函数名(实参列表) 【from 表】
                分类：
                    ##单行函数
                        如：concat,length,ifnull等
                        **字符函数
                            1。length 
                                作用：获取参数值的字节个数
                                使用：select length('join')

                            2.concat
                                作用：拼接字符串
                                使用：select concat(last_name,'_',first_name) 姓名 from employees;

                            3.upper,lower
                                作用：大小写转换
                                使用：
                                    案例：将姓变大写，名变小写，然后拼接
                                    select concat(upper(last_name),lower(first_name)) 姓名 from employees;

                            4.substr,substring
                                作用：截取字符长度
                                注意：索引从1开始，跟java中的索引从0开始不一样
                                使用：
                                    案例：截取从指定索引处后面所有字符
                                    select substr('今天天气真好是吧',7) out_put;

                                    案例：截取从指定索引处指定字符长度的字符
                                    select substr('今天天气真好',1，3) out_put;
                            
                            5.instr
                                作用：返回子串第一次出现的索引，如果找不到返回0
                                使用：
                                    案例：返回太阳在instr参数1中首次出现的位置的索引
                                    select instr('今天天气真好呀太阳好大','太阳') as out_put
                            
                            6.trim
                                作用：去除字符前后的空格或者指定的字符
                                使用：
                                    案例：去除天气好前后的空格
                                    select trim('   天气真好    ') as out_put;
                                    案例：去除天气好前后的字母a
                                    select trim('a' from 'aaaaa天气真好aaaaaa') as out_put;

                            7.lpad
                                作用：用指定的字符实现左填充指定长度
                                使用：select lpad('殷素素',10,'*') as out_put; 输出结果：********殷素素

                            8.rpad
                                作用：用指定的字符实现左填充指定长度
                                使用：select rpad('殷素素',6,'ab') as out_put;输出结果：殷素素aba

                            9.replace
                                作用：全部替换
                                使用：select replace('周芷若周芷若张无忌爱上周芷若','周芷若','赵敏') as out_put;
                                        输出结果：赵敏赵敏张无忌赵敏
                                    

                        **数学函数
                            1.round
                                作用：四舍五入
                                使用:
                                    案例：
                                    select round(-1.55);输出结果-2
                                    select round(1.567,2);四舍五入小数点后保留2位，输出结果1.57

                            2.ceil
                                作用：向上取整，返回>=该参数的最小整数
                                使用：select ceil(1.002);输出：2
                            
                            3.floor
                                作用：向下取整，返回<=该参数的最大整数
                                使用：select floor(9.9);输出：9
                            
                            4.truncate
                                作用：截断
                                使用：select truncate(1.69999,1);输出：1.6
                            
                            5.mod
                                作用：取余(和c，java中的取余是一样的)
                                使用：select mod(-10,-3);输出：1


                        **日期函数
                            1.now 
                                作用：返回当前系统的日期+时间
                                使用：select now(); 输出：2020-12-09 12:19:05
                            
                            2.curdate
                                作用：返回当前系统日期，不包含时间
                                使用：select curdate();输出：2020-12-09
                            
                            3.curtime
                                作用：返回当前时间，不包含日期
                                使用：select curtime();输出：12:19:05

                            4.可以获取指定的部分，年，月，日，小时，分钟，秒
                                select year(now()) 年;      输出：2020
                                select year('1998-1-1') 年; 输出：1998
                                select month(now()) 月;     输出：12
                                select monthname(now()) 月; 输出：December

                            5.str_to_date
                                作用：将字符通过指定的格式转换成日期
                                使用：
                                    格式符          功能
                                    %Y              四位的年份
                                    %y              2位的年份
                                    %m              月份(01,02...11,12)
                                    %c              月份(1,2...11,12)
                                    %d              日
                                    %H              小时(24小时制)
                                    %h              小时(12小时制)
                                    %i              分钟
                                    %s              秒


                                    select str_to_date('1998-3-2','%Y-%c-%d') as out_put; 输出1998-03-02
                                    select str_to_date('3-2-1998','%c-%d-%Y') as out_put; 输出1998-03-02

                            6.date_format
                                作用：将日期转换成字符
                                使用：
                                    select date_format(now(),'%Y年%m月%d日') as out_put; 输出：2020年12月09日

                        **其他函数(指的是系统函数)
                            select version();   查看当前数据库版本
                            select database();  查看当前使用的数据库
                            select user();      查看当前用户


                        **流程控制函数
                            1.if函数
                                作用：if else的效果
                                使用：select if(10<5,'大','小'); 输出：小

                            2.case函数
                                作用：
                                    作用1：switch...case的效果
                                        mysql中的写法：
                                            case 要判断的字段或表达式
                                            when  常量1  then 要显示的值1或语句1
                                            when  常量2  then 要显示的值2或语句2
                                            .....
                                            else  要显示的值n或语句n
                                            end
                                        使用：案例：查询员工的工资，要求：
                                                    部门号=30，显示的工资为1.1倍
                                                    部门号=40，显示的工资为1.2倍
                                                    部门号=50，显示的工资为1.3倍
                                                    其他部门，显示的工资为原工资

                                            select salary 原始工资,department_id,
                                            case department_id
                                            when 30 then salary*1.1
                                            when 40 then salary*1.2
                                            when 50 then salary*1.3
                                            else salary
                                            end as 新工资
                                            from employees;


                                    作用2：类似于，多重if
                                        mysql中的写法：
                                            case 
                                            when  条件1  then 要显示的值1或语句1
                                            when  条件2  then 要显示的值2或语句2
                                            .....
                                            else  要显示的值n或语句n
                                            end

                                        使用：案例：查询员工的工资的情况
                                                如果工资>20000，显示A级别
                                                如果工资>15000，显示B级别
                                                如果工资>10000，显示C级别
                                                否则，显示D级别
                                            
                                            select salary,
                                            case 
                                            when salary>20000 then 'A'
                                            when salary>15000 then 'B'
                                            when salary>10000 then 'C'
                                            else 'D'
                                            end as 工资级别
                                            from employees;

                                    

                    ##分组函数
                        作用：做统计使用，又被称为统计函数，聚合函数，组函数
                        分类：sum求和，avg平均值，max最大值，min最小值，count计算个数
                        特点：
                            1.参数支持哪些类型
                                sum，avg只处理数值型
                                max，min，count可以处理任何类型
                            2.以上分组函数都忽略null值
                            3.可以和distinct搭配(distinct作用：去重的效果)
                            4.count函数单独介绍
                                一般使用count(*)用作统计行数
                            5.和分组函数一同查询的字段要求是group by后的字段


                        使用：
                            select sum(salary) from employees;
                            select avg(salary) from employees;
                            select max(salary) from employees;
                            select min(salary) from employees;
                            select count(salary) from employees;
                            或者
                            select sum(salary),avg(salary),max(salary),min(salary),count(salary) from employees;

                            3.和distinct搭配使用
                                select sum(distinct salary),sum(salary) from employees;

                            4.count函数的详细介绍
                                select count(*) from employees;用来统计表的行数，机制：只要有一列不为null，则统计行数加1
                                select count(1) from employees;相当于给表的每一个行添加一个1，然后统计表的行数
                            
                            5.和分组函数一同查询的字段有限制
                                select avg(salary),employee_id from employees;
                                分析：分组函数avg查询结果是一行，而employee_id查询结果为107行，表格不一致，没有意义

                        案例：查询出生的天数，用到datediff函数(datediff函数参数支持日期类型，datediff作用：时间日期相减)
                            select datediff(now(),'1993-11-16')


            5.分组查询
                语法：
                    select 分组函数,列(要求出现在group by的后面)
                    from 表
                    [where 筛选条件]
                    group by 分组的列表
                    [order by 子句]

                注意：查询列表比较特殊：要求是分组函数和group by后出现的字段一致
                特点：
                    1.分组查询中的筛选条件分为两类
                                            数据源                  位置                    关键字
                        分组前筛选：        原始表                  group by子句的前面        where
                        分组后筛选：        分组后的结果集           group by子句的后面        having
                    
                    2.group by子句支持单个字段分组，多个字段分组(多个字段之间用逗号隔开没有顺序要求)，表达式或函数(用的较少)
                    3.也可以添加排序(排序放在整个分组查询的最后)

                使用1：group by后面跟字段：意思是按照该字段进行分组查询
                    1.简单的分组查询：
                        案例1：查询每个工种的最高工资
                            select max(salary),job_id from employees group by job_id;
                        案例2：查询每个位置上的部门个数
                            select count(*),location_id from employees group by location_id;
                    
                    2.添加分组前的筛选
                        案例1：查询邮箱中包含a字符的，每个部门的平均工资
                            select avg(salary),department_id 
                            from employees
                            where email like '%a%'
                            group by department_id;

                        案例2：查询有奖金的每个领导手下员工的最高工资
                            select max(salary),manager_id
                            from employees
                            where commission_pct is not null
                            group by manager_id;

                    3.添加分组后的筛选
                        案例1：查询那个部门的员工个数>2
                                思考步骤：
                                    一：查询每个部门的员工个数
                                        select count(*),department_id
                                        from employees
                                        group by department_id;
                                    二：根据一的结果进行筛选，查询那个部门的员工个数>2
                                        select count(*),department_id
                                        from employees
                                        group by department_id
                                        having count(*) > 2;

                        案例2：查询每个工种有奖金的员工的最高工资>12000的工种编号和最高工资
                                思考步骤：
                                    一：查询每个工种有奖金的最高工资
                                        select max(salary),job_id
                                        from employees
                                        where commission_pct is not null
                                        group by job_id;
                                    二：根据一的结果继续筛选，最高工资>12000
                                        select max(salary),job_id
                                        from employees
                                        where commission_pct is not null
                                        group by job_id
                                        having max(salary) > 12000;

                        案例2：查询领导编号>102的每个领导手下的最低工资>5000的领导编号是哪个，以及其最低工资
                                思考步骤：
                                    一：查询每个领导手下的员工固定最低工资
                                        select min(salary),manager_id
                                        from employees
                                        group by manager_id;
                                    二：添加筛选条件：编号>102
                                        select min(salary),manager_id
                                        from employees
                                        where manager_id > 102
                                        group by manager_id;
                                    三：添加筛选条件：最低工资>5000
                                        select min(salary),manager_id
                                        from employees
                                        where manager_id > 102
                                        group by manager_id
                                        having min(salary) > 5000;


                使用2：group by后面跟照表达式或函数：意思是按照表达式或函数分组
                    案例：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5的有哪些
                        思考步骤：
                            一：查询每个长度的员工个数
                                select count(*),length(last_name) len_name
                                from employees
                                group by length(last_name);
                            二：添加筛选条件
                                select count(*),length(last_name) len_name
                                from employees
                                group by length(last_name)
                                having count(*) > 5

                使用3：按多个字段分组
                    案例：查询每个部门每个工种的员工的平均工资
                        select avg(salary),department_id,job_id
                        from employees
                        group by department_id,job_id;

                    案例：添加排序，查询每个部门的每个工种的平均工资，并且按照平均工资的高低显示
                        select avg(salary) a,department_id,job_id
                        from employees
                        where department_id is not null
                        group by department_id,job_id
                        having a > 10000
                        order by a desc;

                

            6.连接查询
                概念：多表查询，当查询的字段来自于多个表时，就会用到连接查询
                分类：
                    按年代分
                        sq192标准：仅仅支持内连接
                        sq199标准(推荐)：支持内连接+外连接(左外和右外)+交叉连接
                    按功能分
                        内连接：
                                等值连接
                                非等值连接
                                    概念：筛选条件是非等于即可，可以理解成与等值连接相反
                                自连接
                                    概念：相当于等值连接，与等值连接的区别是：自己连接自己(A表连接A表)

                        外连接：
                                左外连接
                                右外连接
                                全外连接

                        交叉连接

时间2020/12/10
                
                使用：
                    **sq192标准
                        1.等值连接：
                            特点：
                                >多表等值连接的结果为多表的交集部分
                                >n表连接，至少需要n-1个连接条件
                                >多表的顺序没有要求
                                >一般需要为表起别名
                                >可以搭配前面介绍的所有子句使用：比如：排序，分组，筛选

                            案例：查询员工名和对应的部门名
                                select last_name,department_name
                                from employees,departments
                                where employees.department_id = departments.department_id;

                            案例：查询员工名，工种号， 工种名(涉及知识点：为表起别名)
                                select last_name,e.job_id,job_title
                                from employees e,jobs j
                                where e.job_id = j.job_id

                            案例：查询有奖金的员工名，部门名(涉及知识点：可以添加筛选)
                                select last_name,department_name,commission_pct
                                from employees e,departments d
                                where e.department_id = d.department_id
                                and e.commission_pct is not null;

                            案例：查询每个城市的部门的个数(涉及知识点：分组查询)
                                select count(*) 个数,city
                                from departments d,locations l
                                where d.location_id = l.location_id
                                group by city;

                        2.非等值连接
                        
                        3.自连接
                            案例：查询员工和上级领导的名称
                            select e.employee_id,e.last_name, m.employee_id,m.last_name
                            from employees e ,employees m
                            where e.`manager_id` = m.`employee_id`;

                            解释：自连接虽然是同一张表，此案例：员工信息和领导信息在同一张表上，需要将领导表和员工表虚拟成2张表，进行条件筛选

  
                    **q199标准
                        语法：
                            select 查询列表
                            from 表1 别名 【连接类型】
                            join 表2 别名
                            on 连接条件
                            【where 筛选条件】
                            【group by 分组】
                            【having 筛选条件】
                            【order by 排序列表】

                            连接类型分类：
                                    内连接：inner
                                    外连接：
                                        左外：left 【outer】
                                        右外：right 【outer】
                                        全外：full 【outer】
                                    交叉连接：cross
                                    

                        *内连接：  
                            分类：等值连接，非等值连接，自连接
                            语法：
                                select 查询列表
                                from 表1 别名
                                inner join 表2 别名
                                on 连接条件

                            特点：
                                1.添加排序，分组，筛选
                                2.inner可以省略掉
                                3.筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读
                                4.inner join连接和sql192语法中的等值连接效果是一样的，都是查询多表的交集

                            1.等值连接：
                                案例1：查询员工名，部门名
                                    select last_name,department_name
                                    from employees e
                                    inner join departments d
                                    on e.department_id = d.department_id

                                案例2：查询名字中包含e的员工名和工种名(知识点：添加筛选条件)
                                    select last_name,job_title
                                    from employees e
                                    inner join jobs j
                                    on e.job_id = j.job_id
                                    where e.last_name like '%e%';

                                案例3：查询部门个数>3的城市名和部门个数(知识点：添加分组，筛选)
                                    第一步：查询每个城市的部门个数
                                    第二步：在第一步的结果上筛选满足条件的
                                    select city,count(*) 部门个数
                                    from departments d
                                    inner join locations l
                                    on d.location_id = l.location_id
                                    group by city
                                    having count(*) > 3;

                                案例4：查询员工名，部门名，工种名，并按部门名排序(知识点：添加三表连接)
                                    select last_name,department_name,job_title
                                    from employees e
                                    inner join departments d on e.department_id = d.department_id
                                    inner join jobs j on e.job_id = j.job_id
                                    order by department_name desc;

                                    三表连接机制：第一张表和第二张表连接之后形成的表再和第三张表进行连接


                            2.非等值连接
                                案例：查询员工的工资级别
                                    select salary,grade_level
                                    from employees e
                                    join job_grades g
                                    on e.salary between g.lowest_sal and g.highest_sal;

                            3.自连接
                                案例：查询员工的名字，上级的名字
                                    select e.last_name,m.last_name
                                    from employees e
                                    join employees m
                                    on e.manager_id = m.employee_id;


                        *外连接
                            应用场景：用于查询一个表中有，另一个表没有的记录
                            特点:
                                1.外连接的查询结果为主表中的所有记录
                                    若从表中有和主表中的记录匹配的，则显示匹配的值
                                    若从表中没有和主表中的记录匹配的，则显示null
                                    外连接查询结果=内连接查询结果+主表中有而从表从表中没有与之匹配的记录
                                2.如何定义主表，从表
                                    左外连接，left join左边的是主表
                                    右外连接，right join右边的是主表
                                3.左外和右外交换两个表的顺序，可以实现同样的效果
                                4.全外连接 = 内连接的结果+表1中有但表2没有的+表2中有但表1没有的
                            
                            案例：用左外连接查询男朋友 不在男神表的女神名(右外连接也一样)
                                select b.*,bo.*
                                from beauty b
                                left outer join boys bo
                                on b.boyfriend_id = bo.id;

                            案例：用左外查询哪个部门没有员工(右外连接也一样)
                                select d.*,e.employee_id
                                from departments d
                                left outer join employees e
                                on d.department_id = e.department_id


                        *交叉连接
                            概念：用的笛卡尔成积
                            select b.*,bo.*
                            from beauty b
                            cross join boys bo;
                            和
                            SELECT * FROM beauty,boys;效果是一样的

                        
            7.子查询
                概念：出现在其他语句中的select语句，称为子查询或内查询；外部的查询语句，称为主查询或外查询
                分类：
                    按子查询出现的位置分类：
                                    select后面：
                                            仅仅支持标量子查询
                                    from后面：
                                            支持表子查询        解释：from后面是一张表
                                    where或having后面：【重点】
                                            标量子查询(单行子查询)【重点】  解释：标量子查询概念：查询结果为一行一列
                                            列子查询(多行子查询)【重点】    解释：列子查询概念：查询结果为一列多行
                                            行子查询(用的较少) 解释：行子查询概念：结果集为一行多列或多行多列
                                    exists后面(相关子查询)
                                            
                    按结果集的行列数不同分类：
                                    标量子查询(结果集只有一行一列)
                                    列子查询(结果集只有一列多行)
                                    行子查询(结果集有一行多列)
                                    表子查询(结果集一般为多行多列)

                使用：
                    一：子查询出现的位置：where或having后面
                                标量子查询(单行子查询)【重点】
                                列子查询(多行子查询)【重点】
                                行子查询(用的较少)

                        特点：
                            1.子查询放在小括号内
                            2.子查询一般放在条件的右侧
                            3.标量子查询，一般搭配着单行操作符使用
                                    单行操作符：> < >= = <>
                                列子查询，一般搭配着多行操作符使用
                                    多行操作符： in(等于列表中的任意一个)、any/some、all
                            4.子查询的执行优先于主查询执行，因为：主查询的条件用到了子查询的结果

                        **标量子查询：
                            非法使用标量子查询的情况：标量子查询的结果不是一行一列即为非法标量子查询

                            案例：查询员工的工资比Abel高的员工的信息【标量子查询】
                                select *
                                from employees
                                where salary > (
                                    select salary
                                    from employees
                                    where last_name = 'Abel'
                                );
                                
                            案例：返回job_id与141号员工相同，salary比143号员工多的员工 姓名，job_id和工资【标量子查询】
                                步骤一.查询141号员工的job_id
                                    select job_id from employees where employee_id = 141
                                步骤二.查询143号员工的salary
                                    select salary from employees where employee_id = 143
                                步骤三.查询员工的姓名，job_id和工资，要求job_id=步骤一的结果并且salary>步骤二的结果
                                    select last_name,job_id,salary from employees
                                    where job_id = (
                                        select job_id from employees where employee_id = 141
                                    ) and salary > (
                                        select salary from employees where employee_id = 143
                                    );

                            案例：查询最低工资大于50号部门最低工资的部门id和其最低工资【标量子查询】
                                步骤一：查询50号部门的最低工资
                                    select min(salary) from employees where department_id = 50
                                步骤二：查询每个部门的最低工资
                                    select min(salary),department_id from employees group by department_id
                                步骤三：在步骤二的基础上筛选，满足min(salary) > 步骤一
                                    select min(salary),department_id 
                                    from employees 
                                    group by department_id
                                    having min(salary) > (
                                        select min(salary) 
                                        from employees 
                                        where department_id = 50
                                    );


                        **列子查询(多行子查询：结果集为一列多行)
                            案例：返回location_id是1400或1700的部门中的所有员工姓名
                                步骤一：查询location_id是1400或1700的部门中所有员工姓名
                                    select distinct department_id
                                    from departments
                                    where location_id in (1400,1700)
                                步骤二：查询员工姓名，要求部门编号是步骤一列表中的某一个
                                    select last_name from employees
                                    where department_id in (
                                        select distinct department_id
                                        from departments
                                        where location_id in (1400,1700)
                                    );
                                
                            案例：返回其他部门中比job_id为'IT_PROG'部门所有工资都低的员工  的员工号、姓名、job_id以及salary
                                select last_name,employee_id,job_id,salary
                                from employees
                                where salary < all(
                                    select distinct salary
                                    from employees
                                    where job_id = 'IT_PROG'
                                ) and job_id <> 'IT_PROG';

                                或

                                select last_name,employee_id,job_id,salary
                                from employees
                                where salary <(
                                    select min(salary)
                                    from employees
                                    where job_id = 'IT_PROG'
                                ) and job_id <> 'IT_PROG';

                        **行子查询(行子查询，可以转成标量子查询或列子查询)
                            案例：查询员工编号最小且工资最高的员工信息


                    二：子查询出现的位置：select后面
                        案例：查询每个部门的员工个数
                            select d.*,(
                                select count(*)
                                from employees e
                                where e.department_id = d.department_id
                            ) 个数
                            from departments d;

                    三：子查询出现的位置：from后面
                        特点：将子查询结果充当一张表，要求必须起别名

                        案例：查询每个部门的平均的工资等级
                            步骤一：查询每个部门的平均工资
                                select avg(salary),department_id
                                from employees
                                group by department_id
                            步骤二：连接步骤一的结果集和job_grades表，筛选条件平均工资 between lowest_sal and highest_sal
                                select ag_dep.*,g.grade_level
                                from (
                                    select avg(salary) ag,department_id
                                    from employees
                                    group by department_id
                                ) ag_dep
                                inner join job_grades g
                                on ag_dep.ag between lowest_sal and highest_sal;


                    四：子查询出现的位置：exists后面(相关子查询)(exists可以用in或者not in代替)
                        语法： exists(完整的查询语句)
                        返回结果：1或0

                        案例：查询有员工的部门名
                            select department_name
                            from departments d
                            where exists(
                                select *
                                from employees e
                                where d.department_id = e.department_id
                            );

                        案例：查询没有女朋友的男神信息
                            用in：

                            select bo.*
                            from boys bo
                            where bo.id not in(
                                select boyfriend_id
                                from beautify
                            );

                            用exists：

                            select bo.*
                            from boys bo
                            where not exists(
                                select boyfriend_id
                                from beautify b
                                where bo.id = b.boyfriend_id
                            );
                            

            8.分页查询
                应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求
                语法：
                    select 查询列表                  步骤七
                    from 表                         步骤一
                    【join type】 join 表2          步骤二
                    on 连接条件                     步骤三
                    where 筛选条件                  步骤四
                    group by 分组字段               步骤五
                    having 分组后的筛选             步骤六
                    order by 排序的字段             步骤八
                    limit 【offset,】size;          步骤久

                    offset:指，要显示条目的起始索引(起始索引从0开始)
                    size：指，要显示的条目个数

                特点：
                    1.limit语句放在查询语句的最后，执行顺序也在最后
                    2.公式：
                            要显示的页数page，每页的条目数size
                            select 查询列表
                            from 表
                            limit (page-1)*size,size;
                
                使用：
                    案例：查询前五条员工信息
                        select * from employees limit 0,5;
                        select * from employees limit 5;

                    案例：查询第11条到第25条的员工信息
                        select * from employees limit 10,15;

                    案例：有奖金的员工信息，并且工资较高的前10名显示出来
                        select * from employees
                        where commission_pct is not null
                        order by salary desc
                        limit 10;


            9.union联合查询
                概念：union 联合 合并：将多条查询语句的结果合并成一个结果
                语法：
                    查询语句1
                    union
                    查询语句2
                    .....

                应用场景：要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致
                特点：
                    1.要求多条查询语句的查询个数是一致的
                    2。要求多条查询语句的查询的每一列的类型和顺序最好一致
                    3.union关键字默认去重，如果使用union all可以包含重复项

                使用：
                    案例：查询部门编号>90或邮箱中包含a的员工信息
                        普通写法：
                            select * from employees where email like '%a%' or department_id>90;
                        使用联合查询的写法：
                            select * from employees where email like '%a%'
                            union
                            select * from employees where department_id>90;

                    案例：查询中国用户中男性>12岁的信息，以及外国用户男性>12岁的信息
                        select id,cname,csex from t_ca where csex = '男'
                        union
                        select t_id,tName,tGender from t_ua where tGender = 'male';


时间：2020/12/11

###DML语言
    ***DML语言
        概念：数据库操作语言
            分类：插入：insert
                  修改：update
                  删除：delete
        
        **插入语句
            第一种插入语句的方式：经典的插入语句
                语法：insert into 表明(列名,...) values(值1,...);
                使用：
                    1：插入的值的类型要与列的类型一致或兼容
                        insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id) 
                        value(12,'唐一星','女','1990-4-23','188115522562',null,2);

                    2：不是null的列必须插入值。可以为null的列插入值有两种做法
                        方式一：
                            insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
                            value(13,'唐毅行','女','1990-5-23','18922569988',null,2);
                        
                        方式二：可以为null的列，不写列名的同时，省去赋值
                            insert into beauty(id,name,sex,phone)
                            value(15,'娜扎','女','18818899888');
                    
                    3：列的顺序可以调换，同时赋值的顺序要和列的顺序一致
                        insert into beauty(name,sex,id,phone)
                        value('姜星','女',16,'110');

                    4：列数和指的个数必须一致
                        insert into beauty(name,sex,id,phone)
                        value('关晓彤','女',17,'119');

                    5：可以省略列名，默认所有列，而且列的顺序和表中的列的顺序一致,为null的部门也必须写出来
                        insert into beauty value(18,'张飞','男',null,'119',null,null)

            第二种插入语句的方式：
                语法：insert into 表明 set 列名=值,列名=值,....
                使用：
                    1.insert into beauty set id=19,name='刘涛',phone='999';

            插入语句第一种方式和第二方式对比：
                方式一支持插入多行，方式二不支持
                    insert into beauty value(18,'张飞','男',null,'119',null,null),
                    (19,'张飞1','男',null,'119',null,null),
                    (20,'张飞2','男',null,'119',null,null);

                方式一支持子查询，方式二不支持(将宋茜的信息通过子查询的方式，添加到beauty表中)
                    insert into beauty(id,name,phone)
                    select 26,'宋茜','1866666666';
                       

        **修改语句
            1.修改语句的第一种方式：修改单表的记录
                语法：
                    update 表明                 步骤一
                    set 列=新值,列=新值          步骤三
                    where 筛选条件              步骤二

                使用：
                    事例：修改beauty表中姓唐的女生的电话为166555996688
                        update beauty set phone = '166555996688' 
                        where name like '%唐%';
                    
                    事例：修改boys表中id为2的名字为张飞，魅力值10
                        update boys set boyname='张飞',usercp=10
                        where id = 2;

            2.修改语句的第一种方式：修改多表的记录
                语法：
                    sq99语法：
                    update 表1 别名
                    inner|left|right join 表2 别名
                    on 连接条件
                    set 列=值
                    where 筛选条件

                使用：
                    事例：修改张无忌的女朋友的手机号的为114
                        update boys bo
                        inner join beauty b on bo.id=b.boyfriend_id
                        set b.phone='114'
                        where bo.boyname='张无忌';
                    
                    事例：修改没有男朋友的女生的男朋友的编号都为2号
                        update boys bo
                        right join beauty b on bo.id=b.boyfriend_id
                        set b.boyfriend_id=2
                        where bo.id is null;

        **删除语句
            1.删除语句的第一种方式：使用delete关键字删除
                
                使用：
                    1.单表的删除
                        语法：delete from 表名 筛选条件
                        事例：删除手机号以9结尾的女神信息
                            delete from beauty where phone like '%9';

                    2.多表的删除
                        语法：
                            sq99语法：
                            delete 表1的别名，表2的别名
                            from 表1 别名
                            inner|left|right join 表2 别名 on 连接条件
                            where 筛选条件

                        事例：删除张无忌的女朋友的信息(两表连接，删除其中的1张表的记录)
                            delete bo
                            from beauty bo
                            inner join boys b on bo.id=b.boyfriend_id
                            where b.boyname = '张无忌';

                        事例：删除黄晓明以及他女朋友的信息(两表连接，删除其中的2张表的记录)
                            delete b,bo
                            from beauty bo
                            inner join boys b on bo.id=b.boyfriend_id
                            where b.boyname = '黄晓明';
            
            2.删除语句的第二种方式：使用truncate关键字删除
                语法：truncate table 表名

            3.delete和truncate两种方式对比
                1。delete可以加where条件，truncate不能加
                2.truncate删除，效率高一丢丢
                3.假如要删除的表中有自增长列，如果用delete删除后，再插入数据，自增长列的值从断点开始，而truncate删除后，再插入数据，自增长列的值从1开始
                4.truncate删除没有返回值（提示信息）；delete删有返回值（提示信息）
                5.truncate删除不能回滚，delete删除可以回滚


###DDL语言
    概念：数据定义语言，库和表的管理
    ***库的管理
        内容：创建(create)，修改(alter)，删除(drop)
        
        使用：
            1.库的创建
                语法：create database [if not exists]库名
                事例：创建books库
                    create database if not exists books;
                    if not exists作用：当books库存在则不创建，当books库不存在则创建
            2。库的修改
                目前库名的修改，在本地把相应的文件夹手动修改即可，使用语句修改则库会丢失数据
                修改库的字符集：
                    alter database books character set gbk;
            
            3.库的删除
                语法：drop database [if exists] 库名


    ***表的管理
        内容：创建(create)，修改(alter)，删除(drop)
        使用：
            1.创建表
                语法：
                    create table [if exists]表名(
                        列名 列的类型【(长度) 约束】,
                        列名 列的类型【(长度) 约束】,
                        列名 列的类型【(长度) 约束】,
                        ......
                        列名 列的类型【(长度) 约束】
                    )
                事例：创建表book
                    create table book(
                        id int,
                        bname varchar(20),
                        price double,
                        authorId int,
                        publishDate datetime
                    )

            2.修改表
                语法：alter table 表名 add|drop|modify|change column 列名 【列类型 约束】
                内容:
                    1.修改列名
                        alter table 表名 change column 旧列名 新列名 新列名类型
                        
                    2.修改列的类型或约束
                        alter table 表名 modify column 列名 列类型

                    3.添加新列
                        alter table 表名 add column 列名 类型

                    4.删除列
                        alter table 表名 drop column 列名

                    5.修改表名
                        alter table 表名 rename to 新表名

                使用：
                    事例:修改列名
                        alter table book change column publishdate pubdate datetime;
                    事例：修改列的类型或约束
                        alter table book modify column publishdate timestamp;
                    事例：添加新列
                        alter table author add column annual double;
                    事例：删除列
                        alter table author drop column annual;
                    事例：修改表名
                        alter table author rename to book_author;
                    
            3.删除表
                语法：drop table [if exists]表名
            
            4.表的复制
                1.仅仅复制表的结构
                    create table 复制生成的表 like 旧表

                    事例：create table copy like author

                2.复制表的结构+数据
                    create table 复制生成的表
                    select * from 旧表

                    事例：create table copy2 selete * from author

                3.只复制部分数据
                    create table 复制生成的表
                    select 列名,列名
                    from 旧表
                    where 条件

                    事例：
                        create table copy3 
                        select id,au_name from author
                        where nation='中国'

                4.仅仅复制某些结构
                    create table 复制生成的表
                    select id,au_name from 旧表
                    where 0;
                    


    ***常见的数据类型
        **数值型
            *整型
                分类：
                        tinyint、 smallint、mediumint、int/integer、bigint
                占字节数:   1         2          3           4          8
                特点：
                    1.若不设置无符号还是有符号，默认是有符号(即可以是正数也可以是负数),若想设置无符号(即只能是正数),则需要添加unsigned关键字
                    2.若插入的数值超出了整型的范围，会报out of range异常，自动插入临界值
                    3.若不设置长度，会有默认的长度
                        长度的解释：长度代表了显示的最大宽度，若不够会用0在左侧填充，但必须搭配zerofill使用

            *小数：
                分类：
                    浮点型
                        float(M,D)
                        double(M,D)
                    定点型
                        dec(M,D)
                        decimal(M,D)

                解释：
                    1.
                        M：整数+小数部分的长度
                        D：小数部分的长度
                        如果超出范围，则插入临界值
                    
                    2.
                        M和D都可以省略，如果是decimal,则M默认是10，D默认是0；
                        若果是float和double，则会根据插入的数值的精度来决定精度
                    
                    3.定点型的精确度高，如果要求插入数值的精度较高如货币运算等则考虑使用定点型


        **字符型
            较短的文本：char,varchar
                char(M)
                varchar(M)
                其他的字符类型:
                    binary和varbinary用于保存较短的二进制
                    enum用于保存枚举
                    set用于保存集合

                    事例：enum类型的例子
                        create table tab_char(
                            c1 enum('a','b','c');
                        )
                        表tab_char只能选择a,b,c里面的一个值插入
                    
                    事例：set类型的例子
                        create table tab_set(
                            c1 enum('a','b','c');
                        )
                        表tab_set插入一个值，也可以插入多个值
                        insert into tab_set value('a');
                        insert into tab_set value('a','b');


                char和varchar对比：
                               写法            M的意思                          特点             空间的消耗    效率
                    char      char(M)       最大的字符数，可以省略，默认为1      固定长度字符       比较消耗      高
                    varchar   varchar(M)    最大的字符数，不可以省略             可变长度的字符     比较节省      低

            较长的文本：text,blob(较长的二进制数据)

        **日期型
            分类：
                date只保存日期
                time只保存时间
                year只保存年

                datetime保存日期+时间
                timestamp保存日期+时间

                datetime和timestamp对比：
                                        字节        范围           时区等影响
                        datetime         8          1000-9999      不受
                        timestamp        4          1970-2038      受

时间2020/12/14

        **常见约束
            概念：一种限制，用于限制表中的数据，为了保证表中的数据的准确和可靠性
            语法：
                create table 表名(
                    字段名 字段类型 约束
                )
            分类：六大约束
                    NOT NULL：非空，用于保证该字段的值不能为空
                        如：姓名，学号等
                    
                    DEFAULT:默认，用于保证该字段有默认值
                        如：性别
                    
                    PRIMARY KEY：主键，用于保证该字段的值具有唯一性，并且非空
                        如：学号，员工编号
                    
                    UNIQUE：唯一，用于保证该字段的值具有唯一性，可以为空
                        如：座位号

                    CHECK：检查约束【mysql中不支持,语法支持,但是没效果,orcal里面有效果】

                    FOREIGN KEY：外键，用于限制两个表的关系，用于保证该表字段的值，必须来自于主表的关联列的值
                        在从表添加外键约束，用于引用主表中某列的值
                        如：学生的专业编号，员工表的部门编号，员工表的工种编号

            1.添加约束的时机：
                1.创建表时
                2.修改表时

            2.约束的添加分类：
                1.列级约束
                    概念：创建表的时候添加的约束
                        如：
                            create table 表名(
                                字段名 字段类型 列级约束
                            )
                    
                    注意点:对于列级约束而言六大约束语法上都支持，但外键约束没有效果

                    使用：
                        事例：创建表，并添加列级约束
                            create table stuinfo(
                                id int primary key,#主键
                                stuName varchar(20) not null,#非空
                                gender char(1) check(gender='男' or gender='女'),#检查
                                seat int unique,#唯一
                                age int default 18,#默认约束
                                majorId int references major(id) #外键
                                    #设置外键的机制：为majorid设置外键，关联到major表中的id
                            )

                        用法总结：直接在字段名和字段类型后面追加，约束类型即可
                                  只支持：默认，非空，主键，唯一

                2.表级约束
                    概念：表级约束指，创建表的时候没有添加约束，而是在字段名最后一行添加的约束，没有指明对具体哪个字段进行的约束
                        如：
                            create table 表名(
                                字段名 字段类型,
                                表级约束
                            )
                    
                    注意点：对于表级约束而言六大约束，除了非空，默认，其他的都支持
                    语法：在各个字段的最下面添加
                            【constraint 约束名】 约束类型(字段名)

                    使用：
                        事例：创建表，添加表级约束
                            create table stuinfo(
                                id int,
                                stuname varchar(20),
                                gender char(1),
                                seat int,
                                age int,
                                majorid int,

                                constraint pk primary key(id),#给id字段添加主键
                                constraint uq unique(seat),#给seat字段添加唯一键
                                constraint ck check(gender='男' or gender='女'),#检查
                                constraint fk_stuinfo_major foreign key(majorid) references major(id),#外键
                                    #设置外键的机制：为majorid设置外键，关联到major表中的id
                            );
                    
                总结：表级约束结合列级约束的通用的写法：
                        因为，外键约束对于列级约束没有效果，所以外键约束用表级约束，其他约束用列级约束
                        create table if not exists stuinfo(
                            id int primary key,#主键
                            stuName varchar(20) not null,#非空
                            sex char(1),
                            age int default 18,
                            seat int unique,
                            majorid int,
                            constraint fk_stuinfo_major foreign key(majorid) references major(id)
                                #设置外键的机制：为majorid设置外键，关联到major表中的id
                        )   

                主键和唯一键的对比：
                                是否保证唯一性     是否允许为空    一个表中可以有多个     是否允许组合
                    主键           是                不允许            至少有1个           允许,但不推荐    
                    唯一键         是                允许              可以有多个          允许,但不推荐

                    说明：组合的含义

                外键的特点：
                    1.要求在从表设置外键关系
                    2.从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
                    3.主表的关联列必须是一个key(一般是主键或唯一键)
                    4.插入数据时，先插入主表，再插入从表；删除数据时，先删除从表，再删除主表

                修改表时添加约束
                    1.添加非空约束
                        alter table stuinfo modify column stuname varchar(20) not null;
                    2.添加默认约束
                        alter table stuinfo modify column age int default 18;
                    3.添加主键
                        1.列级约束方式
                            alter table stuinfo modify column id int primary key;
                        2.表级约束方式
                            alter table stuinfo add primary key(id);
                    4.添加唯一键
                        1.列级约束
                            alter table stuinfo modify column seat int unique;
                        2.表级约束
                            alter table stuinfo add unique(seat);
                    5.添加外键
                        alter table stuinfo add [constraint fk_stuinfo_major] foreign key(majorid) references major(id);
                    
                    总结：
                        1.添加列级约束
                            alter table 表名 modify column 字段名 字段类型 新约束
                        2.添加表级约束
                            alter table 表名 add  约束类型(字段名) 

                修改表时删除约束
                    1.删除非空约束
                        alter table stuinfo  modify column stuname varchar(20) null;
                    2.删除默认约束
                        alter table stuinfo  modify column age int;
                    3.删除主键
                        alter table stuinfo drop primary key;
                    4.删除唯一
                        alter table stuinfo drop index seat;
                        






                    
                    





