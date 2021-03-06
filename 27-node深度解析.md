###node的作用和应用
    1-脱离浏览器运行js
    2-后端api编写(也就是些接口)
    3-webpack，gulp，npm是基于node写出来的，没有node这些工具是玩不了的 
    4-中间层：服务器中负责IO读写的中间层服务器(文件的读写，数据库查询这些都是由中间层比较好)
        node中间层理解：1-具有异步IO，有很好的性能，处理高并发请求很强
                      2-能够处理服务器返回的数据(如果数据多的话浏览器性能就消耗的比较大，对于客户体验就没那么好了；但是如果有node中间层，通过后端返回的数据经过node中间层处理成我们想要的数据，浏览器拿到数据直接展示，提高了用户体验)
                      3-提高了安全性能(黑客想要攻击服务器，必须要经过node中间层起到了中间一层阻隔的作用)

***我们学习node的优势
    1-便于前端开发入门
    2-性能高(所有的语言都不能和c语言相比，但是node比java，php，python性能要好的多，如是php的90倍)
    3-利于前端代码整合(node和前端都是js代码，所以大部分前端js写的代码也可以在node中使用)

***node的劣势
    1-最大的劣势就是太年轻了，相比java拥有那么多强大的框架而言，node至今没什么框架

***node环境搭建(就是下载node)

***node模块
    1-全局模块(对象)，像之前js里面的document，window，不管什么作用域有多深都可以调用全局模块，node中也有这样的全局模块， 如process
            1-1何时何地都可以访问，不需要引用
            1-2process.env(环境变量) 打印出来就是相应电脑的环境变量
            1-3process.argv(是个数组，第0个位置是node安装包文件路径，第1个是当前node执行的路径)
```js
                process.argv a b c d //打印结果 [
                    // "H":"node安装包",
                    // "C":"当前文件路径",
                    // "a",
                    // "b",
                    // "c",
                    // "d"
                // ]
```
    2-系统模块(系统内置的模块)，用的时候将其引进来就可以了，不需要去下载
        定义：需要require，不需要单独下载
            path：用于处理文件路径和目录路径的实用工具
```js
        let path = require('path')
        path.dirname('/node/a/b/c/1.jpg') //  /node/a/b/c
        path.basename('/node/a/b/c/1.jpg') // /node/a/b/c/1.jpg
        path.extname('/node/a/b/c/1.jpg') //  .jpg

        path.resolve()   //用于文件路径的解析


        let fs = require('fs')  //读文件和写文件的模块  涉及到同步异步读写文件
```
    3-自定义模块(自己写好的，然后将其暴露出去的模块)
        定义：require自己封装的模块
             3-1、exports 单个导出
```js
                exports. a = 1
```
             3-2、module批量导出，也可导出函数，类，对象
```js
                module.exports = {a:1,b:2}
```

             3-3、require
            require寻找路径的原则
                1-如果有路径就去路径里面找
                2-没有路劲就去node_modules里面找
                3-如果还没有就去node的安装目录里面找


***node中的数据交互

***接口设计
    定义：不同功能层之间的通讯规则(通过接口我们可以操作数据，不可能让js直接操作数据库那样太不安全了)

###node写服务端的操作逻辑
    1-前端登录到监听的服务器端口
    2-端口通过fs读取页面返回相应的页面
    3-通过页面，发送ajax请求 (url里面可省略http:localhost:8888服务器地址，本地可以自动补充)
    4-发送的请求，开启的服务器会接受请求并作出响应，返回相应的内容


###调试Node.js的优势
    ***通过Inspector调试Node.js的优势
        1-可查看当前上下文的变量
        2-可观察当前函数调用堆栈
        3-不侵入代码
        4-可在暂停状态下执行指定代码 (通过给函数或者变量＋1减一的操作，不会影响原本的代码)

    ***Inspector的构成以及原理
        1-启动Inspector时会启动WebSockets服务(用语监听命令)
        2-Inspector协议
        3-Http服务(获取元信息)

    ***激活nodejs调试工具
        1-  node --inspect app.js