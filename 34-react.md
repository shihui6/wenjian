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