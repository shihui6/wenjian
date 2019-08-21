***flow
    1.是什么？
        是facebook出品的javascript静态类型检查工具，vuejs的源码利用flow做了静态类型检查。
    2.为什么用flow？
        想在编译阶段尽早发现bug，又不影响代码运行，使编写javascript和编写java等强类型语言相近的体验
    3.flow的工作方式
        3-1.类型推断：通过变量的使用上下文来推断出变量类型，然后根据这些推断来检查类型
        3-2.类型注释：事先注释好我们期待的类型，flow会基于这些注释来判断

    4.flow使用
```js
    1-全局安装flow-bin
    2-进行flow初始化flow init，
        生成.flowconfig文件，此文件能够让flow可以对哪些文件进行检查，哪些文件不检查
    3.
```