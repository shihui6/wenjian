###react
    **安装：使用 create-react-app 快速构建 React 开发环境
        1:cnpm install -g create-react-app(全局安装create-react-app)
        2:create-react-app my-app(用create-react-app快速创建react项目)
    
    **react元素渲染
        1：将元素渲染到dom中
            概念：我们在一个HTML页面中添加一个id="example"的<div>,在此div中的所有内容将由React DOM来管理，所以我们将其称为根节点
        2：用法：
            概念：要将React元素渲染到根节点中，我们通过把他们都传递给ReactDOM.render()的方法来将其渲染到页面上
            实例：

                ```js
                    //创建页面元素
                    const element = <h1>hello,world</h1>
                    // 将创建的元素添加到页面的根节点example里
                    ReactDOM.render(
                        element,
                        document.getElementById('example')
                    )
                ```
                            
        3：更新元素渲染
            3-1：概念：React元素都是不可变的。当元素被创建之后，你是无法改变其内容或属性的
            3-2：解决方法：目前更新界面的唯一方法是创建一个新的元素，然后将它传入ReactDOM.rener()方法
        
    **React JSX
        1：概念：React使用JSX来代替常规的JavaScript，JSX是一个看起来很像XML的JavaScript语法扩展

        2：JSX优点：
            2-1：JSX执行更快，因为它在编译为JavaScript代码后进行了优化
            2-2：它是类型安全的，在编译过程中就能发现错误
            2-3：使用JSX编写模板更加简单快速

        3：JSX原理：
            3-1：JSX是在js内部实现的(jsx就是js)；我们知道元素是构成React应用的最小单位，JSX就是用来声明React当前的元素；
            3-2：与浏览器的DOM元素不同，React当中的元素事实上是普通的对象，React DOM可以确保浏览器DOM的数据内容与React元素保持一致；
            3-3：要将React元素渲染到根DOM节点中，我们通过把它们都传递给ReactDOM.render()的方法来将元素渲染到页面上；

        4：注意点：
            由于JSX就是js，一些标识符像class和for不建议作为XML属性名。作为替代，ReactDOM使用className和htmlFor来做对应的属性

        5：用法：
            事例：

            ```js
                ReactDOM.render(
                    <div>
                        <h1>菜鸟教程</h1>
                        <h2>欢迎学习 React</h2>
                        <p data-myattribute = "somevalue">这是一个很不错的 JavaScript 库!</p>
                    </div>
                    ,
                    document.getElementById('example')
                );
            ```
            注意点：我们可以在以上代码中嵌套多个HTML标签，需要用一个div元素包裹它，实例中的p元素添加了自定义属性data-myattribute，添加自定义属性需要使用data-前缀

        6：js表达式
            6-1:概念：在JSX中使用js表达式。表达式写在花括号{}中

            6-2:注意点：在JSX中不能使用if-else语句，但可以使用三元表达式来替代
            事例：

            ```js
                ReactDOM.render(
                    <div>
                        <h1>{i == 1 ? 'True!' : 'False'}</h1>
                    </div>
                    ,
                    document.getElementById('example')
                );
            ```
            
        7:样式：
            7-1:概念：React推荐使用内联样式。我们可以使用camelCase(驼峰)语法来设置内联样式。React会在指定元素数字后自动添加px
            事例：演示了为h1元素添加mystyle内联样式

                ```js
                    var myStyle = {
                        fontSize: 100,
                        color: '#FF0000'
                    };
                    ReactDOM.render(
                        <h1 style = {myStyle}>菜鸟教程</h1>,
                        document.getElementById('example')
                    );
                ```

        8:注释：
            8-1:概念：注释需要写在花括号中
            实例：

                ```js
                    ReactDOM.render(
                        <div>
                        <h1>菜鸟教程</h1>
                        {/*注释...*/}
                        </div>,
                        document.getElementById('example')
                    );
                ```

        9:数组
            9-1:概念：JSX允许在模板中插入数组，数组会自动展开所有成员
            实例：

            ```js
                var arr = [
                <h1>菜鸟教程</h1>,
                <h2>学的不仅是技术，更是梦想！</h2>,
                ];
                ReactDOM.render(
                <div>{arr}</div>,
                document.getElementById('example')
                );
            ```

    **React组件
        实例：

            ```js
                function HelloMessage(props) {
                    return <h1>Hello World!</h1>;
                }
                
                const element = <HelloMessage />;
                
                ReactDOM.render(
                    element,
                    document.getElementById('example')
                );
            ```
            实例分析：
                我们可以使用函数定义一个组件

                ```js
                    function HelloMessage(props) {
                        return <h1>Hello World!</h1>;
                    }
                ```
                也可以使用es6 class来定义一个组件

                ```js
                    class Welcome extends React.Component {
                        render() {
                            return <h1>Hello World!</h1>;
                        }
                    }
                ```

        1：注意点：原生HTML元素以小写字母开头，而自定义的React类名以大写字母开头，比如HelloMessage不能写成helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错

        2：如果我们需要向组件传递参数，可以使用this.props对象
            事例：  

            ```js
                function HelloMessage(props) {
                    return <h1>Hello {props.name}!</h1>;
                }
                
                const element = <HelloMessage name="Runoob"/>;
                
                ReactDOM.render(
                    element,
                    document.getElementById('example')
                );
            ```
            注意点：在添加属性时，class属性需要写成className,for属性需要写成htmlFor，这是因为class和for是js的保留字


        3：复合组件
            3-1:概念：我们可以通过创建多个组件来合成一个组件，即把组件的不同功能点进行切换
                事例：

                ```js
                    function Name(props) {
                        return <h1>网站名称：{props.name}</h1>;
                    }
                    function Url(props) {
                        return <h1>网站地址：{props.url}</h1>;
                    }
                    function Nickname(props) {
                        return <h1>网站小名：{props.nickname}</h1>;
                    }
                    function App() {
                        return (
                        <div>
                            <Name name="菜鸟教程" />
                            <Url url="http://www.runoob.com" />
                            <Nickname nickname="Runoob" />
                        </div>
                        );
                    }
                    
                    ReactDOM.render(
                        <App />,
                        document.getElementById('example')
                    );
                ```

    **React State(状态)
        1：概念：React把组件看成是一个状态机。通过与用户的交互，实现不同状态，然后渲染UI，让用户界面和数据保持一致；React里，只需更新组件的state，然后根据新的state重新渲染用户界面(不需要操作DOM)(只操作数据，不需要操作DOM)

        实例：

            ```js
                class Clock extends React.Component {
                constructor(props) {
                    super(props);
                    this.state = {date: new Date()};
                }
                
                render() {
                    return (
                    <div>
                        <h1>Hello, world!</h1>
                        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
                    </div>
                    );
                }
                }
                
                ReactDOM.render(
                <Clock />,
                document.getElementById('example')
                );
            ```
            实例解释：实例创建一个名称扩展为React.Component的ES6类，在render()方法中使用this.state来修改当前的时间。添加一个类构造函数来初始化状态this.state，类组件应始终使用props调用基础构造函数
    
        2：生命周期
            定义：(三个阶段)
                创建阶段
                运行阶段
                销毁阶段

                创建阶段：创建阶段经过的五个小阶段
                    this.state = {}
                            因为class类中，只要new这个类，必然会调用constructor构造器，就会执行props创建一个默认值this.state = {}，this.state = {}用来初始化组件的私有数据的，它定义在组件的constructor构造函数中。
                    componentWillMount()：这个阶段相当于vue中的created()生命周期
                            这个阶段组件将要被创建，但还没被创建，我们的数据将在这个阶段拿到。当执行到这个生命周期函数的时候，即将要开始渲染内存中的虚拟DOM，但是页面上尚未真正的显示DOM元素
                    render()
                            创建虚拟DOM
                    componentDidMount()：相当于vue中的mounted()生命周期
                            将虚拟DOM渲染到页面上，在这个函数中，我们可以放心的操作页面上需要的DOM元素(想要操作DOM元素，最早只能在componentDidMount()中操作)
                
                运行阶段的生命周期函数
                    this.setState()
                            更改了state值，页面就会自动更新(通过状态或属性的改变，都会触发组件的更新)
                    componentWillReceiveProps
                            只有当外界传递给组件的属性被修改了，才会触发这个钩子函数


    **React Props
        概念：state和props主要的区别在于props是不可变的，而state可以根据与用户交互来改变。这就是为什么有些容器组件需要定义state来更新和修改数据。而子组件只能通过props来传递数据

        1：使用props
            实例：

            ```js
                function HelloMessage(props) {
                    return <h1>Hello {props.name}!</h1>;
                }
                
                const element = <HelloMessage name="Runoob"/>;
                
                ReactDOM.render(
                    element,
                    document.getElementById('example')
                );
            ```
                实例解析：实例中name属性通过props.name来获取
        
        2：默认props
            概念：通过组件类的defaultProps属性为props设置默认值
                实例：

                    ```js
                        class HelloMessage extends React.Component {
                            render() {
                                return (
                                <h1>Hello, {this.props.name}</h1>
                                );
                            }
                        }
                        // 通过组件类的defaultProps属性为props设置默认值
                        HelloMessage.defaultProps = {
                            name: 'Runoob'   
                        };
                        
                        const element = <HelloMessage/>;
                        
                        ReactDOM.render(
                            element,
                            document.getElementById('example')
                        );
                    ```

        3：state和props
            实例：

                ```js
                    class WebSite extends React.Component {
                        constructor() {
                            super();
                            
                                this.state = {
                                    name: "菜鸟教程",
                                    site: "https://www.runoob.com"
                                }
                            }
                        render() {
                            return (
                            <div>
                                <Name name={this.state.name} />
                                <Link site={this.state.site} />
                            </div>
                            );
                        }
                    }
                    class Name extends React.Component {
                        render() {
                            return (
                                <h1>{this.props.name}</h1>
                            );
                        }
                    }
                    
                    class Link extends React.Component {
                        render() {
                            return (
                                <a href={this.props.site}>
                                    {this.props.site}
                                </a>
                            );
                        }
                    }
                    
                    ReactDOM.render(
                        <WebSite />,
                        document.getElementById('example')
                    );
                ```
                实例解释：实例演示了如何在应用中组合使用state和props。我们可以在父组件中设置state，并通过在子组件上使用props将其传递到子组件上，在render函数中，我们设置name和site来获取父组件传递过来的数据
        
        4：props验证
            4-1：概念：props验证使用propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes提供很多验证器来验证传入数据是否有效，当向props传入无效数据时，js控制台会抛出错误

            4-2：注意点：React.PropTypes在React v15.5版本后已经移到了Prop-types库

                实例：

                ```js
                    // React 16.4版本的实例
                    // MyTitle组件，属性title是必须的且是字符串(如果属性title没有被引用则是可选的不是必须的)，非字符串会控制台会报类型错误
                    var title = "菜鸟教程";
                    // var title = 123;
                    class MyTitle extends React.Component {
                        render() {
                            return (
                                <h1>Hello, {this.props.title}</h1>
                            );
                        }
                    }
                    
                    MyTitle.propTypes = {
                        title: PropTypes.string
                    };
                    ReactDOM.render(
                        <MyTitle title={title} />,
                        document.getElementById('example')
                    );

                    // React 15.4版本实例
                    var title = "菜鸟教程";
                    // var title = 123;
                    var MyTitle = React.createClass({
                        propTypes: {
                            title: React.PropTypes.string.isRequired,
                        },
                    
                        render: function() {
                            return <h1> {this.props.title} </h1>;
                        }
                    });
                    ReactDOM.render(
                        <MyTitle title={title} />,
                        document.getElementById('example')
                    );
                ```

            4-3：更多验证器(以下都是根据16.4版本的验证)

                ```js
                    MyComponent.propTypes = {
                        // 可以声明 prop 为指定的 JS 基本数据类型，默认情况，这些数据是可选的
                            optionalArray: React.PropTypes.array,
                            optionalBool: React.PropTypes.bool,
                            optionalFunc: React.PropTypes.func,
                            optionalNumber: React.PropTypes.number,
                            optionalObject: React.PropTypes.object,
                            optionalString: React.PropTypes.string,
                        
                            // 可以被渲染的对象 numbers, strings, elements 或 array
                            optionalNode: React.PropTypes.node,
                        
                            //  React 元素
                            optionalElement: React.PropTypes.element,
                        
                            // 用 JS 的 instanceof 操作符声明 prop 为类的实例。
                            optionalMessage: React.PropTypes.instanceOf(Message),
                        
                            // 用 enum 来限制 prop 只接受指定的值。
                            optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),
                        
                            // 可以是多个对象类型中的一个
                            optionalUnion: React.PropTypes.oneOfType([
                                React.PropTypes.string,
                                React.PropTypes.number,
                                React.PropTypes.instanceOf(Message)
                            ]),
                        
                            // 指定类型组成的数组
                            optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
                        
                            // 指定类型的属性构成的对象
                            optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),
                        
                            // 特定 shape 参数的对象
                            optionalObjectWithShape: React.PropTypes.shape({
                                color: React.PropTypes.string,
                                fontSize: React.PropTypes.number
                            }),
                        
                            // 任意类型加上 `isRequired` 来使 prop 不可空。
                            requiredFunc: React.PropTypes.func.isRequired,
                        
                            // 不可空的任意类型
                            requiredAny: React.PropTypes.any.isRequired,
                        
                            // 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。
                            customProp: function(props, propName, componentName) {
                                if (!/matchme/.test(props[propName])) {
                                    return new Error('Validation failed!');
                                }
                            }
                        }
                    }
                ```

    
    **React事件处理
        1：概念：React元素的事件处理和DOM元素类似。但是有一点语法上的不同
                    1-1：React事件绑定属性的命名采用驼峰式写法，而不是小写
                    1-2：如果采用JSX的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM元素的写法)
                实例：

                ```js
                    // html通常写法
                        <button onclick="activateLasers()">
                            激活按钮
                        </button>

                    // 在React中的写法
                        <button onClick={activateLasers}>
                            激活按钮
                        </button>
                ```
        
        2：在React中另一个不同是你不能使用返回false的方式阻止默认行为，你必须明确的使用preventDefault
            2-1:事例：通常我们在html中阻止链接默认打开一个新页面，
            ```js
                <a href="#" onclick="console.log('点击链接'); return false">
                点我
                </a>
            ```

            2-2:事例：在React中的写法
            ```js
                function ActionLink() {
                    function handleClick(e) {
                        e.preventDefault();
                        console.log('链接被点击');
                    }
                
                    return (
                        <a href="#" onClick={handleClick}>
                        点我
                        </a>
                    );
                }
            ```
                事例解释：实例中e是一个合成事件；使用React的时候通常你不需要使用addEventListener为一个已创建的DOM元素添加监听器。你仅仅需要在这个元素初始渲染的时候提供一个监听器
            

            2-3:事例：使用es6 class语法来定义一个组件的时候，事件处理器会成为类的一个方法 

                ```js
                    class Toggle extends React.Component {
                        constructor(props) {
                            super(props);
                            this.state = {isToggleOn: true};
                        
                            // 这边绑定是必要的，这样 `this` 才能在回调函数中使用
                            this.handleClick = this.handleClick.bind(this);
                        }
                    
                        handleClick() {
                            this.setState(prevState => ({
                                isToggleOn: !prevState.isToggleOn
                            }));
                        }
                    
                        render() {
                            return (
                                <button onClick={this.handleClick}>
                                    {this.state.isToggleOn ? 'ON' : 'OFF'}
                                </button>
                            );
                        }
                    }
                    
                    ReactDOM.render(
                    <Toggle />,
                    document.getElementById('example')
                    );
                ```
            
        3：向事件处理程序传递参数
            3-1:概念：通常我们会为事件处理程序传递额外的参数

            3-2:事例：给事件参数程序传入id

                ```js
                    <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
                    <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
                ```
                事例解释：上述两种方式是等价的，参数e作为React事件对象将会被作为第二个参数进行传递

            3-3:事例：通过bind方式向监听函数传参，在类组件中定义的监听函数，事件对象e要排在所传递参数的后面

                ```js
                    class Popper extends React.Component{
                        constructor(){
                            super();
                            this.state = {name:'Hello world!'};
                        }
                        
                        preventPop(name, e){    //事件对象e要放在最后
                            e.preventDefault();
                            alert(name);
                        }
                        
                        render(){
                            return (
                                <div>
                                    <p>hello</p>
                                    {/* 通过 bind() 方法传递参数。 */}
                                    <a href="https://reactjs.org" onClick={this.preventPop.bind(this,this.state.name)}>Click</a>
                                </div>
                            );
                        }
                    }
                ```
        
                
    **React条件渲染
        1：概念：在React中，你可以创建不同的组件来封装各种你需要的行为。然后还可以根据应用的状态变化只渲染其中一部分
            事例：创建Greeting组件，它会根据用户是否登录来显示其中之一

            ```js
                function Greeting(props) {
                    const isLoggedIn = props.isLoggedIn;
                    if (isLoggedIn) {
                        return <UserGreeting />;
                    }
                    return <GuestGreeting />;
                }
                
                ReactDOM.render(
                    // 尝试修改 isLoggedIn={true}:
                    <Greeting isLoggedIn={false} />,
                    document.getElementById('example')
                );
            ```

        2：元素变量
            概念：使用元素变量来存储元素。它可以帮助你有条件的渲染组件的一部分，而输出的其他部分不会更改

        3：与运算符&&
            概念：通过用花括号包裹代码在JSX中嵌入任何表达式，也包括js的逻辑与&&，它可以方便地渲染一个元素
            事例：

            ```js
                function Mailbox(props) {
                    const unreadMessages = props.unreadMessages;
                    return (
                        <div>
                        <h1>Hello!</h1>
                        {unreadMessages.length > 0 &&
                            <h2>
                            您有 {unreadMessages.length} 条未读信息。
                            </h2>
                        }
                        </div>
                    );
                }
                
                const messages = ['React', 'Re: React', 'Re:Re: React'];
                ReactDOM.render(
                    <Mailbox unreadMessages={messages} />,
                    document.getElementById('example')
                );
            ```
            事例解释：在js中，true&&expression总是返回expression，而false&&expression总是返回false。因此，如果条件是true，&&右侧的元素会被渲染，如果是false，React会忽略并跳过它

        4：三元运算符
            事例：

            ```js
                render() {
                    const isLoggedIn = this.state.isLoggedIn;
                    return (
                        <div>
                        {isLoggedIn ? (
                            <LogoutButton onClick={this.handleLogoutClick} />
                        ) : (
                            <LoginButton onClick={this.handleLoginClick} />
                        )}
                        </div>
                    );
                }
            ```

        5：阻止组件渲染
            概念：在极少数情况下，你可能希望隐藏组件，即使它被其他组件渲染。让render方法返回null而不是它的渲染结果即可实现
            事例：

            ```js
                function WarningBanner(props) {
                    if (!props.warn) {
                        return null;
                    }
                    
                    return (
                        <div className="warning">
                        警告!
                        </div>
                    );
                }
                
                class Page extends React.Component {
                    constructor(props) {
                        super(props);
                        this.state = {showWarning: true}
                        this.handleToggleClick = this.handleToggleClick.bind(this);
                    }
                    
                    handleToggleClick() {
                        this.setState(prevState => ({
                        showWarning: !prevState.showWarning
                        }));
                    }
                    
                    render() {
                        return (
                        <div>
                            <WarningBanner warn={this.state.showWarning} />
                            <button onClick={this.handleToggleClick}>
                            {this.state.showWarning ? '隐藏' : '显示'}
                            </button>
                        </div>
                        );
                    }
                }
                
                ReactDOM.render(
                    <Page />,
                    document.getElementById('example')
                );
            ```
            事例执行注意点：组件的render方法返回null并不会影响该组件生命周期方法的回调。例如，componentWillUpdate和componentDidUpdate依然可以被调用

    
    **React列表&keys
        用法：使用map()方法创建列表
        实例：使用map()方法遍历数组生成一个1到5的数字列表

        ```js
            function NumberList(props) {
                const numbers = props.numbers;
                const listItems = numbers.map((number) =>
                    <li key={number.toString()}>
                    {number}
                    </li>
                );
                return (
                    <ul>{listItems}</ul>
                );
            }
            
            const numbers = [1, 2, 3, 4, 5];
            ReactDOM.render(
                <NumberList numbers={numbers} />,
                document.getElementById('example')
            );
        ```
        实例注意点：每个列表元素分配一个key，不然会出现警告a key should be provied for list itmes,意思就是需要包含key

        1：keys
            1-1：概念：keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化。因此你应当给数组的每一个元素赋予一个正确的标识
            1-2：用法：
                    1-2-1:一个元素的key最好是这个元素再列表中拥有的一个独一无二的字符串。通常，我们使用来自数据的id作为元素的key
                    ```js
                        const todoItems = todos.map((todo) =>
                            <li key={todo.id}>
                                {todo.text}
                            </li>
                        );
                    ```
                    1-2-2:当元素没有确定的id时，你可以使用他的序列号索引index作为key
                    ```js
                        const todoItems = todos.map((todo, index) =>
                            // 只有在没有确定的 id 时使用
                            <li key={index}>
                                {todo.text}
                            </li>
                        );
                    ```
                    注意点：如果列表可以重新排序，我们不建议使用索引来进行排序，因为这会导致渲染变的很慢
        
        2：用keys提取组件
            概念：当你在map()方法的内部调用元素时，你最好随时记得为每一个元素加上一个独一无二的key
            实例：

            ```js
                function ListItem(props) {
                    // 对啦！这里不需要指定key:
                    return <li>{props.value}</li>;
                }
                
                function NumberList(props) {
                    const numbers = props.numbers;
                    const listItems = numbers.map((number) =>
                        // 又对啦！key应该在数组的上下文中被指定
                        <ListItem key={number.toString()}
                                value={number} />
                    
                    );
                    return (
                        <ul>
                            {listItems}
                        </ul>
                    );
                }
                
                const numbers = [1, 2, 3, 4, 5];
                ReactDOM.render(
                    <NumberList numbers={numbers} />,
                    document.getElementById('example')
                );
            ```
                实例注意点：你应该把key保存在数组的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上
                错误示范的事例：

                ```js
                    function ListItem(props) {
                        const value = props.value;
                        return (
                            // 错啦！你不需要在这里指定key:
                            <li key={value.toString()}>
                                {value}
                            </li>
                        );
                    }

                    function NumberList(props) {
                        const numbers = props.numbers;
                        const listItems = numbers.map((number) =>
                            //错啦！元素的key应该在这里指定：
                            <ListItem value={number} />
                        );
                        return (
                            <ul>
                                {listItems}
                            </ul>
                        );
                    }

                    const numbers = [1, 2, 3, 4, 5];
                    ReactDOM.render(
                        <NumberList numbers={numbers} />,
                        document.getElementById('example')
                    );
                ```
            
        
        3:元素的key在他的兄弟元素之间应该唯一
            3-1：概念：数组元素中使用的key在其兄弟之间应该是独一无二的。然而，他们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的键
            事例：

            ```js
                function Blog(props) {
                    const sidebar = (
                        <ul>
                            {props.posts.map((post) =>
                                <li key={post.id}>
                                    {post.title}
                                </li>
                            )}
                        </ul>
                    );
                    const content = props.posts.map((post) =>
                        <div key={post.id}>
                            <h3>{post.title}</h3>
                            <p>{post.content}</p>
                        </div>
                    );
                return (
                    <div>
                        {sidebar}
                    <hr />
                        {content}
                    </div>
                );
                }
                
                const posts = [
                    {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
                    {id: 2, title: 'Installation', content: 'You can install React from npm.'}
                ];
                ReactDOM.render(
                    <Blog posts={posts} />,
                    document.getElementById('example')
                );
            ```
            事例解释：sidebar和content两个组件之间是全局的关系，而他们两个组件的内部是兄弟组件

            3-2：注意点：key会作为给React的提示，但不会传递给你的组件。如果你的组件中需要和key相同的值，将其作为属性传递就可以
                事例：
                ```js
                    const content = posts.map((post) =>
                        <Post
                            key={post.id}
                            id={post.id}
                            title={post.title} />
                    );
                ```
                事例解释：Post组件可以读出props.id，但不能读出props.key

        
        4：在jsx中嵌入map()
            概念：JSX允许在大括号中嵌入任何表达式，所以我们可以在map()中这样使用
            事例：

            ```js
                function NumberList(props) {
                    const numbers = props.numbers;
                    return (
                        <ul>
                            {numbers.map((number) =>
                                <ListItem key={number.toString()}
                                        value={number} />
                        
                            )}
                        </ul>
                    );
                }
            ```

    **React组件API
        API分为：
            设置状态：setState
            替换状态：replaceState
            设置属性：setProps
            替换属性：replaceProps
            强制更新：forceUpdate
            获取DOM节点：findDOMNode
            判断组件挂载状态：isMounted

        1：设置状态：setState
            1-1:用法：setState(object nextState[, function callback])
                参数说明：
                    nextState:将要设置的新状态，该状态会和当前的state合并
                    callback：可选参数，回调函数。该函数会在setState设置成功，且组件重新渲染后调用

            1-2:概念：
                不能在组件内部直接修改this.state状态，因为该状态会在调用setState()后被替换
                setState()并不会立即修改this.state,而是创建一个即将处理的state，setState()并不一定是同步的，为了提升性能React会批量执行state和DOM渲染。
                setState()总是会触发一次组件重绘，除非在shouldComponentUpdate()中实现了一些条件渲染逻辑

                实例：

                ```js
                    class Counter extends React.Component{
                        constructor(props) {
                            super(props);
                            this.state = {clickCount: 0};
                            this.handleClick = this.handleClick.bind(this);
                        }
                        
                        handleClick() {
                            this.setState(function(state) {
                                // 这里不能使用state.clickCount++，因为对象中不可以使用++运算符，会报语法错误
                                return {clickCount: state.clickCount + 1};
                            });
                        }
                        render () {
                            return (<h2 onClick={this.handleClick}>点我！点击次数为: {this.state.clickCount}</h2>);
                        }
                    }
                    ReactDOM.render(
                        <Counter />,
                        document.getElementById('example')
                    );
                ```

                 
        2:替换状态：replaceState
            2-1：用法：replaceState(object nextState[, function callback])
                参数说明：
                    nextState，将要设置的新状态，该状态会替换当前的state。
                    callback，可选参数，回调函数。该函数会在replaceState设置成功，且组件重新渲染后调用。

                2-1-2：注意点：replaceState()方法与setState()类似，但是方法只会保留nextState中状态，原state不在nextState中的状态都会被删除。
        
        3：设置属性：setProps
            3-1：用法：setProps(object nextProps[, function callback])
                参数说明：
                    nextProps，将要设置的新属性，该状态会和当前的props合并
                    callback，可选参数，回调函数。该函数会在setProps设置成功，且组件重新渲染后调用

            3-2：概念：
                3-2-1：设置组件属性，并重新渲染组件
                3-2-2：props相当于组件的数据流，它总是会从父组件向下传递至所有的子组件中，当和一个外部的js应用集成时，我们可能会需要向组件传递数据或通知React.render()组件需要重新渲染，可以使用setProps()。
                3-2-3：更新组件，可以在节点上再次调用React.render()，也可以通过setProps()方法改变组件属性，触发组件重新渲染。

        4：替换属性：replaceProps
            4-1：用法：replaceProps(object nextProps[, function callback])
                参数说明：
                    nextProps，将要设置的新属性，该属性会替换当前的props。
                    callback，可选参数，回调函数。该函数会在replaceProps设置成功，且组件重新渲染后调用
                
                4-1-1：注意点：replaceProps()方法与setProps类似，但它会删除原有 props。

        5：强制更新：forceUpdate
            5-1：用法：forceUpdate([function callback])
                参数说明:
                    callback，可选参数，回调函数。该函数会在组件render()方法调用后调用。

            5-2：概念：
                forceUpdate()方法会使组件调用自身的render()方法重新渲染组件，组件的子组件也会调用自己的render()。但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM

            5-3：适用场景：
                forceUpdate()方法适用于this.props和this.state之外的组件重绘（如：修改了this.state后），通过该方法通知React需要调用render()一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用

        6：获取DOM节点：findDOMNode
            6-1：用法：DOMElement findDOMNode()
                参数说明：返回值：DOM元素DOMElement
            
            6-2：概念：
                如果组件已经挂载到DOM中，该方法返回对应的本地浏览器 DOM 元素。当render返回null 或 false时，this.findDOMNode()也会返回null。从DOM 中读取值的时候，该方法很有用，如：获取表单字段的值和做一些 DOM 操作

        7：判断组件挂载状态：isMounted
            7-1：用法：bool isMounted()
                参数说明：返回值：true或false，表示组件是否已挂载到DOM中
            
            7-2：概念和应用场景：
                isMounted()方法用于判断组件是否已挂载到DOM中。可以使用该方法保证了setState()和forceUpdate()在异步场景下的调用不会出错

        
    **React组件生命周期
        1：生命周期的方法有：
            1-1：componentWillMount：在渲染前调用，在客户端也在服务端
            1-2：componentDidMount：在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。如果你想和其他js框架一起使用，可以在这个方法中调用setTimeout,setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)
            1-3：componentWillReceiveProps：在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。
            1-4：shouldComponentUpdate：返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用；可以在你确认不需要更新组件时使用
            1-4：componentWillUpdate：在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用
            1-6：componentDidUpdate：在组件完成更新后立即调用。在初始化时不会被调用
            1-7：componentWillUnmount：在组件从 DOM 中移除之前立刻被调用

    
    **React AJAX
        概念：
            React组件的数据可以通过componentDidMount方法中的Ajax来获取，当从服务端获取数据时可以将数据存储在state中，再用this.setState方法重新渲染UI
            当使用异步加载数据时，在组件卸载前使用componentWillUnmount来取消未完成的请求

            事例：

            ```js
                class UserGist extends React.Component {
                    constructor(props) {
                        super(props);
                        this.state = {username: '', lastGistUrl: ''};
                    }
                
                    
                    componentDidMount() {
                        this.serverRequest = $.get(this.props.source, function (result) {
                            var lastGist = result[0];
                            this.setState({
                                username: lastGist.owner.login,
                                lastGistUrl: lastGist.html_url
                            });
                        }.bind(this));
                    }
                    
                    componentWillUnmount() {
                        this.serverRequest.abort();
                    }
                    
                    render() {
                        return (
                        <div>
                            {this.state.username} 用户最新的 Gist 共享地址：
                            <a href={this.state.lastGistUrl}>{this.state.lastGistUrl}</a>
                        </div>
                        );
                    }
                }
                
                ReactDOM.render(
                <UserGist source="https://api.github.com/users/octocat/gists" />,
                document.getElementById('example')
                );
            ```


    **React表单与事件
        1:概念：
            在HTML当中像<input>,<textarea>,和<select>这类表单元素会维持自身状态，并根据用户输入进行更新。但在React中，可变的状态通常保存在组件的状态属性中，并且只能用setState()方法进行更新
        
            实例：

            ```js
                class HelloMessage extends React.Component {
                    constructor(props) {
                        super(props);
                        this.state = {value: 'Hello Runoob!'};
                        this.handleChange = this.handleChange.bind(this);
                    }
                    //输入框输入的信息，会通过参数event传
                    handleChange(event) {   
                        this.setState({value: event.target.value});
                    }
                    render() {
                        var value = this.state.value;
                        return <div>
                                <input type="text" value={value} onChange={this.handleChange} /> 
                                <h4>{value}</h4>
                            </div>;
                    }
                }
                ReactDOM.render(
                    <HelloMessage />,
                    document.getElementById('example')
                );
            ```
            实例解释：在实例中我们设置了输入框input值value={this.state.data}。在输入框值发生变化时我们可以更新state。我们可以使用onChange事件来监听input的变化，并修改state


        2:Select下拉菜单
            注意点：在React中，不使用selected属性，而在根select标签上用value属性来表示选中项
            事例：

            ```js
                class FlavorForm extends React.Component {
                    constructor(props) {
                        super(props);
                        this.state = {value: 'coconut'};
                    
                        this.handleChange = this.handleChange.bind(this);
                        this.handleSubmit = this.handleSubmit.bind(this);
                    }
                    
                    handleChange(event) {
                        this.setState({value: event.target.value});
                    }
                    
                    handleSubmit(event) {
                        alert('Your favorite flavor is: ' + this.state.value);
                        event.preventDefault();
                    }
                    
                    render() {
                        return (
                        <form onSubmit={this.handleSubmit}>
                            <label>
                            选择您最喜欢的网站
                            <select value={this.state.value} onChange={this.handleChange}>
                                <option value="gg">Google</option>
                                <option value="rn">Runoob</option>
                                <option value="tb">Taobao</option>
                                <option value="fb">Facebook</option>
                            </select>
                            </label>
                            <input type="submit" value="提交" />
                        </form>
                        );
                    }
                }
                
                ReactDOM.render(
                    <FlavorForm />,
                    document.getElementById('example')
                );
            ```
                
        3：多个表单
            注意点：当你有处理多个input元素时，你可以通过给每个元素添加一个name属性，来让处理函数根据event.target.name的值来选择做什么

        4：React事件
            概念：当你需要从子组件中更新父组件的state时，你需要在父组件通过创建事件，并作为prop传递到你的子组件上
            事例：

            ```js
                class Content extends React.Component {
                    render() {
                        return  <div>
                                <button onClick = {this.props.updateStateProp}>点我</button>
                                <h4>{this.props.myDataProp}</h4>
                            </div>
                    }
                }
                class HelloMessage extends React.Component {
                    constructor(props) {
                        super(props);
                        this.state = {value: 'Hello Runoob!'};
                        this.handleChange = this.handleChange.bind(this);
                    }
                    handleChange(event) {
                        this.setState({value: '菜鸟教程'})
                    }
                    render() {
                        var value = this.state.value;
                        return <div>
                                <Content myDataProp = {value} 
                                updateStateProp = {this.handleChange}></Content>
                            </div>;
                    }
                }
                ReactDOM.render(
                    <HelloMessage />,
                    document.getElementById('example')
                );
            ```

    
    **React Refs
        概念：React支持一种特殊的属性Ref，你可以用来绑定到render()输出的任何组件上
        作用：这个特殊的属性允许你引用render()返回的响应的支撑。这样就可以确保在任何时间总是拿到正确的实例
        用法：
            实例：
                绑定一个ref属性到render的返回值上

                ```js
                    <input ref="myInput" />

                    //在其它代码中，通过this.refs获取支撑实例
                    var input = this.refs.myInput;
                    var inputValue = input.value;
                    var inputRect = input.getBoundingClientRect();
                ```
            
            实例：通过使用this来获取当前React组件，或使用ref获取组件的引用

            ```js
                class MyComponent extends React.Component {
                    handleClick() {
                        // 使用原生的 DOM API 获取焦点
                        this.refs.myInput.focus();
                    }
                    render() {
                        //  当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs
                        return (
                        <div>
                            <input type="text" ref="myInput" />
                            <input
                            type="button"
                            value="点我输入框获取焦点"
                            onClick={this.handleClick.bind(this)}
                            />
                        </div>
                        );
                    }
                }
                
                ReactDOM.render(
                    <MyComponent />,
                    document.getElementById('example')
                );
            ```



###react学习
    概念：致力于更快的DOM渲染和更易于维护的DOM层；最开始，很多人说react是MVC中的V，总结react就是一套为视图而准备的js框架

    **JSX语法解析
        语法：遇到{}按照js语法解析
             遇到<>按照xml语法解析

    **组件化
        1：在简单的html标签中使用组件
            1-1：用法：通过React.createClass()创建组件
            事例：

            ```js
                <body>
                    <div id="root">
                    </div>
                    <script type="text/babel">
                        // 通过React.createClass()创建组件
                        var Hello = React.createClass({
                            render:function(){
                                return (
                                    <div>hello world</div>
                                )
                            }
                        })

                        ReactDOM.render(<Hello />,document.getElementById('root'))
                    </script>
                </body>
            ```
            1-2：事例注意点：组件的首字母必须要大写，不然没用

        2：组件特点：
            2-1：组件和组件之间可以相互嵌套
            2-2：组件本身可以具有业务逻辑(可以在组件里写js逻辑代码)
            2-3：组件跟组件之间可以相互传值，通过this.props.属性名

        3：样式的添加

            ```js
                render()=>{
                    return (
                        <div className="container"> 
                            <span style={{fontSize:'30px'}}></span>
                        </div>
                    )
                }
            ```
            注意点：
                react中添加类用className
                行内样式：style={{fontSize:'30px'}},对象的形式

        4：生命周期：想一下关于生命周期的图
            componentWillMount:组件渲染之前
            componentDidMount:组件渲染之后
            componentWillUnMount:组件卸载之前
            shouldComponentUpdate:状态改变之后执行
            componentWillUpdate:组件即将更新
            componentDidUpdate:组件更新之后
            componentWillReceiveProps:props改变之后执行


