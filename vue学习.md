***组件之间传递数据
    1-父组件向子组件传递数据
        1-1在父组件内容引入子组件，通过v-bind绑定数据到子组件上
        1-2子组件内部通过prop接受，prop里面的数据和data里的数据一样可以直接使用
        1-3子组件内改变prop里的数据，父组件里面相应的数据也会改变
    2-子组件想父组件传递数据
        2-1子组件内部通过$emit定义事件，通过事件传递数据
        2-2在父组件内部触发$emit定义的事件接受数据
    3-父组件触发子组件内部的事件
        3-1ref拿到子组件的dom接口，直接调用子组件里面定义的事件即可
    4-子组件触发父组件里面的事件
        4-1通过$emit



***vue路由跳转
    1-第一种方式<router-link>方式
```js

1. 不带参数
<router-link :to="{name:'home'}"> 
<router-link :to="{path:'/home'}"> //name,path都行, 建议用name  
// 注意：router-link中链接如果是'/'开始就是从根路由开始，如果开始不带'/'，则从当前路由开始。
 
2.带参数
<router-link :to="{name:'home', params: {id:1}}">  
// params传参数 (类似post)
// 路由配置 path: "/home/:id" 或者 path: "/home:id" 
// 不配置path ,第一次可请求,刷新页面id会消失
// 配置path,刷新页面id会保留
// html 取参  $route.params.id
// script 取参  this.$route.params.id
<router-link :to="{name:'home', query: {id:1}}"> 
// query传参数 (类似get,url后面会显示参数)
// 路由可不配置
// html 取参  $route.query.id
```
    2-第二种方式this.$router.push()
```js

1.  不带参数
this.$router.push('/home')
this.$router.push({name:'home'})
this.$router.push({path:'/home'})
 
2. query传参 
this.$router.push({name:'home',query: {id:'1'}})
this.$router.push({path:'/home',query: {id:'1'}})
// html 取参  $route.query.id
// script 取参  this.$route.query.id
 
3. params传参
this.$router.push({name:'home',params: {id:'1'}})  // 只能用 name
 
// 路由配置 path: "/home/:id" 或者 path: "/home:id" ,
// 不配置path ,第一次可请求,刷新页面id会消失
// 配置path,刷新页面id会保留
 
// html 取参  $route.params.id
// script 取参  this.$route.params.id

4. query和params区别
query类似 get, 跳转之后页面 url后面会拼接参数,类似?id=1, 非重要性的可以这样传, 密码之类还是用params刷新页面id还在
params类似 post, 跳转之后页面 url后面不会拼接参数 , 但是刷新页面id 会消失
```
    3-第三种方式 this.$router.replace() 跟上面this.$router.push()一样



***vue路由引入组件方式
    1-第一种方式
```js
{
    name:'trust',
    path:'trust',
    components:()=>import('@/page/hosting/trust')
}
```
    2-第二种方式
```js
{
  path: 'trust',
  name: 'trust',
  component: require('@/page/hosting/trust').default,
},
```