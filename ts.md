***基础类型
    布尔值
```ts
    let isDone:boolen = false 
```
    数字
```ts
    let dec:number = 20
    let dec:number = 0x14 16进制表示法
    let dec:number = 0b10100  2进制表示法
    let dec:number = 0o24 8进制表示法
```
    字符串
```ts
    let name:string = 'bob'
```
    数组
```ts
    let list:number[]=[1,2,3,4] 数字类型的数组
    let list:Array<number> = [1,2,3,4]  数组泛型
```
    元祖Tuple
```ts
    let x:[string,number]  
    x=['hello',10]
    定义x数组，第一个数据是字符串第二个数字是数字，顺序类型严格要求
```
    枚举
```ts
    enum Color{
        Red,
        Green,
        Blue
    }
    Color默认的对应的枚举值为0,1,2，可以通过点的方式取到枚举值，也可以通过枚举值找到对应的名字值
```
    any
```ts
    用了any类型了 typescript就不做类型检查了
    let list:any[] =  [1,'yee',false]
    let notSure:any = 4
    notSure = 'maybe'
```
    void
```ts
    表示没有任何类型
    function warn():void {   没有任何返回值的函数
        console.log('this is my waring message')
    }
```
    null和undefined
```ts  是所有类型的子类型
    let u:undefined = undefined
    let u:null = null
```
    never和object
```ts
    never类型 必须是不能返回的或者是报错的或不能结束的
    never类型 是任何元素的子类型，其他元素不能赋值给never 
    function error(message:string): never {

    }

    object类型 
    declare function create(o:object | null):void;

```
    类型推断(类型断言或者类型转换)
```ts
    let someValue:any = 'this is a string'
    将any类型强制转换成string类型 第一种语法：<string>
    let strlength:number = (<string>somevalue).length  用<string>someValue 就将someValue强制转换成string类型
    第二种语法 强制转换成string类型
    (someValue as string)
```



***接口
    值所对应的结构进行类型检查，会检查我们的类型和某种结构匹配
    接口的作用：为类型命名和为代码定义契约
    1-接口初探
```ts
    interface LabelledValue {
    label: string;
    }

    function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
    }

    let myObj = {size: 10, label: "Size 10 Object"};
    printLabel(myObj);
    一个参数，并要求这个对象参数有一个名为label类型为string的属性
    类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以
```

    2-可选属性
```ts
    interface Square {
        color:string,
        area:number
    }
    interface SquareConfig {
        color?:string,    问号表示这个属性是可选属性可以有也可以没有
        width?:number
    }
    function createSquare(config:SquareConfig):Square {
        let newSquare = {color:'white',area:100}
        if(config.color){
            newSquare.color = config.color
        }
        return newSquare
    }
    let mySquare = createSquare(config:{color:'black'})

```
    3-只读属性
```ts
方法一：
    interface Point {   
        readonly X:number  定义接口的时候属性前面加上readonly就可以了
        readonly y:number
    }
方法二：
    可以通过赋值一个对象字面量来构造一个Point，赋值后x,y再也不能被改变
    let p1:Point = {x:10,y:20} p1是创建出来的只读属性
    所以不可以修改p1
方法三：
    ***泛型只读数组 rea
    let a:number[] = [1,2,3,4]
    let rea:ReadonlyArray<number> = a
    rea as number[] 
    然后rea就不可以修改了
    可以通过断言改变只读数组的类型，其他方法不行
```
    4-额外属性检查
    5-函数类型
```ts
    对函数的参数名来说和接口定义的属性参数不一定一样，只要保证对应的参数类型是一样的就可以了
    interface SeachFunc {
        (source:string,subString:string):boolen
    }
    let mySearch:SearchFunc  
    mySearch = function(src:string,sub:string):boolen{  
        let result = src.search(sub)
        return result > 1
    }
```
    6-可索引类型
```ts   
    可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型
    TypeScript支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型
    interface StringArray {
        数字的索引
        [index:number]:string  
    }
    let myArray:StringArray
    myArray = ['bob','fred']
    let myStr:string = myArray[0]


```
    7-类类型
```ts
    interface ClockConstructor { 定义构造器的接口或者静态部分的接口
        new (hour: number, minute: number): ClockInterface;
    }
    interface ClockInterface {  定义实例接
        tick();
    }

    function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
        return new ctor(hour, minute);
    }
implements ColocInterface 这个意思--->不是DiaitalClock本身（类的静态部分）应该符合接口规则
而是 --->类 DigitalClock 实例化出来的对象（类的实例部分）应该满足这个接口的规则
    class DigitalClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("beep beep");
        }
    }
    class AnalogClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("tick tock");
        }
    }

    let digital = createClock(DigitalClock, 12, 17);
    let analog = createClock(AnalogClock, 7, 32);
```
    8-继承接口
```ts
    interface Square {
        color:string
    }
    interface PenStroke {
        penwidth:number
    }

    interface Shape extends Square,PenStroke{
        sidewidth:number
    }

    let suar = {} as Shape
    suar.color = 'red'
    suar.penwidth = 10
    suar.sidewidth = 80
```
    9-混合类型

    10-接口继承类



***类
    继承 
```ts
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}
class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}
let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");
sam.move();
tom.move(34);
这个例子演示了如何在子类里写父类的方法，snake类和horse类都创建了move方法，它们重写了从animal继承来的move方法，使得move方法根据不同的类而具有不同的功能。注意即使tom被声明为animal类型，但因为他的值是hourse，调用tom.move(34)时,它会调用hourse里重写的方法

```

    公有，私有与保护的修饰符
```ts
    默认为public


    设置私有private
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

class Rhino extends Animal {
    constructor() { super("Rhino"); }
}

class Employee {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

let animal = new Animal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
animal = employee; // 错误: Animal 与 Employee 不兼容.
因为animal和Rhino共享了来自animal里面私有成员定义 private name:string,因此它们是兼容。
当把Employee赋值给animal的时候，得到一个错误，说它们的类型不兼容，尽管employee里也有一个私有成员name，但它明显不是animal里面定义的那个

问题：构造函数被标记成protected，这意味着这个类不能在包含它的类外被实例化，但是能被继承
```

    readonly修饰符
```ts
    class Outpus{
        readonly name:string;
        readonly numberoflegs:number = 8;
        constructor (theName:string){
            this.name = theName
        }
    }
    let dad = new Outpus('strong')
    dad.name = 'week'   这里就会报一个错，name是只读的
```

    参数属性
```ts
    class Octpus{
        readonly numberofleg:number = 0;
        constructor(readonly name:string){

        }
    }
    仅仅将原来的name属性直接写到了狗仔函数的参数当中
```

    存取器
```ts
let passcode = "secret passcode";
class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }
    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}
let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
存取器的要求你将编译器设置为输出ES5或更高，不支持降级到ES3，其次，只带有get不带有set的存取器自动推断为readonly。类似于拦截器的原理
```

    静态属性
```ts
class Grid {
    constructor(public scale: number) { }
}
编译之后  留意构造函数上的转变
var Grid = /** @class */ (function () {
    function Grid(scale) {
        this.scale = scale;
    }
    return Grid;
}());
```


    ***类型推论
```ts
    最佳通用类型

let x={0,1,null}
为了推断x类型，我们需要考虑所有元素的类型，这里有俩种选择：number和null。计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型

有些时候没有一个候选类型可以作为通用类型，如果没有找到最佳通用类型的话，类型推断的结果为联合数组类型；
```

```ts
    上下文类型

    函数表达式有明确的参数类型注解，那么上下文类型被忽略。
```


    ***高级类型
```ts
    交叉类型
    把俩个类型通过extend拼接到一起，我们称之为交叉类型

```

```ts
    联合类型
    几种类型之一，用 | 表示

interface Bird {
    fly()
    layEagg()
}
interface Fish{
    swim()
    layEagg()
}
function getSmallPet():Fish | Bird{

}
let pet = getSmallPet()
pet.layEagg() pet只能访问Bird和Fish的公有部分layEagg,访问其的会报错。因为不知道类型是Bird还是FIsh
```

```ts
    类型保护

    1-通过类型谓词进行保护  类型谓词是parameterName is Type这种形式
function isFish(pet:Fish | Bird):pet is Fish {
    return (<Fish>pet).swim !==undefined
}
if(isFish(pet)){
    传入pet，是Fish类型的进入，进入else肯定是Bird类型
}else{}

    2-通过typeof类型保护
function padLeft(value:string,padding:string | number){
    if(typeof padding === 'number'){

    }
    if(typeof padding === 'string'){

    }
}


    3-instanceof 类型保护
    跟原型上差不多
```


    可以为null的类型
