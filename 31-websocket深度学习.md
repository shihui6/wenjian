###websocket
    **介绍
        1-概念：websocket协议是基于TCP的一种新的网络协议(http也是网络协议)；它实现了浏览器与服务器全双工通信(浏览器可以向服务器发送消息，服务器也可以向浏览器发送消息)，允许服务器主动向发送给消息给客户端
        2-特点:
            websocket是一种持久协议(服务器可以不断的给客户端发送消息，客户端可以给服务器发送消息；发送请求之后可以不断的发送消息)，http非持久协议(发送一次http请求，执行完了就会断开连接) 
                2-1http实现轮询效果缺陷(每一秒都给服务器发送请求)
                    2-1-1：非常消耗性能，因为请求都会有三次握手
        3-使用场景：
            数据实时更新，数据推送等
    
    **websocket用法
        1-概念：
            H5已经直接提供了websocket的API，所以我们可以直接去使用
        2-使用
            2-1通过websocket的构造函数创建
                ```js
                    var socket = new WebSocket('ws://echo.webscoket.org')
                ```
                2-1-1:构造函数Websocket的参数
                    参数一：websocket的服务地址(一般是自己公司的服务地址)
            
            2-2：open：当和websocket服务连接成功的时候触发
                ```js
                    socket.addEventLister('open',function(){
                        console.log('连接服务器成功')
                    })
                ```
            2-3：send：给服务器发送消息
                socket.send('消息')
            2-4：接受websocket服务的数据
                ```js
                    socket.addEventListener('message',function(e){
                        console.log(e)  //e是从服务接受的数据对象
                    })
                ```
            2-5：服务断开时的触发的事件(把服务断开时会触发)
                ```js
                    socket.addEventListener('close',funciton(){
                        console.log('服务断开')
                    })
                ```
        
    **用nodejs开发websocket服务
        1-导入nodejs-websocket包
            ```js
                const ws = require('nodejs-websocket')
            ```
        2-创建一个server
            ```js
                2-1：每次只要有用户连接，函数就会被执行，会给当前连接的用户创建一个connect对象
                const server = ws.createServer(connect = {
                    console.log('由用户连接请来了')
                    2-1-2：每当接收到用户传递过来的数据，text事件会被触发
                    connect.on('text',data=>{
                        //data是接受的用户发送的数据
                        2-1-2-1：服务给客户端发送数据
                            connect.send(data)
                    })
                    2-1-3：只要websocket连接断开，close事件就会被触发
                    connect.on('close',()=>{
                        console.log('连接断开')
                    })
                    2-1-4：注册一个error事件(用户断开时，如果不注册error事件，服务端会报错)
                    connect.on('error',()=>{
                        console.log('用户连接断开或异常')
                    })
                }).listen(3000)   //监听3000端口
            ```
                2-2：广播，给所有的用户发送消息 (broadcast函数是websocket内部提供的)
                    ```js
                        function broadcast(msg){
                            //server.connections:表示所有的用户，是个数组，每个用户是数组中的一个值
                            server.connections.forEach(item=>{
                                //给每个用户发送消息
                                item.send(msg)
                            })
                        }
                    ```


###websocket的框架socket.io
    **socketio
        1-概念：
            socket.io是一个库，可以在浏览器和服务器之间实现实时，双向和基于事件通讯
        2-特点：
            2-1：可以在浏览器端用也可以在服务器端用相同的api
            2-2：socket.io并不是完全的websocket的实现，socketio里面用到了websocket传输协议，但他不是完整的websocket
            2-3：依赖于nodejs的服务
            2-4：socket.io默认情况下，建立长轮询连接
                ```js
                    const socket = io('http://localhost:8080',{transports:['polling','websocket']})
                ```
                2-4-1:将socket.io的设置成websocket
                    ```js
                        const socket = io('http://localhost:8080',{transports:['websocket']})
                    ```
        3-用法：
            3-1：
                3-1-1:可以在http服务里面用
                3-1-2：可以在express服务里面用
            3-2：安装socket.io包(服务器器端的实现)
                ```js
                    npm install --save socket.io
                ```
                ```js
                    io.on('connection',socket=>{
                        3-2-1//socket表示用户的链接
                        3-2-2// socket.emit方法表示给浏览器发送数据
                             3-2-2-1// socket.emit():参数一：事件的名字(自己定义的，在客户端通过此事件接受消息)，参数二：发送的消息
                        socket.emit('send',{name:'zhangsan'})
                        3-2-3：socket.on表示的注册的某个事件，如果我需要获取浏览器的数据，需要注册一个事件
                            参数一：事件名：任意
                            参数二：获取到的数据
                            socket.on('fasong',data=>{
                                console.log(data)  //data是接受的数据
                            })
                    })
                ```
            3-3：socketio浏览器端的实现
                3-3-1:使用标准的js库写法
                        <script src="/socket.io/socket.io.js"></script>
                        <script>
                            var socket = io('http://localhost:3000')
                        </script>
                3-3-2:使用node编译，则使用require('socket.io-client')
                    3-3-2-1:导入socket.io模块
                                const io = require('socket.io-client')
                                import io from 'socket.io-client'

                3-3-3：服务器发送数据给我客户端，通过'connect'在客户端监听，有数据从服务器传输过来，触发此事件(可不用,可以直接使用事件)
                        socket.on('connect',function(socket){
                        3-3-3：服务器发送数据
                            ```js
                                socket.emit('fasong',{name:'zs',age:18})
                            ```
                        3-3-4：接受服务器返回的数据
                                socket.on('send',function(data){
                                    console.log(data)
                                })
                        })
                
                3-3-4:socket
                    3-3-4-1:socket.id
                            概念：是标识符socket session独一无二的符号，在客户端连接到服务端被设置
                            实例：
                                ```js
                                    const socket = io('http://localhost:8080')
                                    socket.on('connect',()=>{
                                        console.log(socket.id)  //客户端连接到服务端前提下，才有socket.id
                                    })
                                ```
                
                


            3-4：socketio基于express服务端创建
                ```js
                    var app = require('express')()
                    var server = require('http').Server(app)
                    var io = require('socket.io')(server)
                3-4-1//启动服务
                    server.listen(3000,()=>{
                        console.log('服务器启动了')
                    })
                    app.get('/',function(req,res){ 
                        res.sendFile(__dirname+'/index.html')
                    })
                3-4-2：客户端有发送连接，就会触发io下面的connection事件，客户端的每个用户都对应一个单独的socket
                    io.on('connection',function(socket){
                        socket.emit('news',{msg:'连接上了'})
                        3-4-2-1：告诉所有的用户，广播消息
                            io.emit()用法：
                                参数一：定义事件(随意定义)，在客户端通过socket.on()监听
                                参数二：广播的消息
                            io.emit('addUser',data)
                        3-4-2-2:当客户端断开时触发
                            socket.on('discount',()=>{

                            })
                    })
                ```
            
            3-5：发送和接收事件
                socket.io允许你自定义发送和接收事件的名称，除了'connnect','message','disconnect'

            3-6:创建自己的路由
                概念：服务端使用多个路由技术仅仅只需要建立一个连接，客户端就可以发起多个websocket连接
                用法：
                    3-6-1：服务端：
                        ```js
                            var io = require('socket.io')(8080)
                            var chat = io.of('/first').on('connect',function(socket){
                                socket.emit('onemessage',{message:'通过first路由发出的消息'})
                            })
                            var news = io.of('/second').on('connect',function(socket){
                                socket.emit('twomessage',{message:'通过second路由发出的消息'})
                            })
                        ```
                            说明：一个连接可支持多条路由，
                    3-6-2：客户端：
                        ```js
                            var chat = io.connect('http://localhost/first')   //对应上个例子服务端first路由
                            var news = io.connect('http://localhost/second')  //对应上个例子服务端second路由
                            chat.on('onemessage',data=>{
                                console.log(data)
                            })
                            news.on('twomessage',data=>[
                                console.log(data)
                            ])
                        ```

            3-7：广播消息，除了发送的人接收不到，其他人都可以接收到
                用法：通过broadcast能产生效果
                    服务端代码
                        ```js
                            io.on('connection',function(socket){
                                socket.broadcast.emit('hi',{msg:'发送的消息'})
                            })
                        ```
                    客户端代码
                        ```js
                            socket.on('hi',(data)=>{
                                console.log('广播消息，除了发消息的看不到')
                            })
                        ```




    
            

    