### www.awesomes.cn 可以查询各大框架的使用热度以及学习各大框架

***注意点：1-bootstrap是基于jquery，必须在引入bootstrap之前引入jquery
          2-使用bootstrap之前要进行视口设置：在移动端和pc端都适用 在pc端可以不用写，在移动端必须是要写的
                2-1.不允许用户缩放的写法<meta name="viewport" content="width=device-width,initial-scale=1.0">  
                2-2.允许用户缩放的写法  <meta name="viewport" content="width=device-width,user-scalable=no,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0">  


***bootstrap的基本使用
    学习bootstrap就是学习bootstrap里面类的使用  
    这是bootstrap官网首页的起步
```js
    1-引入bootstrap.css
    2-引入jquery.min.js
    3-引入bootstrap.min.js
    <button class="btn btn-danger"></button>  加入类名之后就是红色按钮了
```

***bootstrap的布局容器
    第一种布局容器container：
        1.给盒子加上container类 - 不同的屏幕范围内有不同的宽度，内部自己通过媒体查询实现，无须我们手动设置
        2.加上类的container的盒子有默认的左右padding值 15px
    第二种布局容器container-fluid 他是流式布局容器，也就是宽度100%容器 (那么这么容器什么时候使用呢？就是有要求是满屏效果)
        1.宽度100%
        2.有默认的padding值 15px

```js
    <div class="container">
        <div class="row"></div>   加row类就可以消除container的padding值
    </div>
```

***栅格系统 本质是 百分比 + 浮动 + 媒体查询
    需求：一行里面有俩个盒子，各占用一半空间
    注意点：响应式布局中推荐从小到大分配空间
    栅格系统特点：将一行分成了12份
        col 表示列 
        lg指大屏以上生效 
        md指中屏以上生效
        sm指小屏幕以上生效
        xs指超小屏以上生效(就是所有的屏幕生效)
        col-lg-6: 指12份里面占6份(就是50%加上左浮动) 依次类推
```js
    <div class="container">
        <div class="son col-sm-6 col-md-4 col-lg-6"></div>  小屏幕占6份 中屏幕占4份 大屏占6份
    </div>
``` 
    
***列嵌套
    概念：在每个盒子内部再进行空间分配
```js
    <div class="container">
        <div class="son col-lg-6">    
            <div class="box col-xs-3"></div>  box占son盒子空间的3份
            <div class="box col-xs-9"></div>  box占son盒子空间的9份
        </div>
    </div>
```
***列偏移

    概念：1-就是给盒子设置百分比的margin-left 来控制盒子的位置
          2-还结合了媒体查询
```js
    <div class="container">
        <div class="son col-lg-2 col-lg-offset-2"></div>  这里的col-lg-offset-2 在大屏幕中son盒子左边的margin值占2份
    </div>
```

***表格
    用法：给表格table标签添加table类即可
```js

```

***盒子的显示和隐藏 且不需要在container容器中就可以实现
    hidden 在所有屏幕都隐藏
    hidden-xs 超小屏幕隐藏 其他屏幕显示
    hidden-sm 小屏幕隐藏 其他屏幕显示
    hidden-md 中屏幕隐藏 其他屏幕显示
    hidden-lg 大屏幕隐藏 其他屏幕显示

```js
    <div class="box hidden-xs">
```

***媒体查询
    @media screen and 条件 and 条件 ...
        条件： min-width 最小宽度，只要大于这个宽度就生效
               max-width 最大宽度，只要小于于这个宽度就生效
               width 只要等于这个宽度样式就生效
    用法： 
        需求 在屏幕 500-800生效
        @media screen and (min-width:500) and (max-width:800){
            .box{
                background:red
            }
        }  