###关于pages.json文件的作用
```js
	"pages": [
		{
			"path": "pages/index/index",
			"style": {
				"navigationBarTitleText": "uni-app" //当前所在页面的标题
			}
		}
	],

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