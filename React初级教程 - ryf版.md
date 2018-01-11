```
                                 ___                   __
  ________  _____ _____    _____/  |__           _    / /      _____    ___
  \__  _  \/ __  \\__  \ _/ ___\   ___\  __  __ | |  / /__    /___  \  / _ \
    | / \/ | ___/ / ___ \ \ \___|  |_    \ \/ / | | |  \  | _  __/ _/ | |_| |
    |_|    \____//_/   \_\ \___/ \___|    \__/  |_|  \___/ |_|/_____\  \___/

```
## React入门教程
> 作    者：Henery <br/>
> 邮    箱：henery_002@163.com <br/>
> 参考文章：http://www.ruanyifeng.com/blog/2015/03/react、http://www.runoob.com/react/react-tutorial.html

### 目录
1. [HTML模板](#1-html模板)
2. [JSX语法](#2-jsx语法)


### 1. HTML模板
React页面源码的大致结构为：
```html
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      //...
    </script>
  </body>
</html>
```
需要注意的是：
1. 最后一个 script 标签的 type 属性为 text/babel 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel" 。
2. 上面代码一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，react.js 是 React 的核心库，react-dom.js 是提供与 DOM 相关的功能，Browser.js 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

### 2. JSX语法
JSX是JavaScript的一种语法扩展，它可以将html语言直接写在js语言中，例如JSX使用这种语法声明变量：
```javascript
const elememt = <h1>Hello world!</h1>;
```
还可以在JSX中使用js表达式，表达式要包含在大括号中：
```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

经过编译之后，JSX会被转化为普通的js对象。也就是说可以在 if 或者 for 语句里使用 JSX，将它赋值给变量，当作参数传入，作为返回值：
```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```
#### 2.1 JSX属性
可以使用引号来定义以字符串为值的属性，也可以使用大括号来定义以 JavaScript 表达式为值的属性：
```javascript
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```
需要注意的是，使用了大括号包裹的 JavaScript 表达式时不能再到外面套引号。JSX 会将引号当中的内容识别为字符串而不是表达式。
#### 2.2 JSX嵌套
JSX 标签同样可以相互嵌套：
```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```
> 因为 JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。
> 例如，class 变成了 className，而 tabindex 则对应着 tabIndex.


### 3. 元素渲染
要将元素添加到DOM中，首先在一个 HTML 页面中添加一个 id="root" 的 div:
```html
<div id="root"></div>
```
在此 div 中的所有内容都将由 React DOM 来管理，所以我们将其称之为 “根” DOM 节点。

我们用React 开发应用时一般只会定义一个根节点。但如果你是在一个已有的项目当中引入 React 的话，你可能会需要在不同的部分单独定义 React 根节点。

要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上：
```javascript
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
上述代码会将一个h1标签插入到root节点上展示“Hello world”内容。

### 4. 组件 & props
React中的组件有两种定义方式：函数定义、ES6 class。
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该种方式直接使用js函数定义组件。该函数是一个有效的React组件，它接收一个单一的“props”对象并返回了一个React元素。
项目中常使用ES6 class定义组件：
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
#### 4.1 组件的渲染
React元素除了使用常用的DOM标签，还可以是用户自定义的组件
```javascript
const element = <Welcome name="Sara" />;
```
当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件,这个对象称之为“props”。
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
> 注意：组件名称必须以大写字母开头。例如：<div /> 表示一个DOM标签，但 <Welcome /> 表示一个组件，并且在使用该组件时你必须定义或引入它。

#### 4.2 Props属性
组件可以在它的输出中引用其它组件，这就可以用同一组件来抽象出任意层次的细节。在React应用中，按钮、表单、对话框等通常都被表示为组件。
可以往组件中加入任何属性，比如 <Welcome name="Sara"> ，就是 Welcome 组件加入一个 name 属性，值为 Sara。
组件的属性可以在组件类的 this.props 对象上获取，比如 name 属性就可以通过 this.props.name 读取。
例如，创建一个App组件，用来多次渲染Welcome组件。<br/>
```javascript
class Welcome extends React.component{
  return <h1>Hello, {this.props.name}</h1>;
}

class App extends React.component{
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
> 注意：组件的返回值只能有一个根元素。因此需要用一个div包裹所有Welcome元素。

### 5. State & 生命周期
#### 5.1 State
React把组件看成一个状态机（State Machines）。通过与用户交互，实现不同状态的转换，然后渲染UI，让用户界面和数据保持一致。<br/>
查看下面的例子：
```javascript
class LikeButton extends React.component{
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({
      liked: !this.state.liked
    });
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>You {text} this. Click to toggle.</p>
    );
  }
});

ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);
```
上面定义了一个 LikeButton 组件，它的 getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。当用户点击组件，导致状态变化，this.setState 方法就修改状态值，每次修改以后自动调用 this.render 方法，再次渲染组件。

> 由于 this.props 和 this.state 都用于描述组件的特性，可能会产生混淆。一个简单的区分方法是，this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。

#### 5.2 生命周期
组件的生命周期可分为三个状态：
- Mounting：已插入真实DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实DOM

三种状态共计五中处理函数：
- componentWillMount： 在组件将要渲染前调用，在客户端也在服务端。
- componentDidMount： 在第一次渲染后调用，只在客户端。
之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。如果想和其他js框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。
- componentWillReceiveProps： 在组件接收到一个新的prop时被调用。这个方法在初始化render时不会被调用。
- componentDidUpdate： 在组件完成更新后立即调用。在初始化时不会被调用。
- componentWillUnmount： 在组件从 DOM 中移除的时候立刻被调用。

此外，React还提供了两种特殊状态的处理函数：
- shouldComponentUpdate： 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
- componentWillUpdate： 在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

参考以下示例：
```javascript
class Hello extends React.component{
  getInitialState: function () {
    return {
      opacity: 1.0
    };
  },

  componentDidMount: function () {
    this.timer = setInterval(function () {
      var opacity = this.state.opacity;
      opacity -= .05;
      if (opacity < 0.1) {
        opacity = 1.0;
      }
      this.setState({
        opacity: opacity
      });
    }.bind(this), 100);
  },

  render: function () {
    return (
      <div style={{opacity: this.state.opacity}}>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <Hello name="world"/>,
  document.body
);
```
上面代码在hello组件加载以后，通过 componentDidMount 方法设置一个定时器，每隔100毫秒就重新设置组件的透明度，从而引发重新渲染。
> 注意：该例中，组件的style属性的设置方式不能写成：style="opacity:{this.state.opacity};"，应该写成：style={{opacity:this.state.opacity}}
> 因为 React 组件样式是一个对象，所以第一层大括号表示这是 js 语法，第二层大括号表示样式对象。

### 6. Refs & DOM
在React中，组件并不是真实的DOM节点，而是存在于内存中的一种数据结构，叫做虚拟DOM（virtual DOM）。只有当它插入文档之后才会成为真实DOM。
根据React的设计，所有的DOM变动，都先在虚拟DOM上发生，然后再将实际发生变动的部分反应在真实DOM上，这种算法叫 [DOM diff](https://calendar.perfplanet.com/2013/diff/)。它能够极大提高网页的性能。

但是有时需要从组件获取到DOM节点，就需要用到 ref 属性。

使用时，先将一个ref属性绑定到render返回值上：
```html
<input ref="myInput" />
```
在其他部分代码中，通过 this.refs 获取属性
```javascript
var input = this.refs.myInput;      //input为DOM对象
var inputValue = input.value;
```
查看完整实例：
```javascript
class MyComponent extends React.componet{
  handleClick: function() {
    // 使用原生的 DOM API 获取焦点
    this.refs.myInput.focus();
  },
  render: function() {
    //  当组件插入到 DOM 后，ref 属性添加一个组件的引用到 this.refs
    return (
      <div>
        <input type="text" ref="myInput" />
        <input
          type="button"
          value="点击"
          onClick={this.handleClick}
        />
      </div>
    );
  }
}
 
ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
)
```
此外，也可以使用 [**getDOMNode()**](https://segmentfault.com/q/1010000006198939) 方法获取DOM元素。

更多关于 Refs 的内容，参见[官方文档](https://doc.react-china.org/docs/refs-and-the-dom.html)。

> 注意：ref 属性无法应用在函数式组件上，因为函数式组件没有实例，应该将其转化为 class 组件。

### 7. PropTypes类型检查
> 注意：React.PropTypes 自 React v15.5 起已弃用。请使用 prop-types 库代替。

#### 7.1 PropTypes
我们知道，组件的属性可以接受任意类型的值，如字符串、对象、函数。
这就需要一种机制来验证当使用组件时提供的参数是否符合要求。组件的PropTypes属性就是用作验证组件实例的属性是否符合要求的。

要检查组件的属性，就需要配置特殊的propTypes属性：
```javascript
import PropTypes from 'prop-types';
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

//类型检查
Greeting.propTypes = {
  name: PropTypes.string
};
```
该例中使用了PropTypes.string，它规定name属性必须是字符串类型值，当给属性传递了错误类型的值后js控制台会打印警告信息：
```javascript
Warning: Failed propType: Invalid prop `name` of type `string` supplied to `Greeting`, expected `string`.
```
其他使用类型检查验证器的例子：
```javascript
import PropTypes from 'prop-types';
MyComponent.propTypes = {
  // 可以将属性声明为以下 JS 原生类型
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
  optionalNode: PropTypes.node,

  // 一个 React 元素
  optionalElement: PropTypes.element,

  // 也可以声明属性为某个类的实例，这里使用 JS 的 instanceof 操作符实现。
  optionalMessage: PropTypes.instanceOf(Message),

  // 也可以限制属性值是某个特定值之一
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 限制它为列举类型之一的对象
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个指定元素类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 一个指定类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个指定属性及其类型的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 也可以在任何 PropTypes 属性后面加上 `isRequired` 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,
};
```

#### 7.2 属性默认值
还可以使用 defaultProps 为组件props定义默认值：
```javascript
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 为属性指定默认值:
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染 "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```
这样一来，如果name属性未被赋值的话，它将会有默认值。

> 注意：defaultProps 用来确保 this.props.name 在父组件没有特别指定的情况下，有一个初始值。类型检查发生在 defaultProps 赋值之后，所以类型检查也会应用在 defaultProps 上面。

### 8. 列表 & Keys





### 9. 事件处理


### 10. 条件渲染


### 11. 表单


### 12. 组合 & 继承

