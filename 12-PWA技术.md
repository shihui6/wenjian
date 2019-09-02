PWA的优势
    渐进式：适用于所有的浏览器，因为它是渐进式增强作为宗旨开发的
    流畅：能够借助Service Worker 在离线或者网络交差的情况下正常访问
    可安装：用户可以添加常用的webapp到桌面，免去去应用商店下载的麻烦
    原生体验：可以和app一样，拥有首屏加载动画，可以隐藏地址栏等成沉浸式体验
    粘性：通过推送离线通知等，可以让用户回流
###PWA一系列的核心技术
******web app manifest
        是什么：是应用程序清单的文件
            特点：1-web app manifest可以让网站安装到主屏幕，而不需要用户通过应用商店进行下载。
                  2-web app manifest，在JSON文件中进行配置
                  3-可以将网页添加到桌面，有唯一的图表或者名称;
                  4-启动时界面，免去生硬的过度
                  5-隐藏浏览器相关的UI，比如地址栏等
        怎么使用：
            1-在项目根目录创建一个manifest.json文件
            2-在index.html中引入manifest.json文件
                <link rel="manifest" href="manifest.json">
            3-在manifest.json文件中提供常见的配置
                常见的配置：
                    name：用于指定应用的名称，用户安装横幅提示的名称，和启动画面中的文字
                    short_name:应用的知名度，用于主屏幕显示
                    start_url:指定用户从设备启动应用程序时加载的URL，可以是绝对路径和相对路径
                    icons：用于指定可在各种环境中用作应用程序图标
                    background_color:用于指定启动动画的背景颜色
                    theme_color:用于指定应用程序的主题色，指应用的最上面的颜色
                    display：用于指定web app的显示模式
                        1-fullscreen全屏显示，所有可用的显示区域都被使用，并且不现实状态栏
                        2-standalone：让这个应用看起来像一个独立的应用程序，包括具体不同的窗口，在应用程序启动器中拥有自己的图表等
                        3-minimal-ui该应用程序将看起来像一个独立的应用程序，但会有浏览器地址栏
```js
    {
        "name":"豆瓣APP_PWA",
        "short_name":"豆瓣APP",
        "start_url":"./index.html",
        "icon":[
            {
                "src":"",
                "sizes":"",
                "type":"image/png"
            }
        ],
        "background_color":"",
        "theme_color":"yello",
        "display":"standalone"
    }
```
            4-需要在https协议或者http://localhost下访问项目
******service worker
        是什么：可以让网页在离线或者网速特别慢的情况依旧可以访问，就跟应用差不多
        具体：主要用来做持久的离线缓存
              是一个独立的worker线程，独立于当前网页的进程，是一种特殊的web worker
                    web worker 是一个单独的线程即脱离主线程的，可以单独的去做事情比如从后端请求回来的大量的数据做复杂的运算，做完之后给主线程进行通讯，然后让主线程拿数据
                    web worker是一个独立的环境，里面是不可以操作BOM和DOM的
                    怎么使用web worker？
                            1-创建web worker var worker = new Worker('work.js')
                            2-在web work中进行复杂的计算
                            3-web work计算结束，通过self.postMessage(msg)给主线程发消息
                            4-主线程通过worker.onmessage = function(){}监听消息
                            5-主线程也可以用同样的方式来给web worker进行通讯
                    实例：
```js
    1.创建一个web worker
    const worker = new Worker('work.js')  运算在work.js文件中进行运算

    2.在work.js文件中进行逻辑计算
    self.postMessage(msg)

    3.在主页面通过worker.addEventListener('message',e=>{
        console.log(e.data)
    })
    这样就可以接收到work.js文件里面计算出来的内容，并且还是异步的不影响其他线程的执行

    注意点：serverWorker必须在http协议下才能看到效果，不然会报错
```
        ###Service Worker基本介绍
            1-web Worker是临时的，每次做的事情的结果还不能被持久保存下来，如果下次有同样复杂操作，还得费时间重新算一遍
            2-service worker 一旦被install，就永久存在，除非被手动unregister
            3-用的时候可以直接唤醒，不用的时候自动睡眠
            4-可编程拦截代理请求和返回，缓存文件，缓存的文件可以被网页进程取到(包括网络离线状态)
            5-必须在HTTPS环境下才能工作
            6-异步实现，内部大都是通过Promise实现
            总结：service worker可以操作缓存
                  可以拦截请求和响应，可以读缓存或者读网络
        ###Service Worker使用
            1-在window.onload中注册service worker，防止与其他资源竞争
            2-navigator对象中内置了serviceWorker属性
            3-service worder在老版本浏览器中不支持，需要进行浏览器兼容if('serviceWorker' in navigator){}
            4-注册service worker navigator.serviceWorker.register(./sw.js),返回一个promise对象

            实例：
```js
    1-需要在网页加载完之后进行注册
    window.addEventlistener('load',()=>{
        2.浏览器兼容性检测
        3.进行注册
        navigator.serviceWorker.register(./sw.js)  然后接下来都在sw.js文件里面进行操作
    })
```
        ###Service worker声明周期事件（在serviceworker注册的js文件中进行代码的编写）
            install事件：会在service worker注册成功的时候触发，主要用于缓存资源
            activate事件：会在service worker激活的时候触发，主要用于删除旧的资源
            fetch事件：会在发送请求的时候触发，主要用于操作缓存或者读取网络资源 

            实例：
```js
    在sw.js文件中
    self.addEventListener('install',event=>{
        console.log('install',event)
        event.waitUntil(self.skipWaiting())
        // 会让service worker跳过等待，直接进入到activate状态
        // 等到skipWaiting结束，才进入到activate
    })
    self.addEventListener('activate',event=>{
        console.log('activate',event)
        event.waitUntil(self.clients.claim())
        // 表示service worker激活后，立即获取控制权
    })
    self.addEventListener('fetch',event=>{
        console.log('fetch',event)
    })
```

******promise/async/await
        是什么：es8推出来的技术
******fetch api
        是什么：类似于ajax的发请求的方式
        怎么使用？
            window对象上就有fetch api
```js
        fetch('./data.json').then(res=>{
            // res是请求得到的响应内容，二进制的流
            // 调用res.json()方法，可以把数据变成json格式，读取的是一个json数据，返回的也是一个promise
            return res.json()
        }).then(data=>{
            console.log(data)
        })
```
******Cache storage
    是什么：将资源缓存下来
    怎么使用？
        1-cacheStorage接口表示Cache对象的存储，配合service worker来实现资源的缓存
        2-caches api类似于数据库的操作
            1.caches.open(catchName).then(function(catch){})用于打开缓存，返回一个匹配cacheName的cache对象的promise，类似于链接数据库
            2.caches.keys()返回一个promise对象，包括所有的缓存的key(数据库名称)
            3.caches.delete(key)根据key删除对应的缓存(数据库)
        3-cache对象常用方法(单数据的操作)
            1.cache接口为缓存的Request/Response对象对提供缓存机制
            2.cache.put(req,res)把请求当key，并且把对应的响应存储起来
            3.cache.add(url)根据url发起请求，并且把响应结果存储起来
            4.cache.addAll(urls)抓取一个url数组，并且把结果都存储起来
            5.cache.match(req)获取req对应的response
******常见的缓存策略
        是什么：哪些资源走缓存，哪些资源走网络
            1-需要通过网路请求的走网路
            2-本地资源不变的走本地资源

******notification
        做什么：用来做通知，比如：电脑下面突然跳出来一个通知 
        基本使用：
            1-Notification API的通知接口用于向用户显示桌面通知
            2-Notification.permission 可以获取当前用户的授权情况
                Default：默认的，未授权
                Denied：拒绝的，如果绝句了，无法再次请求授权，也无法弹窗提醒
                Granted：授权的，可以弹窗提醒
            3-通过Notification.requestPermission()可以请求用户的授权
            4-通过new Notification('title',{body:''})可以显示通知
            5-在授权通过的情况下，可以在service worker中显示通知，self.registration.showNotification('你好',{body:''})


