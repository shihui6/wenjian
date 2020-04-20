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


具体实例
***vue子组件调用父组件里面的方法或者传递数据   执行逻辑点击子组件里面的元素，往父组件传值
    思路1：在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了
```js
    父组件
<template>
  <div>
    <child @fatherMethod="fatherMethod"></child>
  </div>
</template>
<script>
  import child from '~/components/dam/child';
  export default {
    components: {
      child
    },
    methods: {
      fatherMethod() {
        console.log('测试');
      }
    }
  };
</script>

子组件

<template>
  <div>
    <button @click="childMethod()">点击</button>
  </div>
</template>
<script>
  export default {
    methods: {
      childMethod() {
        this.$emit('fatherMethod');
      }
    }
  };
</script>
```
思路二:父组件把方法传入子组件中，在子组件里直接调用这个方法
```js
父组件
<template>
  <div>
    <child :fatherMethod="fatherMethod"></child>
  </div>
</template>
<script>
  import child from '~/components/dam/child';
  export default {
    components: {
      child
    },
    methods: {
      fatherMethod() {
        console.log('测试');
      }
    }
  };
</script>


子组件
<template>
  <div>
    <button @click="childMethod()">点击</button>
  </div>
</template>
<script>
  export default {
    props: {
      fatherMethod: {
        type: Function,
        default: null
      }
    },
    methods: {
      childMethod() {
        if (this.fatherMethod) {
          this.fatherMethod();
        }
      }
    }
  };
</script>
```

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

***vue周期函数的理解
    beforeCreate 实例初始化进行数据观察、事件配置
    created 实例创建完成
    beforeMount 挂在开始
    mounted  挂载到实例上
    beforeUpdate 数据更新前
    updated 组件DOM更新
    beforeDestroy 实例销毁前
    destroyed 实例销毁完成

    父级周期函数和子级周期函数执行的顺序
        1-beforeCreate 父级实例初始化进行数据观测/事件配置
        2-created 父级实例创建完成
        3-beforeMount 父级挂在开始
        4-beforeCreate 子级实例初始化
        5-created 子级实例创建完成
        6-beforeMount 子级挂在开始
        7-mounted 子级挂载到实例上
        8-mounted 父级挂载到实例上
        9-beforeUpdate 父级数据更新前
        10-updated 父级组件DOM更新


***vue中直接将组件内容全部迁移到其他页面中的做法
    1-import 组件名
    2-在相应的位置写上import的名称
    3-在当前组件中进行注册components:{组价名}
    特点，可以在当前页面用到import组件的所有的功能


***vue中的列表渲染v-for知识点
```js
    <div v-for="(item,index) in alls"></div>
    // 1-如果此div有css样式，则v-for指令会渲染当前div，当然没有样式也会渲染，只不过咋们看不到
    // 2-div里面有内容，连同内容和div一起渲染
```

**vue中mixins
  1：概念：mixins是混入(mixins),是一种分发Vue组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。
  2：理解：
    2-1：mixins是在引入组件之后，则是将组件内部的内容如data等方法、method等属性与父组件相应内容进行合并。相当于在引入后，父组件的各种属性方法都被扩充了
    2-2：有点像注册了一个vue的公共方法，可以绑定在多个组件或者多个vue对象实例中使用。
  3：用法：
    3-1：定义一个混入对象

    ```js
      // 在mixins.js文件中
      export const myMixn = {
        data(){
          return {
            num:1
          },
          created(){

          },
          methods:{
            hello(){
              console.log('hello from mixin')
            }
          }
        }
      }
    ```

    3-2：把混入对象混入到当前的组件中

    ```js
      // 在template.vue组件中引用minxins混入对象
      import {myMixn} from './mixins.js'
      export default {
        mixins:[myMixin]
      }
    ```

  4：注意点：
    4-1：方法和参数在各组件中不共享
      实例：

      ```js
        let mixin={
            data(){
                return{
                    msg:1
                }
            },
            methods:{
                foo(){
                    console.log('hello from mixin!----'+this.msg++)
                }
            }
        }
        var child=Vue.component('child',{ 
                template:`<h1 @click="foo">child component</h1>`, 
                mixins:[mixin]
        })
        Vue.component('kid',{ 
                template:`<h1 @click="foo">kid component</h1>`, 
                mixins:[mixin]
        })
      ```
        实例解释：虽然此处，两个组件可以通过this.msg引入mixins中定义的msg，但是，两个组件应引用的并不是同一个msg，而是各自创建了一个新的msg。如果在组件中定义相同的data，则此处会引用组件中的msg，而非mixins中的

    4-2：mixins这个属性接受的是一个数组可以有多个mixins的内容，比如：mixins:[mix1,mix2,mix3],前提是定义了这些混入对象，不然就会报错

    4-3：混入对象中如果有声明周期的钩子函数，那么混入对象和被混入对象的钩子都会执行一遍，而且混入对象的钩子将在实例自身钩子之前先执行。因为同名钩子函数将混合为一个数组，因此都将被调用

    4-4：混入的方法methods，计算属性computed，组件components，数据data等(只要值是为对象的)，都是会被合并的。并且如果有相同的键值，则会以被混入对象中的键值即以实力中的内容为准

    







