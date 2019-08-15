***数组和字符串相互转换
    1-数组转字符串   join(',')
    2-字符串转数组   split(',')
    关于数组的方法
        1-push
        2-pop  取出数组的最后一项
        3-shift 取出数组的第一个元素
        4-unshift 在数组的最前面插入项
        5-reverse 翻转数组
        6-sort  对数组进行排序，想按照其他标准进行排序，就需要提供比较函数
        7-cancat 用于俩个或多个数组进行拼接  A.concat(B) A和B都是数组
        8-splice  对数组进行删除或添加  splice(index,删除的项数，添加的项)
        9-slice 从已有的数组中返回选中的数组   slice(start,end)
    关于字符串的方法
        1-replace 字符串替换 replace(条件，替换体)
        2-substr  在字符串中抽取从start下标开始的指定数目的字符 substr(start,length)


***遍历数组的方法
    1-every 数组中所有的项满足返回true
    2-some  数组中只要有一项满足就返回true
    3-map   让数组中的每一项执行这个函数，返回每一项结果组成的数组
    4-filter让数组中的每一项执行，返回满足条件的项的数组
    5-forEach 提供一个函数，让数组中的每一项都执行这个函数，没有返回值v,是数组中的每一项，i是数组中的索引


***switch语句
    switch(表达式){
        case n:
        代码块,
        break;
    }

