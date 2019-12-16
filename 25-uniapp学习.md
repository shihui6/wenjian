###关于pages.json文件的作用
```js
// 作用：配置路由
	"pages": [
		{
			"path": "pages/index/index",
			"style": {
				"navigationBarTitleText": "uni-app" //当前所在页面的标题
			}
		}
	],
	// 1-在pages.json里面位置上写的第一个路由是首页
    // 2-navigationBarTextStyle导航栏标题颜色，仅支持black/white
```

###unpackage文件
作用:打包项目在各平台上的包(测试包和发布包)
unpackage=>dist=>dev(编译后的测试包)
unpackage=>dist=>build(编译发布以后会生成build)

###App.vue
作用：
    1-在style标签内部定义全局样式的页面，且在其他页面中就可以引用，但页面私有样式会覆盖全局样
    2-生命周期

###manifest.json
```js
// 1-基础配置
// 应用名称：打包之后的名称
// 版本号

// 2-App图表配置
// 3-App模块权限配置：要使用到支付，登录权限，地图等的时候，要把相应的权限打上勾
```

###页面的生命周期
1-页面进行加载触发onLoad事件
2-页面展示出来的时候触发onShow事件
3-不管是页面的样式还是数据全部都渲染出来的时候触发初次渲染完成触发onReady事件
4-点击home键将页面隐藏触发onHide事件，跟onUnload监听页面卸载几乎是同时触发的


###尺寸单位
1-在uniapp中响应式的单位upx
	用法：按照iPhone6屏幕大小为基准 1upx = 0.5px
	注意点：有一些尺寸单位是不可用响应式单位的，比如字体大小，边框的宽度往往设死为1px

###事件
tap和click事件在应用中都是一样的效果，一般只要取其中的一个事件就可以


***跨端兼容
	1-条件编译
	条件编译：是用特殊的注释作为标记，在编译时根据这些特殊的注释，将注释里的代码编译到不同的凭条(将不同的代码编译到相应的平台)
	条件编译支持的文件：.vue;.js;.css;pages.json(用的不多)
	条件编译的使用：
		1-1在html中的编译
		<!-- #ifdef app-plus -->
			只在指定平台出现
		<!-- #endif -->
		<!-- #ifndef app-plus -->
			除了在这个平台不编译，其他平台都出现
		<!-- #endif -->

		1-2在js文件中编译
		// #ifdef H5
			只在H5中编译
		// #endif
		// #ifndef MP
			除了在MP中不编译，在其他平台都编译
		// #endif

***uni-app flex布局
	1- Flex布局的概念
		1-1 flexible box:弹性盒状布局
		1-2 容器控制内部元素的布局定位
		1-3 css3引入的新布局模型
		1-4 伸缩元素，自由填充，自适应
	2- Flex布局的优势
		2-1 可在不同方向排列元素
		2-2 控制元素排列的方向
		2-3 控制元素的对齐方式
		2-4 控制元素之间的等距
		2-5 控制单个元素放大与缩小比例，占比，对齐方式
	 3- Flex布局的常用术语
	 	3-1 flex container ：flex容器
		3-2 flex item ：flex项目(元素)
		3-3 flex direction ：布局方向
	4- Flex布局的模型
		主轴，交叉轴俩个方向
	5- Flex容器的属性
		5-1.flex-direction : 设置元素的排列方向
			row和row-reverse是水平排序的，row：水平从左向右排序，row-reverse是从右向左排序
			column和column-reverse是垂直的排序
		5-2 flex-wrap :是容器内的元素换行
				nowrap 不换行 (默认不换行)
				wrap 换行	
				wrap-reverse 逆向换行排序
		5-3 justify-content : 设置元素在主轴上的对齐方式
				flex-start 左对齐 默认
				flex-end   右对齐	
				center	   居中对齐
				space-between 俩端对齐，元素和元素之间留有一定的间隙 并且间隙都是等间距的
				space-around  元素俩边平均等分剩余空白间隙部分，最左或最右和元素之间距离是1:2的关系
		5-4 aligh-items 设置垂直轴的对其方式
				flex-start	在垂直轴上向起点位置(向上/向左)对齐
				flex-end	在垂直轴上向结束位置(向下/向右)对齐
				center
				baseline  将元素(容器)里的文字作为基准线排成一行 (保证每个文字都在同一条线上)
				stretch(默认) 将容器里元素的高度和容器的高度设置成一样

***将style里面的css样式导入进来的方式 在style里有方法
	@import url('地址')