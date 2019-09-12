###什么是webpack
    webpack可以看做是模块打包机:它做的事情是,分析你的项目结构，找到javascript模块以及其它的一些浏览器不能直接运行的拓展语言(scss,typescript等)，并将其打包为合适的格式以供浏览器使用

    构建就是把源代码转换成发布到线上的可执行javascript,css,html代码，包括如下内容
        代码转换:typescript编译成JavaScript，scss编译成css等
        文件优化:压缩JavaScript，css，html代码，压缩合并图片等
        代码分割:提取多个页面的公共代码，提取首屏不需要执行部分的代码让其异步加载
        自动刷新:监听本地源代码的变化，自动重新构建，刷新浏览器
        代码校验:在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过
        自动发布:更新完代码后，自动构建出线上发布代码并传输给发布系统

    构建其实是工程化，自动化思想在前端开发中的体现，把一系列流程用代码去实现，让代码自动化地执行一系列复杂的流程。构建给前端开发注入了更大的活力，解放了我们的生产力


###使用webpack
```js
    查找插件的用法通过:www.npmjs.com/package/postcss-loader 
```
    1-初始化package.json
```
npm init -y
```
    全局安装(webpack4.0之后不建议全局安装)
        npm install webpack -g
    本地安装
        npm install webpack webpack-cli -D (安装webpack以及webpack依赖项)


***webpack的使用
```js
// webpack是基于node的语法，遵循common.js规范的
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')

// A-分别抽离css和less文件 的实现
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
let LessExtract = new ExtractTextWebpackPlugin('css/less.css')
let csssExtract = new ExtractTextWebpackPlugin('css/css.css')

//B- 没用的css会被消除掉
let PurifycssWebpack = require('purifycss-webpack')

module.exports = {
    entry:'./src/index.js', //入口
    output:{
        filename:'build.js',
        // 这个路径必须是绝对路径
        path:path.resolve('./build') //引入path的resolve方法自动解析成当前文件的绝对路径，运行生成build文件夹，同时build.js在build文件内部
    },
    devServer:{  //开发服务器
        contentBase:'./build', //在内存中指定要访问的目录(首页显示的内容)
        part:3000,//端口号
        compress:true,//服务器压缩
        open:true,//自动打开浏览器
        hot:true //热更新插件，相应的在plugins里也要添加相应的插件
    },
    module:{//模块配置
        rules:[
            {
            test:/\.css$/,use:cssExtract.extract({ //A
                use:[
                {loader:'css-loader'}
                ]
                })
            },
            {
                test:/\.less$/,use:LessExtract.extract({  //A
                    use:[
                        {loader:'css-loader'},
                        {loader:'less-loader'}
                    ]
                })
            }
        ]
    },
    plugins:[//插件的配置
        cssExtract, //A
        LessExtract,
        new webpack.HotModuleReplacementPlugin(), //实现热更新
        new CleanWebpackPlugin(['./build']), //将build文件清空
        //  打包html插件
        new HtmlWebpackPlugin({
            template:'./src/index.html',
            title:'天气真好', //需要将html中title进行修改 <%=htmlWebpackPlugin.option.title%>即可将html中的title值换成天气真好
            hash:true, //每次生成的build.js后面有一串hash值，代表最新文件
            minify:{
                removeAttrbuteQuotes:true, //去除index.html中的引号
                collapseWhitespace:true //去除空行，将index.html改成一行
            }
        }),
        // B-没用的css会被消除掉，一定放在htmlwebpackPlugin后面
        new purifycssWebpack({
            paths:glob.sync(path.resolve('src/*.html')) //将 src下面的所有的html用不到的css样式进行删除
        })
    ],
    mode:'development',//可以更改模式 
    resolve:{}，//配置解析

// 1.在webpack中如何配置开发服务器 webpack-dev-server
}
```

***配置多入口多出口
```js
    module.exports = {
        // entry:['./src/index.js','./src/a.js']同时将俩个js文件进行打包
        entry:{
            index:'./src/index.js',
            a:'./src/a.js'
        },
        output:{
            filename:'[name].js', //生成index.js和a.js
            path:path.resolve('./build')
        },
        plugins:[
            new HtmlWebpackPlugin({
                filename:'a.html', //生成的a.html引入的是index.js
                template:'./src/index.html',
                title:'天气真好',
                chunks:['index']  //在a.html引入index.js
            }),
            new HtmlWebpackPlugin({
                filename:'b.html', //生成的b.html引入的是a.js
                template:'./src/index.html',
                title:'天气真好',
                chunks:['a'] //在b.html引入a.js
            })
        ]
    }
```
    