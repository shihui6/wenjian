***echarts起步使用
    1-在echarts官网下载完整版
    2-在项目中引入
        <script src='../echarts.min.js'></script>
    3-为echarts准备一个具备大小(宽高)的DOM
        <div id='main' style='width:900px;height:600px;'></div>
    4-基于准备好的dom，初始化echarts实例
        let myChart = echarts.init(document.getElementById('main'))
    5-指定图表的配置项
    6-将配置项添加到图表中  
        myChart.setOption(option)

###以下是在配置项里面完成的(option)
        ***示例代码介绍
        1-html结构
        2-echarts.init()，初始化echarts实例，setOption用指定数据绘图
        3-Option对象
            3-1标题：title
            3-2图例：legend
            3-3x轴：xAxis
            3-4数据：series：
                    name，type，data

        配置项里面
            1.有关图表的标题的配置：title:{text:'echarts入门实例'}
            2.相关的工具箱的配置：toolbox:{feature:{saveAsImage:{show:true}}} 这是有关点击下载的工具箱
            3.图例(点击图例会隐藏或者显示相关内容  也就是一条数据的名字): legend:{data:['销量']}
            4.x轴 显示的信息  xAxis

        ***标题组件(修饰图表标题的各种样式)
            标题负责显示整个图表的标题
            配置项：
                一般是option配置项底下的title
                    show 代表是否显示，值为true为显示
                    text 指主要标题的文字内容
                    subtext 指副标题
                    left 值为数值，数字就是像素值，指距离左边多少像素也可以是center left right
                    borderColor 指边框颜色
                    borderWidth 指变宽宽度

        ***工具箱组件 (点击工具栏可以下载图片，数据缩放还原等等)
            工具箱配置项toolbox
                show 值为true指工具箱显示
                feature 代表具体需要哪些功能
                    dataView 数据视图
                    restore 还原视图
                    dataZoom 区域缩放视图
                    saveAsImage image保存
                    magicType 做动态类型切换

        ***tooltip组件  (鼠标移动到相应的柱状图上面会出现一条线标注可看到销量)
            组件的工具栏配置
                show 是否显示值为true false
                Trigger 出发方式，axis就是x轴出发


        ***标记线和标记点：markline markpoint(在series系列里面)
            切换成折线图的时候可以设置最大是出和最小值处的标记点的样式，也可以设置平均值

###饼图
    *饼图展示数据的特点
        展示百分比
        Type是pie
    *Center 圆心坐标
    *Radius 半径
    *Name 图例名字
    *selectedMode 是否支持多选

    设置配置：
        在配置项里面的series进行配置
            series:[
                {name:'访问来源'},  饼图的标题
                type:'pie',         类型是饼状图
                radius:'50%',     半径的大小是55%
                center:['50%',60%],    //饼图的位置在水平50%,垂直60%的位置
                data:[
                    {value:335,name:'直接访问'},
                    {value:310,name:'邮件营销'}     //这里name值和图例legend的name对应起来俩者就可以联动起来
                ]
            ]

###仪表图
    仪表图展示数据的特点 Type是 gauge   也可以动态修改仪表图数据
    也是series里面的type：gauge
        series:[
            name:'业务指标',
            type:'gauge',
            detail:{formatter:'{value}%'},
            data:[{value:32,name:'完成指标'}]
        ]

###地图
    只要涉及到地图就会经纬度的概念(只要跟地图相关的还要下载地图 china.js引入即可,世界地图需要下载世界地图.js，上海地图需要下载上海.js)
    1-每个城市的经纬度
    2-一个城市到另外一个城市的值
    3-自己定义的的路径(飞机的路径)
    4-把城市经纬度和每个城市到另外一个城市的格式转换成echarts需要的格式
    5-定义地图的series
    6-定义option配置项
    7-使用刚指定的配置项和数据显示图标

###申请百度秘钥
    1-申请ak(即获取秘钥)，若无百度账号则首先需要注册百度账号
    2-拼写发送http请求的url，注意需使用第一步申请的ak(即是我们第一步申请的秘钥)
    3-接收http请求返回的数据(支持json和xml格式)
        实例：发送一个地址到百度大厦
            http://api.map.baidu.com/geocoder/v2/?address=百度大厦&putput=json(需要返回的格式，我们需要返回json格式)&ak=秘钥&callback=回调函数
            实例中拼接这一段地址就可以进行发送，返回相应地址的经纬度值
        