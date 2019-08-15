###Electron
***什么是Electron
    使用javascript，html和css构建夸平台的桌面应用(跨平台-兼容Mac,Windows,Linux)


***为什么要学习Electron或者说我们能学到什么
    夯实前端开发基础
    深入浅出node.js
    浏览器工作原理
    Electron API

****Electron的第一个应用
    主进程-Main Process的特点
        1-可以使用和系统对接的Electron API-创建菜单，上传文件等等
        2-创建 渲染进程-Renderer Process
        3-全面支持Node.js
        4-只有一个，作为整个程序的入口点
    渲染进程(Renderer Process)的特点
        1-可以有多个，每个对应一个窗口
        2-每个都是一个单独的进程
        3-全面支持Node.js和DOM API
        4-可以使用一部分Electron提供的API

***Main Process和Renderer Process进行通讯


***Renderer Process和Main Process进行通讯
        

***项目打包成不同平台的文件
    1-下载electron-builder

***打包的配置文件
    category //指这个软件安装是在哪个文件目录下面
    "dmg" 指mac具体打包之后的格式
    "dmg":{
        "background":"" 安装时候的背景图片
        "icons":""  图标
        "iconSize":100,
        "contents":[
            {
              "x":380,  这个是右边的图
              "y":280,
              "type":"link",       指超链接
              "path":"/Applications"  指应用程序文件夹
            },
            {
               "x":110,   这个是左边图
               "y":280,
               "type":""file
            }
        ],
        "window":{  框框的大小
            "width":500,
            "height":500
        }
    }