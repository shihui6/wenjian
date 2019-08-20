###Websocket
    Websocket适合在浏览器和服务器建立持久连接
    Websocket伴随着h5产生的
    Websocket是什么？
        浏览器发出websocket请求，与服务器建立web端的socket连接，就允许浏览器和服务器相互发送消息。如果浏览器端和服务端都不断开，那么socket是不会断开的。

    websocket的使用
```js
    1.建立websocket连接
    var websocket = new WebSocket('websocket服务器地址')
    2.websocket回调函数onopen
    websocket.onopen  = function(){
        document.getElementById('sec').innerHtml = 'Connected'
    }
    3.有open就有onclose
    websocket.onclose = function(){
        console.log('websocket close')
    }
    4.websocket接受消息时的回调函数onmessage
    websocket.onmessage = function(e){
        将接受到的消息放到页面中
        document.getElementById('sec').innerHtml = e.data
    }
    5.点击按钮触发事件，通过websocket发送消息
    document.getElementById('sec').onclick = function(){
        var value = document.getElement('Input').value
        websocket.send(value)
    }
```
    websocket建立服务器连接
```js
    1.安装nodejs-websocket模块
    2.引进nodejs-websocket模块
    var ws = require('nodejs-websocket')
    3.调用createServer，回调函数的参数代表链接
    var server = ws.createServer(function(conn){
        4.当客户端有消息发过来的时候会回调 参数.on('text',function(){})
        conn.on('text',function(str){
            5.然后把消息发送回去
            conn.sendText(str)
        })
        6.连接关闭的函数
        conn.on('close',function(code,reason){
            console.log('Connection closed')
        })

        解决当页面关闭的时候，服务端也会挂起的问题
        con.on('error',function(err){console.log(err)})
    }).listen(8001)

```

    ***websocket的框架 socketio
socket需要在客户端和服务器同时引入

服务端
```js
    1.安装websocketio
    2.通过引入http，创建http的server
    var app = require('http').createServer()
    3.引入socket.io将http包装成io类型
    var io = require('socket.io')(app)
    4.服务监听端口
    app.listen(3000)
    5.客户端连接上的时候会触发connection
    var clientCount = 0
    io.on('connection',function(socket){
        clientCount++
        socket.nickname = 'user' + clientCount
        6.进行广播通知用户已经连接
        io.emit('enter',socket.name + 'comes in')

        7.服务端接受浏览器的消息 并且发送消息给浏览器
        socket.on('message',function(str){
            io.emit('message',socket.name + 'says:' + str)
        })
        8.服务端断开
        socket.on('disconnect',function(){
            io.emit('leave',socket.nickname + 'left')
        })
    })


    
    
    
```

浏览器端
```js
    1.建立连接
    var socket = io('ws://localhost:3000/')
    2.点击发送消息
    document.getElementById('sendBth').onclick = function(){
        var text = document.getElementById('sendText').value
        if(text){
            socket.emit('message',text)
        }
    }
    3.监听服务端发送过来的消息
    socket.on('enter',function(data){
        showMessage(data,'enter')
    })
    4.浏览器接受消息的回调
    socket.on('message',function(data){
        showMessage(data,'message')
    })
    5.浏览器离开时的回调
    socket.on('leave',function(data){
        showMessage(data,'leave')
    })

    function showMessage(str,type){
        var div = document.createElement('div')
        div.innerHtml = str
        if(type == 'enter'){
            div.style.color = 'blue'
        }else{
            div.style.color = 'red'
        }
        document.body.appendChild(div)
    }
```