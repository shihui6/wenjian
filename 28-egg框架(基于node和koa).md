###Egg.js框架介绍
    1-Egg.js是基于Node.js和Koa的一个企业级应用框架，可以帮助团队降低开发成本和维护成本
    应用安装
        npm init egg --type=simple
***路由
    Router主要用来描述请求URL和具体承担执行动作的Controller的对应关系，框架约定了app/router.js文件用于统一所有路由规则

```js
/**
 * @param {Egg.Application} app - egg application
 */
module.exports = app => {
  const { router, controller } = app;  //问题：问什么在routerjs里面没有引入home和product就直接可以引用，
    //   是因为app是全局应用对象，这里已经实例化好了，
  // 全局应用对象只能实例化一次；在这里eggjs已经把路由和控制器都设置好了
  router.get('/', controller.home.index);
  router.get('/product',controller.product.index);   //get请求的写法
};
```

<!-- 2-下面是homejs里面的代码 -->
```js
const Controller = require('egg').Controller;
// home.js里面是各种业务逻辑，例如：商品模块，用户信息模块
class HomeController extends Controller {
  async index() {
    const { ctx } = this;  //每次用户请求的时候，都会实例化ctx上下文   ctx主要是处理用户请求的信息，操作返回的内容
    ctx.body = 'shihui, egg';
  }
}

module.exports = HomeController;  //模块导出  然后在routerjs里面进行操作

```

***服务(Service)
    简单来说，Service就是复杂业务场景下用于做业务逻辑封装的一个抽象层，提供这个抽象有以下几个好处：
        1-保持Controller中的逻辑更加简洁
        2-保持业务逻辑独立性，抽象出来的Service可以被多个Controller重复调用
        3-将逻辑和展现分离，更容易编写测试用例
    使用：
        
```js
// 1-在app文件夹中创建serverjs文件
const Service = require('egg').Service;
// 一般的在service中链接数据库
class ProductService extends Service {
    async index(){
        return {
            id:100,
            name:'ceshi'
        }
    }
}
module.exports = ProductService

// 2-在想要引入数据的请求中引入
 let res = ctx.service.product.index()
```

***关于ctx的几个运用
ctx.query   get请求的请求参数
ctx.param   动态路由的请求参数
ctx.request.body  post请求的请求参数

***View模板渲染
    绝大多数情况，我们都需要读取数据后渲染模板，然后呈现给用户。故我们需要引入对应的模板引擎
    框架内置egg-view作为模板解决方案，并支持多模板渲染，每个模板引擎都以插件的方式引入，但保持渲染的API一致。

用法：
    1-先安装  npm i egg-view-ejs --save
```js
    // 2-然后在plugin.js里面配置 
        exports.ejs = {
            enable:true,
            package:"egg-view-ejs"
        }
    //   在config.default.js里配置
        exports.view = {
            mapping：{
                ".ejs":"ejs"  //以.ejs结尾的文件，都用ejs模板进行渲染
            }
        }
    // 3-在页面组件的js文件中使用模板
    ctx.render('index.html',{res})  //ctx.render("模板文件",模板中所需的数据)
```
