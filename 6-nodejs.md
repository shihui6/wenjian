***环境变量
    1-环境变量的作用：可以在任何目录运行文件(一般情况下是在文件所在的目录中运行文件才会有效，在该目录中运行其他目录的文件是不可行的)
    2-设置环境变量：
        2-1.控制面板我的电脑属性->高级系统设置->点开环境变量->找到path->将需要添加到环境变量的文件目录路径添加到path中的环境变量中(文件所在的目录添加就行)  
        2-2.注意修改了环境变量之后需要重新启动cmd





***REPL(cmd中输入node启动，就是所谓的REPL) 也是执行js代码的方式
    REPL的启动：
        1-打开cmd，
        2-在cmd中输入node，
        3-就进入REPL环境，REPL就类似于浏览器中的控制台
        4-我们可以在REPL环境中直接输入js代码进行执行

    理解：nodejs提供的REPL就类似于浏览器的控制台可以在里面直接输入代码执行
    R:read 读取输入的代码
    E:evaluate 执行代码
    P:print 执行完代码之后，打印出结果
    L:loop 循环 继续从第一轮开始 读 执行 打印





***nodejs执行js代码的第二种方式：使用nodejs执行指定js的文件
    命名规则：不要使用node.js作为命名
             不要使用中文
             不要使用大写的字母
             不要使用特殊字符
    怎么做：
        1.打开cmd到当前需要执行js文件目录
        2.输入node js文件路径





***node.js中书写js代码和浏览器中书写js代码的区别
    1-浏览器中的js组成
        1.ECMAScript
        2.DOM  webapi
        3.BOM  webapi
    2-Node.js中的组成
        1.ECMAScript
        2.Node.jsAPI
    意思是nodejs中无法使用webapi提供的功能 如：document window





***node.js写文件的操作(也是node.js异步方式写文件)
    具体操作：
    1-需要将fs引入
    2-调用写文件的方法fs.writeFile(文件的路径，要写入文件的数据，编码格式，回调函数)
```js
    var str = '你好，欢迎来到我的家乡'    
    var fs = require('fs')          引入fs
    fs.writeFile('./home.txt',str,"utf8",function(err){   
        if(err){
            console.log(err)
        }else{
            console.log("写入成功")
        }
    })
    执行结果：
    使用node执行这个文件的时候，会在目录中生成home.txt文件里面有一段str内容
``` 





***node.js读文件操作 将文件的内容读取到js当中 (也是node.js异步方式读文件)
    具体操作：
    1-引入fs模块
    2-读文件，fs.readFile(文件路径，编码格式，回调函数)
```js
    var fs = require('fs')
    fs.readFile('./home.txt',"utf8",function(err,data){   
                            // 根写入文件的回调函数不同，读取文件有俩个参数：参数一：err，参数二：data
                            // 读取文件的时候，不传递utf8编码格式，则最终获取到的数据是Buffer对象(需要调用toString才可以转成字符串)
                            // Buffer对象是什么呢？是16进制的数据表示形式，他是一个对象
        if(err){
            console.log("读取文件失败"，err)
        }else{
            console.log("读取文件成功",data)
        }
    })
```





***同步读取和写入文件的操作(只需要了解即可)
    上面俩个是异步读取和写入文件的操作，接下来是同步读取和写入文件的操作
```js
    var fs = require("fs")
    读取操作
    var str = fs.readFileSync('./home.txt',"utf8")
    console.log(str)
    写入操作
    fs.writeFileSync('./home.txt','我爱我家乡','utf8')
```





***关于文件路径的研究(下面的俩种方法可直接使用，因为是node.js里面直接提供的)
    需求：每次写入文件操作时需要写绝对路径才能把最终生成的文件写入当前文件夹，由于绝对路径书写过于麻烦，所以node.js中提供了一个代表当前文件所在的目录路径的一个变量 
        __dirname 指：当前执行的js文件所在的目录的绝对路径
        __filename 指：当前执行的js文件的绝对路径





***关于路径的拼接 node.js中提供了关于路径拼接的一个方法path.join()  
    原因：在很多情况下，我们需要进行路径的拼接操作，最常见是进行字符串的拼接(用的加号)，其实这样子是比较麻烦的，如果涉及到多文件嵌套就更加麻烦
    用法：
        首先引入require path方法
        1- __dirname + '/a' + '/b' + '/c' + '/home.txt'
        2- path.join(__dirname,'a','b','c','home.txt')
        效果：2方法和1方法到达的效果是一样的，很明显2方法更加的方面书写





***什么时候使用require什么时候不需要使用require
    global对象在nodejs里是类似于window的全局对象
    判断依据：只要在global对象中的方法属性就不需要require，不在global对象中的方法属性需要用require引入





***使用nodejs创建一个简单的服务器应用  (web开发的本质：请求，处理，响应)
    <!-- 第一步基本的概念： -->
        浏览器向服务器发送请求的时候从开始发送到处理响应流程：
                1-浏览器访问8080端口
                2-服务器正好在监听8080，然后服务器就接应上了代表已经来到了指定的端口服务器里面就是下面的第3步
                3-就触发下面的第4步，触发服务器注册的事件request然后响应数据，这里记住一定要有结束响应的过程不然浏览器不知道服务器响应结束会一直请求
    用法：
```js
    // 1.引入一个http模块
    let http = require('http')
    // 2.创建一个http服务器实例
    let server = http.createServer()
    // 3.调用服务器实例的listen方法监听指定的端口 (监听：听着这个端口，如果有消息来就可以听到了)
    server.listen(8080,function(){
        console.log('服务器已经启动了')
    })
    // 4.给服务器注册一个事件request,当服务器监听指定端口的时候，如果有请求到达服务器，request事件就会被触发
    server.on('request',function(request,response){
        在接收到浏览器请求之后，我们需要向浏览器响应内容
        response.write("hello node")
        在向浏览器响应完数据之后，需要告诉浏览器，响应内容结束 用response.end就可以结束响应 
        因为服务器想浏览器返回数据的时候是分片段的，就是一段一段向浏览器发送数据，不告诉浏览器结束响应的话会一直响应
        response.end()
    })
```
    第二个需求根据用户请求的地址不同返回不同的内容 (跟上面的操作比就是第4步不一样)
```js
    server.on('request',function(request,response){
        需求：如果用户请求的是 localhost:8080/index 返回字符串 this is home page
             如果用户请求的是  localhost:8080/login 返回字符串 this is login page
        // 如何知道用户请求的是哪个地址 request.url()
        if(request.url == '/index'){
            response.write('this is home page')
        }else if(resquest.url == '/login'){
            response.write('this is login page')
        }else{
            response.write('not found')
        }
    })
```
    第三个需求：当前用户请求的时候返回index.html文件内容
```js
    server.on('request',function(request,response){
        // 读取文件
        fs.readFile(path.join(__dirname,'index.html'),function(err,data){
            if(err){
                response.write('file not found')
            }else{
                response.write(data)
            }
            response.end()
        })
    })
```



***使用nodejs模拟phpstudy(apache)服务器
```js
    server.on('request',function(request,response){
        request.url = request.url == '/'?'/index.html':request.url
            如果request.url为/的时候页面访问不了内容 所以要是用户访问的是/那么直接让其访问/index.html也就是默认处理
        fs.readFile(path.join(__dirname,'public',request.url),function(err,data){
                读取文件的时候不要加字符编码了，如果加了图片就不能正常处理了，如果不加字符编码response.write会自动给html css等文件正常处理，图片也可以正常加载
            if(err){
                response.write('file not found')
            }else{
                response.write(data)
            }
            response.end()
        })
    })
```
    ****Content-Type作用：Content-Type其实就是服务器在告诉浏览器，响应回来的文件是什么类型，浏览器会根据这个类型，对文件进行不同的处理
                        如：浏览器请求回来的js文件，浏览器会使用js引擎解析执行js代码
                           如果浏览器请求回来的是图片，一般都是用图片的渲染方式将图片展示出来
        Content-Type重要性：如果在响应头中没有Content-Type，那么浏览器很有可能就不知道要怎么处理这个文件，
                            在谷歌浏览器中没有加Content-Type，但是内容能够显示出来是因为谷歌浏览器会根据文件后缀猜测需要用什么渲染方式
                            比如没有加Conten-Type的css文件，在ie浏览器中就没有效果
        nodejs中设置响应头的方式：response.setHeader('键','值')
        用法：(这种设置防范用的少)
```js
        server.on('request',function(request,response){
            // response.setHeader()同时也可以调用多次
            response.setHeader('Content-Type','text/html;charset=utf8')
            // setheader方法必须在write方法之前你调用
            response.write('你好世界')
            response.end()
        })
```
        第二个重点:(这种方法用的多)
        nodejs中一次性设置多个响应头的方式，response.writeHead()类似于response.setHeader()设置多个的效果
        用法：
```js
        server.on('request',function(request,response){
            // response.writeHead(状态码，状态信息，响应头对象)
            response.writeHead(200,'OK',{
                'Content-Type':'text/html;charset=utf8',
                name:'shihui',
                age:12
            })
            response.write('你好')
            response.end()
        })
```     
    ****mime
        概念：多用途互联网邮件扩展类型
        作用：可以根据不同的文件类型，获取到不容的Content-Type,就可以避免自己写大量的if语句判断文件类型使用不同的Content-Type
        用法：mime.getType('index.css') 返回内容为text/csse
```js
        server.on('request',function(request,response){
            // request对象分析
            // request 所有和请求相关的信息，都可以从request对象中获取 
            //     url  请求的地址
            //     headers   请求头中的内容都以对象的形式保存在header属性中，我们可以用来获取浏览器信息，cookie信息等等
            //     method  获取请求的方式

            // response对象分析  
                    // response.write()  给浏览器响应数据，参数可以是字符串，也可以是buffer对象
                    // response.end()  结束响应的，也可以传递参数给浏览器响应数据，参数可以是字符串，也可以是buffer对象
                    // response.setHeader()  设置响应头，一次只能设置一条，必须在write之前调用
                    // response.statusCode  如 response.statusCode = 400
                    // response.statusMessage 如 response.statusMessage = 'Not Found'

            fs.readFile(path.join(__dirname,'public',request.url),function(err,data){
                if(err){
                    response.write('file not found')
                }else{
                    response.setHeader('Content-Type',mime.getType(request.url))
                    response.write(data)
                }
            })
        })
```



***nrm的使用
    作用：因为npm服务器是在国外的，那么通过网络访问国外的服务器，下载速度会比较慢，为了解决下载速度慢的问题，我们使用nrm
    用法：
        nrm安装：npm install nrm -g
        展示所有的可用服务器： nrm ls
        切换到指定的服务器： nrm use 服务器名称
        测试服务器的延迟： nrm test 服务器名称
        删除服务器：nrm del 服务器名称
        例子：切换到相应的淘宝服务器之后 执行 npm命令自动使用的是淘宝的服务器



***express
    概念：express基于nodejs平台，快速，开放，极简的web开发框架
    用法：
```js
        // 1-引入express模块
        var express = require('express')
        // 2-创建express实例
        var app = express()
        // 3-使用app实例监听指定的端口
        app.listen(8080,function(){
            console.log('http://localhost:8080')
        })
        // 4-注册路由
                注册路由的方式有很多  用的最多的方式为第三种方式
                    第一种方式：使用app.METHOD来注册路由，使用请求方式作为方法名
                        app.get(路由路径，function(req,res){})
                        app.post(路由路径，function(req,res){})
                        app.delete(路由路径，function(req,res){})
                        特点：1.使用该方式注册的路由，必须使用指定的请求方式（注册路由的方法名）进行请求，否则请求不到
                             2.使用该方式注册路由，请求的路径必须和注册的路径完全匹配，如：app.get('/login',function(req,res){}) 请求地址输入localhost:8080/login/txt是请求不到的
                    第二种方式：使用app.all来注册路由
                        app.all(路由路径,function(req,res){})
                        特点：1.使用该方式注册的路由，所有的请求方式都能够请求到
                             2.使用该方式注册的路由，请求的路径必须和注册的路径完全匹配 如：app.all('/login',function(req,res){}) 请求地址输入localhost:8080/login/txt是请求不到的
                    第三种方式：使用app.use来注册路由
                        app.use(路由路径,function(req,res){})
                        特点：1.使用该方式注册的路由，所有的请求方式都能够请求到
                             2.使用该方式注册的路由，请求的路径只要是以注册的路径开头就可以访问到 如：app.use('/login',function(req,res){}) 请求地址输入localhost:8080/login/txt可以请求到
                             3.app.use(function(req.res){}) 和 app.use('/',function(req,res){}) 代表的意思是一样的

                express中request对象中的属性和方法
                    app.get('/index',function(req,res){
                        req.path 指url地址中的路径
                        req.query 指get请求参数对象
                        res.send
                        res.json
                        res.jsonp jsonp格式的数据就是一个函数调用里面的参数，这个参数实际事json数据
                        res.redirect 跳转页面 res.redirect('http://baidu.com')
                        res.sendFile(文件路径) 将文件内容读取到之后返回给浏览器 并且自动设置Content-Type
                    })
```
        ****express结合art-template使用
            用法：1.要注册一个模板引擎告诉app使用的是哪个模板引擎 通过app.engine方法注册一个模板引擎 app.engine('模板文件的后缀名',模板引擎提供的处理程序)
                 2.调用res.render()方法进行模板渲染
```js
        app.engine('html',require('expreess-art-template'))
        app.get('/',function(req,res){
            var obj = {
                name :"石惠"，
                age:12
            }
            res.render('index.html',obj)
            注意点：当调用res.render方法进行渲染的时候，express会自动调用根目录下面的views文件夹中查找文件名对应的文件，如果文件的名字后缀为.html express会自动调用我们之前通过app.engine注册的模板引擎处理程序来进行模板渲染
        })
```



***mongodb
    概念：非关系型数据库(就是表和表之间是没有关联的)
    特点：非关系型数据库一般都是内存型数据库
            什么是内存型数据库呢？ 内存型数据库指的就是数据库保存在内存中的数据库
            mysql和mongodb的区别：
                mysql：关系型数据库，数据保存在硬盘中
                mongodb：非关系型数据库，数据暂存在内存中，最终会保存到硬盘中进行永久存储
    验证mongodb安装：在cmd命令行中输入 mongo --version
    ****mongodb使用说明
        在mongodb中术语有所变化：
                                之前的 表 在mongodb中叫 集合
                                之前的 记录 在mongodb中叫 文档
                                之前的 字段 在mongodb中叫 列
        1-启动mongodb数据库：  
            在cmd中 mongod --dbpath ./data 意思是给mongodb提供一个数据路径，让他把数据存到哪，所以要给mongodb创建一个数据文件夹：在系统的任意位置创建一个data文件夹，在文件夹中创建一个db文件夹，创建了文件夹之后才能启动命令  mongod --dbpath ./data (data文件夹创建的目录和启动命令时cmd所在的目录要一致)
        2-启动数据库的数据软件
            作用：操作数据库的，就像之前mysql数据库需要navicat一样
            打开cmd 输入mongo (任何目录下都可以)
        3-通过mongodb数据库操作软件展示所有数据库以及切换数据库
            3-1:查看mongodb中所有的数据库  show databases;
            3-2:切换到指定的数据库(进行增删改查操作必须要在相应的数据库中) use 数据库名称;
        4-数据库的增删改查操作
            1-注意点：在mongodb中不需要提前把数据库和表创建出来，直接use使用就行
                    需求：创建一个alibaixiu数据库出来(且原来没有alibaixiu数据库)
                         use alibaixiu;
                         继续执行show databases;时依然是没有alibaixiu这个集合的 要在alibaixiu中添加数据之后，展示表格的时候才会显示alibaixiu
            2-db的说明：db就是当前正在使用的数据库的对象，可以通过这个对象来给当前数据库进行增删改查操作(use 数据库 切换到相应到集合时候才能使用db)
            3-查看所有的集合(表)  show collections   
            4-(增)给指定的集合插入数据
                db.集合名称.insert(对象)
                db.集合名称.insertOne(对象)
                db.集合名称.insertMany(数组)
                        注意点：当我们给数据库中某个指定的集合插入数据的时候，如果这个集合不存在，则会自动创建这个集合，如果数据库不存在，则会自动创建这个数据库
            5-(查)查询集合中所有的数据
                db.集合名称.find() 如果find方法不传递任何参数，就是查找是所有的数据
                如果要查询特定的数据，则需要给find方法传递参数
                    db.集合名称.find(查询条件) 查询条件是一个对象，对象的属性名就是要找的数据的字段的名称，属性的值就是对应的要查找的字段的值
                    需求：查找name是李明达的数据
                        db.集合名称.find({name:'李明达'})
                    需求：要查找年龄大于18的人
                        db.集合名称.find({age:{$gt:18}})
                        扩展：$gt大于
                              $lt小于
                              $eq等于
                              $gte大于等于
                              $lte小于等于
                              $ne不等于
                              $in在某个数组中
                              $nin不在某个数组中
            6-(删)删除数据
                db.集合名称.deleteOne(条件对象)  只能删除一条，哪怕条件可以找到多条数据，就只能删除一条
                db.集合名称.deleteMany(条件对象)
            7-(改)修改数组
                db.集合名称.updateOne(条件对象，修改之后的对象)
                    如：db.users.updateOne({age:18},{$set:{name:'石惠'}})
                db.集合名称.updateMany(条件对象，修改之后的对象)
                    需求：将所有age大于17的人的gender改成female   db.users.updateMany({age:{$gt:17}},{$set:{gender:female}})




***通过nodejs实现对mongodb的增删改查操作
        需要借助mongodb官方提供的node包  mongodb 下载 npm i mongodb
```js
        1-引入mongoClient 
            var MongoClient = require('mongodb').MongoClient
        2-创建连接字符串   localhost代表mongodb服务器在自己电脑上  27017代表 启动mongodb的时候告诉我们监听的是27017端口
            var connStr = 'mongodb://localhost:27017' 
        3-调用mongoClient的connect方法连接数据库
            MongoClient.connect(connStr,function(err,client){
                4-获取数据库对象
                var db = client.db('alibaixiu')
                5-获取要操作的集合
                var users = db.collection('users')
                6-进行增删改查操作
                    6-1 查操作
                        需求一：查集合中的所有数据
                        users.find({}).toArray(function(err,data){    后面跟的回调函数是异步操作的
                            console.log(data)  返回要查的数据
                        }) 
                        需求二：查集合指定数据
                        users.find({age:{$gt:18}}).toArray(function(err,data){  
                            console.log(data)  返回要查的数据
                        })  
                    6-2 增操作
                        users.insertOne({name:'石惠'，age:12},function(err,data){
                            console.log(data.result)  查看查数据是否成功
                        })
                    6-3 删除操作
                        users.deleteMany({name:{$in:['yyy','zzz']}},function(err,data){
                            console.log(data.result)
                        })
                    6-3 改操作
                        users.updataMany({},{$set:{job:'开发工程师'}},function(err,data){
                            console.log(data.result)
                        })
                7-关闭数据库连接
                client.close()
            })
```            
    ****获取指定的id对应的新闻数据(因为mongodb中的id比较特殊)
            在使用id进行查询的时候，需要将字符串id转换成mongodb中国的id格式
            使用ObjectID函数，就可以进行转换
            ObjectID函数是mongodb提供的，需要被引入
```js
        var ObjectId = require('mongodb').ObjectID
        users.find({_id:ObjectID("5b3b4m4n3n2n4n1029")}).toArray(function(err,data){
            console.log(data)
        })
```




***nodejs操作mysql数据库
    1-下载mysql npm install mysql
    2-连接mysql数据库选项的设置
    3-开启数据的链接
    注意点：connection.end()尽量不要用，关闭数据库之后再执行就会报错，跟上面的mongodbs数据的不一样
```js
    const mysql = require("mysql")
    // 进行数据路选项设置
    const option = {        option参数可以右键查到
        host:"localhost",
        user:"root",        
        password:"root"     数据库密码
        database:'letao'    数据库里面的表明
    }
    // 连接数据库
    var connection = mysql.createConnection(option)  
    // 开启  connection.connect()
    connection.connect()   
    // connection中的query执行查询语句并且返回结果
    connection.query("select * from product",function(err,results,fields){
        console.log(results)
    })
    

```



***使用nodejs编写命令行工具
    需要给全局提供一个命令，来调用当前文件中的代码
    要写命令行工具的步骤如下：
        1.先编写js文件
        2.在package.json文件中添加一个bin属性
        3.bin属性是一个对象，对象中的属性名就是 最终要使用的全局命令的名字
        4.对象中的属性值，最终在执行命令的时候，所要执行的js文件的路径
        5.在js文件最上面添加一个 #!node 是告诉系统需要使用node执行此文件
        6.将这个包上传到npm服务器
        7.进行全局安装已经上传的包
        8.在命令行中就可以使用刚才的命令行工具了
```js
    // 第一步创建js文件
    #!node   告诉系统使用node执行该文件
    var fs = require("fs")
    fs.mkdir('./shihuis',function(err){
        console.log('创建成功')
        fs.mkdir('./shihuis/shihui1',function(err){
                console.log('创建成功')
        })
        fs.mkdir('./shihuis/shihui2',function(err){
                console.log('创建成功')
        })
        fs.mkdir('./shihuis/shihui3',function(err){
                console.log('创建成功')
        })
    })

    // 第二步在package.json中添加一个bin属性且其是一个对象，对象中的属性名就是要执行的命令的名字
    {
        "bin":{
            "shihuicf":"./index.js"   全局安装之后，在命令行中执行shihuicf命令就会调用 index.js文件
        }
    }

    // 第三步，将整个文件夹上传到npm服务器
    nrm use npm
    npm login 登录自己的npm账号
    npm publish  上传文件
    npm install 文件夹名 -g     全局安装就行了
    shihuicf   执行package.json中bin的命令就可以了

```



***计算机知识点
    计算机系统：
        软件系统：应用软件：如qq，chrome
                 操作系统：
                    pc端操作系统：windows,unix,linux,OS
                    移动端操作系统：塞班，android,ios,windowsPhone
        硬件系统：CPU:central processing unit(中央处理单元) 所有的计算机程序都要经过CPU处理之后才能够跑起来的，所以说他是电脑的核心
                 ROM：Read Only Memmory(硬盘)    所有的软件(qq,wechat)安装都是装在硬盘里面
                 RAM：Random Access Memmory(内存)
                 主板：作用：将上面的硬件插在上面运行的
                 输入输出设备
        实际运用：一个软件双击到运行这个中间经历了什么过程？(因为CPU处理的速度是非常快的，大概每秒20亿次，如果CPU直接从硬盘里面拿数据会受到硬盘转速的制约，会一直等着)
                    1-先在硬盘里面把数据读到内存里面来
                    2-CPU再从内存里面拿数据，因为内存的速度是非常快的，内存的速度大概是14万倍

        电脑内存小电脑越卡的原因：当电脑打开软件越多，内存会被沾满，当CPU再去读取数据的时候会直接从硬盘里面读，这时候就比较卡了(这时候会很明显的感觉电脑磁盘在飞速运转)