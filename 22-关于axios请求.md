qs工具
1- qs.parse 方法可以把一段格式化的字符串转换为对象格式
```js
let url = 'http://item.taobao.com/item.htm?a=1&b=2&c=&d=xxx&e';
let data = qs.parse(url.split('?')[1]);

// data的结果是
{
    a: 1, 
    b: 2, 
    c: '', 
    d: xxx, 
    e: ''
}
```

2- qs.stringify 则和 qs.parse 相反，是把一个参数对象格式化为一个字符串。
```js
let params = { c: 'b', a: 'd' };
qs.stringify(params)

// 结果是
'c=b&a=d'
```

排序
甚至可以对格式化后的参数进行排序：
```js
qs.stringify(params, (a,b) => a.localeCompare(b))

// 结果是
'a=b&c=d'
```

指定数组编码格式
```js
let params = [1, 2, 3];

// indices(默认)
qs.stringify({a: params}, {
    arrayFormat: 'indices'
})
// 结果是
'a[0]=1&a[1]=2&a[2]=3'

// brackets 
qs.stringify({a: params}, {
    arrayFormat: 'brackets'
})
// 结果是
'a[]=1&a[]=2&a[]=3'

// repeat
qs.stringify({a: params}, {
    arrayFormat: 'repeat'
})
// 结果是
'a=1&a=2&a=3'
```


二：处理json格式的参数

JSON.parse（用于从一个字符串中解析出json 对象）