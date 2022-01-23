### React 基础

#### 第一章 简介

* 最基本：

  引入react核心库

  引入react-dom用于支持react能操作DOM

  引入babel，jsx --> js and es6 -> es5

* 如果是通过script引入react(jsx语法)代码，必须`<script type="text/bable" src="xxx"></script>`

* 为什么我们需要jsx而不能用js？除了react官方推荐，具体原因是？

  * ```html
    <script type="text/javascript">
      const VDOM = React.createElement('h1', {id: 'title'}, 'Hello, World'); // 类似document.createElement
      ReactDOM.render(VDOM, document.getElementById('container'));
    </script>
    ```

  * ```html
    <script type="text/babel">
      const VDOM = <h1 id="title">Hello, World</h1>;
      ReactDOM.render(VDOM, document.getElementById('container'));
    </script>
    ```

  * 当需求略微复杂或者有nested component时，用React.createElement创建虚拟DOM会非常繁琐。babel会帮助我们将jsx这种语法糖翻译成js到browser上运行。

* VDOM VS DOM
  * 题外话：可以用debugger在code里打断点； typeof；instanceof
  * virtual DOM：
    * 本质是一个Object
    * 比较轻， 没有真实DOM那么多属性。因为虚拟DOM是在react内部使用，不需要那么多properties
    * 最终会被React转化成真实DOM渲染在页面上
* jsx语法规则
  * 全称 Javascript XML
  * XML 是早期用于存储和传输数据的形式 --> JSON
    * js 内置两个JSON转化的function：parse 和 stringfy
  * 定义是不需要引号，因为它不是插入到innerHTML这种
  * 如果要在tag里使用**js表达式**需要用{}括起来
  * className， not class
  * when inline style -> `style={{height: '200px'}}`  外面那个{}是因为里面是个js object（是js表达式）   style prop expects a mapping from style properties to values   这个跟正常html有区别，那个是string
  * style是camel case，而不再是用 `-` 相连，比如fontSize 而不是font-size
  * jsx不允许多个根标签。（可以用div或者React.Fragment包起来）
  * 标签必须闭合
  * 我们写的都是jsx标签（虽然最终会转化成html标签）
    * If you meant to render a <u>React component</u>, starts its name with an **uppercase** letter.
    * 如果以小写字母开头的，会对应到html里的同名元素。如果对应html无同名元素，则会报错。
    * 如果以小写字母开头的，React会渲染对应的组件。如果没有定义，则报错。
  * 可以帮你遍历array，但是object不行
* 区分：<u>js表达式 vs js语句</u>
  * js表达式一定会返回一个值（undefined也算）--> 如何判断？能不能被赋予一个变量
    * a
    * a + b
    * Demo(1)
    * arr.map() 类似这种会return值的build-in methods
    * function test () {} --> function definition
  * js语句 （各种control flow）
    * if () {}
    * for () {}
    * switch (xx) {case : xxx ;}
* 模块 vs 组件
  * js 模块化
  * 组件：用来实现局部功能效果的代码和资源集合（html，css，js，...）
  * 都是为了代码复用和提高效率

#### 第二章 函数式组件和类式组件

##### 函数式组件  functional component

* 命名必须大写开头，在jsx语法规则里讲过
* 函数必须有返回值
* 渲染时，以标签的形式  -->   `<Demo />`
* babel在编译的时候，会开启严格模式：不允许函数内的this指向window --> undefined
* 执行ReactDOM.render(<Demo />, document.getElementById('id')) 之后？
  * react解析组件标签，找到Demo组件
  * 发现Demo组件是function定义，调用该函数，将返回的虚拟DOM转为真实DOM，渲染到页面上

##### class component

* class复习

  * ```javascript
    // 创建一个类
    class Person {
      // 构造器方法
      constructor(name, age) {
        // 构造器中的this --> 类的实例对象
        this.name = name;
        this.age = age;
      }
      // 一般方法（除了构造器方法以外的方法）
      speak() {
        // speak方法放在那里？ 类的原型对象上，供实例对象使用
        // 通过Person实例调用speak方法时，this指向Person实例
        // this的指向取决于是如何被调用的，比如通过call（从第二个参数开始是函数的参数），apply（第二个参数是array，是函数的参数，区别于call），bind等改变this指向，或者callback function可能丢失this信息
        console.log(`我是${this.name}, 我今年${this.age}岁。`);
      }
    }
    
    const p1 = new Person('apple', 1);
    p1.speak();
    ```

  * ```javascript
    // 继承一个类
    class Student extends Person {
      // 构造器方法
      constructor(name, age, grade) {
        super(name, age); // 如果有构造函数，必须先call super，这个函数其实就是在call父类的构造器
        this.grade = grade;
        this.school = 'abc';
      }
      // override, 重写父类中的函数
      speak() {
        console.log(`我是${this.name}, 我今年${this.age}岁, 我现在读${this.grade}年级。`);
      }
      
      study() {
        console.log('我爱学习。');
      }
    }
    
    const s1 = new Student('apple', 1, '高一');
    s1.speak();
    s1.study();
    ```

  * 总结：

    * 构造器并不是必须要写
    * 子类如果有构造器，必然要先call super
    * 类中所定义的方法，都是放在<u>类的原型对象上，供实例使用</u>

* 类式组件 class component

  * 必须要继承React.Component
  * 可以不写constructor
  * render必须写，并且必须返回一些react element
  * render中的this 是 class的实例对象 --> class的**组件**实例对象
  * 执行ReactDOM.render之后？
    * react解析组件标签，找到组件
    * 发现组件是用类定义的，react会new一个该类的实例，并通过该实例调用到原型上的render方法
    * 将render返回的虚拟DOM转为真实DOM，渲染到页面上

##### 组件三大属性：state, props, ref

* state

  * 初始化

  * react中的事件绑定

    * 对比原生js的事件绑定

      * `dom.addEventListener('click', () => {});`

      * `dom.onclick = () => {};`

      * 在标签中定义：(onclick时直接执行赋予的字符串)

        ```html
        <div onclick="demo()">test</div>
        
        <script style="text/javascript">
        function demo() {}
        </script>
        ```

    * 原生的事件绑定的方式在react都可以用，但是react推荐直接在tag上加
    * react中事件绑定要写成类似onClick这种，并且要写成`onClick={demo}`，因为它是把一个函数定义赋值给onClick，react会帮忙调用它。而原生是`onclick="demo()"`，双引号中的string是click发生时要执行的代码。

  * class里方法的this指向

    * 事件触发时，并不是由组件实例去调用事件处理的回调函数。因此this会丢失（undefined）

    * 用类来解释以上问题：

      ```javascript
      class Person extends React.Component{
        constructor(name) {
          this.name = name;
        }
        speak() {
          // speak方法放在那里？ 类的原型对象上，供实例对象使用
          // 通过Person实例调用speak方法时，this指向Person实例
          console.log(this);
        }
      }
      
      const p1 = Person('a');
      p1.speak(); // 实例调用函数，输出{name: 'a'}
      const x = p1.speak; // 将函数定义赋值
      x(); // 直接调用，输出undefined   --> 解释了为什么事件触发后的回调函数（非实例调用）会失去this
      // 类中的方法默认开启局部的严格模式 'use strict'，所以（非实例调用）它也并没有指向window，而是undefined
      ```

    * <u>**如何解决this的指向问题？**</u>

      * 使用bind给原型对象上的函数传入this 然后再赋值给实例对象

        ```react
        class Weather extends React.Component {
          constructor(props) {
            super(props);
            this.state = {isHot: false};
            this.changeWeather = this.changeWeather.bind(this);
            // 分析：赋值语句先看右边。实例对象没有changeWeather这个函数，然后去原型对象找，在Weather原型上找到changeWeather然后通过bind传入this，构造器内的this就是实例对象本身。通过赋值操作，实例对象也拥有了一个名叫changeWeather的函数（完全可以写另外一个函数名）。这个时候如果log this的话，可以看到实例对象上有这个函数，并且原型上也有一个名字一样的函数。
          }
          changeWeather() { // 存在类的原型对象上，供实例对象使用
            console.log(this);
          }
          render() {
            // 这里onClick被赋予的是实例对象的那个changeWeather因为render比构造器晚被call到，所以我们在点击时能看到正确的this打印出来。
            return <div onClick={this.changeWeather}>xxx</div>;
          }
        }
        // 实例调用函数时，会先从实例对象本身去找，如果没有再沿着原型链去原型对象上找
        ```

      * arrow function

    * setState的使用

      * 不能用this.state.isHot去直接更改组件的状态（从编码角度来看已经改了，但是react不允许）

      * 用setState去更改（位于React.Component的原型对象上，原型链寻找：实例对象 --> 原型对象 --> 继承的原型对象 --> .....）

      * setState接收的第一个参数是partial update（可以是object也可以是function），第二个参数是optional的callback function

      * state的简化方式

        * 类中可以直接写赋值语句，是给实例对象添加一个属性。所以我们的state初始化可以不用写在构造器里

        * 解决指向问题时，可以不用之前那种bind的方法（找到原型对象上的函数，bind(this)，然后再赋给实例对象）。我们可以用箭头函数解决。arrow function自己没有this，他会**找到外侧函数的this作为自己的this**。

          ```react
          // 之前那种标准写法也对，但是我们可以利用赋值语句简化成如下版本：
          class Weather extends React.Component {
            // 初始化状态
            state = {isHot: false};
            
            // 自定义方法-- 要用赋值语句的形式 + 箭头函数（大部分自定义方法都是用于用户互动的回调函数，非实例对象直接调用的情况）
            // 存在 实例对象上
          	//（不能在原型对象上找到changeWeather函数了，之前那个版本在实例对象和原型对象上都能找到这个函数，只不过一个有this一个丢失了this）
            changeWeather = () => {
              console.log(this);
            }
            render() {
              return <div onClick={this.changeWeather}>xxx</div>;
            }
          }
          ```

* props

  * 基本使用： 像html一样，在需要组件时，传入类似attribute结构的key-value pair。在组件内部可以通过this.props以object的形式拿到

    ```react
    class Person extends React.Component{
      render() {
        const {name, age, sex} = this.props;
        
        return (
        	<ul>
            <li>{name}</li>
            <li>{age}</li>
            <li>{sex}</li>
          </ul>
        )
      }
    }
    
    ReactDOM.render(<Person name="a" age="b" sex="c" />, xxx);
    ```

  * props批量传递

    ```react
    const props = {name: 'a', age: 'b', sex: 'c'};
    ReactDOM.render(<Person {...props} />, xxx); 
    // react + babel 允许我们展开对象，但是仅仅适用于标签属性的传递
    ```

    * 复习...展开语法： 可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造字面量对象时, 将对象表达式按key-value的方式展开。

      * 展开数组： `console.log(...[1,2,3]);`

      * 连接数组： `const arr3 = [...arr1, ...arr2];`

      * 在函数中使用：

        ```javascript
        function sum(...arr) {
          console.log(arr); // 输出的是一个数组
          arr.reduce((accumulator, cur) => accumulator + cur, 0);
        }
        
        sum([1,2,3,4,5,6]);
        ```

      * 构造或合并或更改字面量对象时使用展开语法

        ```javascript
        const person = {name: 'aa', age: 1};
        const copy = {...person};
        const update = {...person, name: 'bbb'};
        const sex = {sex: 2}; 
        const merge = {...person, ...sex};
        
        // 原生js不允许展开一个对象！！！！ 因为没有iterator接口
        console.log(...person); // no way! 会报错 只有加上{}进行深拷贝时才允许
        ```

  * 对props进行限制

    *  对传递的标签的类型进行限制
    * 对必要性的限制
    * 默认值
    * 版本15.5之前，PropTypes存在React核心库中；版本15.5之后，有个单独的包来存PropTypes --> prop-types.js

    ```react
    class Person extends React.Component{
      render() {
        const {name, age, sex} = this.props;
        
        return (
        	<ul>
            <li>{name}</li>
            <li>{age}</li>
            <li>{sex + 1}</li>
          </ul>
        )
      }
    }
    
    Person.propTypes = {
      name: PropTypes.string.isRequired,
      age: PropTypes.number,
      sex: PropTypes.string,
      speak: PropTypes.func
    };
    
    Person.defaultProps = {
      age: 18,
      sex: '男'
    };
    
    ReactDOM.render(<Person name="a" age={19} sex="c" />, xxx);
    ```

    * props is read-only.

    * 简写方式（给类自身加属性： static语句）

      ```react
      class Person extends React.Component{
        static propTypes = {
          name: PropTypes.string.isRequired,
          age: PropTypes.number,
          sex: PropTypes.string,
          speak: PropTypes.func
        };
      
      	static defaultProps = {
          age: 18,
        	sex: '男'
        };
      
        render() {
          const {name, age, sex} = this.props;
          
          return (
          	<ul>
              <li>{name}</li>
              <li>{age}</li>
              <li>{sex + 1}</li>
            </ul>
          )
        }
      }
      
      ReactDOM.render(<Person name="a" age={19} sex="c" />, xxx);
      ```

  * 构造器和props

    在react中，通常构造器的用途：

    * 通过this.state来初始化实例对象内部的状态
    * 为事件处理绑定实例

    在为React.Component的子类实现构造器时，应该在其他语句之前调用super(props)，否则在构造函数内调用this.props可能为undefined

    所以在类组件中的构造函数几乎都可以省略，而用简写方式代替。

  * functional component使用props

    ```react
    function Person(props) {
      const {name, sex, age} = props;
      
      return (
      	<ul>
            <li>{name}</li>
            <li>{age}</li>
            <li>{sex + 1}</li>
          </ul>
      );
    }
    
    Person.propTypes = {
      name: PropTypes.string.isRequired,
      age: PropTypes.number,
      sex: PropTypes.string,
      speak: PropTypes.func
    };
    
    Person.defaultProps = {
      age: 18,
      sex: '男'
    };
    
    ReactDOM.render(<Person name="a" age={19} sex="c" />, xxx);
    ```

    在不使用的hooks的情况下，函数式组件只能使用props

* refs

  * ref得到的都是真实的DOM node（代替document.getElementById等方法）

  * 字符串形式的ref（已经过时）--> 效率的问题

    ```react
    class Demo extends React.Component{
      handleClick = () => {
        const {input} = this.refs;
        
        alert(input.value);
      }
      return (
      	<div>
          <input ref="input" placeholder="xxx" type="text" />
          <button onClick={this.handleClick}>click on me</button>
      	</div>
      );
    }
    ```

  * 回调形式的ref

    回调函数的接收参数就是ref所在的的node

    ```react
    class Demo extends React.Component{
      handleClick = () => {    
        alert(this.input.value);
      };
      return (
      	<div>
          <input ref={node => this.input = node} placeholder="xxx" type="text" />
          <button onClick={this.handleClick}>click on me</button>
      	</div>
      );
    }
    ```

    关于回调refs的说明：

    如果 `ref` 回调函数是以**内联函数**的方式定义的，在**更新过程**中它会被执行两次，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素。这是因为在<u>每次渲染时会创建一个*新的函数实例*</u>，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数<u>定义成 class 的绑定函数</u>（不会每次render都创建新的函数实例）的方式可以避免上述问题，但是大多数情况下它是无关紧要的（所以其实真实开发中还是*用内联函数的方式比较多*）。

    ```react
    class Demo extends React.Component{
      handleClick = () => {    
        alert(this.input.value);
      };
      saveInputRef = c => this.input = c;
    
      return (
      	<div>
          <input ref={this.saveInputRef} placeholder="xxx" type="text" />
          <button onClick={this.handleClick}>click on me</button>
      	</div>
      );
    }
    ```

  * 利用createRef创建ref容器

    调用React.createRef之后返回一个容器，可以用来储存被ref标识的node，并且是专人专用的。

    需要通过`this.myRef.current`才能访问到节点。

    ```react
    class Demo extends React.Component{
      myRef = React.createRef();
      handleClick = () => {    
        alert(this.myRef.current.value);
      };
    
      return (
      	<div>
          <input ref={this.myRef} placeholder="xxx" type="text" />
          <button onClick={this.handleClick}>click on me</button>
      	</div>
      );
    }
    ```


##### react事件处理

* 通过onXxx去指定事件处理函数（onClick, onBlur...）
  * React中使用的是自定义的事件，不是使用原生DOM的事件 --> 为了兼容性
  * React中的事件是通过事件委托的方式处理的（委托给最外层元素）--> 为了高效
* 通过event.target得到事件发生的DOM元素对象 --> 避免过度使用ref

##### 非受控组件和受控组件 （uncontrolled vs controlled）

* 非受控（现用现取）--> 可能需要ref，可以用受控组件代替

  ```react
  class Login extends React.Component {
    handleSubmit = event => {
      event.preventDefault(); // 阻止Node的默认行为，此处为阻止表单提交和刷新页面
      alert(`用户名是${this.username.value}， 密码是${this.password.value}。`);
      // 应该用ajax进行表单提交 --> one page app
    }
    render() {
      return (
      	<form onSubmit={this.handleSubmit}>
        	用户名：<input type="text" ref={node => this.username = node} />
          密码：<input type="password" ref={node => this.password = node} />
          <button>提交</button>
        </form>
      );
    }
  }
  ```

* 受控组件。所有输入类DOM的内容（包括更新）都存到state里面，等需要用的时候从state里取。

  ```react
  class Login extends React.Component {
    state = {
      username: '',
      password: ''
    };
    handleSubmit = event => {
      event.preventDefault(); // 阻止Node的默认行为，此处为阻止表单提交和刷新页面
      alert(`用户名是${this.state.username}， 密码是${this.state.password}。`);
      // 应该用ajax进行表单提交 --> one page app
    }
    handleUsernameChange = event => {
      this.setState({username: event.target.value});
    };
  	handlePasswordChange = event => {
      this.setState({password: event.target.value});
    };
    render() {
      return (
      	<form onSubmit={this.handleSubmit}>
        	用户名：<input type="text" onChange={this.handleUsernameChange} />
          密码：<input type="password" onChange={this.handlePasswordChange} />
          <button>提交</button>
        </form>
      );
    }
  }
  ```

##### 高阶函数和函数柯里化

* 复习如何用变量读取/更改对象

  ```javascript
  const a = 'name';
  const obj = {};
  
  obj[a] = 'linfeng'; // {name: 'linfeng'}
  
  saveData = (dataType, value) => this.setState({[dataType]: value});
  ```

* 高阶函数：（满足一下两点之一即可）

  * 如果A是函数，并且接收的参数是一个函数；
  * 如果A是函数，并且调用的返回值依然是一个函数。
  * 常见的高阶函数： Promise, setTimeout, arr.map...

* 函数的柯里化：通过函数调用继续返回函数的方式（高阶函数的第二种），实现多次接收参数最后统一处理的函数编码方式。

  ```javascript
  saveFormData = dataType => event => this.setState({[dataType]: event.target.value});
  ```

* 非函数柯里化的写法：

  ```react
  saveFormData = (dataType, event) => this.setState({[dataType]: event.target.value});
  
  render() {
    return <button onClick={e => this.saveFormData('xxx', e)} />;
  }
  
  /*
  在 render 方法中使用箭头函数也会在每次组件渲染时创建一个新的函数，这会破坏React基于恒等比较的性能优化。
  如果你在render方法里创建函数。因为浅比较 props 的时候总会得到 false。
  在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议使用 class 属性的语法来避免这类性能问题。（写成class method）
  */
  ```

##### 组件的生命周期（重要！）

* 挂载（mount）--> ReactDOM.render(xx,xx)，卸载（unmount）

* 如何卸载一个组件： `ReactDOM.unmountComponentAtNode(the dom node where you want to unmount component)` 参数是需要卸载组件的容器DOM

* componentDidMount

  * 组件挂载完成后执行（一次）

* componentWillUnmount

  * 组件将要卸载时执行

* 第一次挂载经历的生命周期

  * constructor
  * UNSAFE_componentWillMount  -- 组件将要挂载时 (legacy)
  * render
  * componentDidMount
  * 可能 componentWillUnmount

* 组件更新时

  * <img src="/Users/hualinfeng/Desktop/react全家桶资料/02_原理图/react生命周期(旧).png" alt="react生命周期(旧)" style="zoom:60%;" />
  * 第一条路线：
    * setState 状态改变了
    * shouldComponentUpdate -- 控制组件是否更新，如果不写，react默认return true，如果写了就必须要return一个boolean type
    * UNSAFE_componentWillUpdate -- 组件即将更新的钩子(legacy)
    * render
    * componentDidUpdate(prevProps, prevState) -- 组件完成更新后的钩子
    * 可能 componentWillUnmount
  * 第二条路线：
    * forceUpdate() -- 虽然状态没有改变，但是我也想要react帮我更新一下组件（用的不多）
    * 跳过shouldComponentUpdate直接到UNSAFE_componentWillUpdate
    * render
    * componentDidUpdate
    * 可能 componentWillUnmount
  * 第三条路线：
    * 父组件rerender
    * UNSAFE_componentWillReceiveProps 组件即将接收到props时的钩子，并不会在第一次挂载的时候被call到 (legacy)
    * shouldComponentUpdate
    * UNSAFE_componentWillUpdate
    * render
    * componentDidUpdate
    * 可能 componentWillUnmount

* 以上为旧版本的生命周期，标有UNSAFE_前缀的可能会在后续版本中移除

* 常用的钩子

  * componentDidMount 一般做一些初始化的事，例如开启定时器、发送网络请求、订阅消息。。
  * componentWillUnmount 一般做一些收尾clean up的事，例如关闭定时器、取消订阅消息。。
  * render

* 新版本是旧版本的微调，新增两个钩子，淘汰三个钩子（UNSAFE_前缀能suppress warning）

* 在source folder下跑`npx react-codemod rename-unsafe-lifecycles` 可以重命名所有deprecated lifecycles

* 被淘汰的三个钩子可能会在未来新版本的react中更有可能出现bug，所以我们要避免使用他们。（尤其是开启异步渲染之后）

* <img src="/Users/hualinfeng/Desktop/react全家桶资料/02_原理图/react生命周期(新).png" alt="react生命周期(新)" style="zoom:60%;" />

* static getDerivedStateFromProps(props, state)

  * 适用于罕见的用例：state的值在任何时候都取决于props
  * 必须返回一个state obj或者null
  * 要小心使用，因为它不仅是在第一次挂载前被call，当props变化或者state变化或者强制update时都能被call到
    * 因为这个生命周期是静态方法，同时要保持它是纯函数，不要产生副作用
    * 在使用此生命周期时，要注意把传入的 prop 值和之前传入的 prop 进行比较（这个 prop 值最好有唯一性，或者使用一个唯一性的 prop 值来专门比较）。
    * 不使用 getDerivedStateFromProps，可以改成组件保持完全不可控模式，通过初始值和 key 值来实现 prop 改变 state 的情景。

* getSnapshotBeforeUpdate

  * 必须返回一个snapshot value（任何值都可以作为快照值，数组、字符串、对象等等）或者null
  * 在更新之前获取快照。在最近一次渲染输出（更新DOM节点）之前调用，使得组件能在发生更新之前从DOM中获取一些信息（例如滚动位置）。返回值将传递给componentDidUpdate --> componentDidUpdate(prevProps, prevState, snapshot)

  例子：模拟新闻产生，但是要求我阅读的新闻sticky在页面（即使产生新新闻，并且超出内容框，也不移动视图）

  ```react
  // 容器150px，每条新闻高度为30px
  class NewsList extends React.Component {
    state = {newsArr: []};
  
  	componentDidMount() {
      // 模拟每秒产生1条新闻
      setInterval(() => 
        this.setState({newsArr: ['新闻' + (newsArr.length + 1), ...newsArr]})
      , 1000);
    }
  
  	getSnapshotBeforeUpdate() {
      // 记录更改DOM前的高度
      return this.refs.list.scrollHeight;
    }
  
  	componnetDidUpdate(a, b, height) {
      // 计算更新前后的高度变化，使scrollTop动态变化，达到想看的新闻sticky
      this.refs.list.scrollTop += this.refs.list.scrollHeight - height;
    }
  
  	render() {
      return (
        <div className="list" ref="list">
          {this.state.newsArr.map((n, index) => <div key={index} className="news">{n}</div>)}
        </div>
      )
    }
  }
  ```

##### DOM的diffing算法

* react维护VDOM 对应到DOM，每次update时，先计算VDOM有哪些有改变，然后再去对应的DOM node做改变，并不是重新渲染所有DOM。最小力度是标签（节点）。并且是逐层比较，如果是标签里面套标签，有可能里面的标签不需要更新。

* key的作用（经典面试题）-- 虚拟DOM没有value属性

  * key有什么作用？内部原理是什么？

    * 简单地说，key是虚拟DOM的标识，在显示更新时，有很重要的作用
    * 具体地说，当状态中数据发生变化时，react会根据新的数据产生新的虚拟DOM，然后对新的虚拟DOM和旧的虚拟DOM做diff比较，比较规则如下：
      * 旧的虚拟DOM中找到了与新虚拟DOM相同的key：
        * 若虚拟DOM中内容没变，则直接复用之前的真实DOM
        * 若内容改变，则生成新的真实DOM然后替换掉页面中之前的真实DOM
      * 未找到相同的key
        * 根据数据产生新的真实DOM然后渲染到页面

  * 为什么遍历列表时，key最好不要用index？（可以举例说明）可能会引发什么问题？

    ```react
    class Person extends React.Component{
      state = {
        data: [{id: 1, name: 'apple'}, {id: 2, name: 'banana'}]
      };
    
    	render(){
        return (
        	<ul>
            {this.state.data.map((item, index) => <li key={index}>{item.name}<input type="text"></li>)}
          </ul>
        )
      }
    }
    // 考虑如果有一个操作在data最前面加了一个数据，diff算法是如何更新真实DOM的
    ```

    * 若对数据进行逆序添加、逆序删除等破坏顺序的操作，会产生没有必要的真实DOM更新 ==> 效率相对很低
    * 若结构中存在输入类DOM，可能会产生错误的DOM更新 --> UI 有问题，严重的bug
    * 注意！如果不存在对数据进行破坏顺序的才做，仅仅用于渲染列表用于展示，也是可以使用index的。

  * 如何选择key？

    * 最好使用唯一标识作为key，比如id、手机号、身份证号、学号等等
    * 如果只是简单地展示数据，也可以用index

#### 第三章 react应用（脚手架）

* react脚手架

  * 脚手架：用来帮助程序员快速创建一个基于xxx库的模板项目
    * 包含了所有需要的配置（语法检查，jsx编译，devServer等等）
    * dependency
    * 简单的页面效果
  * create-react-app
  * 核心技术构架：react + webpack + es6 + eslint
  * 模块化、组件化、工程化（代码写完之后，剩下的编译打包等都是自动进行）

* 可以考虑使用yarn，因为和react都是fb写的（yarn eject 暴露出webpack的配置文件

* 脚手架文件介绍：

  * node_modules

    * react脚手架为我们预装的第三方库

  * public文件夹

    * 仅存放静态文件（资源，样式css，html，图标

    * favicon.ico 图标文件，app的tab上显示

    * 只有一个html文件：**index.html**

      分析该文件内脚手架为我们准备的代码：

      ```html
      <!DOCTYPE html>
      <html lang="en">
        <head>
          <meta charset="utf-8" />
      		<!-- %PUBLIC_URL%达标public文件夹的路径 -->    
          <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
          <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.4.1/semantic.min.css">
          <!-- 开启理想视口，用于做移动端网页的适配 -->  
          <meta name="viewport" content="width=device-width, initial-scale=1" />
          <!-- 用于配置浏览器标签+地址栏的颜色（仅支持安卓手机浏览器，兼容性不好） -->  
          <meta name="theme-color" content="#000000" />
          <!-- 描述网站信息（搜索引擎） -->  
          <meta
            name="description"
            content="Web site created using create-react-app"
          />
          <!-- 用于苹果手机（将网页添加到主屏幕上后的图标） -->  
          <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
          <!--
            manifest.json provides metadata used when your web app is installed on a
            user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
          -->
          <!-- 如以上英文描述，manifest是应用加壳时的配置文件（应用加壳是在html（web app）外面加一个安卓或者ios的壳生成.apk文件供用户下载使用） -->  
          <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
          <script src="https://apis.google.com/js/api.js"></script>
          <!--
            Notice the use of %PUBLIC_URL% in the tags above.
            It will be replaced with the URL of the `public` folder during the build.
            Only files inside the `public` folder can be referenced from the HTML.
      
            Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
            work correctly both with client-side routing and a non-root public URL.
            Learn how to configure a non-root public URL by running `npm run build`.
          -->
          <title>React App</title>
        </head>
        <body>
          <!-- 若浏览器不支持js则显示标签中的内容 -->  
          <noscript>You need to enable JavaScript to run this app.</noscript>
          <!-- 用于render我们写的react app的容器 -->  
          <div id="root"></div>
          <!--
            This HTML file is a template.
            If you open it directly in the browser, you will see an empty page.
      
            You can add webfonts, meta tags, or analytics to this file.
            The build step will place the bundled scripts into the <body> tag.
      
            To begin the development, run `npm start` or `yarn start`.
            To create a production bundle, use `npm run build` or `yarn build`.
          -->
        </body>
      </html>
      ```

    * robots.txt: 爬虫规则文件。规定哪些数据可以让爬虫爬。

    * 该文件夹下对我们来说最重要的就是root node那个div

  * src文件夹

    * **App.js**, App这个组件是react脚手架准备好的
    * **index.js**是入口文件
      * 在`<app />` 外面包裹一层<React.StrictMode> 可以帮忙检查一下子组件有没有写的不合理的地方
      * 在这个文件里，react脚手架（webpack配置）已经保证我们能找到root那个节点
    * reportWebVitals.js用来记录页面性能，需要web-vitals库支持
    * setupTests.js --> 组件单元测试的文件，需要jest.dom库支持

* 从头开始一个项目的步骤和细节：

  * public, src 文件夹一定要有，public中的index.html一定要写好template
  * 在入口文件index.js要做的：
    * 引入核心库
    * 引入react-dom
    * 渲染App到页面
      * App组件
      * root node
  * 每个组件都拥有自己的文件夹（方便管理和维护）
  * 组件文件名首字母要大写，为了区分组件和其他js文件，可以将拓展名改为jsx
  * 组件和组件样式可以命名为index.jsx，好处是在引入组件时，路径简短一些。（因为在文件夹路径下，如果不specify引入的文件名，默认为index） 如果该组件被分成多个小组件存在同一目录下，其他小组件的名字需要详细写出。

* 样式的模块化

  * className相同时，样式存在冲突

    * 可以通过**Less**写嵌套的样式解决该问题  `.hello {.title: { background-color: red}}`

    * 也可以通过**样式的模块化**解决：

      * ```css
        .title {
          background-color: red
        }
        /* 该css文件命名为 index.module.css */
        ```

        ```jsx
        import React from 'react';
        import hello from './index.module.css'; // 对比之前 import './index.css';
        
        export default Hello = () => {
          return <div className={hello.title}>Hello, React!</div>;
        }; // 对比之前的 className="title"
        ```

* 插件安装：快速创建代码片段（代码模板）rcc rfc.....

* 组件化编码流程

  * 拆分组件
  * 实现静态组件（静态页面的效果）
  * 实现动态组件
    * 动态显示初始化数据
      * 数据类型？ array？object？还是primitive data？
      * 数据名称
      * 保存在哪个组件合适？
    * 交互（从绑定事件开始）


##### **To-do List案例实践（当成面试题写一遍）**

* 从已经写好的html css静态文件，拆分到各个组件
* 初始化数据的显示
  * state lifting up状态提升（把子组件都需要用的状态放到父组件）（后期也可以用消息订阅与发布来实现）
  * checked, defaultChecked（页面刚render时是否check） --> checkbox attribute   小心defaultChecked的坑！！
* keyDown  event.target.value   event.keyCode === 13 (回车)
* 从子(孙)组件给父组件传数据： 从上往下**通过props传函数**， 然后从下往上传数据
* id的值？？？  通用唯一识别码 UUID （GUID是其中最广泛的应用） 这里我们用的是nanoid这个库
* input这种element需要注意很多细节
  * invalid输入？
  * 空的？
  * 需要清空吗？
  * 什么条件才会去call回调函数真正想做的事？
  * 等等等
* onMouseEnter 和 onMouseLeave 实现悬浮效果
* checkbox --> onChange事件  --> event.target.checked 而不是value
* 如果回调函数需要提前传参数，可以把回调函数写成高阶函数。
* 状态数据在哪里，那么操作状态更改的函数（方法）就在那里。 --> 不意味着数据非要在那里用
* prop-types 用以限制props （类型，必要性）
* `window.confirm`是浏览器自带的确认对话框，会根据用户点击确认还是取消返回true or false
* 要熟悉array和object自带的方法！！
* 当做一道面试题自己重写一遍！！
* 总结：
  * 拆分组件、实现静态组件，注意className、style的写法
  * 动态初始化列表，如何确定数据放在哪个组件的state中？
    * 某个组件使用：该组件本身的state
    * 某些组件使用：放到共同的父组件state中
  * 父子通信
    * 从上往下： props
    * 从下往上：props，要求父组件提前给子组件传递一个函数
  * 注意defaultChecked 和 checked的区别 （同理 defaultValue 和 value)
  * 状态数据在哪里，那么操作状态更改的函数（方法）就在那里。

#### 第四章 React ajax

* 说明：

  * React本身只关注于界面, 并不包含发送ajax请求的代码

  * 前端应用需要通过ajax请求与后台进行交互(json数据)

  * react应用中需要集成第三方ajax库(或自己封装)  

    推荐使用axios

    * 封装XmlHttpRequest对象的ajax
    * promise风格
    * 可以用在浏览器端和node服务器端


##### 脚手架**配置代理**

* 因为跨域问题，请求**能发送，但不能取回**数据。CORS policy， ajax引擎会拒绝返回。

* 我们需要代理(proxy)帮我们解决跨域

  * client -> proxy -> server then  server -> proxy -> client
  * **proxy跟client同源**，但是因为proxy没有ajax引擎，不会拦截跨域的请求返回。

* 配置代理方法一  --> 并不是所有请求都转发给5000

  在package.json中加一段配置代码（假设后台服务器开在5000端口）

  ```json
  proxy: "https://localhost:5000"
  ```

  修改需要发请求的地方（客户端开在3000端口）

  ```javascript
  axios.get('https://localhost:3000/students').then(xxx);
  // 这里把端口改成3000，刚刚代理会优先去public文件夹下找资源，如果找到的话直接返回，如果没找到会帮我们转发去https://localhost:5000
  ```

  * 优点是配置简单，并且请求时可以不加任何前缀。问题是只能配置一个代理，如果有多个服务器需要请求就不行了。
  * 工作方式：优先匹配前端资源（当请求了3000不存在的资源时，那么该请求会转发给5000）

* 配置代理方法二（方法一的完整版）

  * 需要在src目录下写`setupProxy.js` (react脚手架会自动找到这个文件)
  * 不能使用es6语法，只能使用common js，因为脚手架会把这个file加到webpack的配置里（node语法）

  ```javascript
  const proxy = require('http-proxy-middleware')
     
  module.exports = function(app) {
    app.use(
      proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
        target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
        changeOrigin: true, //控制服务器接收到的请求头中host字段的值
        /*
           	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
           	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
           	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
           */
        pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
      }),
      proxy('/api2', { 
        target: 'http://localhost:5001',
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      })
    )
  }
  ```

  * 优点是可以配置多个代理，可以灵活的控制请求是否走代理；缺点是配置繁琐，前端请求资源时必须加前缀。

##### github搜索案例

* 成型的css文件（比如bootstrap）一般可以直接放在public下

* <a> 中如果`target="_blank"`，最好带上`rel="noreferrer"`  --> security concern

* 连续解构赋值语法 和 重命名（es6语法要熟悉）

* 为什么这次直接call github的api没有产生跨域问题？ 因为github的**后端**服务器已经通过CORS解决了跨域问题，在response header处理了这个问题。

*  前端解决跨域：配置代理

* 完成api请求之后的数据展示

* 展示区域的考虑

  * users（正常数据）
  * 第一次上来并无数据时的文字？
  * loading？
  * error？ --> error.message

* 注意： Objects are not valid as React child. (if wanting to render a collection, use array instead)

* 之前讲了父子组件之间的交互，那么兄弟组件（其实是任意组件）之间的通信交互如何（不通过父组件）实现？？

  消息订阅与发布机制 （有很多库，比较主流的[pubsub-js](https://github.com/mroderick/PubSubJS)）

  * `const token = Pubsub.subscribe('订阅', (_, data) => console.log(data));`
  * `Pubsub.public('订阅名', data)`
  * `Pubsub.unsubscribe(this.token)`
  * 通过这种方式，我们不需要再像之前一样把List需要的state和Search中更改state的函数存放在父组件App中。可以在List中订阅Search发布的消息。

* fetch发送请求

  * 能发送ajax请求的途径：

    * xhr --> new XMLHttpRequest()   并无封装好的api，非常繁琐
    * jQuery （封装好了，问题是容易形成回调地狱。比如要求第一次成功再发第二个等等）
    * Axios --> 回调地狱可以解决，因为它是**Promise**风格
    * 这两种都是对xhr的封装（服务器端的axios不是对xhr而是对http的封装），并且都是第三方

  * fetch

    * 内置的（window就能access到）
    * Promise风格
    * fetch发送请求的第一次返回是告诉我们服务器是否连接成功，请求数据并不直接在response里（与xhr不同，xhr在报error时包括服务器连接成功但响应可能出错，比如404；而fetch会把404等error认定为服务器连接成功，属于fulfilled）
    * 需要通过response.json()才能获取得到的响应数据 --> response.json() 返回一个promise（此时才会对404报错）
    * Promise规则（熟悉）：
      * 如果成功或失败返回一个promise，则该promise的返回值直接作为返回值，该promise的状态成为下一个链接的状态；如果返回值不为promise，则该返回值作为返回值，且状态为成功。

    ```javascript
    fetch(url).then(
    	response => response.json(),
      error => {
        console.log('连接服务器出错', error);
        return new Promise(() => {}); // 如果不写这行，返回值为undefined，也会进入下一个then链，因为这种情况下，promise状态为成功
      }
    ).then (
    	response => console.log('数据请求成功', response),
      error => console.log('数据请求失败', error)
    );
    
    // 错误集中处理
    fetch(url).then(
    	response => response.json()
    ).then (
    	response => console.log('数据请求成功', response)
    ).catch(
    	error => console.log(error)
    )
    
    // async/await 语法
    try {
     	const response = await fetch(url);
      const data = await response.json();
      console.log(data);
    } catch(error) {
      console.log('请求出错', error);
    }
    ```

    * chrome的dev tool已经有fetch的tab了
    * 符合关注分离（Separation of concerns，SoC）的原则（xhr不符合）
    * 目前用的还是不多，兼容性一般。

#### 第五章 React路由（非常重要）

* 相关理解

  * SPA single page application
    * 只有一个完整的页面
    * 点击页面中的链接不会刷新页面，只会局部更新
    * 数据都要通过ajax请求获取，并在前端异步展示
  * 路由的理解
    * 什么是路由？
      * 一个路由就是一个映射关系  key  value pairs
      * key为path，value可能为function或component
    * 路由分类
      * 后端路由
        * 理解： value是function，用来处理客户端的请求
        * 注册路由： router.get(path, function(req,res))
        * 工作过程：当node接收到一个请求时，根据请求路径找到匹配的路由，调用路由中的函数来处理清理，返回响应数据
      * 前端路由
        * 浏览器端路由，value是component，用于展示页面内容
        * 注册路由： <Route path="/test" component={Test} />
        * 工作过程：点击link，改变地址 --> path改成 /test，当前路由组件就会变为Test组件
    * **工作原理**
      * 方法一：**利用了BOM(window对象)申上的history** （H5推出的history身上的API）
        * history.createBrowserHistory()
      * 浏览器的history是个栈的结构，谁在栈顶，就在那个栈顶的path上
      * 因为原生的history API相对难操作，我们在课上引入了另一个库 history.js
        * history.push 往历史栈中压入一条历史记录 （注意需要return false去取消a tag的默认跳转行为）--> 点击link形成路径的改变
        * history.replace 替换历史栈的栈顶元素
        * hostory.goBack 相当于浏览器的后退
        * history.goForward 相当于浏览器的前进
        * history.listen(location => {xxx})  监听history的改变  --> 监听到路径改变，去匹配需要展示的组件
      * 方法二：利用hash值（锚点跳转  href="#tag1"）
        * 兼容性非常好，但是path略丑，hostname:portname/xxx**#/path**
      * `前端路由的基石_history` 供参考


##### react-router-dom (用于web开发)

* react的插件库，基本上react开头的库都是官方出的

* 最基本的使用

  * 明确好导航区和展示区
  * <Link> 和 <Route> 组件必须包裹在同一个Router里面（要由同一个路由器去监听路径变化并决定展示哪个组件。 路由器有 BrowserRouter, HashRouter
  * `<Link to="/home">Home</Link>`编写路由链接  --> 导航区
  * `<Route path="/home" componenet={Home} />  ` 注册路由  --> 展示区
  * 我们可以把整个App组件包裹在Router里。
  * HashRouter，地址里会出现一个#号，#号后面的内容不会作为资源发送给服务器

* 路由组件和一般组件

  * 写法不同：一般组件 直接渲染；路由组件需要通过注册路由然后由路由器决定是否渲染
  * 存放文件夹不同：一般组件 components; 路由组件 pages
  * 接收到的props不同（最大区别）：
    * 一般组件是传什么props就接收到什么props
    * 路由组件会接收到3个固定的props （history，location，match），常用的：
      * history: go, goBack, goForward, push, replace
      * location: pathname, search, state
      * match: params, path, url

* NavLink组件：Link的升级版，可以实现路由链接的高亮

  * 会给选中的link自动增加一个叫active的className
  * 另外还有一个属性叫activeClassName， 会在link选中(active时)的时候，使用该样式（如果我们写了active的样式，就不需要再额外写这个属性了。

* 封装NavLink

  * 目标：使NavLink更好用，不用烦琐地为每一个NavLink赋activeClassName 和 className
  * 关键点：标签体内容是一个特殊的标签属性 --> **props.children**

  ```react
  const MyNavLink = props => <NavLink activeClassName="xxx" className="bbb" {...props} />; // 可以将标签体内容直接通过props传递，key为children
  
  <MyNavLink to={path}>Home</MyNavLink> // -> 实际上props里有两个值
  ```

* Switch组件的使用

  * 正常情况下，一般是一个路径对应一个组件
  * 所以我们希望如果已经匹配到合适的组件了，就不再继续往下找了。（提高效率）
  * 可以把所有路由注册包在<Switch>组件里（单一匹配）

  ```react
  <Switch>
    <Route path="/home" componenet={Home} />
    <Route path="/about" componenet={About} />
    <Route path="/about" componenet={Test} />
  </Switch>
  // 路径为 /about时，只渲染了About组件
  ```

* 解决多级路径刷新页面样式丢失的问题

  * 当我们在public/index.html里引入样式时，可能会出现多级路径刷新页面样式丢失的问题
  * 原因是 样式文件的路径没写对，导致请求样式文件时使用了错误的路径（包含了多级路径的部分路径） --> react脚手架在找不到资源是会fallback to index.html
  * 引入样式时，路径不要写 ./  要写成 /
  * 引入样式时，路径不要写 ./  要写成 *%PUBLIC_URL%*  --> 只能在react脚手架的app中使用
  * 使用HashRouter（比较少用）

* 模糊匹配和严格匹配

  * 模糊匹配：`to="/home/a/b"`   `path="/home"` split by '/' then compare
  * 严格匹配：`exact`放到Route组件上
  * 严格匹配不要随便开启 --> 可能会影响二级理由的匹配

* Redirect组件的使用

  * 一般写在路由注册的最下方，当所有Route都无法匹配时，跳转到Redirect指定的路由
  * `<Redirect to="/home">`

* 嵌套路由（多级路由）

  * 路由的匹配是从最前面（最先注册）的路由开始匹配的
  * 注册子路由和定义子路由链接时，to和path属性要写上父路由的path值，比如 /home/new或者/home/messages
  * 不用再包BrowserRouter，因为大家已经处于同一个路由器

* 向路由组件传递参数（三种方法）

  * params参数

    * 很像ajax请求的params参数（直接写在路径后面）

    ```react
    // 在路由链接的路径最后加上需要传递的参数（携带参数） /${id}/${title}
    <Link to=`/home/messages/${id}/${title}`>消息一</Link>
    // 在注册路由时，要声明接收，/:id/:title，id和title是参数的名字，可以不用跟Link中参数名一样
    <Route path="/home/messages/:id/:title" component={Detail} />
    // 在路由组件里接收传递的params参数
    this.props.match.params --> {id: xxx, title: xxx}
    ```

  * search参数

    * 很像ajax请求的query参数(?xxx=xxx&aaa=aaa)

    ```react
    // 在路由链接的路径后面（携带参数） ?id={id}&title={title}
    <Link to=`/home/messages?id={id}&title={title}`>消息一</Link>
    // 在注册路由时，无须声明接收，正常注册即可
    <Route path="/home/messages" component={Detail} />
    // 在路由组件里接收传递的search参数
    this.props.location.search --> ?id={id}&title={title}
    // 接收到search参数之后，还需要去掉开头的？得到urlencoded string，然后借助querystring（有两个重要方法: stringfy 和 parse）来得到参数对象
    querystring.parse(this.props.location.search.slice(1)) --> {id: xxx, title: xxx}
    ```

  * state参数

    ```react
    // 在路由链接里（携带参数） to传递一个对象，在state里传需要传给路由组件的参数
    <Link to={{pathname="/home/messages", state:{id, title}}}>消息一</Link>
    // 在注册路由时，无须声明接收，正常注册即可
    <Route path="/home/messages" component={Detail} />
    // 在路由组件里接收传递的state参数
    this.props.location.state --> {id: xxx, title: xxx}
    // 刷新页面，state参数也不会丢失，因为是记录在history对象里的。（除非清空缓存或其他重置history的行为）
    ```

  * 总结

    * 三种方式都用的比较多，都需要掌握
    * to可以传string，也可以传对象。 `to={{pathname="/home/messages"}}` 和` to="/home/messages"`是等同的。
    * 当你不想把敏感信息暴露在url里时，可以考虑state参数。
    * 相对来说params用的最多

* push vs replace

  * 路由链接默认的是push操作（在history里留下痕迹）
  * 如果想要开启replace模式（无痕），可以直接在路由链接里加**replace**属性，则点击该链接不留下历史记录

* 编程式路由导航(programmatic navigation)

  * 不借助（点击）Link或者NavLink也能实现路由跳转
  * 利用this.props.history上面的api进行编程式路由导航：跳转，前进，回退
  * history.push(pathname, state参数)  push跳转
  * history.replace(pathname, state参数)  replace跳转
  * history.back()  回退
  * history.goForward() 前进
  * history.go(number) 要前进或者后退n步
  * 在使用编程式导航时，不管我们使用哪种方式传递参数（params，search，state），我们都需要统一路由链接、注册路由和接收参数的路由组件的传参方式。

* withRouter的使用

  * 加工一般组件，让一般组件也能使用路由组件拥有的api
  * `withRouter(Header) `  --> 让一般组件Header也拥有了history, location, match 三个props
  * 返回一个新组件

* BrowserRouter vs HashRouter

  * 底层原理不同
    * BrowserRouter用的是H5的history的Api，不兼容IE9及以下版本
    * HashRouter使用的是url的哈希值。（相当于锚点跳转，#后的path不发给服务器，但是会形成历史记录）
  * path表现形式不同
    * `localhost:3000/demo`   vs   `localhost:3000/#/demo`
  * 刷新后对state参数的影响
    * BrowserRouter没有任何影响，因为state保存在history对象中
    * HashRouter在刷新后会导致state参数的丢失（对于其他两种参数，因为保留在url中，所以不会丢失）
  * HashRouter可以用于解决一些路径错误的相关问题（样式文件丢失等）

#### 第六章 React UI组件库

* material-ui
* Antd
  * 基本使用
    * import时，直接写名字的会去node_modules里找
    * 除了官方文档给的code案例，还需要引入样式  `antd/dist/antd.css`
    * 都是已经封装好了 然后可以直接通过不同props值来改变样式的组件
    * 适用于对ui样式要求不是很高 但要求安全稳定且快速搭建的项目
  * antd样式的按需引入 + 自定义主题
    * 直接引入`antd/dist/antd.css`不太合理，文件太大
    * 在不暴露webpack配置文件的前提下修改配置
    * 跟着官网指导一步步来即可
    * 官网也可能有一些写法由于使用的插件更新而出现问题的。可以再去插件的github主页看一下如何配置。
    * 官方指导链接：https://ant.design/docs/react/use-with-create-react-app-cn / https://3x.ant.design/docs/react/use-with-create-react-app-cn

#### 第七章 Redux

* 简介
  * 是什么？
    * 用于做**状态管理**的js库
    * 不止是用于react，只是跟react配合用的比较多
    * 集中式管理react应用中多个组件**共享**的状态
  * 什么情况下需要？
    * 某个组件的状态，需要让其他组件随时可以拿到（共享）
    * 一个组件需要改变另一个组件的状态（状态）
    * 能不用就不用，除非不用会比较吃力

* 工作流程

  ![redux原理图](/Users/hualinfeng/Desktop/react全家桶资料/02_原理图/redux原理图.png)

* 三个核心概念

  * action
    * 动作对象，可由action creator产生
    * 包含两个属性： type 和 data
      * type是string类型，唯一，必要
      * data是要加工的数据，可选，任意类型
    * { type: 'ADD_STUDENT',data:{name: 'tom',age:18} 
    * 同步action vs 异步action

  * reducer
    * 用于初始化(@@redux/INITxxxx)状态、加工状态
    * 加工时，根据旧的state和action， 产生新的state的**纯函数**
  * store --> 最核心的对象
    * 将state、action、reducer联系在一起的对象
    * 如何得到此对象？
      * import {createStore} from 'redux'
      * import reducer from './reducers'
      * const store = createStore(reducer)
    * 此对象的功能?
      * getState(): 得到state
      * dispatch(action): 分发action, 触发reducer调用, 产生新的state
      * subscribe(listener): 注册监听, 当产生了新的state时, 自动调用

* 核心API -- 边写案例边讲解核心API 

  * createstore()
    * 创建包含指定reducer的store对象
  * store对象
    * redux库最核心的管理对象
    * 1)	getState()
      2)	dispatch(action)
      3)	subscribe(listener)
  * applyMiddleware()
    * 应用上基于redux的中间件(插件库)
  * combineReducers()
    * 合并多个reducer函数

* 编写案例

  * 纯react版本

  * redux精简版（没有action creator，并只有一个状态）

    * 学习了几个API以及如何写reducer，如何分发动作驱使状态改变，如何监听状态改变并引起重新渲染
    * createStore(reducer)
    * store.dispatch({type: 'xxx', data: {xxx}}) --> 分发动作对象
    * store.getState() --> 得到状态
    * store.subscribe(() => {}) --> 监听状态改变，接受一个回调函数

    ```react
    // ~/redux/store.js  注意路径要合理
    import {createStore} from 'redux';
    import countReducer from './count_reducer';
    
    export default createStore(countReducer); // 创建一个store，传入的是为其服务的reducer
    ```

    ```react
    // ~/redux/count_reducer.js
    export default function(prevState = 0, action) {
      // store在初始化的时候，会给reducer发一个action，action type是@@redux/INITxxxx，此时的prevState是undefined
    	const {type, data} = action;
      
      switch(type) {
        case 'increment':
          return prevState + data;
        case 'decrement':
          return prevState - data;
        default:
          return prevState;
      }
    };
    ```

    ```react
    // Count组件
    import React from 'react';
    import store from '../redux/store';
    
    class Count extends React.Component {
      onIncrement = () => {
        store.dispatch({type: 'increment', data: 1}); //此处忽略在select取值
      };
      
    	onDecrement = () => {
        store.dispatch({type: 'decrement', data: 1});//此处忽略在select取值
      }
      
      componentDidMount() {
        store.subscribe(() => this.forceUpdate()); // 监听 redux state变化，引发重新渲染，或者this.setState({})
        // 或者可以在App组件写一下代码，就不用每个component去写监听
        // store.subscribe(() => ReactDOM.render(<App />, xxxxx));
    		// redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己来写
      }
      
      render() {
        return <div>当前和为： ${store.getState()}</div>;  //此处忽略所有button
      }
    }
    ```

  * redux完整版（接着上一个版本）

    * 引入两个文件

      * constants.js --> 用于储存action的type名字，便于管理和维护，以及方式拼写错误
      * action_creator文件 --> 用于编写action creators来创建action对象。这样之后分发对象的时候不用再手动去写action对象，直接调用creator即可。（后续还有async的creators）

      ```javascript
      // ~/reudx.count_reducer.js
      import {INCREMENT, DECREMENT} from './constants';
      
      export const increment = data => ({type: INCREMENT, data});
      export const decrement = data => ({type: DECREMENT, data}); 
      // 调用时直接 store.dispatch(decrement(data))即可
      ```

  * 异步action

    * 如果action是一个Object  --> 同步action
    * 如果**action是一个function**  --> 异步action
    * Action must be plain objects, use custom middleware for async actions. --> 引入**redux-thunk**
    * 通知store帮我执行一下这个function即可，而不是像同步action一样直接让reducer处理（异步action里最后还是会store.dispatch(object) --> 调用同步action）
    * 将react-redux作为middleware引入到store中（配置）

    ```react
    // ~/redux/store.js
    import {createStore, applyMiddleware} from 'redux';
    import thunk from 'redux-thunk';
    import countReducer from './count_reducer';
    
    export default createStore(countReducer, applyMiddleware(thunk));//将react-redux作为middleware引入到store中
    ```

    * 并不是必须要用的，也可以在组件那里做异步

    ```react
    // ~/reudx.count_reducer.js
    import {INCREMENT, DECREMENT} from './constants';
    
    export const increment = data => ({type: INCREMENT, data});
    export const decrement = data => ({type: DECREMENT, data}); 
    // 调用时直接 store.dispatch(decrement(data))即可
    
    export const incrementAsync = (data, time) => dispatch => {
      setTimeOut(() => increment(data), time)
    }; // store会给你传dispatch（其实还有getState为第二个参数），不需要自己去通过store调用api
    ```


##### **react-redux**

* react的插件库

* 容器组件和UI组件

  * 父子关系
  * 容器组件和redux打交道，可以随意使用redux的api （**UI不能**），我们之前的案例都是UI组件直接调用store的api
  * 容器通过props传给UI组件： 1. redux中保存的状态 2. 用于操作状态的方法

* 连接容器组件和UI组件

  * 需要通过`connect`进行容器组件与UI组件和store的连接(connect --> 非常核心的api)
  * 容器组件与store的连接不是在容器组件本身发生的，而需要**通过props的方式把store传进去**

  ```react
  // ~/containers/Count/index.jsx
  import CountUI from '../../components/Count';
  import {connect} from 'react-redux;
  
  export default connect()(CountUI); // 要在第二个括号传入UI组件，第一个括号还没讲
  
  // 同时要在调用Count容器组件的地方把store作为props传入，否则并没有跟store连接（之后会讲Provider）
  <Count store={store} />
  ```

* 基本使用

  * 要用到connect的第一个括号，容器组件与redux沟通得到状态和改变状态的方法，通过props传给UI组件： store状态和改变状态的方法
  * 两个参数为mapStateToProps和mapDispatchToProps
  * 可以对比以下的代码(通过react-redux构建容器组件和UI组件)和之前的redux完整版代码(直接在UI组件里调用store的api)

  ```react
  // ~/containers/Count/index.jsx
  import CountUI from '../../components/Count';
  import {connect} from 'react-redux;
  import {increment, decrement, incrementAsync} from '../../redux/count_actions';
  
  // mapStateToProps要返回一个object(react-redux要求的，如果不这样写，如何通过this.props获得？？)
  // 用于传递状态
  // 此处的state，react-redux已经为我们准备好了，实际就是store.getState()
  const mapStateToProps = state => ({count: state});
  // mapDispatchToProps(react-redux要求的，如果不这样写，如何通过this.props获得？？)
  // 用于传递操作状态的方法
  // 此处的dispatch就是store.dispatch
  const mapDispatchToProps = dispatch => ({
    increment: data => dispatch(increment(data)),
    decrement: data => dispatch(decrement(data)),
    incrementAsync: (data, time) => dispatch(incrementAsync(data, time)),
  })
  
  export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
  // 在UI组件可以直接通过this.props直接获得count, increment, decrement, incrementAsync
  // 相当于 <CountUI count={count} increment={increment} decremen={decrement} incrementAsync={incrementAsync} />
  ```

* 优化现有代码

  * 简写mapDispatchToProps

    mapDispatchToProps可以接受一个函数，也可以接受一个object

    此api底层逻辑会判断你传入的是function还是object，然后做对应的事

    ```react
    export default connect(
      state => ({count: state}),
      {
        increment, // key是传递给UI组件的props里的值，value是需要调用的action creator
        decrement,
        incrementAsync
      } // 省去很多dispatch
    )(CountUI);
    ```

  * 不用再自己去store.subscribe了，在使用react-redux的connect构建容器组件后，容器组件已拥有了监听store变化的能力。 （具体可以参考connect的源码实现）

  * Provider组件的使用

    * 给所有包裹在Provider下的容器组件传store(不用再一个个去给容器组件传store)

    ```react
    import React from 'react';
    import ReactDOM from 'react-dom';
    import {Provider} from 'react-redux';
    import App from '.App';
    import store from './redux/store';
    
    ReactDOM.render(
    	<Provider store={store}>
      	<App />
      </Provider>,
      document.getElementById('root')
    );
    ```

  * 整合容器组件和UI组件

    * 不用分开在两个文件里写（现在是分别在components和containers里写的）

* 数据共享

  * 编写person组件和对应的action creator和reducer

  * 多个状态需要管理时？（多个reducers为store服务时）

    * redux store应该存成key-value pairs
    * 需要合并reducers  --> 通过combineReducers进行合并

    ```react
    // ~/redux/store.js
    import {createStore, applyMiddleware, combineReducers} from 'redux';
    import thunk from 'redux-thunk';
    import countReducer from './count_reducer';
    import personReducer from './person_reducer';
    
    const combinedReducers = combineReducers({
      count: countReducer,
      person: personReducer
    });
    //此时我们的store应该是这种状态：  {count: 0, person: [{xxxxx}]}  --> object
    
    export default createStore(combinedReducers, applyMiddleware(thunk));//将react-redux作为middleware引入到store中
    ```

    * 合并后的总状态是一个对象！！（不再是之前那个一个reducer时的数字）
    * 在mapStatetoPtops中取状态时，注意state已经是一个对象了。

* 纯函数

  * redux在决定要不要更新store的时候，是对前后state的浅比较 --> 注意array，object这种存的是地址（所以一般不用shift，unshift这种方法）

    一般会用到[...prevState, {xxx}], arr.map, arr.reduce, arr.filter等array相关的方法，会返回一个新的array

    对于object，会通过深拷贝产生新的object去改变需要改变的属性 -->  {...obj, abc: 0}

  * **redux的reducer必须是一个纯函数**

  * 纯函数：只要是相同的输入，必定得到相同的输出

  * 必须遵守：

    * 不能修改任何参数数据
    * 不会产生副作用（side effects），比如网络请求，输入输出的设备
    * 不能调用Date.now() 或者Math.random()等不纯的方法


##### redux开发者工具

* 下载redux devtool

* 安装一个库  `redux-devtools-extension`

* 在store文件中进行配置

  ```react
  // ~/redux/store.js
  import {createStore, applyMiddleware, combineReducers} from 'redux';
  import thunk from 'redux-thunk';
  import {composeWithDevTools} from 'redux-devtools-extension';
  import countReducer from './count_reducer';
  import personReducer from './person_reducer';
  
  const combinedReducers = combineReducers({
    count: countReducer,
    person: personReducer
  });
  
  export default createStore(combinedReducers, composeWithDevTools(applyMiddleware(thunk)));
  // 如果我们没有使用中间件的话，可以直接第二个参数传 composeWithDevTools()
  ```

* 可以跳到之前的某个状态；可以看action的内容、store的状态、每个action的diff等；可以进行重播；可以模拟dispatch一个动作等等用途

* 最终版本

  * 一般将所有reducers在reducers/index.js汇总，然后在store里只传入最终汇总好的总reducer
  * 命名优化 --> 文件名，方法的名字，对象的key 

* 项目打包

  * npm build --> 脚手架的项目打包命令
  * 部署到服务器上运行
    * Node.js / java --> 工作中的情况
    * serve
      * 可以在本地开启一台服务器上线项目
      * 先全局安装serve
      * 命令行： `serve build` build作为根目录要上线的文件夹

#### 第八章 react拓展

##### setState有两种写法

* setState引起react更新状态的动作是异步的。
* setState(stateChange, [callback])------对象式的setState
  * stateChange为状态改变对象(该对象可以体现出状态的更改)
  * callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
* setState(updater, [callback])------函数式的setState
  * updater为返回stateChange对象的函数
  * updater可以接收到state和props
  * callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用
* 总结
  * 对象式的setState是函数式的setState的简写方式(语法糖)
  * 如果新状态不依赖于原状态 ===> 使用对象方式；如果新状态依赖于原状态 ===> 使用函数方式。不是个绝对的原则。
  * 如果需要在setState()执行后获取最新的状态数据， 要在第二个callback函数中读取（更新状态的动作是异步的） --> 马上查看this.state是不会变的

##### lazyLoad

* 比较多做懒加载的就是**路由组件**
* 不用懒加载的话，即使路由组件并不需要被展示，也已经被加载了。
* lazy()
* Suspense组件的使用

```react
import React, {lazy, Suspense} from 'react';
import Loading from 'xxx';

//1.通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
const Login = lazy(()=>import('@/pages/Login'))
	
//2.通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
// 不写会报错
<Suspense fallback={<Loading />}>
   <Switch>
      <Route path="/xxx" component={Login} />
      <Redirect to="/login" />
   </Switch>
</Suspense>
```

##### React Hooks

* 是什么？

  React 16.8.0版本增加的新特性/新语法，可以让你在函数组件中使用 state 以及其他的 React 特性

* 三个常用的hook

  * State Hook: React.useState()
  * Effect Hook: React.useEffect()
  * Ref Hook: React.useRef()

* State hook

  * State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作
  * 语法: const [xxx, setXxx] = React.useState(initValue)  
  * useState()说明
    * 参数: 第一次初始化指定的值在内部作缓存（随着state变化，会多次call函数组件，但是count值不会被覆盖）
    * 返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
  * setXxx(newValue)：参数为非函数值, 直接指定新的状态值, 内部用其**覆盖原来的状态值**； setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其**覆盖原来的状态值**

* Effect Hook

  * 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)

  * React中的副作用操作:

    * 发ajax请求数据获取
    * 设置订阅 / 启动定时器
    * 手动更改真实DOM

  * 语法：

    ```react
    useEffect(() => { 
      // 在此可以执行任何带副作用操作 --> componentDidMount + 有条件下的componentDidUpdate
      return () => { // 在组件卸载前执行 --> 相当于componentWillUnmount
      }
    }, [stateValue]) 
    // 第二个参数是需要监听的state和props --> 在这个数组中的state或props变化了才会执行useEffect中的方法
    // 如果指定的是[], 回调函数只会在第一次render()后执行（只有componentDidMount）
    // 如果不传第二个参数，则表示监听所有state和props
    ```

  * 可以把 useEffect Hook 看做如下三个函数的组合

    * componentDidMount()
    * componentDidUpdate()
    * componentWillUnmount() 

* Ref Hook

  * 可以在函数组件中存储/查找组件内的标签或任意其它数据
  * 语法: `const refContainer = useRef()`
  * 作用:保存标签对象,功能与React.createRef()一样。 `refContainer.current.value`

##### Fragment

* 频繁用div去包裹组件会导致层级太复杂  --> 无用的div太多
* 用Fragment去包裹，可以不用必须有一个真实的DOM根标签了，也能符合jsx的语法要求
* `<Fragment><input xxx /><input xxx /><Fragment />`
* Fragment 标签只能接受key属性
* 也可以用空标签<></> 但不能接受任何属性

##### Context

* 常用于【祖组件】与【后代组件】间通信

* 使用语法 --> 跟之前react-redux里那个Provider有些类似

  * 创建Context容器对象：

    `const xxxContext = React.createContext()`

  * 渲染子组时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据：

    ```react
    <xxxContext.Provider value={数据}>  ==> 不止一个数据时可以用object
    		子组件
    </xxxContext.Provider>
    ```

  * 后代组件读取数据： （两种方式）

    * 第一种方式:仅适用于类组件 

      ```react
      static contextType = xxxContext  // 声明接收context，如果只需要接受部分context，可以这样 --> static contextType = {userName: PropTypes.string};
      this.context // 读取context中的value数据
      ```

    * 第二种方式: 函数组件与类组件都可以

      ```react
      <xxxContext.Consumer>
        {
          value => ( // value就是context中的value数据
            要显示的内容
          )
        }
      </xxxContext.Consumer>
      ```

* 在应用开发中一般不用context, 一般都用它的封装react插件

##### 组件优化

* React.Component 有两个问题

  * 只要执行setState(),即使不改变状态数据, 组件也会重新render() ==> 效率低
  * 只当前组件重新render(), 就会自动重新render子组件，纵使子组件没有用到父组件的任何数据 ==> 效率低

* 原因： Component中的shouldComponentUpdate()总是返回true

* 什么是效率高的做法

  * 只有当组件的state或props数据发生改变时才重新render()

* 解决办法

  * 重写shouldComponentUpdate()方法

    * 比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false

    * 但是如果有很多state或者props需要比较时，可能就比较麻烦

  * 使用React.PureComponent 代替React.Component 

    * PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true

    * 注意点：

      * 只是进行state和props**数据的浅比较**, 如果只是数据对象内部数据变了, 返回false 

        比如 a.xx = 'new', setState({a}) --> 不会re-render

      * 不要直接修改state数据, 而是要产生新数据  --> 跟redux里的reducer一样，要返回新数据

  * 项目中一般使用**PureComponent**来优化

##### render props

* 标签体内容是个特殊的属性，key为children
* html标签的标签体内容会自己展示，自己写的component的标签体内容需要通过this.props.children来展示

* 如何向组件里动态传入带内容的标签？

  * children props --> 通过组件标签体传入结构

    ```react
    <A>
      <B>xxxx</B>
    </A>
    
    // 然后在A组件里通过以下语句渲染B组件
    {this.props.children}
    ```

    * 如果B组件需要A组件内的数据？==> 做不到 

  * render props --> 通过组件标签属性传入结构，而且可以携带数据，一般用render函数属性

    ```react
    <A render={(data) => <C data={data}></C>} />
    
    // 在A组件中通过以下语句渲染C组件
    {this.props.render(内部state数据)}
    // 在C组件中通过以下props获取A组件的state数据
    {this.props.data}
    ```

##### 错误边界(Error boundary)

* 将错误限制在发生错误的组件。 用来捕获<u>**后代组件**</u>错误，渲染出备用页面。

* 只能捕获后代组件<u>**生命周期产生的错误**</u>，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误

* 需要在发生错误的组件的**父组件做操作**：getDerivedStateFromError配合componentDidCatch

  ```react
  class Parent extends React.Component {
    state = {
      hasError: false
    };
  	static getDerivedStateFromError(error) { // 一旦后代组件报错，就会触发
      console.log(error);
      // 在render之前触发
      // 返回新的state
      return {hasError: true};
    }
  	componentDidCatch(error, info) { // 可不用
      // 通常在此处统计页面的错误。发送请求发送到后台去
    }
  	render() {
      return (
      	<div>
        	<h1>xxxx</h1>
          {this.state.hasError ? <Child /> : <div>发生错误，请稍后再试</div>}
        </div>
      );
    }
  }
  
  class Child extend React.Component {
    this.state = {
      data: 'xxxx' // incorrect data!
    };
    render() {
      return this.state.data.map(xxxx); // throw error here
    }
  }
  ```

##### 组件通信方式总结

* 组件间的关系
  * 父子组件
  * 兄弟组件（非嵌套组件）
  * 祖孙组件（跨级组件）
* 几种通信方式
  * props：
    		(1).children props
      		(2).render props
  * 消息订阅-发布：pubs-sub、event等等
  * 集中式管理：redux、dva等等
  * context：生产者-消费者模式
* 比较好的搭配方式：
  * 父子组件：props
  * 兄弟组件：消息订阅-发布、集中式管理
  * 祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)



#### 自学补充

##### React Hooks(其余)

* introduction

  * Motivation

    * It’s hard to reuse <u>stateful logic</u> between components
      * Hooks allow you to reuse stateful logic without changing your component hierarchy
      * build your own hooks
    * Complex components become hard to understand
      * Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data)
      * use the effect hook
    * Classes confuse both people and machines

  * what is hooks?

    Hooks are functions that let you “hook into” React **state and lifecycle** features from function components.

    built-in Hooks +  create your own Hooks to reuse *stateful behavio*r between different components

  * when to use hooks?

    If you write a function component and realize you need to add some state to it, previously you had to convert it to a class. Now you can use a Hook inside the existing function component.

    In function components, we have no `this`.

* useState

  * the current state value and a function that lets you update it
  * initial value only used for first render
  * can declare multiple states by using the State Hook more than once

  官网原话讲解：https://reactjs.org/docs/hooks-state.html

  updating a state variable always *replaces* it instead of merging it.

* useEffect

  * side effects / effects

    * data fetching, subscriptions, or manually changing the DOM from React components
    * can affect other components and can’t be done during rendering

  * adds the ability to perform side effects from a function component

  * can use more than a single effect in a component

  * telling React to run your “effect” function after flushing changes to the DOM (can skip if passing the second params)

  * optionally specify how to “clean up” after them by returning a function

  * effects which don't need a clean up:

    * Network requests, manual DOM mutations, and logging....

  * effects which may need a clean up:

    * Subscriptions --> avoid memory leak
    * ...

  * https://reactjs.org/docs/hooks-effect.html#example-using-hooks

  * Every time we re-render, we schedule a different effect, replacing the previous one.

  * The majority of effects don’t need to happen synchronously. For some uncommon cases, they do. (such as measuring the layout --> useLayoutEffect)

  * https://reactjs.org/docs/hooks-effect.html#tips-for-using-effects

    * Use Multiple Effects to Separate Concerns （把相关的写在同一个effect里）--> Hooks let us split the code based on what it is doing rather than a lifecycle method name.

    * Why Effects Run on Each Update

      *  the effect cleanup phase happens after every re-render, and not just once during unmounting  --> this design helps us create components with fewer bugs.

      * It cleans up the previous effects before applying the next effects. --> avoid bugs(props & state changes(updates) both trigger effects)  --> This behavior ensures consistency by default and prevents bugs that are common in class components due to missing update logic.

      * 可以参考官网的subsequence illustration

        ```javascript
        // Mount with { friend: { id: 100 } } props
        ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // Run first effect
        
        // Update with { friend: { id: 200 } } props
        ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // Clean up previous effect
        ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // Run next effect
        
        // Update with { friend: { id: 300 } } props
        ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // Clean up previous effect
        ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // Run next effect
        
        // Unmount
        ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // Clean up last effect
        ```

    * Optimizing Performance by Skipping Effects

      * You can tell React to *skip applying an effect if certain values haven’t changed* between re-renders. To do so, pass an array as an optional second argument to useEffect
      * In the future, the second argument might get added automatically by a build-time transformation.
      * make sure the array **includes all values** from the component scope (such as props and state) that **change over time** and that are **used by the effect**. Otherwise, your code will reference stale values from previous renders
      * While passing [] as the second argument is closer to the familiar componentDidMount and componentWillUnmount.
      * React defers running useEffect until *after the browser has painted*.
      * More the learn: [using function in effects](https://reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies) and [dependencies changing too often](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often)

* Rules of hooks

  * Only call Hooks **at the top level**. Don’t call Hooks inside loops, conditions, or nested functions.
  * Only call Hooks **from React function components**(Or your own custom Hooks)
  * There is a lint plugin to enforece these rules automatically.
  * React relies on the order in which Hooks are called

* Build your own hooks

  * reuse some stateful logic between components

    * Trditionally, higher-order components and render props
    * Custom Hooks --> without adding more components to your tree

  * Example:

    ```react
    // extract the stateful logic into a custom Hook
    import React, { useState, useEffect } from 'react';
    
    function useFriendStatus(friendID) {
      const [isOnline, setIsOnline] = useState(null);
    
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
    
      useEffect(() => {
        ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
        return () => {
          ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
        };
      });
    
      return isOnline;
    }
    ```

    ```react
    // reuse it in FriendStatus component
    function FriendStatus(props) {
      const isOnline = useFriendStatus(props.friend.id);
    
      if (isOnline === null) {
        return 'Loading...';
      }
      return isOnline ? 'Online' : 'Offline';
    }
    ```

    ```react
    // reuse it in FriendListItem component
    function FriendListItem(props) {
      const isOnline = useFriendStatus(props.friend.id);
    
      return (
        <li style={{ color: isOnline ? 'green' : 'black' }}>
          {props.friend.name}
        </li>
      );
    }
    ```

  * The state of each component is completely independent. Hooks are a way to reuse *stateful logic*, **not state itself**. In fact, each *call* to a Hook has a **completely isolated state**.

  * If a function’s name starts with ”`use`” and it calls other Hooks, we say it is a **custom Hook**

  * a wide range of **use cases**:

    * form handling
    * animation
    * declarative subscriptions
    * timers
    * more......

  * Custom Hooks are a convention that naturally follows from the design of Hooks, rather than a React feature.

  * Tips

    * Pass Information Between Hooks --> [link](https://reactjs.org/docs/hooks-custom.html#tip-pass-information-between-hooks)

* Other hooks

  * useContext

    ```javascript
    const value = useContext(MyContext);
    ```

    * subscribe to React context without introducing nesting
    * Accepts a context object (the value returned from React.createContext) and returns the current context value for that context. 
    * Even if an ancestor uses React.memo or shouldComponentUpdate, a rerender will still happen starting at the component itself using useContext.
    * **useContext(MyContext) is equivalent to static contextType = MyContext in a class, or to <MyContext.Consumer>.**
      * React.createContext(defaultValue); 只有当组件所处的树中没有匹配到 Provider 时，其 defaultValue 参数才会生效。

  * useReducer

    * manage **local state** of complex components with a reducer(not using Redux, but similar ideas)
    * `const [state, dispatch] = useReducer(reducer, initialArg, init);`
      * setState的代替者
      * 第一个参数reducer跟redux那个一样，(state, action) => newState， 通过dispatch来更改state
      * 两种初始化状态的方法
        * 传第二个参数
        * 传第三个参数，是一个函数，接受第二个参数为输入，输出为初始状态。init(initialArg)
    * Preferrable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one.
    * 并且只需传递dispatch而不用传递其他回调函数来改变该组件的状态
    * React guarantees that dispatch function identity is stable and won’t change on re-renders.

  * useCallback

    * Memorized
      * an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and **returning the cached result when the same inputs occur again.**
    * Returns a memorized **callback**

    ```javascript
    const memoizedCallback = useCallback(
      () => {
        doSomething(a, b);
      },
      [a, b],
    );
    ```

    * only changes if one of the dependencies has changed
    * `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`.
    * Every value referenced inside the callback should also appear in the dependencies array. In the future, a sufficiently advanced compiler could create this array automatically.

  * useMemo

    * return a memorized **value**

    ```javascript
    const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
    ```

    * only recompute the memoized value when one of the dependencies has changed. --> avoid expensive calculations on every render.
    * In the future, React may choose to “forget” some previously memoized values and recalculate them on next render, e.g. to free memory for offscreen components.

  * useRef

    * returns a mutable **ref object** whose .current property is initialized to the passed argument (initialValue). The returned object will **persist for the full lifetime of the component**.
    * 本质上，是一个在.current属性可以拥有mutable value的容器
    * access the DOM
    * 也可以用来储存任意可变的值。类似类组件中的instance fields
    * useRef doesn’t notify you when its content changes. Mutating the .current property doesn’t cause a re-render.
    * [callback ref](https://reactjs.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node)

  * useImperativeHandle

    * https://reactjs.org/docs/hooks-reference.html#useimperativehandle

  * useLayoutEffect

    * fires synchronously after all DOM mutations. Use this to read layout from the DOM and synchronously re-render. Updates scheduled inside useLayoutEffect will be flushed synchronously, before the browser has a chance to paint.
    * 建议将修改 DOM 的操作里放到 useLayoutEffect 里，而不是 useEffect --> 减少浏览器重绘的次数，提高性能

    * https://reactjs.org/docs/hooks-reference.html#uselayouteffect

  * useDebugValue

    * be used to display a label for *custom hooks* in React DevTools





* 高阶组件
* Portals
* Profiler

